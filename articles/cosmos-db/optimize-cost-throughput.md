---
title: Azure Cosmos DB'de aktarım hızı maliyeti en iyi duruma getirme
description: Bu makalede, Azure Cosmos DB'de depolanan veriler için aktarım hızını iyileştirmek açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: ddbec882675dba4724406ad1ea8079df377c34fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65967311"
---
# <a name="optimize-provisioned-throughput-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de sağlanan aktarım hızı maliyeti iyileştirin

Azure Cosmos DB, sağlanan aktarım hızı modeli sunarak, herhangi bir ölçekte öngörülebilir performans sunar. Ayırma veya önceden işleme sağlama "gürültülü komşu", performansa etkisini ortadan kaldırır. İhtiyacınız olan aktarım hızı miktarda belirtin ve SLA ile desteklenen yapılandırılmış aktarım hızı, Azure Cosmos DB garanti eder.

Bir en az 400 RU/sn aktarım hızı ile başlayın ve en fazla on milyonlarca istek başına saniye veya daha da fazla ölçeklendirin. Azure Cosmos kapsayıcı ya da Okuma isteği, yazma isteği, sorgu isteği, saklı yordamlar gibi veritabanına karşı dağıttığınız her istek, sağlanan aktarım hızınızı çıkarılır karşılık gelen bir maliyeti vardır. 400 RU/sn sağlamak ve 40 RU maliyetleri bir sorgu sorunu, saniyede 10 sorgularını vermek mümkün olacaktır. Ötesinde herhangi bir istek oranı sınırlı alırsınız ve isteği yeniden denemeniz gerekir. İstemci sürücüleri kullanıyorsanız, bunlar otomatik yeniden deneme mantığı destekler.

Aktarım hızı veritabanlarında sağlayabilir veya kapsayıcıları ve her stratejinin senaryoya bağlı olarak maliyetlerinden tasarruf etmenize yardımcı olabilir.

## <a name="optimize-by-provisioning-throughput-at-different-levels"></a>Aktarım hızı farklı düzeylerde sağlayarak en iyi duruma getirme

* Örneğin tüm kapsayıcılar, bir veritabanı aktarım hızını sağlarsanız koleksiyonları/tablolar/grafik veritabanı içinde yüke bağlı işleme paylaşabilirsiniz. Veritabanı düzeyinde ayrılan aktarım hızı eşit olmayan şekilde, kapsayıcıları belirli bir dizi iş yüküne bağlı olarak paylaşılır.

* Bir kapsayıcısında aktarım hızını sağlarsanız, SLA ile desteklenen, bu kapsayıcı için aktarım hızı kesin. Mantıksal bölüm anahtarı seçimi tüm mantıksal bölümler kapsayıcısının yükünü barındırabilmesi için önemlidir. Bkz: [bölümleme](partitioning-overview.md) ve [yatay ölçeklendirme](partition-data.md) makaleleri daha fazla ayrıntı için.

Sağlanan aktarım hızı stratejisi olarak karar vermek için bazı yönergeler aşağıda verilmiştir:

**Aktarım hızı (kapsayıcılar kümesini içeren) bir Azure Cosmos DB veritabanı sağlama düşünün**:

1. Birkaç düzine Azure Cosmos kapsayıcılar içerir ve aktarım hızı bazılarını veya tümünü arasında paylaşmak istiyorsunuz. 

2. Iaas tarafından barındırılan sanal makineleri veya şirket içinde NoSQL veya Azure Cosmos DB için ilişkisel veritabanları gibi çalışması için tasarlanmış tek kiracılı veritabanı geçiriliyor. Veri modelinizi değişiklik yapmak birçok tabloları/koleksiyon/graflar ve, varsa istemezsiniz. Unutmayın, veri modelinizi bir şirket içi veritabanından geçirilirken değil güncelleştiriyorsanız, Azure Cosmos DB tarafından sunulan avantajlardan bazıları tehlikeye gerekebilir. Veri modelinizi performans açısından en iyi şekilde yararlanmak ve maliyetleri iyileştirmek için her zaman reaccess önerilir. 

