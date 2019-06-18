---
title: Azure Cosmos DB faturanızı anlama
description: Bu makalede, bazı örnekler ile Azure Cosmos DB faturanızı anlayın bölümü açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: a097539e51aa2a2130dead236d553d60f2ebb89d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65965676"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Azure Cosmos DB faturanızı anlama

Bir tam olarak yönetilen bir bulutta çalışan veritabanı hizmeti olarak Azure Cosmos DB ile yalnızca sağlanan aktarım hızı için ücretlendirme faturalandırma basitleştirir ve depolama miktarı. Hiçbir ek lisans ücretleri, donanım, yardımcı programı maliyetlerini veya şirket içi veya Iaas tarafından barındırılan alternatifleri karşılaştırıldığında tesis maliyetleri vardır. Veritabanı hizmeti, çoklu bölge özellikleri Azure Cosmos DB'nin düşünürken, mevcut şirket içi veya Iaas çözümleri kıyasla önemli ölçüde azaltılmasını sağlar.

Azure Cosmos DB'de sağlanan aktarım hızı ve tüketilen depolama saatlere göre faturalandırılır. Sağlanan aktarım hızı için faturalandırma birimi saatte 100 RU/sn, bkz. $ standart genel fiyatlandırması, varsayılarak 0.008 saat başına ücretlendirilir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/). Ay başına depolama alanı 1 GB başına faturalandırılan $0,25 olduğunuz tüketilen depolama alanı için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/). 

Bu makalede aylık faturanızda gördüğünüz ayrıntıları anlamanıza yardımcı olması için bazı örnekler kullanır. Örneklerde gösterilen sayıların farklı bir miktarda birden çok bölgede span veya farklı bir dönem için bir ay boyunca çalıştırmak, sağlanan aktarım hızının Azure Cosmos kapsayıcılarınızı sahip farklı olabilir.

## <a name="billing-examples"></a>Faturalandırma örnekleri

### <a name="billing-example---throughput-on-a-container-full-month"></a>Faturalandırma örneği - aktarım hızını bir kapsayıcı (tam ay)

* Bir kapsayıcıdaki aktarım hızı, 1.000 RU/sn yapılandırın ve 24 saat * 30 gün ay için mevcut varsayalım = toplam 720 saat.  

* 1\.000 RU/sn olan 10 birim kapsayıcıları mevcut olduğu her saat için saat başına 100 RU/sn (diğer bir deyişle, 1000/100 = 10). 

* Saat başına 10 birime maliyeti $0.008 (başına 100 RU/sn saat başına) ile çarpılmasıyla elde saatte $0.08 =. 

* Eşittir $0.08 * 24 saatte $0.08 ayının saat sayısına göre saat başına çarparak * $57.60 = 30 gün ay için.  

* Toplam aylık faturanızda $57.60 maliyetini (100 RU),'ın 7.200 birimleri gösterir.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>Faturalandırma örneği - aktarım hızını bir kapsayıcı (kısmi ay)

* 2\.500 RU/sn sağlanan aktarım hızı ile bir kapsayıcı oluşturacağız varsayalım. 24 saat boyunca ayı için kapsayıcı yaşadığı (örneğin, 24 saat biz oluşturduktan sonra sileceğiz).  

* 600 birim faturanızda görüyoruz sonra (2.500 RU/sn / 100 RU/sn/birim * 24 saat). Maliyeti $4.80 olacaktır (600 birim * $0.008/birim).

* Toplam fatura ayı için $4.80 olacaktır.

### <a name="billing-rate-if-storage-size-changes"></a>Depolama boyutu değişirse TL

Depolama kapasitesi, bir aylık bir dönemde depolanan saatlik en fazla veri miktarı birimi (GB cinsinden) üzerinden faturalandırılır. Örneğin; ayın ilk yarısında 100 GB’lık, ikinci yarısında ise 50 GB’lık depolama kullandıysanız, bu ay için 75 GB’a eşdeğer depolama tutarı üzerinden faturalandırılırsınız.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>Kapsayıcı ya da bir dizi kapsayıcıları olduğunuzda etkin bir saatten az için faturalandırma tarifesi

İşiniz her saat kapsayıcı ya da veritabanı, ne olursa olsun veya kapsayıcının veya veritabanının bir saatten az için etkinse, bir sabit fiyat üzerinden faturalandırılırsınız. Örneğin, bir kapsayıcı veya veritabanı oluşturup 5 dakika sonra silerseniz, faturanıza bir saat içerir.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>Bir kapsayıcı veya veritabanı aktarım hızını yukarı/aşağı ölçeklendirildiğinde TL

