# AlikeAudience Data Engineer Skill Test

A data exploration and enrichment project for an anonymized user-event dataset containing five source fields: `user_id`, `timestamp`, `lat_long`, `ip_address`, and `user_agent`.

The project follows a simple pipeline:

1. Read the CSV and check schema, uniqueness, and missing values.
2. Correct and enrich coordinates with country, continent, nearest reference city, and timezone.
3. Convert UTC timestamps to local date, hour, weekday, and holiday indicators.
4. Parse IPv4/IPv6 properties and match historical country and BGP origin ASN data.
5. Parse User-Agent strings into operating system, version, device brand/model, and app/WebView context.
6. Validate row preservation and derived fields, then build aggregate charts and summary tables.

## Deliverables

- [`AlikeAudience_Skill_Test.ipynb`](AlikeAudience_Skill_Test.ipynb) — complete Task 1 analysis with inline Python code.
- [`AlikeAudience_Skill_Test_Presentation.pptx`](AlikeAudience_Skill_Test_Presentation.pptx) — editable presentation.
- [`DATA_DICTIONARY_ZH.md`](DATA_DICTIONARY_ZH.md) — definitions of the 38 final fields.
- [`outputs/simple/charts`](outputs/simple/charts) — 19 aggregate charts used in the analysis.
- [`requirements.txt`](requirements.txt) — Python dependencies used for the validated run.

## Main observations

- The dataset contains 100,000 event rows and each `user_id` appears once. This supports event-level profiling, but not retention, repeat-visit, or movement-path analysis.
- Only `user_agent` contains missing values. Those rows are retained because their timestamp, coordinates, and IP address can still be useful.
- The stated `lat/long` field is stored in the opposite order: the first component falls outside the valid latitude range, while the second fits latitude. The analysis therefore assigns the second value to latitude and the first to longitude.
- All source timestamps end in `Z`, meaning UTC. Local time features are derived only after assigning a timezone from coordinates.
- IP country and coordinate country are treated as two different signals. A historical IP snapshot is used for the December 2022 sample, and disagreement is kept as an analytical result rather than silently overwritten.
- User-Agent parsing extracts device and client context while keeping missing or unresolved values explicit.

## Data and privacy

The company-provided raw CSV is intentionally excluded. It contains IP addresses and coordinates and should not be published in a public repository. The public notebook also has all execution outputs cleared so no individual record is exposed. Event-level enriched Parquet data and row-level CSV outputs are excluded; only aggregate charts are included.

## Reproducing the analysis

1. Place the supplied source file in the repository root as `alikeaudience_data_test.csv`.
2. Install dependencies with `pip install -r requirements.txt`.
3. Download the reference files listed below and place them at the paths used in the notebook. Please follow each source's licence and acceptable-use terms.
4. Open `AlikeAudience_Skill_Test.ipynb` and run all cells in order.

Validated private run: 100,000 rows, 38 final fields, 52 code cells completed without errors, and 19 charts generated.

## Reference data

- [Natural Earth country boundaries](https://github.com/nvkelso/natural-earth-vector) — public domain.
- [GeoNames cities15000](https://download.geonames.org/export/dump/) — CC BY 4.0.
- [DB-IP country database archived by ip-location-db](https://github.com/sapics/ip-location-db) — DB-IP CC BY 4.0; December 2022 snapshot used.
- [CAIDA RouteViews Prefix-to-AS mappings](https://www.caida.org/catalog/datasets/routeviews-prefix2as/) — December 15, 2022 snapshot used.
- [CAIDA AS Organizations dataset](https://www.caida.org/catalog/datasets/as-organizations/) — October 2022 snapshot used.

## 中文说明

这是 AlikeAudience Data Engineer skill test 的公开展示版本。仓库保留完整代码、方法、PPT 和汇总图，但不公开招聘方提供的原始 CSV、逐条 IP/经纬度记录、扩充后的明细数据或外部数据库文件。完整 Notebook 按照数据概览、地理位置、时间、IP、User Agent 和可视化的顺序组织。
