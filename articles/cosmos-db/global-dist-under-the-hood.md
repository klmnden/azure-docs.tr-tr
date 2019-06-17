---
title: Genel dağıtım ile Azure Cosmos DB - başlık altında
description: Bu makalede Azure Cosmos DB genel dağıtımını ilgili teknik ayrıntılar sağlar
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: c490657eb67a34e79c8dbaea31cb59b49cc6448e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241102"
---
# <a name="global-data-distribution-with-azure-cosmos-db---under-the-hood"></a>Azure Cosmos DB - başlık altında genel veri dağılımı

Azure Cosmos DB, azure'da temel bir hizmet olduğundan tüm Azure bölgeleri arasında genel, bağımsız, Savunma Bakanlığı (DoD) ve kamu bulutlarında dahil olmak üzere dünya çapında dağıtılır. Bir veri merkezi içinde dağıtın ve Azure Cosmos DB, makinelerin büyük Damgalar üzerinde her ayrılmış yerel depolama ile yönetin. Bir veri merkezi içinde Azure Cosmos DB her potansiyel olarak birden fazla donanım Nesilleri çalıştıran birçok kümeler arasında dağıtılır. Kümesindeki makineleri 10-20 hata etki alanına bir bölgede yüksek kullanılabilirlik için genellikle yayılır. Cosmos DB genel dağıtım sistemin topolojisi aşağıdaki resimde gösterilmektedir:

![Sistemin topolojisi](./media/global-dist-under-the-hood/distributed-system-topology.png)

**Azure Cosmos DB'de Global dağıtım, kullanıma hazır:** Dilediğiniz zaman birkaç tıklama ile veya programlama yoluyla tek bir API çağrısıyla ekleyebilir veya Cosmos veritabanınızla ilişkili coğrafi bölgeler kaldırın. Bir Cosmos veritabanı, sırasıyla Cosmos kapsayıcıların bir kümesinden oluşur. Cosmos DB'de kapsayıcıları dağıtımı ve ölçeklenebilirliği mantıksal birimleri görev yapar. Koleksiyonlar, tablolar ve grafikler oluşturduğunuz (dahili) yalnızca Cosmos kapsayıcılardır. Kapsayıcılar tamamen şemadan bağımsızdır ve bir sorgu için bir kapsam belirtin. Bir Cosmos kapsayıcıdaki veri alımı sırasında otomatik olarak dizine alınır. Otomatik dizin oluşturma, özellikle de Global olarak dağıtılan bir kurulumda, şema veya dizin yönetimi olmadan verileri sorgulamak kullanıcıların sağlar.  

- Belirli bir bölgede bir bölüm sağlayın ve temel alınan fiziksel bölümler tarafından şeffaf bir şekilde yönetilen anahtarı, kullanarak bir kapsayıcı içindeki veriler dağıtılır (*yerel dağıtım*).  

- Her bir fiziksel bölüm ayrıca coğrafi bölgeler arasında çoğaltılır (*genel dağıtım*). 

Cosmos DB esnek bir biçimde kullanarak uygulama Cosmos kapsayıcısında aktarım hızını ölçeklendirir ya da daha fazla depolama alanını kullanan Cosmos DB bölüm yönetimi işlemlerini şeffaf bir şekilde işler (bölme, kopyalama, silme) tüm bölgeler arasında. Ölçek, dağıtım veya hatalardan bağımsız, Cosmos DB verilerin herhangi sayıda bölgesinde Global olarak dağıtılmış kapsayıcılar içinde tek sistem görüntüsünü sağlamaya devam eder.  

Aşağıdaki görüntüde gösterildiği gibi bir kapsayıcı içindeki verileri - bir bölge içinde ve bölgeler, dünya çapında iki boyutta boyunca dağıtılır:  

![Fiziksel bölümler](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)

Bir fiziksel bölüm bir adlı çoğaltma grubu tarafından uygulanan bir *çoğaltma kümesine*. Her makine yukarıdaki görüntüde gösterildiği gibi çeşitli fiziksel bölümler işlemlerin sabit bir dizi karşılık çoğaltmaları yüzlerce barındırır. Çoğaltmaları karşılık gelen fiziksel bölümlere dinamik olarak yerleştirilir ve bir küme ve veri merkezleri bölge içindeki makineler arasında Yük Dengelemesi.  