9: 30'da 400 RU/sn'den 1.000 RU/sn, sağlanan aktarım hızı artırmak ve sağlanan aktarım hızı anda 10. 45'te yeniden 400 RU/sn için daha düşük, iki saatlik 1.000 RU/sn için ücretlendirilirsiniz. 

9: 30'da K 100 RU/sn 200-K RU/sn ve ardından alt sağlanan aktarım hızı 10: 45'te dön K 100 RU/sn, kapsayıcı ya da bir dizi kapsayıcı için sağlanan aktarım hızı artırmak istiyorsanız, iki saatlik 200 K RU/sn için ücret ödersiniz.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>Faturalandırma örneği: birden çok kapsayıcı her adanmış sağlanan aktarım hızı modu

* Bir Azure Cosmos hesabı Doğu ABD 2 500 RU/sn ve 700 RU/sn, sağlanan aktarım hızı ile iki kapsayıcı sırasıyla oluşturursanız, 1.200 RU/sn sağlanan aktarım gerekir.  

* 1\.200/100 ücretlendirilebilir * $0.008 = 0,096 / saat. 

* Aktarım hızınızın değişmesi ve 20.000 RU/sn ile yeni bir sınırsız kapsayıcı oluştururken aynı zamanda her kapsayıcının kapasitesine 500 RU/sn artırdık, toplam sağlanan kapasiteniz 22.200 RU/sn olacaktır (1.000 RU/sn + 1.200 RU/sn + 20 000RU/sn).  

* Faturanız şöyle değişir: $0.008 x 222 = 1.776 $/ saat. 

* 720 saat (24 saat * 30 500 saatliğine aktarım hızı sağladıysanız gün), bir ayda 1.200 RU/sn ve 22.200 RU/sn, sağlanan aktarım hızı aylık faturanız gösterir kalan 220 saatliğine şeklindeydi: 500 x 0,096 / saat + 220 x 1.776 $/ saat = $438.72 / ay.

![Adanmış aktarım hızı fatura örneği](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>Faturalandırma örnek: paylaşılan aktarım modu ile kapsayıcıları

* 50-K RU/sn ve 70-K RU/sn sağlanan aktarım hızı ile sırasıyla Doğu ABD 2 (aktarım hızı ve veritabanı düzeyinde paylaşımı kapsayıcıları kümesiyle) iki Azure Cosmos veritabanı ile bir Azure Cosmos hesap oluşturursanız sağlanan toplam gerekir 120 K RU/sn aktarım hızı.  

* 1200 ücretlendirilebilir x $0.008 = 9.60 $/ saat. 

* Aktarım hızınızın değişmesi ve her veritabanının her veritabanı için 10 K RU/sn sağlanan aktarım hızı artar ve yeni bir kapsayıcı ilk veritabanı adanmış aktarım hızı modu 15 bin RU/sn ile paylaşılan aktarım hızı veritabanınıza eklediğiniz, sağlanan genel kapasiteye 155-K RU/sn (60 K RU/sn + 80 K RU/sn + 15 bin RU/sn) olur.  

* Bu durumda, faturanız şöyle değişir: 1.550 * $0.008 = 12.40 $/ saat.  

* 720 saatlik bir ayda 300 saati için sağlanan aktarım hızı 120-K RU/sn ve kalan 420 saatliğine 155-K RU/sn sağlanan aktarım hızı şeklindeydi, aylık faturanızda görünür: 300 x 9.60 $/ saat + 420 x 12.40 $/ saat = $2,880 + $5,208 = $8,088 / ay. 

![Aktarım hızı fatura örnek paylaşılan](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>Coğrafi çoğaltma ve çok yöneticili faturalandırma örnekleri  

Ekleme/Azure bölgeleri dünyanın her yerinden Azure Cosmos DB veritabanı hesabınızı dilediğiniz zaman kaldırabilirsiniz. Çeşitli Azure Cosmos DB veritabanları ve kapsayıcılar için yapılandırılmış aktarım hızı, her Azure Cosmos veritabanı hesabınızla ilişkili Azure bölgelerinin ayrılacaktır. Sağlanan aktarım hızına (RU/sn) toplamını yapılandırılmışsa tüm veritabanları ve Azure Cosmos veritabanı kapsayıcılara T (saat sağlanan) hesabı, ve veritabanı hesabınızla ilişkili Azure bölgeleri N sonra toplam sayısıdır sağlanan aktarım hızı, Azure Cosmos veritabanı hesabı, (a) bir tek bir yazma bölgesi ile yapılandırılmış, yapılandırılmış olan tüm bölgeleri yazma işleyebilen T x N RU/sn ve (b) eşittir için belirli bir saat eşittir (, N + 1) için T RU/sn , sırasıyla. Sağlanan aktarım hızı (tek bir yazma bölgesi) $0.008 başına 100 RU/sn saatlik maliyetleri ve birden çok yazılabilir bölge (çok yöneticili config) ile sağlanan aktarım hızı maliyeti $0,016 / saat başına 100 RU/sn (bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/)). Bunun tek olsun bölge veya birden çok yazma bölgeleri, Azure Cosmos DB herhangi bir bölgeden veri okumanıza izin verir.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>Faturalandırma örnek: çok bölgeli Azure Cosmos hesabı, tek bölge yazar.

Batı ABD bölgesinde bir Azure Cosmos kapsayıcımız varsayalım. Kapsayıcı 10 K RU/sn aktarım hızı ile oluşturulur ve bu ay 1 TB veri depoladınız. Üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğiniz Azure Cosmos hesabınıza her aynı depolama ve üretilen iş hacmiyle varsayalım. Aylık toplam faturanız (ayda varsayılıyor 30 gün) olacaktır. Faturanız şu şekilde olacaktır: 

|**Öğesi** |**Kullanım (aylık)** |**Oranı** |**Aylık maliyet** |
|---------|---------|---------|-------|
|Batı ABD’deki kapsayıcı için aktarım hızı faturası      | 10 K RU/sn * 24 * 30    |saat başına 100 RU/sn başına $0.008   |$576|
|3 ek bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) için aktarım hızı faturası       | 3 * 10K RU/sn * 24 * 30    |saat başına 100 RU/sn başına $0.008  |$1,728|
|Batı ABD’deki kapsayıcı için depolama faturası      | 250 GB    |0,25 DOLAR/GB  |$62.50|
|3 ek bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) için depolama faturası      | 3 * 250 GB    |0,25 DOLAR/GB  |$187.50|
|**Toplam**     |     |  |**$2,554**|

