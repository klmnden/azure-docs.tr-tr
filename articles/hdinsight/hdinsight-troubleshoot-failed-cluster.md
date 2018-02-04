---
title: "Yavaş ya da başarısız olan Hdınsight küme - Azure Hdınsight sorunlarını giderme | Microsoft Docs"
description: "Tanılama ve sorun giderme yavaş veya başarısız olan bir Hdınsight kümesi."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 00c4ac0e2ac059efebbfbe0b2426b27361ad8e37
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-a-slow-or-failing-hdinsight-cluster"></a>Yavaş ya da başarısız olan bir HDInsight kümesinde sorun giderme

Hdınsight kümesi yavaş çalışıyor veya bir hata kodu ile başarısız olursa sorun giderme birkaç seçeneğiniz vardır. İşleriniz çalıştırılması beklenenden uzun sürüyor veya yavaş yanıt sürelerini genel görüyorsanız, küme üzerinde çalışan hizmetleri gibi kümenizi upstream arızasından olabilir. Ancak, en yaygın bu yavaşlamalara yetersiz ölçeklendirme nedenidir. Yeni bir Hdınsight kümesi oluştururken uygun seçin [sanal makine boyutları](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters)

Yavaş ya da başarısız olan bir küme tanılamak için ilişkili Azure hizmetlerini, küme yapılandırması ve iş yürütme bilgileri gibi ortam tüm yönleri hakkında bilgi toplayın. Başka bir kümede hata durumu yeniden oluşturmak yararlı bir tanılama denemektir.

* Adım 1: sorun hakkında veri toplama
* 2. adım: Hdınsight küme ortamında doğrulama 
* 3. adım: kümenizin durumunu görüntülemek
* 4. adım: sürümleri ve ortam yığını gözden geçirin
* 5. adım: küme günlük dosyalarını inceleyin
* 6. adım: yapılandırma ayarlarını kontrol edin
* 7. adım: farklı bir kümede hatayı yeniden oluşturun 

## <a name="step-1-gather-data-about-the-issue"></a>Adım 1: sorun hakkında veri toplama

Hdınsight tanımlamak ve kümeleri ile ilgili sorunları gidermek için kullanabileceğiniz birçok araçlar sağlar. Aşağıdaki adımları bu araçları üzerinden Kılavuzu ve sorunu yerini tam olarak belirtmekte yönelik öneriler sağlar.

### <a name="identify-the-problem"></a>Sorunu tanımla

Sorunu tanımlamaya yardımcı olması için aşağıdaki soruları göz önünde bulundurun:

* Ne ı olmasını beklediğiniz neydi? Bunun yerine ne oldu?
* Ne kadar süreyle işlemi çalıştırmak için sürdü? Ne kadar süreyle çalışmalı?
* Her zaman bu kümede yavaş çalışan görev var mı? Daha hızlı farklı bir küme üzerinde çalışır mı?
* Bu sorun öncelikle ortaya çıktığında? Ne sıklıkta itibaren oluştu?
* Herhangi bir şey my küme yapılandırmasında değişti mi?

### <a name="cluster-details"></a>Küme ayrıntıları

Önemli küme bilgileri içerir:

