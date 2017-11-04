Özel ölçüler koleksiyonu. Bu koleksiyona telemetri öğeyle ilişkili ölçüm adlı raporu kullanın. Genel kullanım örnekleri şunlardır:
- Bağımlılık Telemetrisi yükü boyutu
- Telemetri isteği tarafından işlenen sıra öğelerin sayısı
- zaman o müşteri sürdü Sihirbazı Adım tamamlama olayı Telemetri adımda tamamlamak için.

Sorgulayabileceğiniz [özel ölçümleri](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) uygulama analytics'te:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Özel ölçüler ait telemetri öğesi ile ilişkilendirilmiş. Bu ölçümler içeren telemetri öğesiyle tabi örnekleme oldukları. Diğer telemetri türlerinden bağımsız bir değere sahip bir ölçü izlemek için [ölçüm telemetri](../articles/application-insights/app-insights-api-custom-events-metrics.md).

En fazla anahtar uzunluğu: 150
