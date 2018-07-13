```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxBatchSize|64|Alma döngü alınan en yüksek olay sayısı.|
|prefetchCount|yok|Varsayılan temel alınan EventProcessorHost tarafından kullanılacak PrefetchCount.| 
|batchCheckpointFrequency|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay yığın sayısı.| 
