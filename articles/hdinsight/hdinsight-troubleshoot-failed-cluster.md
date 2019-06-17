---
title: Bir HDInsight kümesi - Azure HDInsight üzerinde yavaş ya da başarısız olan bir işi sorunlarını giderme
description: Tanılama ve yavaş ya da başarısız olan bir HDInsight kümesinde sorun giderme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 0f405f542a8408c290704f1707ca10a24b08f861
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65203621"
---
# <a name="troubleshoot-a-slow-or-failing-job-on-a-hdinsight-cluster"></a>Bir HDInsight kümesinde bir yavaş ya da başarısız olan işi sorunlarını giderme

Bir HDInsight kümesinde veri yavaş çalışan veya bir hata kodu ile başarısız olan bir uygulama işlemi, sorun giderme birkaç seçeneğiniz vardır. İşlerinizi beklenenden daha uzun çalışması daha uzun sürüyor veya yavaş yanıt sürelerinin genel görüyorsunuz kümenizden küme üzerinde çalışan hizmetleri gibi Yukarı Akış hataları olabilir. Ancak, bu yavaşlamalardan en yaygın nedeni, yetersiz ölçeklendirme olur. Yeni bir HDInsight kümesi oluşturduğunuzda, uygun seçin [sanal makine boyutları](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters).

Yavaş ya da başarısız olan bir küme tanılamak için ilgili Azure Hizmetleri, küme yapılandırması ve iş yürütme bilgileri gibi ortamın tüm yönleri hakkında bilgi toplayın. Başka bir küme üzerinde hata durumunda yeniden oluşturmak yararlı bir tanılama denemektir.

* 1\. adım: Sorunla ilgili verileri toplayın.
* 2\. adım: HDInsight küme ortamında doğrulayın.
* 3\. adım: Kümenizin sistem durumu görüntüleyin.
* 4\. Adım: Ortam yığını ve sürümlerini gözden geçirin.
* 5\. Adım: Küme günlük dosyalarını inceleyin.
* 6\. Adım: Yapılandırma ayarlarını kontrol edin.
* 7\. Adım: Farklı bir kümede hatayı yeniden oluşturun.

## <a name="step-1-gather-data-about-the-issue"></a>1\. adım: Sorun hakkında veri toplama

HDInsight kümeleri ile ilgili sorunları gidermek üzere kullanabileceğiniz birçok araç sağlar. Aşağıdaki adımlar, bu araçlar Kılavuzu ve sorunun kaynağını yönelik öneriler sağlar.

### <a name="identify-the-problem"></a>Sorunu belirleyin

Sorunu belirlemenize yardımcı olması için aşağıdaki soruları göz önünde bulundurun:

* Ne ı olmasını bekliyorsunuz? Bunun yerine ne oldu?
* Ne kadar işlem çalıştırılacak uyguladınız mı? Ne zaman çalışmalı?
* Her zaman bu kümede yavaş çalışmasına Görevlerim var mı? Daha hızlı bir şekilde farklı bir kümede karşılaştınız mı?
* Ne zaman bu sorun öncelikle gerçekleşti? Ne sıklıkta beri oluştu?
* Herhangi bir şey my küme yapılandırmasında değişti mi?

### <a name="cluster-details"></a>Küme ayrıntıları

Önemli küme bilgilerini içerir:

* Küme adı.
* Küme bölge - denetle [bölgede kesintiler](https://azure.microsoft.com/status/).
* HDInsight küme türü ve sürümü.
* Tür ve baş ve çalışan düğümleri belirtilen HDInsight örnek sayısı.

Azure portalı bu bilgileri sağlayabilir:

![HDInsight Azure portalı bilgileri](./media/hdinsight-troubleshoot-failed-cluster/portal.png)

Ayrıca [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```azurecli
az hdinsight list --resource-group <ResourceGroup>
az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Başka bir seçenek PowerShell kullanıyor. Daha fazla bilgi için [yönetme Apache Hadoop kümeleri, Azure PowerShell ile HDInsight](hdinsight-administer-use-powershell.md).

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>2\. adım: HDInsight Küme ortamı doğrulama

Her bir HDInsight kümesi, çeşitli Azure Hizmetleri ve Apache HBase ve Apache Spark gibi açık kaynak yazılımlar kullanır. HDInsight kümeleri, Azure sanal ağları gibi diğer Azure hizmetlerinde de çağırabilirsiniz.  Küme hatası kümenizdeki çalışmakta olan hizmetlerin birini veya bir dış hizmet neden olabilir.  Küme hizmeti yapılandırma değişikliği da kümenin çökmesine neden olabilir.

### <a name="service-details"></a>Hizmet ayrıntıları

* Açık kaynak kitaplık yayın sürümleri denetleyin.
* Denetle [bir Azure hizmet kesintisi](https://azure.microsoft.com/status/).  
* Azure hizmeti için kullanım sınırlarını denetleyin. 
* Azure sanal ağ alt ağ yapılandırmasını denetleyin.  

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Görüntülemek Ambari UI ile küme yapılandırma ayarları

Apache Ambari, yönetim ve REST API ile bir web kullanıcı Arabirimi ile HDInsight kümesi ve izleme sağlar. Linux tabanlı HDInsight kümelerinde Ambari dahildir. Seçin **küme Panosu** Azure portal HDInsight sayfadaki bölmesini.  Seçin **HDInsight küme Panosu** bölmesinde Ambari UI'ı açın ve küme oturum açma kimlik bilgilerini girin.  

![Ambari UI](./media/hdinsight-troubleshoot-failed-cluster/ambari-ui.png)

Hizmet görünümlerini listesini açmak için seçmeniz **Ambari görünümleri** Azure portal sayfasında.  Bu liste, hangi kitaplıkların yüklü bağlıdır. Örneğin, kuyruk Yöneticisi YARN, Hive görünümü ve Tez görünümü görebilirsiniz.  Yapılandırma ve hizmet bilgileri görmek için bir hizmet bağlantısını seçin.

#### <a name="check-for-azure-service-outages"></a>Bir Azure hizmet kesintisi denetle

HDInsight üzerinde birden fazla Azure hizmetini kullanır. Sanal sunucular verilerini depolar ve komut dosyalarını Azure Blob Depolama veya Azure Data Lake Storage, Azure HDInsight üzerinde çalışır ve Azure tablo Depolama dizinleri günlük dosyaları. Bu hizmetler kesintileri ender olsa da, HDInsight sorunlara neden olabilir. Varsa beklenmeyen yavaşlamalara veya hatalar, kümenizdeki denetleyin [Azure durum Panosu](https://azure.microsoft.com/status/). Her hizmetin durumunu bölgeye göre listelenir. Kümenizin bölge ve ayrıca bölgeler ilgili tüm hizmetler için denetleyin.

#### <a name="check-azure-service-usage-limits"></a>Azure hizmeti kullanım limitlerini denetleme

Büyük bir küme başlatma ya da birçok kümeleri aynı anda başlatılan bir Azure hizmeti sınır aşılırsa bir küme başarısız olabilir. Hizmet sınırları, Azure aboneliğinize bağlı olarak değişebilir. Daha fazla bilgi için [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits).
Microsoft HDInsight kaynakları (sanal makine çekirdeklerine ve sanal makine örnekleri gibi) kullanılabilir sayısını artırma isteği ile bir [Resource Manager çekirdek kota artışı isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

#### <a name="check-the-release-version"></a>Yayın sürümünü denetleyin

Küme sürümünü en son HDInsight sürüm ile karşılaştırın. Her bir HDInsight sürüm iyileştirmeleri gibi yeni uygulamalar, özellikler, düzeltme ekleri ve hata düzeltmeleri içerir. Kümenizi etkileyen bir sorun en son yayın sürümünde düzeltilen. Mümkünse, HDInsight ve Apache HBase, Apache Spark ve diğerleri gibi ilişkili kitaplıklarının en son sürümünü kullanarak kümenize yeniden çalıştırın.

#### <a name="restart-your-cluster-services"></a>Küme hizmetlerinizi yeniden başlatın

Kümenizde yavaşlamalara karşılaşıyorsanız, hizmetlerinizi Ambari UI veya Azure Klasik CLI aracılığıyla yeniden başlatmayı düşünün. Kümeyi geçici hataları yaşıyor olabilirsiniz ve yeniden başlatmayı ortamınızı Sabitle ve büyük olasılıkla performansı artırmak için en hızlı yoludur.

## <a name="step-3-view-your-clusters-health"></a>3\. adım: Kümenizin sistem durumu görüntüleme

HDInsight kümeleri farklı tür sanal makine örneklerinde çalıştıran düğümlerden oluşur. Her düğüm için kaynak yetersizliğini, ağ bağlantısı sorunları ve kümeyi yavaşlatabilir diğer sorunları izlenebilir. Her küme iki baş düğümü ve alt ve kenar düğümlerine çoğu küme türleri içerir. 

Her küme türü kullanan çeşitli düğümler açıklaması için bkz: [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight kümelerinde ayarlama](hdinsight-hadoop-provision-linux-clusters.md).

Aşağıdaki bölümlerde her düğümün ve genel küme sistem durumu nasıl kontrol edileceğini açıklar.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Ambari UI Panoyu kullanarak küme durumunun bir anlık görüntü al

[Ambari UI Pano](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) çalışma süresi, bellek, ağ ve CPU kullanımı, HDFS disk kullanımı ve benzeri gibi küme durumunun genel bir bakış sağlar. Konak düzeyinde kaynakları görüntülemek için Ambari ana bölümünü kullanın. Ayrıca, durdurun ve hizmetlerini yeniden başlatın.

### <a name="check-your-webhcat-service"></a>WebHCat hizmetinizi denetleyin

Apache Hive, Apache Pig ve Apache Sqoop işleri başarısız oluyor sık karşılaşılan senaryolardan biri olan bir hata ile [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (veya *templeton da*) hizmeti. WebHCat, Hive, Pig, Ayrıntılar ve MapReduce gibi uzak iş yürütme için bir REST arabirimidir. WebHCat Apache Hadoop YARN uygulamaları iş gönderme İstekleri çevirir ve YARN uygulama durumu türetilmiş durumuna döndürür.  Aşağıdaki bölümlerde, yaygın WebHCat HTTP durum kodları açıklanmaktadır.

#### <a name="badgateway-502-status-code"></a>BadGateway (502 durum kodu)

Bu genel bir ağ geçidi düğümleri iletisidir ve en yaygın hata durum kodu. Bunun olası bir nedeni, etkin baş düğümünde kapandıktan WebHCat hizmetidir. Bu olasılığı için kontrol etmek için aşağıdaki CURL komutunu kullanın:

```bash
curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari WebHCat hizmeti kapalı olduğu ana bilgisayarları gösteren bir uyarı görüntüler. WebHCat hizmeti konağındaki hizmetini yeniden başlatarak yedekleme getirmek deneyebilirsiniz.

![Restart WebHCat Server](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat.png)

WebHCat sunucusu hala gündeme değil, hata iletileri için işlem günlüğünü denetleyin. Daha ayrıntılı bilgi için kontrol `stderr` ve `stdout` düğümde başvurulan dosyaları.

#### <a name="webhcat-times-out"></a>WebHCat zaman aşımına uğruyor

Bir HDInsight ağ geçidi iki dakikadan uzun süren yanıtları kullanıma döndüren zaman `502 BadGateway`. WebHCat YARN Hizmetleri iş durumları için sorgu gönderir ve YARN yanıt vermek için iki dakikadan uzun sürerse, zaman aşımı can isteyebilirsiniz.

Bu durumda, aşağıdaki günlüklerde gözden `/var/log/webhcat` dizini:

* **webhcat.log** hangi sunucu yazma günlüklerinden log4j günlük
* **webhcat console.log** STDOUT başlatıldığında sunucusunun
* **webhcat konsol error.log** sunucu işleminin stderr olduğu

> [!NOTE]  
> Her `webhcat.log` günlük, adlandırılmış dosyalar oluşturma toplanmadan `webhcat.log.YYYY-MM-DD`. Size, İncelemekte olduğunuz zaman aralığı için uygun dosyayı seçin.

Aşağıdaki bölümlerde, WebHCat zaman aşımları için bazı olası nedenleri açıklanmaktadır.

##### <a name="webhcat-level-timeout"></a>WebHCat düzeyi zaman aşımı

WebHCat 10'dan fazla açık yuvaları ile yük altında olduğunda zaman aşımı sonuçlanabilir yeni yuva bağlantı kurmak için daha uzun sürer. WebHCat gelen ve giden ağ bağlantılarını listelemek için kullanın `netstat` geçerli etkin baş düğüm üzerinde:

```bash
netstat | grep 30111
```

30111 WebHCat dinlediği bağlantı noktasıdır. Açık yuva sayısı 10'dan az olmalıdır.

Hiçbir açık yuva varsa, önceki komutta bir sonuç üretmez. Templeton da yukarı olup olmadığını denetleyin ve 30111, bağlantı noktası üzerinde dinleme kullanmak için:

```bash
netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>YARN düzeyi zaman aşımı

İşleri çalıştırmak için YARN templeton da çağırır ve bir zaman aşımı templeton da YARN arasındaki iletişimi neden olabilir.

YARN düzeyinde, zaman aşımları iki tür vardır:

1. YARN işi gönderme zaman aşımı neden için yeterince uzun sürebilir.

    Açarsanız `/var/log/webhcat/webhcat.log` günlük dosyası ve "sıradaki iş" Ara karşılaşabilirsiniz birden çok girişi yürütme süresinin aşırı uzun olduğu (> 2000 ms), artan gösteren girişler kez bekleyin.

    Sıraya alınan işler için süre, hangi yeni işleri gönderilene oranı tamamlanana ve eski işleri hızından daha yüksek olduğundan artmaya devam eder. YARN bellek kullanıldığında, %100 olduğunda *joblauncher kuyruk* kapasiteden artık kullanırım *varsayılan kuyruk*. Bu nedenle, artık yeni işleri joblauncher kuyruğuna kabul edilebilir. Bu davranış, bekleme süresi uzar neden olabilir ve daha uzun bir zaman aşımı hatası, neden genellikle birçok başkaları tarafından izlenir.

    Aşağıdaki görüntüde 714.4 aşırı kullanılmasına % joblauncher kuyruk gösterilmektedir. Bu, var olduğu sürece hala boş kapasite gelen ödünç almak için varsayılan sırasındaki kabul edilebilir. Ancak, kümenin tamamı kullanılana ve YARN bellek % 100 kapasitede olduğunda yeni işleri, sonunda zaman aşımı neden olan beklemek zorundadır.

    ![Joblauncher sırası](./media/hdinsight-troubleshoot-failed-cluster/joblauncher-queue.png)

    Bu sorunu çözmek için iki yolu vardır: ya da gönderilen yeni işleri ya da artırma tüketim hızını eski işleri küme ölçeklendirme tarafından hızını azaltabilir.

2. YARN işleme zaman aşımı neden olabilir bir uzun zaman alabilir.

    * Tüm işleri listeleyin: Bu zaman alan bir çağrıdır. Bu çağrı, YARN ResourceManager ve tamamlanan her uygulama için uygulamaları numaralandırır, YARN JobHistoryServer durumunu alır. İşleri daha yüksek sayıda ile bu çağrı zaman aşımı olabilir.

    * Liste işleri yedi günden eski: HDInsight YARN JobHistoryServer yedi gün için tamamlanan iş bilgilerini korumak için yapılandırılmış (`mapreduce.jobhistory.max-age-ms` değeri). Bir zaman aşımı Temizlenen işleri sonuçlarında numaralandırılmaya çalışılırken.

Bu sorunları tanılamak için:

1. Sorun giderme için UTC zaman aralığı belirleyin
2. Uygun seçin `webhcat.log` dosyalar
3. Bu süre boyunca UYAR ve hata iletileri için Ara

#### <a name="other-webhcat-failures"></a>Diğer WebHCat hatalar

1. HTTP durum kodunu 500

    WebHCat 500 döndüğü çoğu durumda, hata iletisi hatayla ilgili ayrıntıları içerir. Aksi halde, konum `webhcat.log` UYAR ve hata iletileri için.

2. İş hataları

    Burada WebHCat etkileşim başarılı, ancak işler başarısız olan durumlar olabilir.

    Templeton da toplar işi konsol çıktısı olarak `stderr` içinde `statusdir`, durum genellikle bu şekildedir sorun giderme için kullanışlı. `stderr` Asıl sorguyu YARN uygulama tanımlayıcısını içerir.

## <a name="step-4-review-the-environment-stack-and-versions"></a>4\. Adım: Ortam yığını ve sürümlerini gözden geçirin

Ambari UI **yığını ve sürüm** sayfa küme hizmetlerini yapılandırma ve hizmet sürüm geçmişi hakkında bilgi sağlar.  Yanlış Hadoop hizmeti kitaplık sürümleri küme hata bir neden olabilir.  Ambari UI'nızda seçin **yönetici** menüsünü ve ardından **yığınları ve sürümleri**.  Seçin **sürümleri** sayfasındaki hizmeti sürüm bilgisini görmek için sekmesinde:

![Yığın ve sürümler](./media/hdinsight-troubleshoot-failed-cluster/stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>5\. Adım: Günlük dosyalarını inceleyin

Birçok Hizmetleri ve bir HDInsight kümesi oluşturan bileşenleri oluşturulan günlüklerin birçok türü vardır. [WebHCat günlük dosyalarını](#check-your-webhcat-service) daha önce açıklanmıştır. Aşağıdaki bölümlerde açıklandığı şekilde, kümenizin sorunlarla daraltmak için araştırabilirsiniz diğer birçok kullanışlı günlük dosyası vardır.

* HDInsight kümeleri birçok düğümlerinin oluşur çoğunu görevli gönderilen işlerini çalıştırmak için. İşleri eşzamanlı olarak çalıştırın, ancak günlük dosyalarını yalnızca sonuçları doğrusal olarak görüntüleyebilirsiniz. HDInsight, başkalarının önce tamamlanamayacak sonlandırma yeni görevleri yürütür. Bu etkinlik için oturum açmış `stderr` ve `syslog` dosyaları.

* Betik eylemi günlük dosyaları, küme oluşturma işlemi sırasında hatalar ya da beklenmeyen yapılandırma değişikliklerini gösterir.

* Hata içeren bir adımının bir parçası başlatılan Hadoop işlerini Hadoop adım günlükleri belirleyin.

### <a name="check-the-script-action-logs"></a>Betik Eylem günlükleri denetleyin

HDInsight [betik eylemlerini](hdinsight-hadoop-customize-cluster-linux.md) betikleri el ile veya ne zaman belirtilen küme üzerinde çalıştırılır. Örneğin, betik eylemleri, küme üzerinde ek yazılım yüklemeniz veya varsayılan değerleri aracılığıyla yapılandırma ayarlarınızı değiştirmek için kullanılabilir. Betik Eylem günlükleri denetleme Küme kurulumu ve yapılandırması sırasında oluşan hataların Öngörüler sağlayabilir.  Betik eylemi durumunu'i seçerek görüntüleyebilirsiniz **ops** düğmesi Ambari UI veya varsayılan depolama hesabından günlüklerine erişme.

Betik Eylem günlükleri bulunan `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` dizin.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>Hızlı bağlantılar Ambari kullanarak HDInsight günlüklerini görüntüle

HDInsight Ambari UI içerir **hızlı bağlantılar** bölümler.  HDInsight kümenizdeki belirli bir hizmet için günlük bağlantılara erişmek için Ambari UI, kümeniz açın ve soldaki listeden hizmet bağlantısını seçin. Seçin **hızlı bağlantılar** açılır listesinde, daha sonra HDInsight düğümü faiz ve onun ilişkili günlük bağlantısını seçin.

Örneğin, HDFS günlükler için:

![Günlük dosyaları için Ambari hızlı bağlantıları](./media/hdinsight-troubleshoot-failed-cluster/quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Hadoop tarafından oluşturulan günlük dosyalarını görüntüle

Bir HDInsight kümesi, Azure tabloları ve Azure Blob depolamaya yazılan günlükler oluşturur. YARN kendi yürütme günlüklerini oluşturur. Daha fazla bilgi için [bir HDInsight kümesi için günlükleri yönetme](hdinsight-log-management.md#access-the-hadoop-log-files).

### <a name="review-heap-dumps"></a>Yığın dökümlerini gözden geçirin

Yığın dökümlerini başlatıldığında, çalışma zamanında meydana gelen sorunları tanılamak için yararlı olan değişkenlerin değerlerini de dahil olmak üzere, uygulamanın bellek anlık görüntüsünü içerir. Daha fazla bilgi için [etkin yığın dökümleri Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için](hdinsight-hadoop-collect-debug-heap-dump-linux.md).

## <a name="step-6-check-configuration-settings"></a>6\. Adım: Yapılandırma ayarlarını kontrol edin

HDInsight kümeleri, Hadoop, Hive, HBase ve benzeri gibi ilgili hizmetler için varsayılan ayarlarla önceden yapılandırılmış. Küme, donanım yapılandırması, kendi düğüm sayısını türüne bağlı olarak, işlerin türleri çalıştırıyorsanız ve birlikte çalıştığınız veriler (ve bu verileri nasıl işleniyor), yapılandırmanızı en iyi duruma getirme gerekebilir.

Çoğu senaryo için performans yapılandırmaları en iyi duruma getirme hakkında ayrıntılı yönergeler için bkz. [Apache Ambari ile küme yapılandırmalarını en iyi duruma getirme](hdinsight-changing-configs-via-ambari.md). Spark kullanırken görmeyi [performans için en iyi duruma getirme Apache Spark işleri](spark/apache-spark-perf.md). 

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>7\. Adım: Farklı bir kümede hatayı yeniden oluşturun

Bir küme hatanın kaynağını tanılanmasına yardımcı olmak için aynı yapılandırmaya sahip yeni bir küme başlatın ve sonra başarısız işin adımları tek tek yeniden gönderin. Bir sonraki işlenmeden önce her adımın sonuçlarını denetleyin. Bu yöntem, düzeltin ve başarısız olan tek bir adımda yeniden çalıştırma olanağı sunar. Bu yöntem, aynı zamanda yalnızca bir kez girişinizi yükleme avantajına sahiptir.

1. Başarısız bir küme aynı yapılandırmaya sahip yeni bir test kümesi oluşturun.
2. Test kümesi için ilk iş adımı gönderin.
3. Adım işlemi tamamlandığında, adım günlük dosyalarındaki hatalara bakın. Test kümenin ana düğümüne bağlanma ve günlük dosyalarını görüntüleyebilirsiniz. Adım günlük dosyaları, yalnızca adım biraz zaman, tamamlandığında, çalışan veya başarısız olduktan sonra görünür.
4. İlk adım başarılı olursa, sonraki adıma çalıştırın. Hatalar varsa, günlük dosyalarını hatayı araştırın. Kodunuzda hata olduysa, düzeltme yapmak ve adımın yeniden çalıştırın.
5. Hatasız tüm adımlarını çalıştırmayı kadar devam edin.
6. İşiniz bittiğinde silin test kümesi, hata ayıklama.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](hdinsight-hadoop-manage-ambari.md)
* [HDInsight günlüklerini çözümleme](hdinsight-debug-jobs.md)
* [Linux tabanlı HDInsight Apache Hadoop YARN uygulama oturum erişim](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight üzerinde Apache Spark kümesi için bilinen sorunlar](hdinsight-apache-spark-known-issues.md)