3. Planlanmamış iş yükleri ve veritabanı düzeyinde iş yükünde beklenmeyen depo için tabi havuza alınmış aktarım hızı da artış devralarak istiyorsunuz. 

4. Ayar belirli aktarım hızını yerine tek tek kapsayıcılar, kapsayıcı veritabanı içinde bir dizi toplam aktarım hızı alma hakkında dikkatli olun.

**Bir kapsayıcının aktarım hızını, sağlamayı göz önünde bulundurun:**

1. Birkaç Azure Cosmos kapsayıcılar var. Azure Cosmos DB, şemadan olduğundan, bir kapsayıcı heterojen şemasına sahip ve birden çok kapsayıcı türü, her varlık için bir tane oluşturmak müşterilerin gerektirmez öğelerini içerebilir. Her zaman bir seçenek gruplandırma ayrı 10-20 kapsayıcıları tek bir kapsayıcıya derseniz dikkate alınması gereken mantıklı olur. Bir 400 RU ile kapsayıcılar için en düşük, tüm 10-20 kapsayıcıları bir havuzu daha uygun maliyetli olabilir. 

2. Belirli bir kapsayıcısında aktarım hızını denetleyebilir ve garantili aktarım hızı belirli bir kapsayıcıdaki SLA ile desteklenen alın istiyorsunuz.

**Yukarıdaki iki stratejiler karma göz önünde bulundurun:**

1. Daha önce belirtildiği gibi Azure Cosmos DB, karıştırıp artık Azure Cosmos veritabanı, veritabanı yanı sıra, aynı veritabanındaki bazı kapsayıcıları üzerinde sağlanan aktarım hızı paylaşabiliriz bazı kapsayıcılara olması için yukarıdaki iki stratejileri eşleştirmenize olanak sağlar , hangi adanmış miktarda sağlanan aktarım hızı. 

2. Her iki veritabanı düzeyinde sağlanan aktarım hızı adanmış aktarım hızı bazı kapsayıcılar ile sahip olduğunuz bir karma yapılandırmasıyla görünmesi yukarıdaki stratejileri uygulayabilirsiniz.

API, seçimi bağlı olarak aşağıdaki tabloda gösterildiği gibi aktarım hızını farklı ayrıntı düzeylerinde sağlayabilirsiniz.

|API|İçin **paylaşılan** aktarım hızı, yapılandırma |İçin **adanmış** aktarım hızı, yapılandırma |
|----|----|----|
|SQL API’si|Database|Kapsayıcı|
|MongoDB için Azure Cosmos DB API'si|Database|Koleksiyon|
|Cassandra API’si|Keyspace|Tablo|
|Gremlin API|Veritabanı hesabı|Graf|
|Tablo API’si|Veritabanı hesabı|Tablo|

Farklı düzeylerde sağlama aktarım hızı ile maliyetlerinizi iş yükü özelliklerine dayanan iyileştirebilirsiniz. Daha önce bahsedildiği gibi program aracılığıyla ve herhangi bir zaman artış yapabilir veya ya da tek tek kapsayıcı veya toplu bir dizi kapsayıcıları sağladığınız aktarım azaltabilirsiniz. Aktarım hızı, iş yükü değiştikçe esnek ölçeklendirme tarafından yalnızca yapılandırdığınız aktarım hızı için ödeme yaparsınız. Birden çok bölgede kapsayıcınızı veya kapsayıcıları kümesi dağıtılırsa, sonra kapsayıcıdaki yapılandırmanız aktarım hızı veya bir dizi kapsayıcıları tüm bölgelerde kullanılabilir olmasını garanti edilir.

## <a name="optimize-with-rate-limiting-your-requests"></a>Hız isteklerinizi sınırlama ile en iyi duruma getirme

Gecikme süresi için hassas olmayan iş yükleri için daha az aktarım hızına ve sağlanan aktarım hızı gerçek aktarım hızı aştığında, hız sınırlama işlemek uygulamanın sağlar. Sunucu sıd'lerde istek RequestRateTooLarge (HTTP durum kodu 429) ile bitemez ve dönüş `x-ms-retry-after-ms` Kullanıcı isteği yeniden denemeden önce beklemesi gereken milisaniye cinsinden süre miktarını belirten başlığı. 

