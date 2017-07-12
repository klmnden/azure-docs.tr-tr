---
title: "Azure Cosmos DB’ye Giriş | Microsoft Belgeleri"
description: "Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik ve yüksek kullanılabilirlik için oluşturulmuştur."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 49eb2e4f7d57de44a3b7a877dfdd138f4c374436
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---

<a id="welcome-to-azure-cosmos-db" class="xliff"></a>

# Azure Cosmos DB’ye hoş geldiniz

Azure Cosmos DB, Microsoft’un genel olarak dağıtılan çok modelli veritabanıdır. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Diğer veritabanı hizmetlerinin sunamadığı kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA) ile birlikte aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantileri sunar.

![Azure Cosmos DB, Microsoft'un esnek ölçek artırma, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

Azure Cosmos DB, birden fazla veri modelini (anahtar-değer, belgeler, grafikler ve sütunlu) yerel olarak destekleyen, yazma bakımından iyileştirilmiş, kaynakları yönetilen, şemadan bağımsız bir veritabanı altyapısı içerir. Verilere erişmek için [MongoDB](mongodb-introduction.md), [DocumentDB SQL](documentdb-introduction.md), [Gremlin](graph-introduction.md) (önizleme) ve [Azure Tabloları](table-introduction.md) (önizleme) dahil çok sayıda API’yi genişletilebilir biçimde destekler. 

Azure Cosmos DB, 2010 yılının sonunda geliştiricilerin Microsoft içindeki büyük ölçekli uygulamalarda karşılaştığı önemli sorunları ele almak üzere kullanıma sunulmuştur. Genel olarak dağıtılan uygulamaların oluşturulması Microsoft'a özel bir sorun olmadığından hizmeti Azure DocumentDB biçiminde tüm Azure geliştiricilerinin kullanımına açtık. Azure Cosmos DB, DocumentDB’nin gelişimindeki bir sonraki büyük adımdır ve kullanımınıza sunulmaktadır. Azure Cosmos DB’nin bu sürümünün bir parçası olarak, DocumentDB müşterileri (verileriyle birlikte) otomatik olarak Azure Cosmos DB müşterileridir. Geçiş sorunsuzdur ve müşteriler artık Azure Cosmos DB tarafından sunulan daha fazla yeni özelliğe erişebilmektedir. 

<a id="capability-comparison" class="xliff"></a>

## Özellik karşılaştırması

Azure Cosmos DB, ilişkisel ve ilişkisel olmayan veritabanlarının en iyi özelliklerini sağlar.

| Özellikler | İlişkisel DB’ler | İlişkisel olmayan (NoSQL) DB’ler |  Azure Cosmos DB |
| --- | --- | --- | --- |
| Genel dağıtım | x | x | ✓ Turnkey, 30’u aşkın bölge, çok girişli |
| Yatay ölçeklendirme | x | ✓ | ✓ Depolama ve aktarım hızını bağımsız olarak ölçeklendirme | 
| Gecikme süresi garantileri | x | ✓ | ✓ p99’da okuma için <10 ms, yazma için <15 ms | 
| Yüksek kullanılabilirlik | x | ✓ | ✓ Her zaman açık, PACELC dengeleme, otomatik ve el ile yük devretme |
| Veri modeli + API | İlişkisel + SQL | Çoklu model + OSS API’si | Çoklu model + SQL + OSS API’si (yakında daha fazlası sunulacak) |
| SLA’lar | ✓ | x | ✓ Gecikme süresi, aktarım hızı, tutarlılık ve kullanılabilirlik için kapsamlı SLA’lar |

<a id="key-capabilities" class="xliff"></a>

## Temel işlevler
Genel olarak dağıtılan bir veritabanı hizmeti olarak Azure Cosmos DB, ölçeklenebilir, genel olarak dağıtılan, son derece hızlı yanıt veren uygulamalar oluşturmanıza yardımcı olacak aşağıdaki özellikleri sağlar:

* [**Anahtar teslim genel dağıtım**](#global-distribution)
    * Uygulamanız kullanıcılarınız tarafından hemen ve her yerde kullanılabilir. Bundan böyle, verileriniz de kullanılabilir.
    * Donanım, düğüm ekleme, VM veya çekirdekler hakkında endişelenmeyin. Yalnızca üzerine gelip tıkladığınızda verileriniz oradadır. 

* [**Çoklu veri modelleri ve verilere erişim ile veri sorgulamaya yönelik popüler API’ler**](#data-models)
    * Anahtar-değer, belge, grafik ve sütunlu dahil çoklu veri modeli desteği.
    * Node.js, Java, .NET, .NET Core, Python ve MongoDB için genişletilebilir API’ler.
    * Sorgular için SQL ve Gremlin. 

* [**Aktarım hızı ve depolamayı dünyanın dört bir yanında, talep üzerine esnek bir şekilde ölçeklendirin**](#horizontal-scale)
    * Aktarım hızını [saniye](request-units.md) ve [dakika](https://aka.ms/acdbrupm) ayrıntı düzeylerinde kolayca ölçeklendirin ve dilediğiniz zaman değiştirin. 
    * Boyut gereksinimlerinizi şimdi ve sonsuza dek karşılamak için depolama alanınızı [şeffaf ve otomatik bir şekilde](partition-data.md) ölçeklendirin.

* [**Yüksek hızda yanıt veren ve görev açısından kritik uygulamalar oluşturun**](#low-latency) 
    * 99 yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleriyle verilerinize erişin. 

* [**"Her zaman açık" özelliği ile kullanılabilirlik endişeniz olmaz**](#high-availability)
    * Tek bir bölge içinde %99,99 kullanılabilirlik.
    * Daha yüksek kullanılabilirlik için dilediğiniz sayıda [Azure bölgesine](https://azure.microsoft.com/regions) dağıtın.
    * Sıfır veri kaybı garantisi ile bir veya daha fazla bölgenin [hata benzetimini gerçekleştirin](regional-failover.md). 

* [**Genel olarak dağıtılan uygulamaları, doğru şekilde yazın**](#consistency)
    * [Beş tutarlılık modeli](consistency-levels.md), NoSQL benzeri nihai tutarlılık, güçlü SQL benzeri tutarlılığı ve çok daha fazlasını sunar. 
  
* [**Para iadesi garantisi**](#sla) 
    * Verileriniz gitmesi gereken yere hızlı bir şekilde ulaşmazsa paranız iade edilir. 
    * Kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılık için [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla). 

* [**Veritabanı şema/dizin yönetimi yok**](#schema-free)
    * Veritabanı şema ve dizinlerinizi uygulamanızın şemasıyla eşitleme konusunda endişelenmeyi bırakın. Bizde şema yok. 

* [**Düşük sahip olma maliyeti**](#tco)
    * Yönetilmeyen bir çözüme göre beş ila on kat [daha uygun maliyetli](https://aka.ms/documentdb-tco-paper).
    * DynamoDB’den üç kat daha ucuz.

<a id="global-distribution"></a>

<a id="global-distribution" class="xliff"></a>

## Genel dağıtım
Azure Cosmos DB kapsayıcıları iki boyutta dağıtılır: 

1. Belirli bir bölge içinde tüm kaynaklar, kaynak bölümleri kullanılarak yatay yönde bölümlenir (yerel dağıtım). 
2. Her kaynak bölümü aynı zamanda coğrafi bölgeler arasında çoğaltılır (genel dağıtım). 

![Azure Cosmos DB'nin genel dağıtımını gösteren diyagram](./media/introduction/azure-cosmos-db-global-distribution.png) 

Depolama ve aktarım hızınızın ölçeklenmesi gerektiğinde, Cosmos DB tüm bölgelerde bölüm yönetimi işlemlerini şeffaf bir şekilde gerçekleştirir. Ölçek, dağıtım veya hatalardan bağımsız olan Cosmos DB, genel olarak dağıtılmış kaynakların tek bir sistem görüntüsünü sağlamaya devam eder. 

Cosmos DB’de kaynakların genel dağıtımı [kullanıma hazırdır](distribute-data-globally.md). Dilediğiniz zaman birkaç düğmeye tıklayarak (veya program aracılığıyla tek bir API çağrısı ile), dilediğiniz sayıda coğrafi bölgeyi veritabanı hesabınızla ilişkilendirebilirsiniz. 

Veri miktarına veya bölge sayısına bakılmaksızın, Cosmos DB yeni ilişkilendirilmiş her bir bölgenin 99. yüzdebirlik dilimde istemci isteklerini bir saatin altında işlemeye başlayacağını garanti eder. Bu işlem, dağıtım paralel hale getirilip tüm kaynak bölümlerinden yeni ilişkilendirilen bölgeye veriler kopyalanarak yapılır. Müşteriler ayrıca var olan bir bölgeyi kaldırabilir veya daha önce veritabanı hesabıyla ilişkilendirilmiş bir bölgeyi çevrimdışı yapabilir.

<a id="data-models"></a>
<a id="multi-model-multi-api-support" class="xliff"></a>

## Çoklu model, çoklu API desteği
 Azure Cosmos DB belgeler, anahtar-değer, grafik ve sütun ailesi dahil birden fazla veri modelini destekler. Cosmos DB veritabanı altyapısının temel içerik modeli, atom kayıt sırasını (ARS) temel alır. Atomlar dize, bool ve sayı gibi basit türlerin küçük bir kümesinden oluşur. Kayıtlar bu türlerden oluşan yapılardır. Sıralar; atom, kayıt veya sıralardan oluşan dizilerdir. 
 
 Veritabanı altyapısı, farklı veri modellerini ARS tabanlı veri modeline etkili bir şekilde çevirip yansıtabilir. Cosmos DB’nin temel veri modeline dinamik olarak yazılan programlama dillerinden yerel olarak erişilebilir ve bunlar JSON olarak olduğu gibi kullanıma sunulabilir. 
 
 Hizmet, veri erişimi ve sorgulama için popüler veritabanı API'lerini de destekler. Cosmos DB’nin veritabanı altyapısı şu anda [DocumentDB SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md), [Azure Tabloları](table-introduction.md) (önizleme) ve [Gremlin](graph-introduction.md) (önizleme) API’lerini desteklemektedir. Popüler OSS API’lerini kullanarak uygulama oluşturmaya devam edebilir ve kendini ispatlamış, tamamen yönetilen, genel olarak dağıtılan bir veritabanı hizmetinin tüm avantajlarından yararlanabilirsiniz. 

<a id="horizontal-scale"></a>
<a id="horizontal-scaling-of-storage-and-throughput" class="xliff"></a>

## Depolama ve aktarım hızı yatay ölçeklendirmesi
Bir Cosmos DB kapsayıcısındaki tüm veriler (örneğin bir belge koleksiyonu, tablo veya grafik), yatay olarak bölümlenir ve kaynak bölümler tarafından şeffaf bir şekilde yönetilir. Kaynak bölümü, [müşteri tarafından belirtilen bir bölüm anahtarı](partition-data.md) ile bölümlenmiş, tutarlı ve yüksek oranda kullanılabilir bir veri kapsayıcısıdır. Bu kapsayıcı, yönettiği kaynak kümesi için tek bir sistem görüntüsü sağlar ve ölçeklenebilirlik ile dağıtımın temel birimidir. Cosmos DB, coğrafi bölge ve saate göre farklılık gösteren dalgalı iş yüklerini desteklemek üzere farklı coğrafi bölgelerdeki uygulama trafiği düzenlerine göre aktarım hızını esnek bir şekilde ölçeklendirmenize olanak tanır. Hizmet bir Cosmos DB kapsayıcısının kullanılabilirlik, tutarlılık, gecikme süresi veya aktarım hızından ödün vermeden bölümleri şeffaf bir şekilde yönetir.  
 
![Azure Cosmos DB yatay yönde ölçeklenebilir](./media/introduction/azure-cosmos-db-partitioning.png) 

Bir Azure Cosmos DB kapsayıcısının aktarım hızını, [saniye başına istek birimi (RU/sn)](request-units.md) ile program aracılığıyla esnek bir şekilde ölçekleyebilirsiniz. Dahili olarak, hizmet belirli bir kapsayıcıdaki aktarım hızını iletmek için kaynak bölümlerini şeffaf bir şekilde yönetir. Cosmos DB, aktarım hızının kapsayıcı ile ilişkili tüm bölgelerde kullanılabilmesini sağlar. Yeni aktarım hızı, yapılandırılmış aktarım hızı değerinde değişiklik yapılmasından sonraki beş saniye içinde etkili olur. 

Bir Cosmos DB kapsayıcısında aktarım hızını saniye başına ve [dakika başına (RU/dk)](request-units-per-minute.md) ayrıntı düzeylerinde sağlayabilirsiniz. Dakika başına ayrıntı düzeyinde sağlanan aktarım hızı, saniye başına ayrıntı düzeyinde iş yükünde oluşan beklenmedik artışları yönetmek için kullanılır. 

<a id="low-latency"></a>
<a id="low-latency-guarantees-at-the-99th-percentile" class="xliff"></a>

## 99. yüzdebirlik dilimde düşük gecikme süresi garanti edilir
SLA’larının bir parçası olarak Cosmos DB, müşterilerine 99. yüzdebirlik dilimde uçtan uca düşük gecikme süresi garanti etmektedir. Cosmos DB, tipik bir 1 KB’lik öğe için aynı Azure bölgesindeki 99. yüzdebirlik dilimde 10 ms’nin altında okumalar ve 15 ms’nin altında dizini oluşturulmuş yazmalar için uçtan uca gecikme süresi garanti eder. Ortalama gecikme süreleri çok daha düşüktür (5 ms’nin altında).  Her veritabanı işlemindeki istek işleme üst sınırı ile Cosmos DB, müşterilerin yüksek gecikme süresine sahip işlemleri kullanılamayan bir veritabanından açıkça ayırt etmesine olanak tanır.

<a id="high-availability"></a>
<a id="transparent-multi-homing-and-9999-high-availability" class="xliff"></a>

## Şeffaf çoklu giriş ve %99,99 yüksek kullanılabilirlik
"Öncelikleri", Azure Cosmos DB veritabanı hesabınızla ilişkili bölgelerle dinamik olarak ilişkilendirebilirsiniz. Öncelikler, bölgesel hatalar olması durumunda istekleri belirli bölgelere yönlendirmek için kullanılır. Bölgesel bir olağanüstü durumun yaşandığı nadir durumlarda, Cosmos DB öncelik sırasına göre otomatik olarak yük devreder.

Uygulamanın uçtan uca kullanılabilirliğini sınamak için [yük devretmeyi el ile tetikleyebilirsiniz](regional-failover.md) (hız saat başına iki işlemle sınırlıdır). Cosmos DB, el ile bölgesel yük devretme işlemleri sırasında veri kaybı olmayacağını garanti eder. Bölgesel bir olağanüstü durum oluşursa, Cosmos DB sistem tarafından başlatılan otomatik yük devretme sırasında veri kaybına ilişkin bir üst sınır olacağını garanti eder. Bölgesel yük devretme sonrasında uygulamanızı yeniden dağıtmanız gerekmez ve kullanılabilirlik SLA'ları Azure Cosmos DB tarafından korunur. 

Bu senaryo için Cosmos DB, mantıksal (bölgeden bağımsız) veya fiziksel (bölgeye özel) uç noktalar kullanarak kaynaklarla etkileşimde bulunmanıza olanak tanır. Mantıksal uç noktalar, uygulamanın yük devretme durumunda şeffaf bir şekilde çok ana bilgisayarlı olabilmesini sağlar. Fiziksel uç noktalar ise uygulamaya okuma ve yazma işlemlerini belirli bölgelere yönlendirmesi için ayrıntılı denetim sağlar. Cosmos DB, her veritabanı hesabı için %99,99 kullanılabilir SLA’sı garanti eder. Kullanılabilirlik garantileri ölçek (sağlanan aktarım hızı ve depolama), bölge sayısı veya belirli bir veritabanıyla ilişkili bölgeler arasındaki coğrafi uzaklıktan bağımsızdır. 

<a id="consistency"></a>
<a id="multiple-well-defined-consistency-models" class="xliff"></a>

## Birden çok, iyi tanımlanmış tutarlılık modeli
Ticari olarak dağıtılmış veritabanları iki kategoriye ayrılır: iyi tanımlanmış, kanıtlanabilir tutarlılık seçenekleri sunmayan veritabanları ve iki olağan dışı programlama seçeneği (güçlü ve nihai tutarlılık) sunan veritabanları. İlk veritabanı grubu, uygulama geliştiricilerini çoğaltma protokollerinin küçük ayrıntılarından kurtarır ve tutarlılık, kullanılabilirlik, gecikme süresi ve aktarım hızı arasında zorlu seçimler yapmalarını bekler. İkinci veritabanı grubu ise iki olağan dışı örnekten birinin seçilmesini gerektirir. 50’den fazla tutarlılık modeli üzerinde yapılan çok sayıda araştırma ve teklife rağmen, dağıtılmış veritabanı topluluğu güçlü ve nihai tutarlılığı aşan tutarlılık düzeylerini ticari düzeye taşıyamamıştır. 

Cosmos DB, tutarlılık spektrumunda [iyi tanımlanmış beş tutarlılık modeli](consistency-levels.md) arasından seçim yapmanıza olanak tanır: güçlü, sınırlanmış eskime durumu, [oturum](http://dl.acm.org/citation.cfm?id=383631), tutarlı ön ek ve nihai. 

![Azure Cosmos DB, aralarından seçim yapabileceğiniz birden fazla iyi tanımlanmış (esnek) tutarlılık modeli sunar](media/introduction/azure-cosmos-db-consistency-levels.png)

Aşağıdaki tabloda her tutarlılık düzeyinin sunduğu garantiler gösterilmektedir.
 
**Tutarlılık Düzeyleri ve garantiler**

| Tutarlılık Düzeyi | Garantiler |
| --- | --- |
| Güçlü | Doğrusallaştırma |
| Sınırlanmış Eskime Durumu | Tutarlı Ön Ek. -k ön eklerine veya t aralığına göre yazma işlemlerinin arkasındaki gecikmeyi okur |
| Oturum   | Tutarlı Ön Ek. Monoton okuma, monoton yazma, yazdıklarınızı okuma, okuduktan sonra yazma |
| Tutarlı Ön Ek | Döndürülen güncelleştirmeler, boşluk olmadan tüm güncelleştirmelerin bazı ön ekleridir |
| Nihai  | Bozuk okumalar |

Cosmos DB hesabınızdaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz (ve daha sonra belirli bir okuma isteğinde tutarlılığı geçersiz kılabilirsiniz). Dahili olarak, varsayılan tutarlılık düzeyi, yayılmış bölgeleri olabilecek bölüm kümelerindeki verilere uygulanır. 


<a id="sla"></a>
<a id="guaranteed-service-level-agreements" class="xliff"></a>

## Garantili hizmet düzeyi sözleşmeleri

Cosmos DB kullanılabilirlik, aktarım hızı, düşük gecikme süresi ve tutarlılık için %99,99 [SLA garantisi](https://aka.ms/acdbsla) sunan ilk yönetilen veritabanı hizmetidir.
* Kullanılabilirlik: veri ve kontrol panosu işlemlerinin her biri için %99,99 çalışma süresi SLA'sı.
* Aktarım hızı: isteklerin %99,99’u başarıyla tamamlandı 
* Gecikme süresi: 99. yüzdebirlik dilimde 10 ms’nin altındaki gecikme sürelerinin %99,99’u
* Tutarlılık: Okuma isteklerinin %100'ü, istediğiniz tutarlılık düzeyi için tutarlılık garantisini karşılar.


<a id="schema-free"></a>
<a id="schema-free" class="xliff"></a>

## Şemasız

Hem ilişkisel hem de NoSQL veritabanları, sizi şema ve dizin yönetimi, sürüm oluşturma ve geçiş gibi işlemler yapmaya zorlar; genel olarak dağıtılan bir kurulumda bu işlemlerin tümü son derece zorludur. Ancak endişelenmeyin; Cosmos DB bu sorunu ortadan kaldırıyor! Cosmos DB ile şema ve dizinleri yönetmekten, şema sürümü oluşturmayla uğraşmaktan veya şemaları geçirirken uygulamanın kapalı kalma süresinden endişe duymanız gerekmez. Cosmos DB’nin veritabanı altyapısı tam olarak şemadan bağımsızdır; aldığı tüm verileri, herhangi bir şema veya dizin gerektirmeden otomatik olarak dizine alır ve ışık hızında sorgular sunar. 

<a id="tco"></a>
<a id="low-cost-of-ownership" class="xliff"></a>

## Düşük sahip olma maliyeti

 Tüm toplam sahip olma maliyeti (TCO) konuları hesaba katıldığında, Azure Cosmos DB gibi yönetilen bulut hizmetleri şirket içinde veya sanal makinelerde çalışan OSS muadillerinden beş ila on kat daha uygun maliyetli olabilir. Azure Cosmos DB ayrıca yüksek hacimli iş yükleri için DynamoDB’den iki ila üç kat daha ucuzdur. [TCO teknik incelemesini](https://aka.ms/documentdb-tco-paper) okuyarak daha fazla bilgi edinin. 

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB'nin DocumentDB API’si ile çalışmaya başlama](create-documentdb-dotnet.md)
* [Azure Cosmos DB'nin MongoDB API’si ile çalışmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB'nin Grafik API’si ile çalışmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB'nin Tablo API’si ile çalışmaya başlama](create-table-dotnet.md)

