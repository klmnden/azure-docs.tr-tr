---
title: "Azure Data Lake Store Storm performans yönergeleri ayarlama | Microsoft Docs"
description: "Azure Data Lake Store Storm performans kuralları ayarlama"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 1dfa93643f45a96ded3fd022aa8b1c71d487acb4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Performans Kılavuzu Hdınsight ve Azure Data Lake Store üzerinde Storm için ayarlama

Bir Azure Storm topolojisinin performansı ayarlamak zaman düşünülmesi gereken faktörler anlayın. Örneğin, spout'lar ve (iş g/ç yoğun olup) Cıvatalar tarafından yapılan iş özelliklerini anlamak önemlidir. Bu makalede, performans ayarlama yönergeleri, ortak sorunları da dahil olmak üzere çeşitli yer almaktadır.

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.
* **Data Lake Store üzerinde Storm kümesi çalıştıran**. Daha fazla bilgi için bkz: [Hdınsight üzerinde Storm](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview).
* **Performans ayarlama yönergeleri Data Lake Store üzerinde**.  Genel performans için bkz [Data Lake deposu performans ayarlama Kılavuzu](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-the-parallelism-of-the-topology"></a>Topoloji paralellik ayarlama

G/ç için ve Data Lake Store eşzamanlılığı artırarak performansı mümkün olabilir. Bir Storm topolojisinin paralellik belirlemek yapılandırmaları kümesi vardır:
* (VM'ler üzerindeki çalışanları olacak şekilde eşit dağıtılır) çalışan işlem sayısı.
* Spout Yürütücü örneği sayısı.
* Cıvata Yürütücü örneği sayısı.
* Spout görev sayısı.
* Cıvata görev sayısı.

Örneğin, 4 VM'ler ve 4 çalışan işlemleri, 32 spout yürütücüler 32 spout görevleri ve 256 Cıvata yürütücüler ve 512 Cıvata görevleri ile bir kümede aşağıdakileri dikkate alın:

Çalışan düğümüne olan her yönetici tek bir çalışan Java sanal makine (JVM) işlemi vardır. Bu JVM işlem 4 spout iş parçacıkları ve 64 Cıvata iş parçacığı yönetir. Her iş parçacığı içinde görevleri sırayla çalışır. Önceki yapılandırmayla 1 görev her spout iş parçacığı vardır ve 2 görevleri her Cıvata iş parçacığı vardır.

Storm, söz konusu çeşitli bileşenleri şunlardır ve sahip olduğunuz paralellik düzeyini nasıl etkilediklerini:
* (Nimbus Storm denir) baş düğümü göndermek ve işlerini yönetmek için kullanılır. Bu düğümler paralellik derecesini bir etkisi yoktur.
* Yönetici düğümleri. Hdınsight'ta, bu çalışan düğümüne Azure VM karşılık gelir.
* Vm'lerde çalışan Storm işlemler çalışan görevlerdir. Her çalışan görev JVM örneğine karşılık gelir. Storm çalışan düğümlerine mümkün olduğunca eşit belirttiğiniz çalışan işlem sayısı dağıtır.
* Spout ve yürütücü örnekleri Cıvata. Her bir yürütücü örneği çalışanları (JVMs) içinde çalışan iş parçacığı karşılık gelir.
* Storm görevler. Bunların her birini Çalıştır iş parçacıkları mantıksal görevlerdir. Yürütücü başına birden çok görev veya gerekiyorsa değerlendirmelidir şekilde bu paralellik, düzeyini değiştirmez.

### <a name="get-the-best-performance-from-data-lake-store"></a>Data Lake Deposu'ndan veri en iyi performansı elde

Aşağıdakileri, Data Lake Store ile çalışırken en iyi performansı elde:
* Birleşim küçük ve büyük boyutları (İdeal olarak 4 MB) ekler.
* Mümkün olduğu kadar eşzamanlı istek yapın. Her Cıvata iş parçacığı engelleme okuma yapmak için herhangi bir yerde çekirdek başına 8-12 iş parçacığı aralığında istiyorsanız. Bu, NIC ve iyi kullanılan CPU tutar. Daha büyük bir VM daha fazla istek sağlar.  

### <a name="example-topology"></a>Örnek topoloji

Bir 8 çalışan düğümü küme D13v2 Azure VM ile sahip varsayalım. Bu VM 8 sahip Çekirdek nedenle 8 çalışan düğümleri arasında 64 toplam çekirdeğe sahip.

Biz çekirdek başına 8 Cıvata iş parçacığı yapmak varsayalım. 64 çekirdek, 512 toplam Cıvata Yürütücü örnekleri (diğer bir deyişle, iş parçacıkları) istiyoruz anlamına gelir. Bu durumda, biz VM başına bir JVM başlayın ve çoğunlukla eşzamanlılık elde etmek için iş parçacığı eşzamanlılık JVM içinde kullanmak varsayalım. 8 çalışan görevler (Azure VM başına bir tane) ve 512 Cıvata yürütücüler ihtiyacımız anlamına gelir. Bu yapılandırma verildiğinde, her bir çalışan düğümünün 1 vermiş çalışanları çalışan düğümleri (olarak da bilinen yönetici düğümler), arasında eşit olarak dağıtmak Storm çalışır JVM. Yürütücüler denetçiler arasında eşit olarak dağıtmak Storm denetçiler içinde çalışır artık her yönetici (diğer bir deyişle, JVM) vermiş 8 her iş parçacıkları.

## <a name="tune-additional-parameters"></a>Ek parametrelerini ayarlama
Temel topoloji aldıktan sonra parametrelerinden herhangi birini ince ayar isteyip istemediğinizi düşünebilirsiniz:
* **Çalışan düğüm başına JVMs sayısı.** Büyük veri yapısı (örneğin, bir arama tablosu) varsa, barındıran bellekte her JVM ayrı bir kopyasını gerektirir. Alternatif olarak, daha az JVMs varsa birçok iş parçacıkları arasında veri yapısı kullanabilirsiniz. Cıvata 's g/ç JVMs sayısı kadar bu JVMs eklenen iş parçacığı sayısı olarak farkının yapmaz. Kolaylık olması için çalışan her bir JVM sağlamak için bir fikirdir. Bu numarayı değiştirmeniz gerekebilir rağmen Cıvata yaptıklarını veya hangi uygulama, işleme bağlı olarak, gerektirir.
* **Spout yürütücüler sayısı.** Önceki örnekte, Data Lake Store'a yazmak için Cıvatalar kullandığından, spout'lar sayısı Cıvata performansı doğrudan ilgili değildir. Ancak, işleme veya spout gerçekleştiği g/ç miktarına bağlı olarak, en iyi performans için spout'lar ayarlamak için iyi bir fikir değil. Cıvatalar meşgul devam edebilmek için yeterli spout'lar olduğundan emin olun. Spout'lar çıkış oranlarda Cıvatalar verimini eşleşmelidir. Spout üzerinde gerçek yapılandırmasına bağlıdır.
* **Görev sayısı.** Her Cıvata tek bir iş parçacığı çalıştırır. Ek görevler Cıvata başına herhangi bir ek eşzamanlılık sağlamaz. Tuple aktarımının, işlemi Cıvata yürütme süresi büyük bir kısmının alır, yararı oldukları yalnızca zamanı gelmiştir. Bu Cıvata onay göndermeden önce birçok başlıkları daha geniş bir içine ekleme bir gruba fikirdir. Bu nedenle, çoğu durumda, birden çok görevler ek fayda sağlar.
* **Yerel veya karışık gruplandırma.** Bu ayar etkinleştirildiğinde, tanımlama grupları aynı çalışan işlemi içinde Cıvatalar gönderilir. Bu işlemler arası iletişim ve ağ çağrıları azaltır. Bu, çoğu Topolojileri için önerilir.

Bu temel senaryo iyi bir başlangıç noktasıdır. En iyi performans elde etmek için önceki parametreleri ince ayar yapma kendi verilerinizle sınayın.

## <a name="tune-the-spout"></a>Spout ayarlama

Spout ayarlamak için aşağıdaki ayarları değiştirebilirsiniz.

- **Tuple zaman aşımı: topology.message.timeout.secs**. Bu ayarı tamamlamak ve onay, almak için bir ileti gereken süre miktarını belirler başarısız kabul edilmeden önce.

- **Çalışan işlemi başına maksimum bellek: worker.childopts**. Bu ayar Java çalışanları için ek komut satırı parametreleri belirtmenize olanak sağlar. En yaygın kullanılan ayar burada JVM'ın yığın için ayrılan maksimum bellek belirler XmX'dir.

- **Max spout bekleyen: topology.max.spout.pending**. Bu ayar (henüz onaylanan da topolojideki tüm düğümlerde) uçuş spout iş parçacığı başına herhangi bir zamanda buna olabilir başlıkların sayısını belirler.

 Yapmak için iyi bir hesaplama her, başlık boyutunu tahmin etmek için kullanılır. Sonra ne kadar bellek bir spout iş parçacığı olduğunu göstermektedir. Bu değer ile ayrılmış bir iş parçacığı için ayrılan toplam bellek parametresi bekleyen max spout için üst sınır vermesi gerekir.

## <a name="tune-the-bolt"></a>Cıvata ayarlama
Data Lake Store'a yazarken boyutu eşitleme ilkesi (istemci tarafı arabelleği) 4 MB olarak ayarlayın. Arabellek boyutu olduğunda bir temizleme veya hsync() sonra gerçekleştirilen bu değerde. Açıkça bir hsync() gerçekleştirmediğiniz sürece çalışan VM Data Lake Store sürücüsünde bu arabelleğe alma, otomatik olarak yapar.

Varsayılan Data Lake deposu Storm Cıvata bu parametreyi ayarlamak için kullanılan bir boyutu eşitleme ilkesi parametre (fileBufferSize) içeriyor.

G/Ç kullanımı yoğun topolojide kendi dosyasına yazma her Cıvata iş parçacığı vardır ve bir dosya döndürme İlkesi (fileRotationSize) ayarlamak için iyi bir fikir değil. Dosya belirli bir boyuta ulaştığında, akış otomatik olarak Temizlenen ve yeni bir dosya yazılır. Döndürme için önerilen dosya boyutu 1 GB'tır.

### <a name="handle-tuple-data"></a>Tuple veri işleme

Açıkça Cıvata tarafından onaylanan kadar Storm bir spout bir tanımlama grubu tutan. Bir tanımlama grubu Cıvata tarafından okunabilir ancak henüz onaylanan değil, Data Lake Store arka uç spout kalıcı. Bir tanımlama grubu kabul edildikten sonra spout Cıvata tarafından Kalıcılık güvence altına alınabilir ve veri kaynağını içinden okuma ne olursa olsun kaynağından sonra silebilirsiniz.  

Data Lake Store üzerinde en iyi performans için Cıvata sahip 4 MB tanımlama grubu veri arabellek. Ardından Data Lake Store'a geri son bir 4 MB yazma. Veri deposuna başarıyla yazıldıktan sonra (arama hflush()) tarafından Cıvata veri spout kabul. Burada sağlanan örnek Cıvata yaptığı budur. Çok sayıda hflush() çağrısı yapılmadan önce tanımlama grupları ve onaylanan diziler tutmak için kullanılabilir. Ancak, bu tutmak spout gerekir ve bu nedenle, bellek miktarı arttıkça JVM gerekir yürütülen başlıkların sayısını artırır.

> [!NOTE]
Uygulamalar, diziler daha sık (adresindeki veri boyutları MB'den küçük 4) diğer olmayan performans nedenleriyle onaylamak için bir gereksinim olabilir. Ancak, depolama arka ucu için g/ç işleme etkileyebilir. Bu kolaylığını Cıvata 's g/ç performansı karşı dikkatle tartmanız.

4 MB arabelleği doldurmak için uzun süren şekilde diziler Geliş hızından değil, yüksekse, bu azaltıcı göz önünde bulundurun:
* Cıvatalar sayısının azaltılması, bu nedenle vardır doldurmak için daha az arabellek.
* Bir hflush() olduğu bir saat veya sayısı tabanlı ilke sahip her Boşaltılma x veya her y milisaniye tetiklenir ve o ana kadarki birikmiş başlıklar geri onaylanır.

Verimlilik bu durumda düşüktür, ancak olayları yavaş oranı ile en yüksek verimlilik büyük hedefi yine de değil unutmayın. Bu Azaltıcı Etkenler deposuna akışına bir tanımlama grubu geçen toplam süreyi azaltmak yardımcı olur. Düşük olay hızı bile ile gerçek zamanlı bir işlem hattı istiyorsanız bu önemli. Ayrıca, gelen tanımlama grubu hızı düşükse, bunlar alma sırasında zaman aşımı başlıklar olmayan şekilde, topology.message.timeout_secs parametresi olarak ayarlaması gerektiğini unutmayın arabelleğe veya işlenen.

## <a name="monitor-your-topology-in-storm"></a>Topolojiniz Storm içindeki izleme  
Topolojiniz çalışıyorken Storm kullanıcı arabiriminde izleyebilirsiniz. Bakmak için ana Parametreler şunlardır:

* **Toplam işlem yürütme gecikme süresi.** Bir tanımlama grubu spout'un yayılan Cıvata tarafından işlenir ve onaylanan için geçen ortalama süreyi budur.

* **Toplam Cıvata işlem gecikme süresi.** Bir bildirim alıncaya kadar Cıvata konumunda yer alan tanımlama grubu tarafından harcanan ortalama süre budur.

* **Toplam Cıvata gecikme yürütün.** Execute yöntemi Cıvata tarafından harcanan ortalama süre budur.

* **Başarısızlık sayısı.** Bu, zaman aşımına uğramadan önce tam olarak işlenmesi başarısız oldu başlıkların sayısını ifade eder.

* **Kapasitesi.** Bu, sisteminizi ne kadar meşgul olduğundan bir ölçüsüdür. Bu sayı 1 ise, Cıvatalar yapabilir kadar hızlı çalışmaktadır. 1'den küçük, paralellik artırın. 1'den büyükse, paralellik azaltın.

## <a name="troubleshoot-common-problems"></a>Sık karşılaşılan sorunları giderme
Birkaç genel sorun giderme senaryoları şunlardır.
* **Birçok diziler zaman aşımına uğruyor.** Her düğüm sıkışıklık olduğu belirlemek için topolojideki bakın. Bunun en yaygın nedeni Cıvatalar ile spout'lar takip edin mümkün olmamasıdır. Bu, işlenmeyi bekleyen iç arabelleklerini tıkamasını diziler neden olmaktadır. Zaman aşımı değerini artırmayı veya max spout bekleyen azaltarak göz önünde bulundurun.

* **Yüksek toplam işlem yürütme gecikmesi, ancak düşük Cıvata işlem gecikme yoktur.** Bu durumda, başlıklar yeterince hızlı alınıyor değil, mümkündür. Acknowledgers yeterli sayıda olup olmadığını denetleyin. Başka bir olasılık işlemeden Cıvatalar başlamadan önce bunlar için çok uzun süre sırada bekleyen emin olabilir. Bekleyen max spout azaltın.

* **Yüksek Cıvata yürütme gecikme süresi yok.** Başka bir deyişle, Cıvata execute() yöntemiyle çok uzun sürüyor. Kodu en iyi duruma veya yazma boyutlarda görünüş ve davranışı temizleme.

### <a name="data-lake-store-throttling"></a>Data Lake Store azaltma
Data Lake Store tarafından sağlanan bant genişliği sınırına ulaşıp görev hataları görebilirsiniz. Hataları azaltma için görev günlüklerini denetleyin. Kapsayıcı boyutu artırarak paralellik düşürebilir.    

Kısıtlanan denetlemek için hata ayıklama istemci tarafında günlüğü etkinleştir:

1. İçinde **Ambari** > **Storm** > **Config** > **storm çalışan log4j Gelişmiş**, değiştirme  **&lt;kök düzeyi "bilgi" =&gt;**  için  **&lt;kök düzeyinde "hata ayıklama" =&gt;**. Tüm düğümleri/hizmet yapılandırmasının etkili olması yeniden başlatın.
2. Storm topolojisini günlüklerini çalışan düğümlerine İzleyicisi (/var/log/storm/worker-artifacts altında /&lt;TopologyName&gt;/&lt;bağlantı noktası&gt;/worker.log) özel durumları azaltma Data Lake Store için.

## <a name="next-steps"></a>Sonraki adımlar
Storm olarak başvurulabilir için ek performans ayarlaması [bu blog](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Çalıştırmak ek örnek için bkz: [github'daki bu bir](https://github.com/hdinsight/storm-performance-automation).