* Küme adı.
* Küme bölge - denetle [bölge kesintileri](https://azure.microsoft.com/status/).
* Hdınsight küme türü ve sürümü.
* Tür ve head ve çalışan düğümleri için belirtilen Hdınsight örneği sayısı.

Azure portalı, bu bilgileri sağlayabilir:

![Hdınsight Azure portalı bilgileri](./media/hdinsight-troubleshoot-failed-cluster/portal.png)

Azure CLI kullanarak şunları yapabilirsiniz:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

Başka bir seçenek PowerShell kullanıyor. Daha fazla bilgi için bkz: [yönetmek Hadoop kümeleri Azure PowerShell ile hdınsight'ta](hdinsight-administer-use-powershell.md).

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>2. adım: Hdınsight küme ortamında doğrulama

Her Hdınsight kümesi çeşitli Azure hizmetlerine ve Apache HBase ve Apache Spark gibi açık kaynaklı yazılım dayanır. Hdınsight kümeleri, ayrıca Azure sanal ağları gibi diğer Azure hizmetleri hakkında çağırabilirsiniz.  Kümenizde çalışan hizmetlerden herhangi biri tarafından veya bir dış hizmet tarafından küme hatasına neden olabilir.  Bir küme hizmeti yapılandırma değişikliği ayrıca küme başarısız olmasına neden olabilir.

### <a name="service-details"></a>Hizmet ayrıntıları

* Açık kaynak kitaplık yayın sürümleri denetleyin
* Denetleme [Azure hizmet kesintisi](https://azure.microsoft.com/status/) 
* Azure hizmet kullanım sınırları denetle 
* Azure sanal ağ alt ağ yapılandırmasını denetleyin 

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ambari UI ile küme yapılandırma ayarlarını görüntüleme

Apache Ambari yönetimi ve izlenmesi web kullanıcı Arabirimi ile bir Hdınsight kümesi ve bir REST API sağlar. Linux tabanlı Hdınsight kümelerinde Ambari dahil edilir. Seçin **küme Panosu** sayfasında Azure portalı Hdınsight bölmesi.  Seçin **Hdınsight küme Panosu** bölmesinde Ambari kullanıcı arabirimini açın ve küme oturum açma kimlik bilgilerini girin.  

![Ambari UI](./media/hdinsight-troubleshoot-failed-cluster/ambari-ui.png)

Hizmet görünümlerini listesini açmak için seçin **Ambari görünümleri** Azure portal sayfasında.  Bu liste, hangi kitaplıkların yüklü bağlıdır. Örneğin, YARN sıra yöneticisinin, Hive görünümü ve Tez görünümü görebilirsiniz.  Yapılandırma ve hizmet bilgileri görmek için bir hizmet bağlantı seçin.

#### <a name="check-for-azure-service-outages"></a>Azure hizmet kesintisi denetle

Hdınsight birkaç Azure hizmetlerini kullanır. Sanal sunucuları Azure Hdınsight, veriyi depolar ve komut dosyaları Azure Blob storage veya Azure DataLake depolama üzerinde çalışır ve günlük dosyalarının Azure Table storage ' dizinleri. Bu hizmetler kesintileri ender olsa Hdınsight'ta sorunlara neden olabilir. Varsa beklenmeyen yavaşlamalara veya kümenizde hataları kontrol edin [Azure durum Panosu](https://azure.microsoft.com/status/). Her hizmet durumunu bölgeye göre listelenir. Kümenizin bölge ve aynı zamanda tüm ilgili hizmetler için bölgeler denetleyin.

#### <a name="check-azure-service-usage-limits"></a>Azure hizmeti kullanım sınırlarını denetleyin

Büyük bir küme başlatma ya da birçok kümeleri aynı anda başlatılan bir Azure hizmeti sınırını aştınız küme başarısız. Hizmet sınırları, Azure aboneliğinize bağlı olarak değişebilir. Daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits).
Microsoft Hdınsight kaynaklarının (VM çekirdeğe ve VM örneği gibi) kullanılabilir sayısını artırmak isteyebilirsiniz ile bir [Resource Manager çekirdek kota artışı isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

#### <a name="check-the-release-version"></a>' Nin sürümünü denetleme

En son Hdınsight sürüm küme sürümüyle karşılaştırın. Her Hdınsight sürüm yeni uygulamalar, özellikler, düzeltme ekleri ve hata düzeltmeleri gibi geliştirmeler içerir. Kümenizi etkileyen sorunu en son sürüm sürümde sabit. Mümkünse, Hdınsight ve ilişkili kitaplıkları Apache HBase, Apache Spark ve diğerleri gibi en son sürümünü kullanarak kümenizi yeniden çalıştırın.

#### <a name="restart-your-cluster-services"></a>Küme hizmetlerini yeniden başlatın

Kümenizdeki yavaşlamalara karşılaşıyorsanız, hizmetlerinizi Ambari UI veya Azure CLI aracılığıyla yeniden başlatmayı düşünün. Küme geçici hatayla karşılaşıyor olabilir ve yeniden başlatmayı ortamınızı Sabitle ve büyük olasılıkla performansı geliştirmek için en hızlı yoludur.

## <a name="step-3-view-your-clusters-health"></a>3. adım: kümenizin durumunu görüntülemek

Hdınsight kümeleri farklı türde bir sanal makine örnekleri üzerinde çalışan düğümleri oluşur. Her düğüm, kaynak yetersizliğini, ağ bağlantısı sorunları ve kümeyi yavaşlatabilir diğer sorunlar için izlenebilir. Her küme iki baş düğümler ve çoğu küme türleri çalışan ve kenar düğümleri bir birleşimini içerir. 

Her küme türü kullanan çeşitli düğümleri açıklaması için bkz: [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](hdinsight-hadoop-provision-linux-clusters.md).

Aşağıdaki bölümlerde, her düğümün ve genel kümesinin durumu Denetim açıklar.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Ambari UI Panoyu kullanarak küme Sistem görüntüsünü Al

[Ambari kullanıcı Arabirimi Panosu](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) açık kalma süresi, bellek, ağ ve CPU kullanımı, HDFS disk kullanımı ve diğerleri gibi küme durumu genel bir bakış sağlar. Bir ana bilgisayar düzeyinde kaynakları görüntülemek için Ambari ana bölümünü kullanın. Ayrıca, durdurabilir ve hizmetlerini yeniden başlatın.

### <a name="check-your-webhcat-service"></a>WebHCat hizmetinizi denetleyin

Yaygın bir senaryo Hive, Pig ve Sqoop işleri başarısız oluyor için olan bir hata ile [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (veya *Templeton*) hizmeti. WebHCat, Hive, Pig, bilgi ve MapReduce gibi uzak iş yürütme için bir REST arabirimidir. WebHCat iş gönderme istekleri YARN uygulamalara dönüşür ve YARN uygulama durumundan türetilmiş bir durum döndürür.  Aşağıdaki bölümlerde ortak WebHCat HTTP durum kodları açıklanmaktadır.

#### <a name="badgateway-502-status-code"></a>BadGateway (502 durum kodu)

Bu genel bir ağ geçidi düğümleri iletisidir ve en sık karşılaşılan hata durum kodu. Bu olası bir nedeni, etkin baş düğümünde kapandıktan WebHCat hizmetidir. Bu olasılığı için kontrol etmek için aşağıdaki CURL komutunu kullanın:

```bash
$ curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari WebHCat hizmeti kapalı olduğu konaklar gösteren bir uyarı görüntüler. WebHCat hizmeti ana bilgisayar hizmeti yeniden başlatarak yedekleme getirilecek deneyebilirsiniz.

![WebHCat sunucuyu yeniden başlatın](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat.png)

WebHCat sunucu hala gündeme değil, hata iletileri için işlem günlüğünü denetleyin. Daha ayrıntılı bilgi için kontrol `stderr` ve `stdout` düğümde başvurulan dosyaları.

#### <a name="webhcat-times-out"></a>WebHCat zaman aşımına uğradı

Döndürme iki dakikadan daha uzun sürer yanıtları çıkışı bir Hdınsight ağ geçidi zaman `502 BadGateway`. İş durumları için YARN Hizmetleri WebHCat sorgular ve YARN yanıtlamak için iki dakikadan uzun sürerse, isteği can zaman aşımı.

Bu durumda, aşağıdaki günlüklere gözden `/var/log/webhcat` dizini:

* **webhcat.log** hangi sunucu yazma günlüklerine log4j günlüğü
* **webhcat console.log** başlatıldığında sunucu STDOUT
* **webhcat konsol error.log** sunucu işlemi stderr

> [!NOTE]
> Her `webhcat.log` günlük, adlı dosyalar oluşturma toplanmadan `webhcat.log.YYYY-MM-DD`. Araştırdığınız zaman aralığı için uygun dosyayı seçin.

Aşağıdaki bölümlerde WebHCat zaman aşımı için bazı olası nedenleri açıklanmaktadır.

##### <a name="webhcat-level-timeout"></a>WebHCat düzeyi zaman aşımı

WebHCat 10'dan fazla açık yuva ile yük altında olduğunda zaman aşımı sonuçlanabilir yeni yuva bağlantıları kurmak için daha uzun sürer. Ağ bağlantıları için ve WebHCat listelemek için kullanın `netstat` geçerli etkin headnode üzerinde:

```bash
$ netstat | grep 30111
```

30111 WebHCat dinlediği bağlantı noktasıdır. Açık yuva sayısı 10'dan az olmalıdır.

Hiçbir açık yuva varsa, önceki komutu bir sonuç üretmez. Templeton yukarı olup olmadığını denetleyin ve 30111, bağlantı noktasında dinleme kullanmak için:

```bash
$ netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>YARN düzeyi zaman aşımı

İşlerini çalıştırmak için YARN Templeton çağırır ve bir zaman aşımı Templeton ve YARN arasındaki iletişimi neden olabilir.

YARN düzeyinde zaman aşımları iki tür vardır:

1. YARN işi gönderme zaman aşımı neden için yeterince uzun sürebilir.

    Açarsanız `/var/log/webhcat/webhcat.log` günlük dosyası ve "sıraya alınan işi" için arama, karşılaşabileceğiniz birden fazla varlık yürütme süresi aşırı uzun olduğu (> 2000 ms), artan gösteren girişlerle kez bekleyin.

    Sıraya alınan işleri için zaman en yeni iş gönderilene hızı en eski işleri tamamlanana hızından daha yüksek olduğu için artmaya devam eder. YARN bellek kullanıldığında, % 100 olduğunda *joblauncher sıra* kapasiteden artık kullanırım *varsayılan sıra*. Bu nedenle, daha fazla yeni işleri joblauncher sıraya kabul edilebilir. Bu davranış bekleme süresi uzar neden olabilir ve daha uzun bir zaman aşımı hatası neden genellikle birçok başkaları tarafından izlenir.

    Aşağıdaki resimde joblauncher sırası 714.4 aşırı kullanılmasına % gösterir. Var olduğu sürece hala boş kapasite gelen ödünç almak için varsayılan sırasındaki bu kabul edilebilir. Ancak, küme tam olarak kullanılan ve YARN bellek % 100 kapasitede olduğunda, yeni işleri, sonunda zaman aşımlarına neden olan beklemeniz gerekir.

    ![Joblauncher queue](./media/hdinsight-troubleshoot-failed-cluster/joblauncher-queue.png)

    Bu sorunu çözmek için iki yolu vardır: ya da gönderilen yeni işleri ya da tüketim hızı eski işleri küme ölçeklendirme tarafından artış hızına azaltın.

2. YARN işleme zaman aşımlarına neden uzun zaman alabilir.

    * Tüm işleri listelemek: zaman alan bir çağrı budur. Bu çağrı YARN ResourceManager ve tamamlanan her uygulama için uygulamaları sıralar, YARN JobHistoryServer durumunu alır. Bu çağrı işleri daha yüksek numaralarıyla zaman aşımı olabilir.

    * Liste işleri yedi günden daha eski: Hdınsight YARN JobHistoryServer yedi gün için tamamlanan iş bilgilerini korumak üzere yapılandırılmış (`mapreduce.jobhistory.max-age-ms` değeri). Zaman aşımı silinen işleri sonuçlarında numaralandırılmaya çalışılırken.

Bu sorunları tanılamak için:

    1. Sorun giderme için UTC zaman aralığı belirler
    2. Uygun seçin `webhcat.log` dosyaları
    3. Bu işlem sırasında UYAR ve hata iletileri için Ara

#### <a name="other-webhcat-failures"></a>Diğer WebHCat hataları

1. HTTP durum kodu 500

    WebHCat 500 döndüğü çoğu durumda, hata iletisi hata hakkında ayrıntılar içerir. Aksi takdirde, Ara `webhcat.log` UYAR ve hata iletileri için.

2. İş hataları

    Burada WebHCat ile etkileşim başarılı olur, ancak işleri başarısız durumlar olabilir.

    Templeton olarak iş konsol çıktısı toplar `stderr` içinde `statusdir`, olduğu genellikle sorun giderme için yararlıdır. `stderr`Gerçek sorgu YARN uygulama kimliğini içerir.

## <a name="step-4-review-the-environment-stack-and-versions"></a>4. adım: sürümleri ve ortam yığını gözden geçirin

Ambari UI **yığını ve sürüm** sayfası, Küme Hizmetleri Yapılandırma ve hizmet sürüm geçmişi hakkında bilgi sağlar.  Yanlış Hadoop hizmeti kitaplık sürümleri küme hatanın nedenini olabilir.  Ambari Arabiriminde seçin **yönetici** menüsünü ve sonra **yığınları ve sürümleri**.  Seçin **sürümleri** hizmet sürüm bilgileri görmek için page sekmesinde:

![Yığın ve sürümleri](./media/hdinsight-troubleshoot-failed-cluster/stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>5. adım: günlük dosyalarını inceleyin

Birçok Hizmetleri ve Hdınsight kümesi oluşturan bileşen oluşturulan günlükler birçok türü vardır. [WebHCat günlük dosyaları](#check-your-webhcat-service) daha önce açıklanan. Aşağıdaki bölümlerde açıklandığı gibi küme ile ilgili sorunları daraltmak için araştırabilirsiniz diğer çeşitli yararlı günlük dosyalarını vardır.

![Hdınsight günlük dosyası örneği](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

* Hdınsight kümeleri oluşur birkaç düğümlerinin çoğunu görevli gönderilen işlerini çalıştırmak için. İşlerini aynı anda çalıştırmak, ancak günlük dosyaları yalnızca sonuçlarını doğrusal olarak görüntüleyebilirsiniz. Hdınsight, ilk tamamlamak için başarısız olan diğer sonlandırma yeni görevleri yürütür. Bu etkinlik için oturum açmış `stderr` ve `syslog` dosyaları.

* Betik eylemi günlük dosyaları, küme oluşturma işlemi sırasında hatalar veya beklenmeyen yapılandırma değişiklikleri gösterir.

* Hadoop adım günlükleri hata içeren bir adımının bir parçası başlatılan Hadoop işleri tanımlar.

### <a name="check-the-script-action-logs"></a>Betik Eylem günlükleri denetleyin

Hdınsight [betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) komut dosyaları el ile veya ne zaman belirtilen küme üzerinde çalışacak. Örneğin, betik eylemleri kümede ek yazılım yüklemesi veya varsayılan değerlerden yapılandırma ayarlarını değiştirmek için kullanılabilir. Betik Eylem günlükleri denetimi Küme kurulumu ve yapılandırması sırasında oluşan hataların fikirler sağlayabilir.  Betik eylemi durumunu seçerek görüntüleyebilirsiniz **ops** düğmesini Ambari kullanıcı arabiriminde ya da varsayılan depolama hesabından günlükleri erişerek.

Betik Eylem günlükleri bulunan `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` dizin.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>Hızlı bağlantılar Ambari kullanarak Hdınsight günlüklerini görüntüle

Hdınsight Ambari UI içerir **hızlı bağlantılar** bölümler.  Hdınsight kümenizdeki belirli bir hizmet için günlük bağlantıları erişmek için kümeniz için Ambari kullanıcı arabirimini açın ve sol listeden hizmet bağlantısını seçin. Seçin **hızlı bağlantılar** açılan listesinde, ardından Hdınsight düğümü faiz ve onun ilişkili günlük bağlantısını seçin.

Örneğin, HDFS günlükler için:

![Günlük dosyaları Ambari hızlı bağlantılar](./media/hdinsight-troubleshoot-failed-cluster/quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Hadoop oluşturulan günlük dosyalarını görüntülemek

Hdınsight kümesi Azure tabloları ve Azure Blob Depolama birimine yazılan günlükler oluşturur. YARN kendi yürütme günlüklerini oluşturur. Daha fazla bilgi için bkz: [yönetmek Hdınsight kümesi için günlükleri](hdinsight-log-management.md#access-the-hadoop-log-files).

### <a name="review-heap-dumps"></a>Yığın dökümleri gözden geçirin

Yığın dökümleri değişkenlerin değerleri, çalışma zamanında oluşan sorunları tanılamak için yararlı olan o zaman dahil olmak üzere uygulamanın belleğin anlık görüntüsünü içerir. Daha fazla bilgi için bkz: [etkinleştir yığın dökümleri Linux tabanlı hdınsight'ta Hadoop Hizmetleri için](hdinsight-hadoop-collect-debug-heap-dump-linux.md).

## <a name="step-6-check-configuration-settings"></a>6. adım: yapılandırma ayarlarını kontrol edin

Hdınsight kümeleri için Hadoop, Hive, HBase vb. gibi ilgili hizmetler varsayılan ayarlarla önceden yapılandırılmış. Küme, donanım yapılandırmasına, düğüm sayısının üst türüne bağlı olarak, işi türlerini çalıştırıyorsanız, birlikte çalıştığınız veri (ve nasıl verileri işleniyor) yapılandırmanızı en iyi duruma getirme gerekebilir.

Çoğu senaryo için performans yapılandırmaları en iyi duruma getirme hakkında ayrıntılı yönergeler için bkz: [Ambari ile küme yapılandırmaları en iyi duruma](hdinsight-changing-configs-via-ambari.md). Spark kullanırken bkz [performansı en iyi duruma getirme Spark işleri](spark/apache-spark-perf.md). 

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>7. adım: farklı bir kümede hatayı yeniden oluşturun

Bir küme hatanın kaynağını tanımlanmasına yardımcı olmak için aynı yapılandırmaya sahip yeni bir küme başlatın ve başarısız iş adımları tek tek yeniden gönderin. Her adım, bir sonraki işlemeden önce denetleyin. Bu yöntem düzeltin ve başarısız olan tek bir adımda yeniden çalıştırma olanağı sağlar. Bu yöntem aynı zamanda yalnızca bir kez giriş verilerinizi yükleme avantajına sahiptir.

1. Başarısız küme aynı yapılandırmaya sahip yeni bir sınama kümesi oluşturun.
2. İlk işi adımı test kümeye Gönder.
3. Adım işlem tamamlandığında, adım günlük dosyalarındaki hataları denetleyin. Test kümenin ana düğümüne bağlanmak ve günlük dosyalarını görüntüleyin. Adım günlük dosyaları, yalnızca adım süredir sonlandığında çalışır veya başarısız olduktan sonra görüntülenir.
4. İlk adım başarılı olursa, sonraki adıma çalıştırın. Hatalar oluşursa, günlük dosyalarında hata araştırın. Kodunuzda bir hata oluştu, düzeltmeler yapmak ve adım yeniden çalıştırın. 
5. Hatasız adımların tümünü gerçekleştirene kadar devam eder.
6. İşiniz bittiğinde silin test küme hata ayıklaması.

## <a name="next-steps"></a>Sonraki adımlar

* [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
* [Hdınsight günlüklerini analiz edin](hdinsight-debug-jobs.md)
* [Linux tabanlı Hdınsight üzerindeki erişim YARN uygulama günlüğü](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı hdınsight'ta Hadoop Hizmetleri için yığın dökümleri etkinleştir](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Hdınsight'ta Apache Spark kümesi için bilinen sorunlar](hdinsight-apache-spark-known-issues.md)