```html
HTTP Status 429, 
 Status Line: RequestRateTooLarge 
 x-ms-retry-after-ms :100
```

### <a name="retry-logic-in-sdks"></a>Yeniden deneme mantığı, SDK'ları 

Yerel SDK'ları (.NET/.NET Core, Java, Node.js ve Python) örtük olarak bu yanıt catch, sunucu tarafından belirtilen retry-after üst bilgisi saygı ve isteği yeniden deneyin. Hesabınızda aynı anda birden çok istemci tarafından erişilen sürece sonraki yeniden deneme işlemi başarılı olur.

Üst üste istek hızı tutarlı bir şekilde çalışan birden fazla istemciniz varsa, 9'a ayarlanmış olan varsayılan yeniden deneme sayısı şu anda yeterli olmayabilir. Böyle bir durumda, istemci oluşturur bir `DocumentClientException` 429 uygulama durumuyla kod. Varsayılan yeniden deneme sayısını ayarlayarak değiştirilebilir `RetryOptions` ConnectionPolicy örneğinde. İstek, istek hızı üzerinde çalışmaya devam ederse varsayılan olarak, durum kodu 429 DocumentClientException 30 saniye sonra bir toplam bekleme süresi döndürülür. Bu geçerli bir yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır. 

[MaxRetryAttemptsOnThrottledRequests](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretryattemptsonthrottledrequests?view=azure-dotnet) ayarlanır 3'e kadar bu durumda, bir istek işlemi tarafından ayrılmış aktarım hızı koleksiyon aşan sınırlı oranı ise istek işlemini yeniden deneme üç kez atamadan önce uygulamaya özel durum.  [MaxRetryWaitTimeInSeconds](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.retryoptions.maxretrywaittimeinseconds?view=azure-dotnet#Microsoft_Azure_Documents_Client_RetryOptions_MaxRetryWaitTimeInSeconds) 60 olarak ayarlanır, bu durumda toplam yeniden deneme zaman bekliyorsa saniye ilk 60 saniyede istek aşıyor, özel durum oluşturulur.

```csharp
ConnectionPolicy connectionPolicy = new ConnectionPolicy(); 

connectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 3; 

connectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 60;
```

## <a name="partitioning-strategy-and-provisioned-throughput-costs"></a>Bölümleme stratejisi ve sağlanan aktarım hızı maliyetleri

Azure Cosmos DB'de maliyetleri iyileştirmek iyi bir bölümleme stratejisi önemlidir. Olduğunu depolama ölçümleri sunulan bölümlerin, eğme emin olun. Olduğunu verimlilik metriklerini ile kullanıma sunulan bir bölüm için aktarım hızının eğriltme emin olun. Olduğunu belirli bölüm anahtarları Hayır eğriltme emin olun. Baskın anahtarları depolama ölçümleri sunulur ancak anahtar, uygulama erişim desen bağımlı olacaktır. Şu mantıksal bölüm anahtarı hakkında düşünmek idealdir. İyi bir bölüm anahtarı, aşağıdaki özelliklere sahip olması beklenir:

* Tüm bölümler arasında ve eşit olarak zaman içinde iş yükü eşit olarak yayılır bir bölüm anahtarı seçin. Diğer bir deyişle, veriler ve daha az veya hiç veri bazı anahtarlarla çoğunluğu ile bazı anahtarlarına sahip olmamalıdır. 

* Etkinleştirir erişim desenlerini mantıksal bölümler arasında eşit oranda yaymak için bir bölüm anahtarı seçin. Makul bile tüm anahtarları arasında iş yüküdür. Diğer bir deyişle, iş yükü çoğunu birkaç belirli anahtarlar üzerinde odaklı olmamalıdır. 

* Çok çeşitli değerleri olan bir bölüm anahtarı seçin. 

Böylece bu kaynaklara veri depolama ve aktarım hızı için mantıksal bölümler arasında dağıtılabilir temel veriler ve etkinliğin kapsayıcınızda mantıksal bölümler kümesi arasında yaymak için olur. Bölüm anahtarları için aday sık sorgularınızı filtre olarak görüntülenen özelliklerini içerebilir. Sorguları içinde filtre koşulu bölüm anahtarını dahil ederek, verimli bir şekilde yeniden yönlendirilebilir. Böyle bir bölümleme stratejisi, sağlanan aktarım hızını en iyi duruma getirme çok daha kolay olacaktır. 

