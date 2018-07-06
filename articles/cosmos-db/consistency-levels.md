---
title: Azure Cosmos DB'deki tutarlılık düzeyleri | Microsoft Docs
description: Azure Cosmos DB Bakiye nihai tutarlılık, kullanılabilirlik ve gecikme süresi dengelemeler yardımcı olmak üzere beş tutarlılık düzeyi vardır.
keywords: Nihai tutarlılık, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 20edcd5e8e3ec3a9d3d294f7a81a2e97b4958f50
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857193"
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'de veri ayarlanabilir tutarlılık düzeyleri
Azure Cosmos DB baştan yukarı genel dağıtım aklınızda her veri modeli için tasarlanmıştır. Öngörülebilir düşük gecikme süresi garantileri ve birden fazla gevşek iyi tanımlanmış tutarlılık modeli sunmak üzere tasarlanmıştır. Şu anda, Azure Cosmos DB, beş tutarlılık düzeyi sunar: güçlü, bağımlı eskime, oturum, tutarlı ön ek ve nihai. Kullanılabilen en son derece tutarlı modeli olan daha az tutarlılık daha güçlü, sağladıkları gibi sınırlanmış eskime, oturum, tutarlı ön ek ve nihai olan "tutarlılıkla modelleri olarak" gösteriyor. 

Yanında **güçlü** ve **nihai tutarlılık** sunulan yaygın olarak dağıtılmış veritabanı tarafından modelleri Azure Cosmos DB, üç daha fazla dikkatli bir şekilde kod oluşturulmuş ve çalışır hale getirilen tutarlılık modeli sunar:  **sınırlanmış eskime durumu**, **oturumu**, ve **tutarlı ön ek**. Bu tutarlılık düzeylerinin her biri kullanışlılığını gerçek dünya kullanımlara karşı doğrulandı. Topluca bu beş tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında iyi reasoned dengelemeler yapmak etkinleştirin. 

Aşağıdaki videoda, Azure Cosmos DB Program Yöneticisi Manager Andrew Liu anahtar teslim küresel dağıtım özelliklerini gösterir.

