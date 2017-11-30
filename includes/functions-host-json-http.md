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
|maxOutstandingRequests|-1|Belirli bir zamanda (-1 anlamına gelir sınırsız) bekletilir Bekleyen isteklerin sayısı. Sınır, sıraya alınan ancak, tüm süren yürütmeleri yanı sıra yürütme başlamadı isteklerini içerir. Bu sınır üzerinden gelen tüm istekleri 429 "Meşgul" yanıtıyla reddedilir. Arayanlar, yeniden deneme zaman tabanlı stratejileri kullanımlar için bu yanıtı kullanabilirsiniz. Yalnızca queuing bu ayarı denetimleri iş konak yürütme yol içinde gerçekleşir. ASP.NET istek sırası gibi diğer sıraları, bu ayar tarafından etkilenmez. |
|maxConcurrentRequests|-1|(-1 anlamına gelir sınırsız) paralel olarak yürütülecek HTTP işlevleri maksimum sayısı. Örneğin, HTTP işlevlerinizi çok fazla sistem kaynakları kullanırsanız eşzamanlılık yüksek olduğunda bir sınır ayarlayabilirsiniz. Veya işlevlerinizi bir üçüncü taraf hizmetine giden istekleri yaparsanız, bu çağrıları oranı sınırlı olması gerekebilir.|
|dynamicThrottlesEnabled|yanlış|İstek ardışık düzen işleme düzenli aralıklarla sistem performans sayaçlarını denetleyin neden olur. Bağlantılar, iş parçacıkları, işlemler, bellek ve cpu sayaçları içerir. Sayaçları yerleşik bir eşiğin üstünde (% 80) varsa, normal düzeylere sayaca dönüş kadar istekler 429 "Meşgul" yanıtıyla reddedilir.|