Bir yinelemenin benzersiz bir Azure Cosmos DB kiracıya ait. Her yineleme Cosmos DB'NİN'ın bir örneğini barındıran [veritabanı altyapısı](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), ilişkili dizinleri yanı sıra kaynakları yönetir. Cosmos DB'nin veritabanı altyapısı bir atom kayıt sırasını (ARS) temel tür sisteminde çalışır. Altyapı kayıtların yapısını ve örnek değerleri arasındaki sınırı bulanık bir şema, kavramını bağımsızdır. Cosmos DB, otomatik olarak her şeyi alımı sırasında şema veya dizin yönetimiyle ilgilenmenize gerek kalmadan, Global olarak dağıtılmış verileri sorgulamak kullanıcılara verimli bir biçimde dizin oluşturma tarafından tam şema agnosticism ulaşır.

Cosmos veritabanı altyapısı, çeşitli koordinasyon temelleri, dil çalışma zamanlarını, Sorgu işlemcisi, depolama ve alt sistemlerin sorumlu için işlem depolama dizini oluşturma ve veri dizin uygulaması dahil olmak üzere bileşenden oluşur, sırasıyla. Dayanıklılık ve yüksek kullanılabilirlik sağlamak için veritabanı altyapısı, veri ve dizin ssd'lerde devam ediyorsa ve yineleme içinde veritabanı altyapısı örneği arasında çoğaltır-kümelerini sırasıyla. Daha büyük kiracılar için aktarım hızını ve depolamayı, daha yüksek ölçek karşılık gelir ve sahip olmanız büyük veya daha fazla yineleme ya da her ikisini de. Sistemin her bileşenin tam olarak zaman uyumsuz: hiç olmadığı kadar hiçbir iş parçacığını engeller ve her bir iş parçacığı kısa süreli bir gereksiz iş parçacığı anahtar ödemeden çalışır. Genelinde tüm giriş denetimi yığından tüm g/ç yolu için oran sınırlandırma ve arka baskısı genişletilmeyecek. Cosmos veritabanı altyapısı, ayrıntılı eşzamanlılık yararlanmaya ve yüksek aktarım hızı frugal miktarda sistem kaynağı içinde çalışırken sunmak için tasarlanmıştır.

Cosmos DB'nin genel dağıtım üzerinde iki anahtar soyutlama – kullanır *çoğaltma kümeleri* ve *bölüm kümeleri*. Çoğaltma kümesi modüler bir Lego blok düzenlemesi için ve dinamik bir katmana coğrafi olarak dağıtılmış bir veya daha fazla fiziksel bölüm bölüm kümesi ise. Nasıl genel dağıtımını anlamak için çalışır, ihtiyacımız bu iki anahtar soyutlama anlamak. 

## <a name="replica-sets"></a>Çoğaltma-ayarlar

Bir fiziksel bölüm çoğaltmalarının çoğaltma kümesi olarak adlandırılan birden çok hata etki, alanlarına kendi kendini yöneten ve dinamik olarak yük dengelemesi yapılmış bir grup olarak tanımdır. Bu verilerin fiziksel bölüm içinde yüksek oranda kullanılabilir, dayanıklı ve tutarlı hale getirmek için çoğaltılan durum makine Protokolü topluca uygular. Çoğaltma kümesi üyelik *N* dinamik – arasında geciktirmeye tutar *NMin* ve *NMax* hataları, yönetim işlemleri ve zaman başarısız göre çoğaltmalar, Regenerate/kurtarın. Üyelik değişiklikleri bağlı olarak, çoğaltma, ayrıca yeniden yapılandırır Okuma boyutu protokolünü ve çekirdekleri yazma. Aktarım hızı belirli bir fiziksel birime atanan birörnek dağıtmak için iki fikirleri kullanıyoruz: 

- İlk olarak, öncü yazma isteklerini işleme maliyeti üzerinde İzleyici güncelleştirmelerini uygulama maliyeti daha yüksek. Öncü ayrılmaması gelenlere, takipçi değerinden daha fazla sistem kaynakları. 

- İkincisi, mümkün olduğunca uzak onaylamaktan okuma çekirdek verilen tutarlılık düzeyi için özel olarak İzleyici çoğaltmaları oluşur. Biz okuma gerekmedikçe hizmet vermek için öncü başvurarak kaçının. Bir dizi arasındaki ilişkiyi üzerinde yapılan araştırma fikirlerinden kullanıyoruz [yükü ve kapasite](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) için çekirdek tabanlı sistemlerde [beş tutarlılık modeli](consistency-levels.md) Cosmos DB destekleyen.  

## <a name="partition-sets"></a>Bölüm-ayarlar

