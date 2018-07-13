```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Bir kerede yalnızca tek bir kuyruk veya konuda ileti işleme için çalışma zamanının ayarlayın `maxConcurrentCalls` 1. | 
|prefetchCount|yok|Varsayılan temel alınan MessageReceiver tarafından kullanılacak PrefetchCount.| 
|autoRenewTimeout|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.| 
