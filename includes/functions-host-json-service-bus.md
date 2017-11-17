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
|maxConcurrentCalls|16|İleti Pompalama başlatmak geri arama eşzamanlı çağrı maksimum sayısı. Varsayılan olarak, işlevleri çalışma zamanı aynı anda birden fazla ileti işler. Bir kerede yalnızca bir tek kuyruk veya konu ileti işleme için çalışma zamanı yönlendirmek için ayarlanmış `maxConcurrentCalls` 1. | 
|prefetchCount|yok|Temel alınan MessageReceiver tarafından kullanılacak PrefetchCount varsayılan.| 
|autoRenewTimeout|00:05:00|En uzun süre içinde otomatik olarak ileti kilit yenilenir.| 