Cosmos veritabanı bölge ile yapılandırılmış her fiziksel bölüm grubu, yapılandırılan tüm bölgeler arasında çoğaltılan anahtarları aynı kümesini yönetmek için oluşur. Bu daha yüksek koordinasyon temel olarak adlandırılan bir *bölüm kümesi* -coğrafi olarak dağıtılmış bir dinamik katmana fiziksel bölüm belirli bir anahtar kümesini yönetme. Belirli bir fiziksel bölüm (bir çoğaltma kümesi) bir küme içinde kapsamlıdır, ancak bir bölüm kümesi kümeler, yayılabilir veri merkezleri ve aşağıdaki resimde gösterildiği gibi coğrafi bölgeler:  

![Bölüm ayarlar](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)

Bir coğrafi olarak dağınık "süper çoğaltma çoğaltma-anahtarları kümesinin aynısına sahip olan birden fazla oluşan kümesi", bölüm bir dizi düşünebilirsiniz. Çoğaltma kümesi, bir bölümü kümenin üyelik ayrıca dinamik benzer – belirli bir bölüm kümesi/yeni bölümler eklemek/kaldırmak için örtük fiziksel bölüm yönetimi işlemlerini göre dalgalanma (örneğin, ne zaman ölçeği aktarım hızı üzerinde bir kapsayıcı, bölge, Cosmos veritabanı veya hata gerçekleştiğinde bir ekleme/kaldırma). Her bölümlerini (bir bölüm kümesi), kendi çoğaltma kümesi içinde bölüm kümesi üyeliği yönetmek zorunda da üyelik tamamen merkezi olmayan ve yüksek oranda kullanılabilir. Bir bölüm kümesi yeniden yapılandırma sırasında topolojisi, fiziksel bölümler arasında katman da oluşturulur. Topoloji tutarlılık düzeyi, coğrafi uzaklıktan ve kaynak ve hedef fiziksel bölümler arasında kullanılabilir ağ bant genişliği temelinde dinamik olarak seçilir.  

Hizmet bir tek bir yazma bölgesi veya birden çok yazma bölgeleri ile Cosmos veritabanlarınızı yapılandırmanıza olanak tanır ve seçime bağlı olarak, bölüm kümeleri yazma tam olarak bir ya da tüm bölgelerde kabul edecek şekilde yapılandırılmış. İki düzeyli, iç içe geçmiş fikir birliğine varılmış Protokolü sistemi kullanır: bir düzey yazma kabul eden bir fiziksel bölüm dizi çoğaltma çoğaltmalarının içinde çalışır ve diğer tüm tam sıralama garantisi sağlamak için bir bölüm kümesi düzeyinde çalışır bölüm kümesindeki taahhüt yazar. Bu çok katmanlı, iç içe geçmiş fikir birliğine varılmış, Cosmos DB, müşterilerine sağlar. tutarlılık modeli uygulamasının yanı sıra, yüksek kullanılabilirlik için katı SLA'larımız uygulanması için önemlidir.  

## <a name="conflict-resolution"></a>Çakışma çözümleme

