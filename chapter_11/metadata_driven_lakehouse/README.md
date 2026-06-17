# AquaShack-Medallion

A pico-example of a meta-data driven lakehouse for Microsoft Fabric, forked from
[AquaShack](https://github.com/ChristianHenrikReich/AquaShack).

> **Status:** the upstream [AquaShack](https://github.com/ChristianHenrikReich/AquaShack)
> project is under code freeze. AquaShack-Medallion is where I will be
> implementing my ideas about building lakehouse solutions going forward.

Two differences from the original:

1. The three layers are renamed to the classic medallion names: **Bronze**,
   **Silver**, and **Gold** (previously Landing, Base, Curated).
2. The meta data schema supports a `custom_transform` property. When set, the
   Bronze to Silver stage dispatches to a named custom function instead of
   running the default dedupe + fill-nulls flow. This is the extension point
   for entities that need shaping the default flow does not cover.

## Installation

`Setup.ipynb` is a single-script installer; run it end to end and everything
needed is provisioned.

## Documentation

Medallion architecture, three lakehouses:

<img width="800" height="450" alt="image" src="https://github.com/user-attachments/assets/e3a30981-9a2a-409d-bafa-1f79d9d026e1" />

- **Bronze**: data in original format where possible. Relational sources land
  as parquet. No Delta, no re-writing, no querying.
- **Silver**: aligned, cleaned data. All Delta tables.
- **Gold**: serving layer. Business logic, star schemas, etc.

Notebooks:

- `1_AquaShack_Bronze_To_Silver.ipynb` moves meta-data-described entities from
  Bronze to Silver. Default flow is dedupe + fill nulls. If an entity has a
  `custom_transform` in the meta data, that function runs instead.
- `2_AquaShack_Silver_To_Gold.ipynb` coordinates the Gold loaders.
- `3_*` and `4_*` load dimensions and facts into Gold.

## The `custom_transform` extension point

Default flow in Bronze to Silver:

```python
df = df.dropDuplicates()
df = df.na.fill(0)
df = df.na.fill("N/A")
```

That is correct for most entities. For entities that need more, name a custom
function in `data/meta-data/meta_data.json`:

```json
{
    "source": "Files/data/Sales/Transactions/Transactions.csv",
    "format": "csv",
    "destination": "sales_Transactions",
    "projected_columns": [],
    "custom_transform": "enrich_transactions"
}
```

And register the function in `notebooks/AquaShack_functions.ipynb`:

```python
@register_custom_transform("enrich_transactions")
def enrich_transactions(df: DataFrame, meta_entity: dict) -> DataFrame:
    df = df.dropDuplicates()
    df = df.na.fill(0)
    df = df.na.fill("N/A")
    df = df.withColumn("TransactionDateId",
                       F.expr("cast(date_format(to_date(TransactionDate), 'yyyyMMdd') as int)"))
    df = df.withColumn("LoadedAt", F.current_timestamp())
    return df
```

Signature contract: `(df, meta_entity) -> df`. The caller still handles the
write, so all write policy (IDs, audit, mode, projection) stays in one place.

Transform implementations live in `notebooks/AquaShack_custom_transforms.ipynb`.
The Bronze to Silver notebook `%run`s `AquaShack_functions` first (so the
registry and decorator exist) and then `AquaShack_custom_transforms` (so each
`@register_custom_transform(...)` populates the registry) before the
orchestration loop dispatches.

## Unstructured data and Microsoft AI Foundry

The `custom_transform` hook is not limited to tabular cleanup. It is also how
AquaShack handles **unstructured data** — things the default dedupe + fill
nulls flow cannot express — by reading files with a binary reader and calling
an AI service to reduce each file to a row.

The included example is `count_cows_from_overview`: PNGs partitioned under
`Files/data/Sales/Cow_Camera_Overviews/year=YYYY/month=MM/day=DD/` are read as
binary, scored by Azure AI Vision (SynapseML `AnalyzeImage`), and reduced to
one row per image with `id`, `cow_count`, and the partition columns. The meta
entity just looks like any other:

```json
{
    "source": "Files/data/Sales/Cow_Camera_Overviews",
    "format": "image",
    "destination": "dbo.sales_cow_camera_overviews",
    "projected_columns": [],
    "custom_transform": "count_cows_from_overview"
}
```

### Enabling the AI Foundry integration

The computer vision transform calls an **Azure AI Vision** resource. You
provision one in **Microsoft AI Foundry** (or directly in the Azure portal as
an *Azure AI services* / *Computer Vision* resource) and copy its **key** and
**endpoint**. Then in `Setup.ipynb`:

```python
ENABLE_COMPUTER_VISION = True
COMPUTER_VISION_SERVICE_KEY = "<key from AI Foundry>"
COMPUTER_VISION_ENDPOINT   = "<endpoint from AI Foundry>"
```

On the next Setup run:

1. Cow camera PNGs are seeded into the bronze lakehouse.
2. `meta_data.json` is seeded **with** the `count_cows_from_overview` entity.
3. The key and endpoint are **stamped into the parameters cell** at the top
   of `AquaShack_custom_transforms.ipynb` as it is imported into the
   workspace, so the transform can read them as module-level variables once
   `%run` pulls the notebook in.

When `ENABLE_COMPUTER_VISION = False` (default):

1. Cow camera PNGs are not seeded.
2. The `count_cows_from_overview` entity is stripped out of the seeded
   `meta_data.json`, so Bronze to Silver never tries to process it.
3. The parameters cell is stamped with placeholder values. The transform is
   still registered but will never be dispatched.

This way the workshop runs end to end without an AI Foundry resource, and
enabling the AI path is a single boolean + two strings in Setup — no edits to
the transform notebook, the pipeline, or the canonical `meta_data.json` in
the repo.

## Articles

Same articles as the upstream project:

- [Delta and Parquet: Integer, GUID/UUID or SHA256 as ID?](https://medium.com/@christianhenrikreich/delta-and-parquet-integer-guid-uuid-or-sha256-as-id-67ba15b4437f)
- [Spark SQL: Why the choice of language does not impact performance](https://medium.com/@christianhenrikreich/spark-sql-why-the-choice-of-language-doesnt-impact-performance-747ff3f854ae)
- [Data Architecture: Data capture time and event time in medallion architecture](https://medium.com/@christianhenrikreich/data-architecture-data-capture-time-and-event-time-in-medallion-architecture-dceb93980e0a)
- [Microsoft Fabric: Building Pseudo Identity Columns Without monotonically_increasing_id() in Spark](https://medium.com/@christianhenrikreich/microsoft-fabric-building-pseudo-identity-columns-without-monotonically-increasing-id-in-spark-09b1efb577d1)
- [Lakehousing: Removing one of the biggest performance killers in Silver-to-Gold processing](https://medium.com/@christianhenrikreich/lakehousing-removing-one-of-the-biggest-performance-killers-in-bronze-to-silver-processing-8ed4ce372de6)
- [Microsoft Fabric: Sentiment analysis from speech files with SynapseML in Spark](https://medium.com/@christianhenrikreich/microsoft-fabric-sentiment-analysis-from-speech-files-with-synapseml-in-spark-1f5d9f010fb4)
