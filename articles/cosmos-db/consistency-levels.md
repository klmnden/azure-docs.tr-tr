---
title: "Azure Cosmos veritabanı tutarlılık düzeylerini | Microsoft Docs"
description: "Azure Cosmos DB Bakiye nihai tutarlılık, kullanılabilirlik ve gecikme dengelemeler yardımcı olmak üzere beş tutarlılık düzeyleri vardır."
keywords: "Nihai tutarlılık, azure cosmos db, azure, Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3bd28316e3d2e7596021d6964594002d47d160a
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>İnce ayarlanabilir veri tutarlılık düzeylerini Azure Cosmos veritabanı
Azure Cosmos DB sıfırdan yukarı genel dağıtım aklınızda her veri modeli için tasarlanmıştır. Tahmin edilebilir düşük gecikme süresi garanti ve birden çok iyi tanımlanmış gevşek tutarlılık modelleri sunmak üzere tasarlanmıştır. Şu anda Azure Cosmos DB beş tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı öneki ve son. En yüksek oranda tutarlı bir model kullanılabilir olan daha az tutarlılık daha güçlü, sağladıkları gibi sınırlanmış eskime durumu, oturum, tutarlı öneki ve nihai olan "gevşek tutarlılık modelleri olarak" gösteriyor. 

Yanında **güçlü** ve **nihai tutarlılık** sunulan yaygın olarak dağıtılan veritabanları tarafından modelleri Azure Cosmos DB üç daha fazla dikkatle kod oluşturulmuş ve kullanıma hazır hale getirilmiş tutarlılık modeli sunar:  **sınırlanmış eskime durumu**, **oturum**, ve **tutarlı önek**. Bu tutarlılık düzeylerin her birinde yararlılığı gerçek dünya kullanımı talepleri doğrulandı. Topluca bu beş tutarlılık düzeyleri iyi reasoned dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında yapmanızı sağlar. 

## <a name="distributed-databases-and-consistency"></a>Dağıtılmış veritabanları ve tutarlılık
Ticari dağıtılmış veritabanlarının iki kategoriye ayrılır: provable iyi tanımlanmış tutarlılık seçimler hiç sunmazlar veritabanları ve iki aşırı programlama (nihai tutarlılık güçlü) seçimlere veritabanları. 

