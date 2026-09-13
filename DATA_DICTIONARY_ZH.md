# 最终数据字典

最终明细文件为 `outputs/simple/events_enriched.parquet`，共 100,000 行、38 列。

| 字段 | 中文解释 |
|---|---|
| `user_id` | 原始用户 ID；在本样本中全部唯一。 |
| `timestamp` | 原始时间字符串。 |
| `lat_long` | 原始坐标字符串；实测顺序为“经度 纬度”。 |
| `ip_address` | 原始 IPv4 或 IPv6 字符串。 |
| `user_agent` | 原始 UA 字符串；允许为空。 |
| `latitude` | 修正后的纬度，合法范围为 −90 至 +90。 |
| `longitude` | 修正后的经度，合法范围为 −180 至 +180。 |
| `loc_continent_en` | 根据坐标匹配的洲，英文。 |
| `loc_continent_zh` | 根据坐标匹配的洲，中文。 |
| `loc_country_en` | 根据坐标匹配的国家／地区，英文。 |
| `loc_country_zh` | 根据坐标匹配的国家／地区，中文。 |
| `loc_city_en` | 同国家 GeoNames 中距离最近的参考城市，英文；不代表行政城市归属。 |
| `loc_city_zh` | 有中文／CJK 别名时使用该别名，否则回退为英文参考城市名。 |
| `loc_timezone` | 根据坐标得到的 IANA 时区。 |
| `timestamp_utc` | 解析后的 UTC 时区时间。 |
| `date` | 按坐标时区转换后的当地日期。 |
| `hour` | 当地小时，0–23。 |
| `weekday` | 当地星期名称。 |
| `is_holiday` | 当地日期是否为坐标国家／地区的公共节日。 |
| `ip_version` | IP 版本，4 或 6。 |
| `ip_is_global` | Python `ipaddress.is_global`；同时作为国家与 ASN 查询筛选条件。 |
| `ip_is_private` | Python `ipaddress.is_private`。 |
| `ip_is_reserved` | Python `ipaddress.is_reserved`。 |
| `ip_is_multicast` | Python `ipaddress.is_multicast`。 |
| `ip_is_loopback` | Python `ipaddress.is_loopback`。 |
| `ip_is_link_local` | Python `ipaddress.is_link_local`。 |
| `ip_is_unspecified` | Python `ipaddress.is_unspecified`。 |
| `ip_country_en` | 2022-12-02 发布的 DB-IP 历史快照匹配国家／地区，英文；仅查询公网 IP。 |
| `ip_country_zh` | 同一历史 IP 国家／地区，中文。 |
| `ip_asn` | 2022-12-15 12:00 UTC RouteViews 快照中的单一 BGP 起源 ASN；仅查询公网 IP，多起源结果留空。 |
| `ip_asn_name` | CAIDA 2022-10 映射中的 ASN 历史短名称，例如 `CHINANET-BACKBONE`。 |
| `ip_asn_organization` | 根据 CAIDA AS Organizations 2022-10 快照匹配的历史组织名称；该文件创建于 2022-12-19。 |
| `ua_engine` | UA 中的内核／运行环境标识，例如 AppleWebKit、Dalvik、Gecko 或 Trident。 |
| `os` | 解析后的操作系统。 |
| `os_version` | 解析后的操作系统版本。 |
| `device_brand` | 解析后的设备品牌。 |
| `device_model` | 解析后的设备型号；去除末尾 Android `Build/…` 构建号。 |
| `is_android_app` | Android UA 中明确出现 `wv`，或以 `Dalvik/` 开头时为 True；UA 缺失时为空。 |

七个 IP 布尔属性可能互相重叠，不能把它们当作互斥类别相加。IP 国家与坐标国家是两个独立信号；两者不一致本身不能判断用户的真实位置。
