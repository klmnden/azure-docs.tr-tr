```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxPollingInterval|60000|En büyük zaman aralığını milisaniye cinsinden sıra arasında yoklar.| 
|visibilityTimeout|0|Zaman aralığı bir ileti işlenirken denemeler başarısız olur.| 
|batchSize|16|Almak ve paralel olarak işlemek için sıra iletilerinin sayısı. Maksimum değer 32'dir.| 
|maxDequeueCount|5|Kaç kez zararlı kuyruğuna taşınmadan önce bir ileti işlenirken deneyin.| 
|newBatchThreshold|batchSize/2|En yeni toplu iletiler getirilen eşiği.| 
