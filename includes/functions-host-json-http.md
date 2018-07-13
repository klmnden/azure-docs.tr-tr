```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|routeprefix öğesi|api|Tüm yollar için geçerli bir rota öneki. Varsayılan ön ekini kaldırmak için boş bir dize kullanın. |
|maxOutstandingRequests|-1|Belirli bir zamanda tutulan bekleyen istek sayısı. Bu sınır, kuyruğa alınır, ancak yürütme devam eden yürütmeler birinde yanı sıra başlamamış olan istekleri içerir. "Çok meşgul" bir 429 yanıtı bu sınırın üzerindeki tüm gelen istekler reddedilir. Zamana bağlı bir yeniden deneme stratejileri kullanmak istemiyorsunuz arayanlara izin verir ve ayrıca en fazla istek gecikme denetlemenize yardımcı olur. Bu, yalnızca betik konak yürütme yolu içinde oluşan queuing denetler. ASP.NET istek kuyruğu gibi diğer kuyrukları, efekt ve bu ayar etkilenmez görünmeye devam edecektir. Varsayılan büyük/küçük harf sınırsızdır.|
|maxConcurrentRequests|-1|En fazla paralel olarak yürütülen http işlev sayısı. Bu, kaynak kullanımını yönetmenize yardımcı olabilir denetimi eşzamanlılık için sağlar. Örneğin, sistem kaynakları (cpu/bellek/yuvaları) çok fazla eşzamanlılık çok fazla olduğunda sorunlarına neden olur, kullanan bir http işleve sahip olabilir. Veya hizmet bir üçüncü tarafa Giden istekleri oluşturan bir işlev olabilir ve bu çağrıları oranı sınırlı olması gerekir. Bu gibi durumlarda, burada bir kısıtlama uygulama yardımcı olabilir. Varsayılan büyük/küçük harf sınırsızdır.|
|dynamicThrottlesEnabled|false|Etkinleştirildiğinde, istek işleme ardışık düzenli aralıklarla sistem performansını denetlemek için bağlantı/iş parçacığı/işlemler/bellek/cpu/vb. gibi sayaçları ve bu sayaçlar, herhangi bir yerleşik yüksek eşiği (% 80), üzerinde olması durumunda istekleri bu ayarı neden olacaktır Sayaç normal düzeylerine geri dönene kadar "Çok meşgul" bir 429 yanıtı reddetti.|