>[!VIDEO https://www.youtube.com/embed/-4FsGysVD14]

## <a name="distributed-databases-and-consistency"></a>Veritabanlarını ve tutarlılık
Ticari olarak dağıtılmış veritabanları iki kategoriye ayrılır: iyi tanımlanmış, kanıtlanabilir tutarlılık seçenekleri hiç sunmaz veritabanları ve iki olağan dışı programlama seçeneği (güçlü ve nihai tutarlılık) sunan veritabanları. 

İlk veritabanı grubu, uygulama geliştiricilerini çoğaltma protokollerinin küçük ayrıntılarından kurtarır ve tutarlılık, kullanılabilirlik, gecikme süresi ve aktarım hızı arasında zorlu seçimler yapmalarını bekler. İkinci veritabanı grubu ise iki olağan dışı örnekten birinin seçilmesini gerektirir. 50’den fazla tutarlılık modeli üzerinde yapılan çok sayıda araştırma ve teklife rağmen, dağıtılmış veritabanı topluluğu güçlü ve nihai tutarlılığı aşan tutarlılık düzeylerini ticari düzeye taşıyamamıştır. Cosmos DB, sınırlanmış eskime durumu tutarlılık spektrumu – güçlü, birlikte beş iyi tanımlanmış tutarlılık modeli işaretlemeniz geliştiricilerin sağlar [oturumu](http://dl.acm.org/citation.cfm?id=383631), tutarlı ön ek ve nihai. 

![Azure Cosmos DB, aralarından seçim yapabileceğiniz birden fazla iyi tanımlanmış (esnek) tutarlılık modeli sunar](./media/consistency-levels/five-consistency-levels.png)

Aşağıdaki tabloda her tutarlılık düzeyinin sunduğu garantiler gösterilmektedir.
 
**Tutarlılık Düzeyleri ve garantiler**

| Tutarlılık Düzeyi | Garantiler |
| --- | --- |
| Güçlü | Doğrusallaştırılabilirlik. Bir öğe en son sürümünü döndürmek için okuma garanti edilir.|
| Sınırlanmış Eskime Durumu | Tutarlı Ön Ek. En fazla k ön eklerine veya t aralığına göre yazma işlemlerinin arkasındaki okuma lag |
| Oturum   | Tutarlı Ön Ek. Monoton okuma, monoton yazma, yazdıklarınızı okuma, okuduktan sonra yazma |
| Tutarlı Ön Ek | Döndürülen güncelleştirmeler, boşluk olmadan tüm güncelleştirmelerin bazı ön ekleridir |
| Nihai  | Bozuk okumalar |

Cosmos DB hesabınızdaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz (ve daha sonra belirli bir okuma isteğinde tutarlılığı geçersiz kılabilirsiniz). Dahili olarak, varsayılan tutarlılık düzeyini bölgeleri kapsayabilir bölüm kümeleri içinde veriler için geçerlidir. Sınırlanmış eskime durumu, Azure Cosmos DB kiracılar kullanım oturum tutarlılığı yaklaşık %73 ve % 20 tercih eder. Azure Cosmos DB müşterilerin yaklaşık %3 çeşitli tutarlılık düzeyleri ile kendi uygulama için belirli tutarlılık seçim sonlandırma önce başlangıçta deneyin. Yalnızca %2 kiracıların Azure Cosmos DB tutarlılık düzeyleri istek başına temelinde geçersiz kılar. 

Cosmos DB, okuma bölgesinden oturum, tutarlı ön ek ve nihai tutarlılık olarak okur ile güçlü veya sınırlanmış eskime durumu tutarlılık ucuz iki kez. Cosmos DB, sektör lideri kapsamlı SLA'lar tutarlılık Garantisi ile kullanılabilirlik, aktarım hızı ve gecikme süresi dahil olmak üzere sahiptir. Azure Cosmos DB kullanan bir [doğrusallaştırılabilirlik denetleyicisi](http://dl.acm.org/citation.cfm?id=1806634), hizmet telemetrisi üzerinde sürekli olarak çalışır ve açık yayımlanmaması tutarlılık ihlaller size bildirir. Sınırlanmış eskime durumu, Azure Cosmos DB izler ve herhangi bir ihlali k ve t sınırları bildirir. Azure Cosmos DB, ayrıca tüm beş gevşek tutarlılık düzeyi için raporları [eskime ölçüm'probabilistically sınırlanmış](http://dl.acm.org/citation.cfm?id=2212359) doğrudan.  

## <a name="service-level-agreements"></a>Hizmet düzeyi sözleşmeleri

Azure Cosmos DB, kapsamlı % 99,99 oranında sunar [SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) garantisi aktarım hızı, tutarlılık, kullanılabilirlik ve gecikme süresi, Azure Cosmos DB veritabanı herhangi beş tutarlılık ile yapılandırılmış tek bir Azure bölgesine kapsamlı hesapları düzeyi veya tüm dört gevşek tutarlılık düzeyleri ile yapılandırılmış birden fazla Azure bölgesini kapsayan veritabanı hesaplar. Ayrıca, bir tutarlılık düzeyi seçimi, bağımsız olarak, Azure Cosmos DB okuma kullanılabilirlik için iki veya daha fazla Azure bölgesini kapsayan veritabanı hesaplarında % 99,999 SLA'sı sunar.

## <a name="scope-of-consistency"></a>Tutarlılık kapsamı
Ayrıntı düzeyini tutarlılık için tek bir kullanıcı isteği kapsamlıdır. Bir yazma isteği karşılık gelen bir ekleme, değiştirme, upsert veya işlem silin. Yazma gibi bir okuma/sorgu işlem de tek bir kullanıcı isteği için kapsama alınır. Kullanıcı üzerinde büyük bir gösterilecek şekilde sayfalara bölmek için sonuç kümesi, birden çok bölüme yayılan gerekebilir, ancak her işlem tek bir sayfaya kapsamlı ve gelen tek bir bölüm içinde sunulan okuyun.

## <a name="consistency-levels"></a>Tutarlılık düzeyleri
Cosmos DB hesabınız kapsamında tüm kapsayıcıları (ve veritabanları) için geçerlidir, veritabanı hesabındaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz. Varsayılan olarak, tüm okuma ve kullanıcı tanımlı kaynaklarında verilen sorgular veritabanı hesabında belirtilen varsayılan tutarlılık düzeyini kullanır. Özel bir okuma/sorgu isteği her desteklenen API'leri kullanarak bir tutarlılık düzeyi ılımlı hale getirebilir. Bu bölümde açıklandığı gibi belirli tutarlılık garantisi ve performans arasında NET bir denge sağlayan bir Azure Cosmos DB çoğaltma protokolü tarafından desteklenen tutarlılık düzeyleri beş tür vardır.

<a id="strong"></a>
**Güçlü**: 

* Güçlü tutarlılık sunan bir [doğrusallaştırılabilirlik](https://aphyr.com/posts/313-strong-consistency-models) garanti öğenin en son sürümünü dönmesi garanti okur ile. 
* Güçlü tutarlılık Çoğunluk çekirdeği tarafından depolanmasına kararlıdır sonra yazma yalnızca görünür olmasını garanti eder. Bir yazma ya da eşzamanlı olarak birincil ve ikincil veritabanı çekirdeği tarafından dizinlendiğini veya iptal edildi. Okuma her zaman okuma çoğunluğu tarafından onaylandı, bir istemci her zaman en son kabul edilen yazma okumak için garanti edilir ve hiçbir zaman işlenmemiş ya da kısmi bir yazma görebilirsiniz. 
* Güçlü tutarlılık kullanacak şekilde yapılandırılmış olan azure Cosmos DB hesapları ile Azure Cosmos DB hesabını birden fazla Azure bölgesine ilişkilendiremezsiniz.  
* Okuma işlemi maliyeti (açısından [istek birimi](request-units.md) tüketilen) yüksek oturum ve nihai, güçlü tutarlılık ile olan ancak sınırlanmış eskime durumu ile aynı.

<a id="bounded-staleness"></a>
**Sınırlanmış eskime durumu**: 

* Sınırlanmış eskime durumu tutarlılık garantileri okuma göre yazma işlemlerinin arkasındaki en fazla lag *K* sürümleri veya bir öğenin ön ekleri veya *t* zaman aralığı. 
* Seçme sınırlanmış eskime durumu, bu nedenle, "eskime" iki şekilde yapılandırılabilir: sürümleri sayısı *K* olarak okuma yazma ve zaman aralığını arkasındaki lag öğesinin *t* 
* Sınırlanmış eskime durumu teklifler toplam genel sıra dışında "eskime durumu penceresi içinde." Monoton okuma garantisi mevcut bir bölgede hem iç hem de dış "eskime durumu pencere." 
* Sınırlanmış eskime durumu, oturum, tutarlı ön ek veya nihai tutarlılık değerinden daha güçlü tutarlılık garantisi sağlar. Global olarak dağıtılmış uygulamalar için % 99,99 kullanılabilirlik ve düşük gecikme süresine sahip güçlü tutarlılık ancak aynı zamanda istediğiniz senaryolar için sınırlanmış eskime durumu kullanmanızı öneririz.   
* Sınırlanmış eskime durumu tutarlılık ile yapılandırılmış olan azure Cosmos DB hesapları dilediğiniz sayıda Azure bölgesinde, Azure Cosmos DB hesabınızla ilişkilendirebilirsiniz. 
* Bir okuma işlemi (tüketilen RU) açısından maliyeti ile sınırlanmış eskime durumu, oturum ve nihai tutarlılık, ancak güçlü tutarlılık ile aynı daha yüksektir.

<a id="session"></a>
**Oturum**: 

* Güçlü ve sınırlanmış eskime durumu tutarlılık düzeyi tarafından sunulan genel tutarlılık modeli, oturum tutarlılığı için bir istemci oturumundan kapsamlıdır. 
* Oturum tutarlılığı monoton okuma, monoton yazma ve okuma yazma (RYW) garanti garanti olduğundan bir cihaz veya kullanıcı oturumu burada dahil tüm senaryolar için idealdir. 
* Oturum tutarlılığı, oturum açmak için tahmin edilebilir tutarlılık sağlar ve düşük gecikme süresi yazma ve okuma sunmaya devam ederken en fazla aktarım hızı okuyun. 
* Oturum tutarlılığı ile yapılandırılmış olan azure Cosmos DB hesapları dilediğiniz sayıda Azure bölgesinde, Azure Cosmos DB hesabınızla ilişkilendirebilirsiniz. 
* Bir okuma işlemi (tüketilen RU) açısından maliyeti küçüktür güçlü ve sınırlanmış eskime durumu, ancak birden fazla nihai tutarlılık ile oturum tutarlılık düzeyi olan.

<a id="consistent-prefix"></a>
**Tutarlı ön ek**: 

* Tutarlı ön ek başka yazma işlemlerinin olmaması durumunda, bu grup içindeki çoğaltmalar sonunda yakınsama garanti eder. 
* Tutarlı ön ek okumalar hiçbir zaman sıralamaya yazma gördüğünüzü garanti eder. Yazma sırayla gerçekleştirilmişse `A, B, C`, sonra da bir istemci ya da görür `A`, `A,B`, veya `A,B,C`, ancak hiçbir zaman sıralamaya gibi `A,C` veya `B,A,C`.
* Tutarlı ön ek tutarlılık ile yapılandırılmış olan azure Cosmos DB hesapları dilediğiniz sayıda Azure bölgesinde, Azure Cosmos DB hesabınızla ilişkilendirebilirsiniz. 

<a id="eventual"></a>
**Nihai**: 

* Son tutarlılık başka yazma işlemlerinin olmaması durumunda, bu grup içindeki çoğaltmalar sonunda yakınsama garanti eder. 
* Son tutarlılık zayıf tutarlılık istemci önce gördü yapılandırılanlardan eski değerlerin nereden biçimidir.
* Nihai tutarlılık, zayıf okuma tutarlılığı sağlar ancak okuma ve yazma işlemleri için düşük gecikme süresi sunar.
* Nihai tutarlılık ile yapılandırılmış olan azure Cosmos DB hesapları dilediğiniz sayıda Azure bölgesinde, Azure Cosmos DB hesabınızla ilişkilendirebilirsiniz. 
* Bir okuma işlemi (tüketilen RU) açısından maliyeti nihai tutarlılık ile tüm Azure Cosmos DB tutarlılık düzeyi en düşük düzeydir.

## <a name="configuring-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma
1. İçinde [Azure portalında](https://portal.azure.com/), atlama Çubuğu'nda tıklatın **Azure Cosmos DB**.
2. **Azure Cosmos DB** sayfasında, değiştirilecek veritabanı hesabını seçin.
3. Hesap sayfasındaki tıklayın **varsayılan tutarlılık**.
4. İçinde **varsayılan tutarlılık** sayfasında yeni tutarlılık düzeyi seçin ve tıklayın **Kaydet**.
   
    ![Ayarlar simgesi ve varsayılan tutarlılık giriş seçeneklerinin vurgulandığı ekran görüntüsü](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Sorgular için tutarlılık düzeyleri
Varsayılan olarak, kullanıcı tarafından tanımlanan kaynakları için tutarlılık düzeyi sorgular için tutarlılık düzeyi okuma için aynıdır. Varsayılan olarak, dizin, her ekleme, değiştirme veya silme Cosmos DB kapsayıcısı için bir öğenin üzerinde zaman uyumlu olarak güncelleştirilir. Bu sorguları uymanız noktadan okuma aynı tutarlılık düzeyinde sağlar. Azure Cosmos DB yazma için iyileştirilmiş ve yazma işlemleri, zaman uyumlu dizin Bakımı ve tutarlı sorguları tanıtıcısıyla Sürdürülen birimleri destekler olsa da, kendi dizini gevşek güncelleştirmek için belirli bir kapsayıcı yapılandırabilirsiniz. Yavaş dizin oluşturma daha fazla yazma performansını artırıyor ve öncelikli olarak okuma yoğun bir iş yükü olduğunda toplu alımı senaryolar için idealdir.  

| Dizin oluşturma modu | Okur | Sorgular |
| --- | --- | --- |
| Consistent (varsayılan) |Select güçlü, sınırlanmış eskime, oturum, tutarlı ön ek veya nihai |Güçlü, sınırlanmış eskime durumu, arasından seçim oturumu veya nihai |
| Geç |Select güçlü, sınırlanmış eskime, oturum, tutarlı ön ek veya nihai |Nihai |
| None |Select güçlü, sınırlanmış eskime, oturum, tutarlı ön ek veya nihai |Uygulanamaz |

Olarak okuma isteklerinin ile her API belirli sorgu istekte tutarlılık düzeyini düşürün.

## <a name="consistency-levels-for-the-mongodb-api"></a>MongoDB API'si için tutarlılık düzeyleri

Azure Cosmos DB, MongoDB, güçlü ve nihai iki tutarlılık ayarları olan sürüm 3.4, şu anda uygular. Azure Cosmos DB birden çok API'li olduğundan tutarlılık ayarları hesap düzeyinde geçerlidir ve tutarlılığın uygulatılması her API tarafından denetlenmektedir.  MongoDB'nin 3.6 sürümüne kadar oturum tutarlılığı kavramı yoktu, bu nedenle MongoDB API'si hesabını oturum tutarlılığı kullanacak şekilde ayarlarsanız, tutarlılık MongoDB API'leri kullanılırken son düzeyine düşürülür. Bir MongoDB API'si hesabı için "kendi yazmanı okuma" garantisine ihtiyacınız varsa, hesabın varsayılan tutarlılık düzeyi güçlü veya sınırlanmış eskime durumu olarak ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Tutarlılık düzeyleri ve seçenekleri hakkında daha fazla okuma yapmak istiyorsanız, aşağıdaki kaynakları öneririz:

* [Doug Terry tarafından Beyzbol (video) üzerinden çoğaltılan verilerin tutarlılık açıklaması](https://www.youtube.com/watch?v=gluIh8zd26I)
* [Doug Terry tarafından Beyzbol (Teknik İnceleme) üzerinden çoğaltılan verilerin tutarlılık açıklaması](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* [Zayıf sahipli tutarlı çoğaltılan veriler için oturumu garanti eder](http://dl.acm.org/citation.cfm?id=383631)
* [Modern dağıtılmış veritabanı sistemleri tasarım tutarlılık seçenekleri: UÇ hikayeyi yalnızca bir parçası değildir](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Olasılığa dayalı sınırlanmış eskime durumu (PBS) için pratik kısmi çekirdekleri](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* [Nihai tutarlı - Revisited](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* [Yük, kapasite ve çekirdek sistemleri, bilgi işlem üzerinde SIAM günlük kullanılabilirliği](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* [Line-Up: bir tam ve otomatik doğrusallaştırılabilirlik denetleyicisi Study of dil tasarım ve uygulama programlama 2010 SIGPLAN ACM konferansı](http://dl.acm.org/citation.cfm?id=1806634)
* [Pratik kısmi çekirdekleri için probabilistically sınırlanmış eskime durumu](http://dl.acm.org/citation.cfm?id=2212359)
