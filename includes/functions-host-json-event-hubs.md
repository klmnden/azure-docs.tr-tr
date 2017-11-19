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
|maxBatchSize|64|Alma döngü alınan en fazla olay sayısı.|
|prefetchCount|yok|Temel alınan EventProcessorHost tarafından kullanılacak PrefetchCount varsayılan.| 
|batchCheckpointFrequency|1|EventHub imleç denetim noktası oluşturmadan önce işlemek için olay yığını sayısı.| 
