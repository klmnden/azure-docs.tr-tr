Aşağıdaki tabloda Azure işlevleri çalışma zamanı ana iki sürümleri desteklenir bağlamaları gösterir.

| Tür | 1.x | 2.x | Tetikleyici | Girdi | Çıktı |  
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob Depolama](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|  
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔<sup>1</sup>|✔|✔|✔|  
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|  
| [Dış dosya](../articles/azure-functions/functions-bindings-external-file.md)<sup>2</sup>    |✔|| |✔|✔|  
| [Dış tablo](../articles/azure-functions/functions-bindings-external-table.md)<sup>2</sup>  |✔|| |✔|✔|  
| [HTTP](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>Excel tabloları](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔<sup>1</sup>| |✔|✔|
| [Microsoft Graph<br/>OneDrive dosyaları](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔<sup>1</sup>| |✔|✔|
| [Microsoft Graph<br/>Outlook e-posta](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔<sup>1</sup>| | |✔|
| [Microsoft Graph<br/>olayları](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔<sup>1</sup>|✔|✔|✔|
| [Microsoft Graph<br/>kimlik doğrulama belirteçleri](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔<sup>1</sup>| |✔| |
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔|✔<sup>1</sup>| |✔|✔|  
| [Notification Hubs](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Kuyruk depolama](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|  
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔<sup>1</sup>| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔<sup>1</sup>|✔| |✔|  
| [Tablo depolama](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|  
| [Zamanlayıcı](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔<sup>1</sup>| | |✔|
| [Web kancaları](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔||✔| |✔|
  
<sup>1</sup> 2.x bağlama uzantısında olarak kayıtlı olması gerekir. Bkz: [bilinen sorunlar 2.x içinde](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

<sup>2</sup> Experimental &mdash; desteklenen ve gelecekte durdurulmuş.