*Ayrıca kapsayıcısından Doğu ABD, Kuzey Avrupa ve Doğu Asya veri çoğaltmak için Batı ABD bölgesinde ayda 100 GB veri çıkış varsayalım. Veri aktarım hızına göre çıkışı için faturalandırılırsınız.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>Faturalandırma örneği: Azure Cosmos hesabı çok bölgeli, çoklu bölge Yazar

Batı ABD bölgesinde bir Azure Cosmos kapsayıcısı oluşturma varsayalım. Kapsayıcı 10 K RU/sn aktarım hızı ile oluşturulur ve bu ay 1 TB veri depoladınız. Üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğiniz varsayalım ile aynı depolama ve aktarım hızını ve her birinin istediğiniz kapsayıcılarına, Azure Cosmos hesabınızla ilişkili tüm bölgelerde yazma olanağı. Aylık toplam faturanız (30 günlük ay gibi varsayılarak olması):

|**Öğesi** |**Kullanım (aylık)**|**Oranı** |**Aylık maliyet** |
|---------|---------|---------|-------|
|(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası       | 10 K RU/sn * 24 * 30    |0,016 başına saat başına 100 RU/sn    |$1,152 |
|3 ek bölge - Doğu ABD, Kuzey Avrupa ve Doğu Asya (tüm bölgeler yazılabilir) için aktarım hızı faturası        | (3 + 1) * 10 K RU/sn * 24 * 30    |0,016 başına saat başına 100 RU/sn   |$4,608 |
|Batı ABD’deki kapsayıcı için depolama faturası      | 250 GB    |0,25 DOLAR/GB  |$62.50|
|3 ek bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) için depolama faturası      | 3 * 250 GB    |0,25 DOLAR/GB  |$187.50|
|**Toplam**     |     |  |**$6,010**|

*Ayrıca kapsayıcısından Doğu ABD, Kuzey Avrupa ve Doğu Asya veri çoğaltmak için Batı ABD bölgesinde ayda 100 GB veri çıkış varsayalım. Veri aktarım hızına göre çıkışı için faturalandırılırsınız.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>Faturalandırma örnek: Azure Cosmos hesabıyla bazı kapsayıcıları için adanmış aktarım hızı modu da dahil olmak üzere çok yöneticili, veritabanı düzeyinde işleme

Aşağıdaki örnekte, çok bölgeli bir Azure Cosmos hesabı tüm bölgeler yazılabilir (çok yöneticili config) nerede sahip olduğumuz düşünelim. Kolaylık olması için depolama boyutu sabit kalır ve değil değiştirin ve örneği basit tutmak için buradan atlayın varsayacağız. Sağlanan aktarım hızı ay boyunca, (varsayılıyor 30 gün veya saat 720) gibi çeşitli: 