İlk veritabanı grubu, uygulama geliştiricilerini çoğaltma protokollerinin küçük ayrıntılarından kurtarır ve tutarlılık, kullanılabilirlik, gecikme süresi ve aktarım hızı arasında zorlu seçimler yapmalarını bekler. İkinci veritabanı grubu ise iki olağan dışı örnekten birinin seçilmesini gerektirir. 50’den fazla tutarlılık modeli üzerinde yapılan çok sayıda araştırma ve teklife rağmen, dağıtılmış veritabanı topluluğu güçlü ve nihai tutarlılığı aşan tutarlılık düzeylerini ticari düzeye taşıyamamıştır. Cosmos DB geliştiriciler sınırlanmış eskime durumu tutarlılığı spektrumun – güçlü, boyunca beş iyi tanımlanmış tutarlılık modelleri arasında seçmenizi sağlayan [oturum](http://dl.acm.org/citation.cfm?id=383631), tutarlı öneki ve son. 

![Azure Cosmos DB, aralarından seçim yapabileceğiniz birden fazla iyi tanımlanmış (esnek) tutarlılık modeli sunar](./media/consistency-levels/five-consistency-levels.png)

Aşağıdaki tabloda her tutarlılık düzeyinin sunduğu garantiler gösterilmektedir.
 
**Tutarlılık Düzeyleri ve garantiler**

| Tutarlılık Düzeyi | Garantiler |
| --- | --- |
| Güçlü | Linearizability. Bir öğe en son sürümüne geri dönmek için okuma garanti.|
| Sınırlanmış Eskime Durumu | Tutarlı Ön Ek. -k ön eklerine veya t aralığına göre yazma işlemlerinin arkasındaki gecikmeyi okur |
| Oturum   | Tutarlı Ön Ek. Monoton okuma, monoton yazma, yazdıklarınızı okuma, okuduktan sonra yazma |
| Tutarlı Ön Ek | Döndürülen güncelleştirmeler, boşluk olmadan tüm güncelleştirmelerin bazı ön ekleridir |
| Nihai  | Bozuk okumalar |

Cosmos DB hesabınızdaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz (ve daha sonra belirli bir okuma isteğinde tutarlılığı geçersiz kılabilirsiniz). Dahili olarak, varsayılan tutarlılık düzeyi bölgeler yayılabilir bölüm kümeleri içinde veriler için geçerlidir. Yaklaşık Azure Cosmos DB kiracılar kullanım oturum tutarlılığı %73 ve % 20 sınırlanmış eskime durumu tercih eder. Yaklaşık %3 Azure Cosmos DB müşterilerin kendi uygulama için belirli tutarlılık seçim şekilde önce çeşitli tutarlılık düzeylerini başlangıçta deneyin. Azure Cosmos DB kiracılar % 2'yalnızca bir istek başına tutarlılık düzeylerini geçersiz kılar. 

Cosmos DB nihai tutarlılık güçlü veya sınırlanmış eskime durumu tutarlılığı ile okuma olarak ucuz iki kez ve okuma oturumunda, tutarlı önek sunulamıyor. Cosmos DB endüstri lideri tutarlılığı garanti birlikte kullanılabilirliği, performansı ve gecikme süresi dahil olmak üzere kapsamlı SLA sahiptir. Azure Cosmos DB kullanan bir [linearizability denetleyicisi](http://dl.acm.org/citation.cfm?id=1806634), sürekli olarak hizmet telemetri çalışır ve açık yayımlanmaması herhangi bir tutarlılık ihlali size bildirir. Sınırlanmış eskime durumu için Azure Cosmos DB izler ve herhangi bir ihlali k ve t sınırları için raporlar. Tüm beş gevşek tutarlılık düzeyleri için de Azure Cosmos DB raporları [probabilistically, eskime durumu ölçüm ilişkisindeki](http://dl.acm.org/citation.cfm?id=2212359) doğrudan.  

## <a name="service-level-agreements"></a>Hizmet düzeyi sözleşmeleri

Azure Cosmos DB sunan kapsamlı % 99,99 [SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) hangi garantisi verimlilik, tutarlılık, kullanılabilirlik ve gecikme Azure Cosmos DB için veritabanı herhangi beş tutarlılık denetimi ile yapılandırılmış tek bir Azure bölgesine kapsamlı hesapları düzeyleri veya herhangi bir dört gevşek tutarlılık düzeyleri ile yapılandırılmış birden fazla Azure bölgesine yayılan veritabanı hesaplar. Ayrıca, bağımsız bir tutarlılık düzeyi tercih Azure Cosmos DB %99.999 SLA iki veya daha fazla Azure bölgeleri kapsayıcı veritabanı hesapları için okuma kullanılabilirlik sunar.

## <a name="scope-of-consistency"></a>Tutarlılık kapsamı
Tutarlılık kesinliği tek bir kullanıcı isteğine kapsamlıdır. Yazma isteği karşılık gelen bir ekleme, değiştirme, upsert veya işlem silin. Yazma gibi bir okuma/sorgu işlem de bir tek kullanıcı isteğine kapsama alınır. Kullanıcı büyük sayfalara bölme için sonuç kümesi, birden çok bölüm kapsayıcı gerekebilir, ancak her işlem için tek bir sayfayla kapsamlı ve gelen tek bir bölüm içinde sunulan okuyun.

## <a name="consistency-levels"></a>Tutarlılık düzeyleri
Cosmos DB hesabınızın altında veritabanı hesabınızdaki tüm koleksiyonlar (ve veritabanları için) geçerli bir varsayılan tutarlılık düzeyi yapılandırabilirsiniz. Varsayılan olarak, tüm okuma ve kullanıcı tanımlı kaynaklarına karşı verilen sorguları veritabanı hesabındaki belirtilen varsayılan tutarlılık düzeyi kullanın. Belirli okuma/kullanarak bir sorgu isteği her desteklenen API'leri tutarlılık düzeyi hafifletin. Bu bölümde açıklandığı gibi belirli tutarlılık ve performans arasında NET bir denge sağlayan Azure Cosmos DB çoğaltma protokolü tarafından desteklenen tutarlılık düzeylerini beş türü vardır.

**Güçlü**: 

* Güçlü tutarlılık sunan bir [linearizability](https://aphyr.com/posts/313-strong-consistency-models) garanti öğeyi en son sürümüne geri dönmek için garanti okuma ile. 
* Güçlü tutarlılık, çoğaltmaların çoğunluğu çekirdek tarafından işlemi yürütüldükten sonra yazma yalnızca görünür olmasını sağlar. Bir yazma ya da zaman uyumlu olarak işlemi hem birincil hem de ikincil kopya çekirdek tarafından kararlıdır veya iptal edilir. Okuma her zaman çekirdek okuma çoğunluğu tarafından onaylanan, bir istemci her zaman en son alınan yazma okumak için garanti ve kaydedilmemiş ya da kısmi bir yazma hiçbir zaman görebilirsiniz. 
* Güçlü tutarlılık kullanmak üzere yapılandırılmış azure Cosmos DB hesapları birden fazla Azure bölgesine Azure Cosmos DB hesaplarıyla ilişkilendiremezsiniz.  
* Okuma işlemi maliyetini (cinsinden [istek birimleri](request-units.md) tüketilen) ile güçlü tutarlılık oturum daha yüksek ya da Sonuçta, olan ancak sınırlanmış eskime durumu ile aynı.

**Sınırlanmış eskime durumu**: 

* Okuma yazma tarafından arkasında en fazla geri kalabilir eskime durumu tutarlılığı garanti ilişkisindeki *K* sürümleri veya bir öğenin önekleri veya *t* zaman aralığı. 
* Sınırlanmış eskime durumu seçme gerektiğinde, bu nedenle, "eskime durumu" iki şekilde yapılandırılabilir: sürümleri sayısı *K* olarak okumaları öteleme yazma ve zaman aralığını arkasındaki öğesinin *t* 
* Sınırlanmış eskime durumu teklifleri toplam genel dışında içinde sırada "eskime durumu penceresi." Bir bölge içinde hem iç hem de dış "eskime durumu penceresi." monoton okuma garanti var 
* Sınırlanmış eskime durumu daha güçlü bir tutarlılığı garanti oturum, önek tutarlı ya da nihai tutarlılık sağlar. Genel olarak dağıtılmış uygulamalar için burada güçlü tutarlılık sahip ancak % 99,99 kullanılabilirlik ve düşük gecikme süresi de istiyorsanız istediğiniz sınırlanmış eskime durumu senaryoları için kullanmanızı öneririz.   
* Sınırlanmış eskime durumu tutarlılığı ile yapılandırılmış olan azure Cosmos DB hesapları Azure bölgeleri herhangi bir sayıda Azure Cosmos DB hesaplarıyla ilişkilendirebilirsiniz. 
* Okuma işlemi (RUs tüketilen) bakımından maliyetini sınırlanmış eskime durumu ile oturum ve nihai tutarlılık, ancak güçlü tutarlılık aynı daha yüksektir.

**Oturum**: 

* Güçlü ve sınırlanmış eskime durumu tutarlılığı düzeyleri tarafından sunulan genel tutarlılık modeller, bir istemci oturumundan oturum tutarlılığı kapsamlıdır. 
* Oturum tutarlılığı monoton okuma, monoton yazma ve okuma kendi yazma (RYW) garanti garanti bu yana bir aygıt veya kullanıcı oturumu burada dahil tüm senaryoları için idealdir. 
* Oturum tutarlılığı, oturum açmak için tahmin edilebilir tutarlılık sağlar ve en düşük gecikme süreli yazma işlemleri ve okuma sunarken verimlilik maksimum okuyun. 
* Oturum tutarlılığı ile yapılandırılmış olan azure Cosmos DB hesapları Azure bölgeleri herhangi bir sayıda Azure Cosmos DB hesaplarıyla ilişkilendirebilirsiniz. 
* Okuma işlemi (RUs tüketilen) bakımından maliyetini oturum tutarlılığı ile değerinden güçlü ve sınırlanmış eskime durumu, ancak birden fazla nihai tutarlılık düzeydir.

<a id="consistent-prefix"></a>
**Tutarlı önek**: 

* Tutarlı önek daha fazla yazma olmaması durumunda, Grup içerisinde bulunan çoğaltmalar sonunda yakınsamasını garanti eder. 
* Tutarlı önek okuma hiçbir zaman bozuk yazma bkz güvence altına alır. Yazma sırayla gerçekleştirilen varsa `A, B, C`, bir istemci ya da görür sonra `A`, `A,B`, veya `A,B,C`, ancak hiçbir zaman bozuk gibi `A,C` veya `B,A,C`.
* Tutarlı önek tutarlılık ile yapılandırılmış olan azure Cosmos DB hesapları Azure bölgeleri herhangi bir sayıda Azure Cosmos DB hesaplarıyla ilişkilendirebilirsiniz. 

**Son**: 

* Nihai tutarlılık, daha fazla yazma olmaması durumunda, Grup içerisinde bulunan çoğaltmalar sonunda yakınsamasını garanti eder. 
* Nihai tutarlılık zayıf tutarlılık istemci önce gördü olanları daha eski değerlerin nereden biçimidir.
* Nihai tutarlılık, zayıf okuma tutarlılığı sunar ancak hem okuma hem de yazma işlemleri için en düşük gecikme sunar.
* Nihai tutarlılık ile yapılandırılmış olan azure Cosmos DB hesapları Azure bölgeleri herhangi bir sayıda Azure Cosmos DB hesaplarıyla ilişkilendirebilirsiniz. 
* Okuma işlemi (RUs tüketilen) bakımından maliyetini nihai tutarlılık denetimi ile tüm Azure Cosmos DB tutarlılık düzeylerini en düşük düzeydir.

## <a name="configuring-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyi yapılandırılıyor
1. İçinde [Azure portal](https://portal.azure.com/), çubuğunda **Azure Cosmos DB**.
2. İçinde **Azure Cosmos DB** sayfasında, değiştirmek için veritabanı hesabı seçin.
3. Hesap sayfasında tıklatın **varsayılan tutarlılık**.
4. İçinde **varsayılan tutarlılık** sayfasında, yeni tutarlılık düzeyi seçin ve tıklayın **kaydetmek**.
   
    ![Ayarlar simgesine ve varsayılan tutarlılık giriş vurgulama ekran görüntüsü](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Sorgular için tutarlılık düzeyleri
Varsayılan olarak, kullanıcı tanımlı, kaynaklar için sorgular için tutarlılık düzeyi tutarlılık düzeyi okuma için aynıdır. Varsayılan olarak, dizin her Ekle, değiştirme veya silme öğenin Cosmos DB kapsayıcısı zaman uyumlu olarak güncelleştirilir. Bu sorguları noktası okuma aynı tutarlılık düzeydeki vermenizin sağlar. Azure Cosmos DB yazma en iyi duruma getirilmiş ve yazma, zaman uyumlu dizin Bakım ve tutarlı sorguları hizmet veren sürekli birimi destekleyen olsa da, belirli koleksiyonlar kendi dizini gevşek güncelleştirmek için yapılandırabilirsiniz. Yavaş dizin daha fazla yazma performansı artırır ve bir iş yükü öncelikle okuma ağır olduğunda toplu alım senaryoları için idealdir.  

| Dizin oluşturma modu | Okur | Sorgular |
| --- | --- | --- |
| CONSISTENT (varsayılan) |Güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek arasından seçin ya da son |Güçlü, sınırlanmış eskime durumu, seçim oturumu veya son |
| Lazy |Güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek arasından seçin ya da son |Nihai |
| Hiçbiri |Güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek arasından seçin ya da son |Uygulanamaz |

Olarak okuma istekleri ile her API belirli sorgu istekte tutarlılık düzeyine düşürebilirsiniz.

## <a name="consistency-levels-for-the-mongodb-api"></a>MongoDB API için tutarlılık düzeyleri

Azure Cosmos DB MongoDB iki tutarlılık ayarları, güçlü ve nihai olan sürüm 3.4, şu anda uygular. Azure Cosmos DB multi-API olduğundan, tutarlılık ayarlar hesap düzeyinde uygulanabilir ve tutarlılık zorlama her API tarafından denetlenir.  MongoDB 3.6 kadar oturum tutarlılığı kavramına oluştu oturum tutarlılığı kullanmak için MongoDB API hesabını ayarlarsanız, tutarlılık için son MongoDB API'leri kullanırken bir alt sürüme şekilde. Bilgisayarınızı-kendi-salt okunur garantisi MongoDB API hesabı için gerekiyorsa, hesap için varsayılan tutarlılık düzeyi strong ayarlanmalı veya sınırlanmış eskime durumu.

## <a name="next-steps"></a>Sonraki adımlar
Tutarlılık düzeyleri ve bileşim hakkında daha fazla okuma yapmak istiyorsanız, aşağıdaki kaynaklara öneririz:

* Doug Terry. Çoğaltılan verilerin tutarlılık Beyzbol (video) açıklanmıştır.   
  [https://www.youtube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. Çoğaltılan verilerin tutarlılık Beyzbol açıklanmıştır.   
  [http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Oturum garanti zayıf tutarlı çoğaltılan veriler için.   
  [http://dl.acm.org/citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
* Daniel Abadi. Modern dağıtılmış veritabanı sistemleri tasarım tutarlılık bileşim: CAP Öykü yalnızca bir parçası olan ".   
  [http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica. İçin pratik kısmi çekirdekleri sınırlanmış eskime durumu (PBS) probabilistic.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Werner Vogels. Son tutarlı - tekrar ziyaret.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.html](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Moni Naor, Avishai Wool, yükü, kapasitesi ve çekirdek sistemleri, bilgi işlem, v.27 n.2, p.423 447, Nisan 1998 SIAM günlük kullanılabilirliği.
  [http://epubs.siam.org/doi/abs/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, Line-up: bir tam ve otomatik linearizability denetleyicisi, dil tasarım ve uygulama, Haziran 05-10, 2010, Toronto, Ontario programlama 2010 ACM SIGPLAN konferans bildirileri, Kanada [DOI > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein , Ion Stoica, Probabilistically bounded staleness for practical partial quorums, Proceedings of the VLDB Endowment, v.5 n.8, p.776-787, April 2012 [http://dl.acm.org/citation.cfm?id=2212359](http://dl.acm.org/citation.cfm?id=2212359)