### <a name="design-smaller-items-for-higher-throughput"></a>Daha küçük öğe daha yüksek performans için tasarlama 

İstek yükü veya belirli bir işlemi istek işleme maliyeti doğrudan öğesi boyutunu ilişkilendirilir. Büyük öğeler üzerinde işlemler birden çok küçük öğeler üzerinde işlemler maliyetlidir. 

## <a name="data-access-patterns"></a>Veri erişim desenleri 

Mantıksal olarak ne sıklıkta veri erişim tabanlı mantıksal kategoriler halinde verilerinizi ayırmak için her zaman iyi bir uygulamadır. Sık erişimli, Orta veya soğuk verileri olarak gruplayarak kullanılan depolama alanına ve gerekli performansa hassas ayarlamalar yapabilirsiniz. Erişim sıklığına bağlı olarak, verileri ayrı kapsayıcılarına (örneğin, tablolar, grafikler ve koleksiyonları) koyun ve bunları veri kesiminin ihtiyaçlarını karşılamak için sağlanan aktarım hızını ince ayar yapma. 

Ayrıca, Azure Cosmos DB kullanıyorsanız ve belirli veri değerlerine göre aramak için yapmayacağınız veya bunları nadiren erişecek bildiğiniz sıkıştırılmış değerleri bu özniteliklerin depolamanız gerekir. Bu yöntem ile depolama alanı, dizin alan ve sağlanan aktarım hızı kaydedin ve düşük maliyetlerden de neden.

## <a name="optimize-by-changing-indexing-policy"></a>Dizin oluşturma ilkesini değiştirerek en iyi duruma getirme 

Varsayılan olarak, Azure Cosmos DB her kaydın her özelliğin otomatik olarak dizinini oluşturur. Bu, geliştirme sürecini kolaylaştırır ve birçok farklı türde geçici sorgular arasında mükemmel bir performans sağlamak için tasarlanmıştır. Özellikleri binlerce büyük kaydınız varsa, her özelliğin dizin oluşturma için aktarım hızı maliyeti ödeme özellikle, yalnızca 10 veya bu özellikleri 20 karşı sorgularsanız yararlı olmayabilir. Belirli iş yükünüze dayalı bir tanıtıcı tamamlanmasında yakınlaşın, kılavuzumuzu dizin ilkenizi ayarlamak için aynıdır. Azure Cosmos DB, dizin oluşturma ilkesi ilgili tüm ayrıntılara bulunabilir [burada](indexing-policies.md). 

## <a name="monitoring-provisioned-and-consumed-throughput"></a>İzleme ve sağlanan aktarım hızı tüketilen 

Azure portalında tükettiniz RU sayısını yanı sıra toplam RU sayısı, oranı sınırlı istek sayısı izleyebilirsiniz. Aşağıdaki görüntüde bir örnek kullanım ölçümü gösterilmektedir:

![Azure portalında izleme istek birimleri](./media/optimize-cost-throughput/monitoring.png)

Oranı sınırlı isteklerinin sayısı belirli bir eşiği aşıp aşmadığını denetlemek için uyarıları da ayarlayabilirsiniz. Bkz: [Azure Cosmos DB izleme](use-metrics.md) makale daha fazla ayrıntı için. Bu uyarılar, hesap yöneticilerine bir e-posta gönderin ya da otomatik olarak sağlanan aktarım hızı artırmak için özel bir HTTP Web kancası veya bir Azure işlevi çağırabilir. 

## <a name="scale-your-throughput-elastically-and-on-demand"></a>Esnek bir biçimde aktarım hızınızı ölçeklendirebileceğiniz ve isteğe bağlı 

Sağlanan aktarım hızı için faturalandırılırsınız olduğundan, sağlanan aktarım hızı gereksinimlerinizi için eşleşen, kullanılmayan aktarım hızı ücretleri önlemenize yardımcı olabilir. Gerektiğinde, her zaman aşağı veya yukarı, sağlanan aktarım hızı ölçeklendirebilirsiniz.  