[0-100 saat]:  

* Tüm bölgeler yazılabilir olduğu bir üç bölgede Azure Cosmos hesabı (Batı ABD, Doğu ABD, Kuzey Avrupa), oluşturduğumuz 

* Bir veritabanı (D1) paylaşılan iş hacmiyle 10 bin oluşturduğumuz RU/sn 

* Paylaşılan iş hacmiyle 30-K RU/sn (D2) bir veritabanı oluşturduk ve  

* Bir kapsayıcı (C1) 20 bin adanmış aktarım hızı ile oluşturduğumuz RU/sn 

[101-200 saat]:  

* Biz 50 K RU/sn (D1) veritabanı Ölçeklendirildi 

* Biz 70 K RU/sn (D2) veritabanına yukarı ölçeklendirilemez  

* Kapsayıcı (C1) sildik.  

[201-300 saat]:  

* Kapsayıcı (C1) yeniden 20 bin adanmış aktarım hızı ile oluşturduğumuz RU/sn 

[301 400 saat]:  

* Azure Cosmos hesabınızdan kaldırdık bölgelerinden birini (yazılabilir bölge sayısı, artık 2) 

* Biz 10 K RU/sn (D1) veritabanına aşağı ölçeklendirilebilir 

* Biz 80 K RU/sn (D2) veritabanına yukarı ölçeklendirilemez  

* Yeniden sildik kapsayıcı (C1) 

[saat 401-500]:  

* Biz 10 K RU/sn (D2) veritabanına aşağı ölçeklendirilebilir  

* Kapsayıcı (C1) yeniden 20 bin adanmış aktarım hızı ile oluşturduğumuz RU/sn 

[501-700 saat]:  

* Biz 20 bin RU/sn (D1) veritabanına yukarı ölçeklendirilemez  

* Biz 100 K RU/sn (D2) veritabanına yukarı ölçeklendirilemez  

* Yeniden sildik kapsayıcı (C1)  

[701 720 saat]:  

* Biz 50 K RU/sn (D2) veritabanına aşağı ölçeklendirilebilir  

Görsel olarak toplam sağlanan aktarım hızı 720 saatliğine ay sırasında yapılan değişiklikler, aşağıdaki şekilde gösterilmiştir: 

![Gerçek Hayatta örneği](./media/understand-your-bill/bill-example3.png)

(30 gün/720 saat bir ay içinde varsayılarak) toplam aylık faturanız şu şekilde hesaplanan değer:

|**saat**  |**RU/sn** |**Öğesi** |**Kullanım (saatlik)** |**Maliyet** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10K <br/>D2:30K <br/>C1:20K |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$960  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[101-200] |D1:50K <br/>D2:70K <br/>C1:-- |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |$1920  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |$5,760  |
|[201-300]  |D1:50K <br/>D2:70K <br/>C1:20K |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$2,240  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |$6,720 |
|[301-400] |D1:10K <br/>D2:80K <br/>C1:-- |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |$1,440   |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[401-500] |D1:10K <br>D2:10K <br>C1:20K |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$640  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |$1,280  |
|[501-700] |D1:20K <br>D2:100K <br>C1:-- |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |$3,840  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |$7,680  |
|[701-720] |D1:20K <br/>D2:50K <br/>C1:-- |(Tüm bölgeler yazılabilir) Batı ABD'deki kapsayıcı için aktarım hızı faturası  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |$224  |
| | |Aktarım hızı faturası 2 ek bölgeler için: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |$224  |
|| |**Toplam aylık maliyet**  | |**$38,688**   |

## <a name="proactively-estimating-your-monthly-bill"></a>Proaktif olarak aylık faturanızı tahmin etme  

Faturanız ayın son önce proaktif olarak tahmin etmek istediğiniz başka bir örnek düşünelim. Şu şekilde faturanızı tahmin edebilirsiniz:

|**Depolama maliyeti** | |
|----|----|
|Ortalama kayıt boyutu (KB) |1 |
|Kayıt sayısı  |100,000,000  |
|Toplam depolama alanı (GB)  |100 |
|GB başına aylık maliyet  |$0.25  |
|Depolama için beklenen aylık maliyet   |$25.00  |

<br>

|**Aktarım hızı maliyeti** | | | |
|----|----|----|----|
|İşlem Türü| İsteği/sn| Ort. RU/istek| Gerekli ru|
|Yazma| 100 | 5 | 500|
|Okuma| 400| 1| 400|

