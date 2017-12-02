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
|routeprefix öğesi|api|Tüm yollar için geçerlidir rota öneki. Varsayılan önek kaldırmak için boş bir dize kullanın. |
|maxOutstandingRequests|-1|Herhangi bir anda tutulan Bekleyen isteklerin sayısı. Bu sınır, sıraya alınan ancak yürütme ilerleme yürütmeleri birinde yanı sıra başlamadı isteklerini içerir. Bu sınır üzerinden gelen tüm istekleri 429 "Meşgul" yanıtıyla reddedilir. Yeniden deneme zaman tabanlı stratejileri kullanmayı arayanlara izin verir ve ayrıca en büyük istek gecikmeleri denetlemenize yardımcı olur. Bu, yalnızca komut dosyası ana bilgisayarı yürütme yol içinde oluşan queuing denetler. ASP.NET istek sırası gibi diğer sıraları etkisi ve bu ayar tarafından etkilenmemesini olmaya devam eder. Sınırsız varsayılandır.|
|maxConcurrentRequests|-1|Paralel olarak yürütülecek http işlevleri maksimum sayısı. Bu, kaynak kullanımı yönetmeye yardımcı olabilen denetim eşzamanlılık için sağlar. Örneğin, eşzamanlılık çok yüksek olduğunda sorunlarına neden olur, çok fazla sistem kaynakları (cpu/bellek/yuva) kullanan bir http işleve sahip olabilir. Veya hizmet bir üçüncü taraf için Giden istekleri yapan bir işleve sahip olabilir ve bu çağrıları oranı sınırlı olması gerekir. Bu durumlarda, burada bir kısıtlama uygulama yardımcı olabilir. Sınırsız varsayılandır.|
|dynamicThrottlesEnabled|yanlış|Etkinleştirildiğinde, düzenli aralıklarla sistem performansını denetlemek için istek işleme ardışık düzenine bağlantıları iş parçacığı işlemleri/bellek/cpu/vb. gibi sayaçları ve yerleşik yüksek eşiği (% 80) olan bu sayaçları istekleri bu ayarı neden olur normal düzeylere sayaca dönüş kadar 429 "Meşgul" yanıtta reddetti.|