* İzleme, RU kullanımını ve oranı sınırlı istekleri oranını gün veya hafta boyunca sabiti boyunca sağlanan tutmak gerekmez gösterilmesine neden olabilir. Geceleri veya hafta sırasında daha az trafik alabilirsiniz. Azure portal veya Azure Cosmos DB yerel SDK veya REST API'sini kullanarak, sağladığınız aktarım herhangi bir zamanda ölçeklendirebilirsiniz. Azure Cosmos DB'nin REST API uç noktaları, program aracılığıyla zamana bağlı olarak gün veya haftanın günü kodunuzdan aktarım hızını ayarlamak basit hale getirme kapsayıcılarınızı performans düzeyini güncelleştirmek için sağlar. İşlem, kapalı kalma süresi gerçekleştirilen ve genellikle bir dakika içinde etkinleşir. 

* Bir ölçeği alanları, Azure Cosmos DB'ye, örneğin, veri geçişi sırasında alabilen aktarım hızı andır. Geçişi tamamladıktan sonra sağlanan aktarım hızı çözümün kararlı bir duruma işlemek için ölçeği azaltabilirsiniz.  

* Faturalandırma, sağlanan aktarım hızı anda bir saat daha sık değiştirirseniz paranın kaydedilmeyecektir bir saatlik ayrıntı düzeyinde olduğundan unutmayın.

## <a name="determine-the-throughput-needed-for-a-new-workload"></a>Yeni bir iş yükü için gereken aktarım hızı belirleme 

Yeni bir iş yükü için sağlanan aktarım hızı belirlemek için aşağıdaki adımları kullanabilirsiniz: 

1. Kapasite Planlayıcı'ı kullanarak ilk, kaba değerlendirme gerçekleştirmek ve Azure portalında Azure Cosmos Explorer'ın yardımıyla tahminlerinizi ayarlayın. 

2. Beklenenden daha yüksek verim ve daha sonra gerektiğinde ölçeği de kapsayıcı oluşturmak için önerilir. 

3. Otomatik yeniden denemeler gelen istekleri oranı sınırlı aldığınızda yararlanmak için yerel Azure Cosmos DB Sdk'lardan birini kullanmanız önerilir. Desteklenmeyen bir platform üzerinde çalışıyoruz ve Cosmos DB'nin REST API kullanıyorsanız, kendi yeniden deneme ilkesini kullanarak uygulama `x-ms-retry-after-ms` başlığı. 

4. Tüm yeniden deneme işlemleri başarısız olduğunda, uygulama kodunuzun düzgün bir şekilde çalışması desteklediğinden emin olun. 

5. Oran sınırlandırma için bildirim almak için Azure portalında uyarıları yapılandırabilirsiniz. Gerçek kullanımınızı şekil sonra son 15 dakika ile daha istekli kuralları anahtara üzerinden 10 oranı sınırlı istekleri gibi koruyucu sınırlarıyla başlatabilirsiniz. Bazen oran sınırları güzel, ayarladığınız ve tam olarak ne yapmak istiyorsunuz olan sınırsız çalan göster. 

6. İzleme, trafik desenini dinamik olarak bir hafta veya gün aktarım hızı sağlamanız ayarlamak için gereken değerlendirebilmesi anlamak için kullanın. 

7. Düzenli olarak en fazla kapsayıcılar ve veritabanları gereken sayıda sağlanan değil emin olmak için kullanılan aktarım hızını oranı karşılaştırması, sağlanan izleyin. Biraz sağlanan aktarım hızına sahip bir iyi güvenliği olup olmadığını denetler.  

### <a name="best-practices-to-optimize-provisioned-throughput"></a>Sağlanan aktarım hızını iyileştirmek için en iyi uygulamalar 

Aşağıdaki adımlar çözümlerinizi yüksek oranda ölçeklenebilir ve ekonomik Azure Cosmos DB kullanarak yapmaya yardımcı olur.  

