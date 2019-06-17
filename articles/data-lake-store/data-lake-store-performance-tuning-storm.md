---
title: Azure Data Lake depolama Gen1 Storm performans ayarlama yönergeleri | Microsoft Docs
description: Azure Data Lake depolama Gen1 Storm performans ayarlama yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 8066a759cf80be6e9ca232bcd3693a5fa4d2f2f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61436486"
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Data Lake depolama Gen1 üzerinde Storm için performans ayarlama Kılavuzu

Azure Storm topolojisi performansını ayarlama, dikkate alınması faktörleri öğrenin. Örneğin, spout ve bolt (iş g/ç yoğun bellek olup) tarafından çalışmanın özelliklerini anlamak önemlidir. Bu makalede, performans ayarlama yönergeleri, yaygın sorunları giderme de dahil olmak üzere çeşitli kapsar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md).
* **Bir Azure HDInsight kümesi** bir Data Lake depolama Gen1 hesabına erişim. Bkz: [ile Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **Data Lake depolama Gen1 bir Storm kümesi çalıştıran**. Daha fazla bilgi için [HDInsight üzerinde Storm](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-overview).
* **Performans ayarlama yönergeleri Data Lake depolama Gen1**.  Genel performans için bkz [Data Lake depolama Gen1 performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-the-parallelism-of-the-topology"></a>Topolojinin paralelliğini ayarlama

Eşzamanlı g/ç için ve Data Lake depolama Gen1 artırarak performansını mümkün olabilir. Bir Storm topolojisinin paralelliğini belirlemek yapılandırmalarını bir dizi sahiptir:
* (Çalışan Vm'lere olarak eşit olarak dağıtılır) alt işlemlerin sayısı.
* Spout Yürütücü örnek sayısı.
* Bolt Yürütücü örnek sayısı.
* Spout görevleri sayısı.
* Bolt görevleri sayısı.

Örneğin, bir kümede 4 sanal makine ve 4 alt işlemlerin, 32 spout yürütücüler 32 spout görevleri ve 256 bolt yürütücüler ve 512 bolt görevler aşağıdakileri dikkate alın:

Bir çalışan düğümü olan her gözetmenin tek bir çalışan Java sanal makinesi (JVM) işlemi var. Bu JVM işlemi 4 spout iş parçacıkları ve 64 bolt iş parçacığı yönetir. Her iş parçacığı içinde Görevler sırayla çalıştırılır. Önceki yapılandırma ile her spout iş parçacığı 1 görev varsa ve her bir bolt iş parçacığı 2 görevleri.

Storm, ilgili çeşitli bileşenleri şunlardır ve sahip olduğunuz paralellik düzeyini nasıl etkiler:
* Baş düğüm (Nimbus Storm adlandırılır), göndermek ve işlerini yönetmek için kullanılır. Bu düğümler üzerinde çalıştırılır. paralellik derecesini etkisi yok.
* Gözetmen düğümleri. HDInsight bu bir çalışan düğümüne Azure VM karşılık gelir.
* Çalışan görevleri, Vm'lerde çalışan Storm işlemlerdir. Her bir alt görevin bir JVM örneğine karşılık gelir. Storm, çalışan düğümlerine mümkün olduğunca eşit olarak belirttiğiniz alt işlemlerin sayısını dağıtır.
* Spout ve bolt Yürütücü örnekleri. Her bir yürütücü örneği çalışanları (JVMs) içinde çalışan bir iş parçacığına karşılık gelir.
* Storm görevler. Bunlar her birinde çalıştırma iş parçacıkları mantıksal görevlerdir. Yürütücü başına birden çok görevi veya gerekirse değerlendirmelidir şekilde paralelliği düzeyini değiştirmez.

### <a name="get-the-best-performance-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 en iyi performansı elde

Aşağıdakileri, Data Lake depolama Gen1 ile çalışırken en iyi performansı elde:
* Birleşim küçük ve büyük boyutları (İdeal olarak, 4 MB) halinde ekler.
* Yaptığınız gibi gibi birçok eşzamanlı isteği yapın. Her bir bolt iş parçacığı engelleme okuma yaptığını olduğundan, çekirdek başına 8-12 iş parçacığı aralığında yere sahip olmak istersiniz. Bu, NIC ve iyi kullanılan CPU tutar. Daha büyük bir VM, daha fazla eşzamanlı istek sağlar.  

### <a name="example-topology"></a>Örnek topoloji

Bir 8 çalışan düğümü küme D13v2 Azure VM ile sahip olduğunuz varsayalım. Bu VM 8 sahiptir çekirdeği, 8 çalışan düğümü arasında 64 toplam çekirdek sahip.

Çekirdek başına 8 bolt iş parçacığı yaptığımız varsayalım. 64 çekirdek, 512 toplam bolt Yürütücü örnekleri (diğer bir deyişle, iş parçacıkları) istiyoruz anlamına gelir. Bu durumda, VM başına bir JVM başlayın ve kullanırız çoğunlukla içinde JVM iş parçacığı eşzamanlılık eşzamanlılık elde etmek için varsayalım. 8 çalışan görevleri (bir Azure VM başına) ve 512 bolt yürütücüler ihtiyacımız anlamına gelir. Bu yapılandırmayı göz önünde bulundurulduğunda, her çalışan düğümü 1 vererek çalışanları (diğer adıyla gözetmen düğümleri), çalışan düğümleri arasında eşit olarak dağıtılacak Storm çalışır JVM. Yürütücü denetçiler arasında eşit olarak dağıtılacak Storm denetçiler içinde çalışır artık her yönetici (JVM) verme 8 her iş parçacıklarını yönlendirebilir.

## <a name="tune-additional-parameters"></a>Ek parametrelerini ayarlama
Temel topoloji sonra tüm parametrelerinin ince ayarlamalar yapmak isteyip istemediğinizi düşünebilirsiniz:
* **Çalışan düğümü başına JVMs sayısı.** Büyük veri yapısı (örneğin, bir arama tablosu) varsa, ayrı bir kopyasını barındırdığınız her JVM bellek gerektirir. Alternatif olarak, daha az JVMs varsa birçok iş parçacığı arasında veri yapısını kullanabilirsiniz. Bolt'un g/ç JVMs sayısı kadar bu JVMs eklenen bir iş parçacığı sayısı olarak banttan yapmaz. Kolaylık olması için çalışan başına bir JVM sağlamak için bir fikirdir. Bu sayıyı değiştirmenizin gerekli ancak, bolt yaptığı veya hangi uygulama, işleme bağlı olarak, gerektirir.
* **Spout Yürütücü sayısı.** Yukarıdaki örnekte, Data Lake depolama Gen1 için yazmak için Cıvatalar kullandığından, spout sayısını bolt performans için doğrudan ilgili değil. Ancak, işleme veya g/ç içinde spout'olmuyor miktarına bağlı olarak, bu en iyi performans için spout ayarlamak için iyi bir fikirdir. Boltlar meşgul tutmak için yeterli spout olduğundan emin olun. Spout çıkış oranlarda Cıvatalar verimini eşleşmesi gerekir. Gerçek yapılandırmasını spout üzerinde bağlıdır.
* **Görev sayısı.** Her bolt tek bir iş parçacığı çalıştırır. Bolt başına ek görevler, herhangi bir ek eşzamanlılık sağlaması gerekmez. Avantajı, bunlar yalnızca bir kez işleminizi tanımlama grubu sıkan, büyük bir kısmı, bolt yürütme süresi sürüyorsa ' dir. Buna bolt bir onay göndermeden önce büyük içine birçok diziler eklemek iyi bir fikir grubuna var. Bu nedenle, çoğu durumda, ek bir avantaja birden çok görevi sağlar.
* **Yerel veya karışık gruplandırma.** Bu ayar etkinleştirildiğinde, diziler aynı çalışan işlemde Cıvatalar gönderilir. Bu işlemler arası iletişimi ve ağ çağrıları azaltır. Bu, çoğu Topolojileri için önerilir.

Bu temel senaryo iyi bir başlangıç noktasıdır. En iyi performans elde etmek için önceki parametrelerinin ince ayar için kendi verilerinizle test edin.

## <a name="tune-the-spout"></a>Spout ayarlama

Spout ayarlamak için aşağıdaki ayarları değiştirebilirsiniz.

- **Kayıt zaman aşımı: topology.message.timeout.secs**. Bu ayar bir ileti alır ve bildirim, alma zamanı belirler başarısız kabul edilmeden önce.

- **Çalışan işlemi başına en fazla bellek: worker.childopts**. Bu ayar Java çalışanları için ek komut satırı parametreleri belirtmenize olanak sağlar. En sık kullanılan burada JVM'ın yığın için ayrılan maksimum belleğin belirleyen XmX ayardır.

- **En fazla spout bekleyen: topology.max.spout.pending**. Bu ayar, herhangi bir zamanda (henüz onaylanır topolojideki tüm düğümlerde) uçuş spout iş parçacığı başına de olabilir başlıkların sayısını belirler.

  Yapmak iyi bir hesaplama, tanımlama gruplarının her boyutunu tahmin etmektir. Sonra ne kadar bellek bir spout iş parçacığı olduğunu göstermektedir. Bu değer tarafından ayrılmış bir iş parçacığına ayrılan toplam bellek max spout bekleyen parametresi için üst sınır vermeniz gerekir.

## <a name="tune-the-bolt"></a>Bolt ayarlama
Data Lake depolama Gen1 için yazarken boyutu eşitleme ilkesi (istemci tarafı arabelleği) 4 MB olarak ayarlayın. Arabellek boyutu olduğunda bir temizlemeye veya hsync() sonra gerçekleştirilen bu değerde. Data Lake depolama Gen1 sürücü çalışan VM üzerinde açıkça bir hsync() gerçekleştirmediğiniz sürece bu arabelleğe otomatik olarak yapar.

Varsayılan Data Lake depolama Gen1 Storm bolt Bu parametre ayarlamak için kullanılan boyutu eşitleme ilke parametresi (fileBufferSize) sahiptir.

G/Ç açısından yoğun Topolojilerde her bolt iş parçacığının kendi dosyaya yazmak için ve bir dosya döndürme İlkesi (fileRotationSize) ayarlamak için iyi bir fikir olduğunu. Dosya belirli bir boyuta ulaştığında, akışa otomatik olarak temizlenir ve yeni bir dosya yazılır. Döndürme için önerilen dosya boyutu 1 GB ' dir.

### <a name="handle-tuple-data"></a>Tanımlama grubu verileri işleyen

Bolt tarafından açıkça kabul edilen kadar içerisindeki Storm, bir spout için bir tanımlama grubu tutan. Bir demet Cıvata tarafından Okunmuş ancak henüz onaylanır değil, Data Lake depolama Gen1 arka ucuna spout kalıcı. Bir demet kabul edildikten sonra spout Kalıcılık Cıvata tarafından garanti ve kaynak veriler her kaynaktan gelen okuma sonra silebilirsiniz.  

Data Lake depolama Gen1 ile ilgili en iyi performans için bolt sahip tanımlama grubu veri 4 MB arabellek. Ardından bir Data Lake depolama Gen1 bir 4 MB'lık yazma olarak son geri yazma. Veri deposuna başarıyla yazıldıktan sonra (arama hflush()) tarafından bolt veri spout kabul. Burada verilen örnek bolt yaptığı budur. Çok sayıda hflush() çağrı yapılmadan önce diziler ve kabul edilen tanımlama grubu tutmak için kullanılabilir. Ancak, bu tutacak spout gerekir ve bu nedenle bellek miktarı arttıkça JVM gerekli uçuştaki başlıkların sayısını artırır.

> [!NOTE]
> Uygulamalar, diziler daha sık (en veri boyutu 4 MB'den küçük) diğer olmayan performansla ilgili nedenlerle onaylamak için bir gereksinim olabilir. Ancak, depolama arka ucu için g/ç aktarım hızını etkileyebilir. Bu bir tradeoff bolt'un g/ç performansı karşı dikkatlice değerlendirin.

4 MB'lık arabellek dolduracak şekilde uzun sürer diziler hızı yüksek olmayan ise bu azaltıcı göz önünde bulundurun:
* Boltlar sayısının azaltılması, bu nedenle vardır doldurmak için daha az arabellek.
* Y milisaniye temizleme sayısı x her biri veya her bir hflush() olduğu zaman veya sayısı tabanlı bir ilke olması tetiklenen ve şu ana kadar toplanan diziler geri kabul edildiğini.

Aktarım hızı bu durumda düşüktür, ancak yavaş olayları oranı ile en yüksek aktarım büyük hedefi yine de değil unutmayın. Bu risk azaltma işlemleri, depoya akışına bir demet için geçen toplam süreyi azaltmanıza yardımcı olur. Daha düşük olayı oranı ile gerçek zamanlı bir işlem hattı istiyorsanız bu önemli. Ayrıca, gelen tanımlama grubu oranı düşükse, bunlar alma sırasında zaman aşımı diziler kalmaz, topology.message.timeout_secs parametre olarak ayarlaması gerektiğini unutmayın. arabelleğe alınan veya işlenebilir.

## <a name="monitor-your-topology-in-storm"></a>Storm topolojinizde izleyin  
Topolojiniz çalışırken, Storm kullanıcı arabirimini izleyebilirsiniz. Bakmak için ana parametreleri şunlardır:

* **Toplam işlem yürütme gecikme süresi.** Bir demet olmaya spout'un yayılan Cıvata tarafından işlenen ve onaylanır geçen ortalama süreyi budur.

* **Toplam bolt işlem gecikme süresi.** Bir bildirim alıncaya kadar bolt, kayıt tarafından geçirilen ortalama süreyi budur.

* **Toplam bolt gecikmesi yürütün.** Bu yürütme yönteminin içinde Cıvata tarafından harcanan ortalama süredir.

* **Başarısızlık sayısı.** Bu, zaman aşımına uğramadan önce tam olarak işlenmesi başarısız olan tanımlama grubu sayısı ifade eder.

* **Kapasite.** Sisteminizde ne kadar meşgul olduğundan bir ölçü budur. Bu sayı 1 ise, Cıvatalar geliştiricilerimizin mümkün olduğunca hızlı çalışıyor. 1'den az ise, paralellik artırın. 1'den büyükse, paralellik azaltın.

## <a name="troubleshoot-common-problems"></a>Sık karşılaşılan sorunları giderme
Sık karşılaşılan birkaç sorun giderme senaryoları aşağıda verilmiştir.
* **Birçok diziler zaman aşımına uğruyor.** Her bir düğümünde nereden kaynaklanıyor belirlemek için topoloji bakın. Bunun en yaygın nedeni Cıvatalar ile spout ayak olmamasıdır. Bu, işlenmeyi bekleyen iç arabellek tıka basa dolduruyor diziler için yol açar. Zaman aşımı değerini artırmayı veya bekleyen max spout azaltmayı göz önünde bulundurun.

* **Bir yüksek toplam işlem yürütme gecikme süresi, ancak düşük bolt işlem gecikme süresi yok.** Bu durumda, diziler yeterince hızlı onaylanmaması gerektiğini mümkündür. Yeterli sayıda acknowledgers olup olmadığını denetleyin. Boltlar işlemeden başlamadan önce bunlar sırasındaki çok uzun süre beklediğini başka bir olasılıktır. Bekleyen max spout azaltın.

* **Yürütme gecikme süresi yüksek bolt yoktur.** Başka bir deyişle, bolt execute() yöntemi çok uzun sürüyor. Kodu En İyileştir veya yazma boyutlarda arayın ve davranışı temizler.

### <a name="data-lake-storage-gen1-throttling"></a>Data Lake depolama Gen1 azaltma
Data Lake depolama Gen1 tarafından sağlanan bant genişliği sınırına ulaşırsanız, görev hataları görebilirsiniz. Azaltma hataları için görev günlüklerine bakın. Kapsayıcı boyutu artırarak paralellik düşürebilir.    

Kaynaklarınızın azaltılıp olmadığını denetlemek için hata ayıklama istemci tarafında günlüğü etkinleştir:

1. İçinde **Ambari** > **Storm** > **Config** > **storm çalışan log4j Gelişmiş**, değiştirme **&lt;kök düzey = "bilgi"&gt;** için  **&lt;kök düzey "debug" =&gt;** . Tüm düğümleri/hizmet yapılandırmasının etkili olması yeniden başlatın.
2. Storm topoloji çalışan düğümlerinde oturum İzleyici (/var/log/storm/worker-artifacts altında /&lt;TopologyName&gt;/&lt;bağlantı noktası&gt;/worker.log) için Data Lake depolama Gen1 azaltma özel durumları.

## <a name="next-steps"></a>Sonraki adımlar
Storm, başvurulan için ek performans ayarlama [bu blog](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Ek bir örnek çalıştırmak bilgi için bkz [bunu github'da](https://github.com/hdinsight/storm-performance-automation).