Güncelleştirme yayma, çakışma çözümü ve izleme nedensellik ilişkilerini bizim tasarımı üzerinde Önceki işten esinlenilmiştir [epidemic algoritmaları](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) ve [Bayou](https://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf) sistem. Fikirleri çekirdekler kurtulan ve Cosmos DB'nin sistem tasarımı iletişim kurmak için kullanışlı bir çerçeve başvuru sağlar, ancak Cosmos DB sistemde uyguladığımız bunları gibi bunlar da önemli dönüştürme yapılmıştır. Önceki sistemleri kaynak İdaresi ne, Cosmos DB çalışılacak ya da (örneğin, sınırlanmış eskime durumu tutarlılık) özellikleri ve katı sağlamak için ölçekleme ile tasarlanmış çünkü bu gerekli ve Cosmos DB, müşterilerine sunduğu geniş kapsamlı SLA'lar.  

Bir bölüm kümesi birden çok bölgede dağıtılır ve belirli bir bölüm kümesi oluşturan fiziksel bölümler arasında veri çoğaltmak için Cosmos Db'ler (çok ana) çoğaltmayı Protokolü aşağıdaki geri çağırma. Her bir fiziksel bölüm (bölüm-kümesi), yazma kabul eder ve okuma bu bölge için yerel istemcileri genellikle yarar. Yazma kabul eden bir fiziksel bölüm bir bölge içinde depolanmasına taahhüt verdiniz ve istemciye kabul edildiğini önce fiziksel bölüm içinde yüksek oranda kullanılabilir hale. Bunlar belirsiz yazma ve koruma entropi kanalı kullanılarak bölüm kümesi içinde fiziksel diğer bölümlerine yayılır. İstemciler, bir istek üst bilgisi geçirerek belirsiz veya kaydedilmiş yazma isteyebilir. (Yayma sıklığını dahil) koruma entropi yayma dinamik fiziksel bölümler ve tutarlılık düzeyi yapılandırılmış bölüm kümesi, bölgesel yakınlık topolojiye bağlı. Bir bölüm kümesinde Cosmos DB, dinamik olarak seçilen arbiter bölümü olan bir birincil işleme düzeni izler. Arbiter seçimi dinamiktir ve yeniden yapılandırma bölümü kümesinin bir parçası katmana topolojisine dayanır. Taahhüt edilen (çok-row/toplu güncelleştirmeler dahil) yazma sipariş edilmesi garanti edilir. 

Nedensellik ilişkilerini izlemek ve algılamak ve çözmek için vektörleri güncelleştirme çakışması sürümü için kodlanmış vektör saatleri (sırasıyla bölge kodu ve mantıksal saatler her çoğaltma kümesi, fikir birliğine varılmış düzeyine karşılık gelen ve bölüm kümesi içeren) kullanıyoruz. Topoloji ve eş seçimi algoritması, sabit ve en az depolama ve sürüm vektörü, en az bir ağ yükünü sağlamak için tasarlanmıştır. Algoritma katı yakınsama özelliği garanti eder.  

Birden çok yazma bölgeleri ile yapılandırılmış Cosmos veritabanları için sistem esnek otomatik çakışma çözümleme ilkeler arasından, geliştiriciler için de dahil olmak üzere sunar: 

- **Son yazma WINS (LWW)** , varsayılan olarak kullanır (zaman eşitleme saati protokolünü temel alır) bir sistem tanımlı bir zaman damgası özelliği. Cosmos DB Çakışma çözümlemesi için kullanılan diğer özelliklerden herhangi birini özel sayısal belirtmenizi sağlar.  
- **Uygulama tanımlı (özel) çakışma çözüm İlkesi** (birleştirme yordamları ifade edilir), uygulama tanımlı semantiği mutabakat çakışmaları için tasarlanmıştır. Bu yordamlar, bir veritabanı işlemi sunucu tarafı şirket himayesinde yazma yazma çakışmaları algılanması üzerine çağrılır. Tam olarak sistemidir taahhüt protokolünün bir parçası olarak bir birleştirme yordamının yürütülmesi için bir kez garanti. Vardır [birkaç çözüm örnekleri çakışan](how-to-manage-conflicts.md) ile yürütmek kullanılabilir.  

## <a name="consistency-models"></a>Tutarlılık modeli

Tek bir veya birden çok yazma bölgeleri ile Cosmos veritabanı yapılandırma olsun, beş iyi tanımlanmış tutarlılık modellerinden seçebilirsiniz. Birden çok yazma bölgeleri ile tutarlılık düzeyleri birkaç önemli yönleri şunlardır:  

Sınırlanmış eskime durumu tutarlılığını, tüm okuma içinde olacağını garanti eder *K* ön ekleri veya *T* saniyeler içinde bölgelerden en son yazma. Ayrıca, sınırlanmış eskime durumu tutarlılık okuma monoton ve tutarlı ön ek Garantisi ile sağlanır. Koruma entropi protokolü, oranı sınırlı bir biçimde çalışır ve ön ekler değil accumulate ve yazma backpressure uygulanacak yok sağlar. Kendi Yazar oturumu tutarlılık garantisi monoton okuma, monoton yazma, okuma, yazma, okuma ve tutarlı ön ek garantisi, dünya çapında izler. Güçlü tutarlılık ile yapılandırılmış veritabanları için birden fazla yazma bölgeleri avantajları (düşük yazma gecikme süresi, yüksek yazma kullanılabilirliği) geçerli değildir, bölgeler arasında zaman uyumlu çoğaltma nedeniyle.

Cosmos DB'de beş tutarlılık modeli semantiği açıklanan [burada](consistency-levels.md)ve üst düzey bir TLA + belirtimlerini kullanarak matematiksel olarak açıklanan [burada](https://github.com/Azure/azure-cosmos-tla).

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makaleleri kullanarak genel dağıtımı yapılandırma işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Bölge veritabanı hesabınızdan Ekle/Kaldır](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