1. Önemli ölçüde sağlanan aktarım hızı kapsayıcılar ve veritabanları arasında varsa, RU sağlanan ve tüketilen RU gözden geçirin ve iş yükleri üzerinde ince ayar gerekir.  

2. Temsili bir Azure Cosmos kapsayıcı veya uygulamanız tarafından kullanılan veritabanı karşı tipik işlemlerin çalıştırmayla ilgili istek birimi RU ücreti kaydettiğinizden ayrılmış aktarım hızı uygulamanız için gereken miktarı tahmin etmek için bir yöntem olduğundan ve ardından her saniye gerçekleştirmek için tahmin işlemlerin sayısını tahmin edin. Ölçün ve tipik sorguları ve bunların kullanımını da emin olun. Sorguların RU maliyetleri programlı olarak tahmin etme veya portal bakın kullanarak öğrenmek [sorguları maliyetini en iyi duruma getirme](online-backup-and-restore.md). 

3. İşlemler ve bunların maliyetini düşük RU almak için başka bir işlem/süresi ve istek ücreti dökümünü erişmenizi sağlayan Azure İzleyici günlüklerine sağlayarak yoludur. Her işlem ücretine geri yanıttan depolanan ve ardından analizi için kullanılan azure Cosmos DB her işlem için istek ücretsiz olarak sağlar. 

4. İş yükü gereksinimlerinizi karşılamak ihtiyaç sağlanan aktarım hızına göre esnek olarak ölçeklendirebilirsiniz. 

5. Ekleme ve kaldırma gerekir ve maliyetleri denetim gibi Azure Cosmos hesabıyla ilişkili bölge. 

6. Kapsayıcılarınızı mantıksal bölümler arasında eşit dağıtım verileri ve iş yükleri olduğundan emin olun. Düzensiz bölüm dağıtım varsa, bu aktarım hızı yüksek miktarda gereken değerden sağlamak için neden olabilir. Dengesiz dağılımı olduğunu belirlerseniz, biz iş yükü bölümler arasında eşit olarak dağıtma önerilir veya verilerin yeniden bölümlenip. 

7. Çok sayıda kapsayıcı varsa ve bu kapsayıcıların SLA gerektirmeyen durumlarda veritabanı tabanlı teklif kullanabilirsiniz. burada kapsayıcının aktarım hızını SLA uygulanmaz. Hangi veritabanı düzeyi aktarım hızını geçirmek istediğiniz Azure Cosmos kapsayıcılar sunar ve değişiklik akışı tabanlı bir çözüm kullanarak geçirmenize tanımlamanız gerekir. 

8. Geliştirme/test senaryoları için "Cosmos DB ücretsiz katmanı" (bir yıl boyunca ücretsiz), Cosmos DB deneyin (en fazla üç bölgeleri için) veya indirilebilir Cosmos DB öykünücüsü'nü kullanmayı düşünün. Test-geliştirme için bu seçenekleri kullanarak, maliyetlerinizi önemli ölçüde düşürebilirsiniz.  

9. Daha fazla iş yüküne özgü maliyet iyileştirmelerini – Örneğin, birden çok bölgede toplu iş boyutu, Yük Dengeleme okuma artırma ve XML'deki varsa, veri çoğaltma işlemlerini gerçekleştirebilirsiniz.

10. Azure Cosmos DB ayrılmış kapasite ile üç yıl boyunca en fazla % 65'e için önemli ölçüde indirimler alabilirsiniz. Azure Cosmos DB ayrılmış kapasite modeli olduğundan bir ön taahhüt zamanla Birimi'nin ister. Daha fazla istek birimleri uzun bir döneme kullandığınız şekilde indirimleri katmanlı halde bulunan daha fazla, indirim olacaktır. Bu indirimler hemen uygulanır. Sağlanan değerlerinizi kullanılan herhangi bir RU ayrılmamış kapasite maliyeti üzerinden ücretlendirilir. Bkz: [Cosmos DB ayrılan kapasite](cosmos-db-reserved-capacity.md)) daha fazla ayrıntı için. Düşük sağlanan üretilen iş maliyetlerinizi ilerletmek için ayrılmış bir kapasite satın alma göz önünde bulundurun.  

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında daha fazla bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)

