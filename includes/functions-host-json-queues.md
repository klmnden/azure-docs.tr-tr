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
|batchSize|16|İşlevler çalışma zamanı aynı anda alır ve paralel olarak işler sıra iletilerinin sayısı. Ne zaman işlenmekte olan numarasını alır aşağı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. En fazla eş zamanlı ileti işlevi işlenen sayısı olacak şekilde `batchSize` artı `newBatchThreshold`. Bu sınır ayrı ayrı her sıra tetiklemeli işlevin geçerlidir. <br><br>Paralel yürütme üzerinde bir Sıraya alınan iletileri önlemek istiyorsanız, ayarlayabileceğiniz `batchSize` 1. Ancak, bu ayar yalnızca tek bir sanal makinede (VM) işlevi uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. Birden çok VM çıkışı işlev uygulaması ölçeklendirir varsa, her VM her sıra tetiklemeli işlevin bir örneği çalıştırabilir.<br><br>En fazla `batchSize` 32'dir. | 
|maxDequeueCount|5|Kaç kez zararlı kuyruğuna taşınmadan önce bir ileti işlenirken deneyin.| 
|newBatchThreshold|batchSize/2|Bu sayı aynı anda işlenmekte olan iletilerin sayısını alır olduğunda, başka bir toplu iş çalışma zamanı alır.| 