Toplam RU/sn: 500 + 400 = saatlik maliyet 900: 900/100 * $0.008 = $0.072 beklenen aylık Maliyet (31 gün varsayılarak) aktarım hızı: $0.072 * 24 * 31 $53.57 =

**Toplam aylık maliyet**

Toplam aylık ücret = aylık depolama maliyeti + toplam aylık maliyet aktarım hızı için aylık ücret = $25.00 + $53.57 $78.57 =

*Fiyatlandırma, bölgeye göre değişiklik gösterebilir. Güncel fiyatlandırma için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ile faturalandırma ayrılmış kapasite

Azure Cosmos DB ayrılmış kapasite tüm Azure Cosmos DB veritabanları ve kapsayıcılar (tüm API veya veri modeli için) tüm Azure bölgeleri arasında uygulanabilir sağlanan aktarım hızına önceden (ayrılmış bir kapasite veya bir ayırma) satın almanıza olanak sağlar. Bölge başına sağlanan aktarım hızı fiyat değişir olduğundan, gelen her bölgede ilgili fiyat düzeyinde sağlanan aktarım hızı için kurulabilir indirimli, satın aldığınız bir parasal kredi olarak ayrılmış kapasite düşünmek yardımcı olur. Örneğin, bir Azure hesabı ile tek bir kapsayıcıya 50-K RU/sn ile sağlanır ve iki bölge Doğu ABD ve Doğu Japonya genel olarak çoğaltılmasına Cosmos sahip varsayalım. Kullandıkça Öde seçeneği tercih ederseniz, ödeyeceğiniz:  

* Doğu ABD: 50-K RU/sn hızında bölgede 100 RU/sn başına $0.008 için 

* Japonya Doğu: 50-K RU/sn hızında 0.009 bölgede 100 RU/sn başına için

Toplam faturanız (olmadan ayrılmış Kapasite) (varsayılıyor 30 gün veya saat 720) şöyle olacaktır: 

|**Bölge**| **100 RU/sn başına saatlik fiyat**|**Birimleri (RU/sn)**|**Faturalandırılan miktar (saat)**| **Faturalandırılan miktar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|$0.008 |50 BİN|$4|$2,880 |
|Japonya Doğu|$0.009 |50 BİN| $4.50 |$3,240 |
|Toplam|||$8.50|$6,120 |

Ayrılmış kapasite bunun yerine, satın aldığınız olduğunu düşünelim. 100 bin cinsinden RU/sn 56,064 (% 20 indirim), bir yıl boyunca veya 6.40 saatlik fiyat üzerinden kapasite satın alabilirsiniz. Ayrılmış kapasite üzerinde fiyatlandırmaya [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/)).  

* Aktarım hızı (Kullandıkça Öde) maliyeti: 100.000 RU/sn/100 * $0.008/saat * $70,080 yıllık 8760 saat = 

* Aktarım hızı (ile ayrılmış Kapasite) maliyeti $% 20 indirimli 70,080 $56,064 = 

Etkili bir şekilde önceden satın aldıklarınızı 8 saatte 100 K RU/sn'ye 6.40 saat başına fiyatına Doğu ABD, liste fiyatı kullanarak kredi olur. Ardından bu ön ödemeli aktarım hızı ayırma sağlanan aktarım hızı kapasitesi herhangi genel bir Azure bölgesinde fiyatlarla ilgili bölgesel listesi için aboneliği ayarlamak için saatlik olarak çizebilirsiniz. Bu örnekte, burada sağladığınız 50 K RU/sn her Doğu ABD, Doğu Japonya, $8.00 değerinde saat başına sağlanan aktarım hızının çizmek mümkün olacaktır ve olacak her saat (veya $360/ay) 0,50 ABD Doları kapasite aşımı olarak faturalandırılır. 

|**Bölge**| **100 RU/sn başına saatlik fiyat**|**Birimleri (RU/sn)**| **Faturalandırılan miktar (saat)**| **Faturalandırılan miktar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|$0.008 |50 BİN|$4|$2,880 |
|Japonya Doğu|$0.009 |50 BİN| $4.50 |$3,240 |
|||Kullandıkça öde|$8.50|$6120|
|Satın Alınan Ayrılmış Kapasite|$0.0064 (% 20 indirim) |100 RU/sn veya önceden satın alınan $8 kapasitesi |-$8|-$5,760 |
|Net Fatura|||$0.50 |$360 |

## <a name="next-steps"></a>Sonraki Adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [fiyatlandırma modeli nasıl Cosmos DB, müşterilerin uygun maliyetli](total-cost-ownership.md)
* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)
