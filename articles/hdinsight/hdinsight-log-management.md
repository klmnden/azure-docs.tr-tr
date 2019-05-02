---
title: Bir HDInsight kümesi - Azure HDInsight için günlükleri yönetme
description: Türler, boyutları ve HDInsight faaliyet günlük dosyaları için bekletme ilkeleri belirleyin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: hrasheed
ms.openlocfilehash: b42eb51b510423ffc0d15ee3a646bca3d4392f7f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686848"
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>HDInsight kümesi için günlükleri yönetme

Günlük dosyaları farklı bir HDInsight kümesi oluşturur. Örneğin, Apache Hadoop ve Apache Spark gibi ilgili hizmetlerin ayrıntılı iş yürütme günlükleri üretir. Günlük dosyası yönetimi, sağlıklı bir HDInsight kümesi bakımını yapma, parçasıdır. Günlük arşivlemek için yasal gereksinimleri olabilir.  Sayısı ve günlük dosyalarının boyutu nedeniyle, maliyet Yönetimi hizmet günlük depolama en iyi duruma getirme ve arşivleme ile yardımcı olur.

HDInsight küme günlüklerini yönetme küme ortamında tüm yönleri hakkında bilgi koruma içerir. Bu bilgiler, gerektiği gibi tüm ilişkili Azure hizmeti günlükleri, küme yapılandırması, iş yürütme bilgileri, tüm hata durumları ve diğer verileri içerir.

HDInsight günlük Yönetimi'ndeki tipik adımlar şunlardır:

* 1. Adım: Günlük bekletme ilkeleri belirleme
* 2. Adım: Küme hizmeti sürümleri yapılandırma günlüklerini yönetme
* 3. Adım: Küme iş yürütme günlük dosyalarını yönetme
* 4. Adım: Günlük birim depolama boyutları ve maliyetleri tahmin edin
* 5. Adım: Günlük arşiv ilkeleri ve işlemlerini belirleyin

## <a name="step-1-determine-log-retention-policies"></a>1. Adım: Günlük bekletme ilkeleri belirleme

Bir HDInsight kümesi günlük yönetimi stratejisi oluşturmanın ilk adımı, iş senaryoları ve iş yürütme geçmişi depolama alanı gereksinimleri hakkında bilgi toplamak sağlamaktır.

### <a name="cluster-details"></a>Küme Ayrıntıları

Aşağıdaki küme ayrıntıları, günlük yönetimi stratejinizin bilgileri toplamak için yardımcı kullanışlıdır. Bu bilgiler, belirli bir Azure hesabı oluşturduğunuz tüm HDInsight kümelerinin toplayın.

* Küme adı
* Bölge kümesi ve Azure kullanılabilirlik alanı
* Son durum değişikliği ayrıntılar dahil olmak üzere, küme durumu
* Tür ve ana, çekirdek ve görev düğümleri belirtilen HDInsight örnek sayısı

Azure portalını kullanarak bu üst düzey bilgilerin çoğunu elde edebilirsiniz.  Alternatif olarak, [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) , HDInsight kümesi hakkında bilgi almak için:

```azurecli
    az hdinsight list --resource-group <ResourceGroup>
    az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Bu bilgileri görüntülemek için PowerShell de kullanabilirsiniz.  Daha fazla bilgi için [yönetme Apache Hadoop, Azure PowerShell kullanarak HDInsight kümeleri](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Kümeleriniz üzerinde çalışan iş yüklerini anlama

Tasarım uygun stratejiler her türü için günlüğe kaydetme, HDInsight kümesi üzerinde çalışan iş yükü türlerini anlamak önemlidir.

* İş yükleri (örneğin, geliştirme veya test) Deneysel veya üretim kalitesinde misiniz?
* Ne sıklıkta üretim kalitesinde iş yükleri normalde çalıştırılır?
* Tüm iş yükleri, yoğun kaynak ve/veya uzun süre çalışan misiniz?
* Tüm iş yükleri, Hadoop Hizmetleri için birden çok günlükleri üretilir karmaşık bir dizi kullanıyor musunuz?
* İş yüklerinin sahip ilgili yasal yürütme kökenini gereksinimleri?

### <a name="example-log-retention-patterns-and-practices"></a>Örnek günlük bekletme desenler ve uygulamalar

* Veri kökenini tanımlayıcının her günlük girişinin veya diğer teknikleri aracılığıyla ekleyerek izleme koruma göz önünde bulundurun. Bu, özgün veri kaynağını ve işlemi geri izleme ve takip edin, tutarlılık ve geçerlilik anlamak için her bir aşamayla verilerine sağlar.

* Nasıl, günlükleri, birden fazla küme veya küme toplayabilir ve bunları denetim, izleme, planlama ve uyarı verme gibi amaçlar için harmanlama göz önünde bulundurun. Erişim ve düzenli olarak, günlük dosyalarını indirin ve birleştirin ve Pano görünen sağlamak için bunları çözümlemek için özel bir çözüm kullanabilirsiniz. Ayrıca, güvenlik veya hata algılama için uyarı vermek için ek özellikler de ekleyebilirsiniz. PowerShell, HDInsight SDK veya Azure Klasik dağıtım modeli erişen bir kod kullanarak bu yardımcı programlar oluşturabilirsiniz.

* İzleme bir çözüm ya da hizmet yararlı bir yararı olup göz önünde bulundurun. Microsoft System Center sağlayan bir [HDInsight Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=42521). Ayrıca, toplamak ve günlükleri merkezileştirmek için Apache Chukwa ve Ganglia gibi üçüncü taraf araçları da kullanabilirsiniz. Birçok şirket, Hadoop tabanlı büyük veri çözümleri, örneğin izlemek için hizmetleri sunar: Centerity, Compuware APM, Sematext SPM ve Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>2. Adım: Küme hizmeti sürümleri yönetmek ve betik eylemi günlüklerini görüntüleme

Tipik bir HDInsight kümesi, çeşitli hizmetler ve açık kaynak yazılım paketleri (örneğin, Apache HBase, Apache Spark ve diğerleri) kullanır. Bioinformatics gibi bazı iş yükleri için hizmet yapılandırması günlük geçmişi ek iş yürütme günlükleri tutmak için gerekebilir.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Görüntülemek Ambari UI ile küme yapılandırma ayarları

Apache Ambari basitleştirir, yönetim, yapılandırma ve bir web sağlayarak bir HDInsight kümesini izleme kullanıcı Arabirimi ve REST API. Linux tabanlı HDInsight kümelerinde Ambari dahildir. Seçin **küme Panosu** bölmesini açmak için Azure portal HDInsight sayfasında **küme panoları** bağlantı sayfası.  Ardından, **HDInsight küme Panosu** bölmesinde Ambari UI'ı açın.  Küme oturum açma kimlik bilgileriniz istenir.

Hizmet görünümlerini listesini açmak için seçmeniz **Ambari görünümleri** HDInsight için Azure portal sayfasındaki bölmesi.  Bu liste, yüklediğiniz hangi kitaplıkların bağlı olarak değişir.  Örneğin, kuyruk Yöneticisi YARN, Hive görünümü ve Tez görünümü görebilirsiniz.  Yapılandırma ve hizmet bilgileri görmek için herhangi bir hizmeti bağlantıyı seçin.  Ambari UI **yığını ve sürüm** sayfa küme hizmetlerini yapılandırma ve hizmet sürüm geçmişi hakkında bilgi sağlar. Ambari UI kısmına gitmek için **yönetici** menüsünü ve ardından **yığınları ve sürümleri**.  Seçin **sürümleri** hizmeti sürüm bilgisini görmek için sekmesinde.

![Yığın ve sürümler](./media/hdinsight-log-management/stack-versions.png)

Ambari UI kullanarak, belirli bir ana bilgisayar (veya düğüm) kümede çalıştırılan tüm (veya tüm) Hizmetleri için yapılandırma indirebilirsiniz.  Seçin **konakları** menüsü, ardından bağlantı için ana ilgi. Bu konağın sayfasında seçin **konak Eylemler** düğmesine ve ardından **indirme istemci yapılandırmaları**. 

![Ana istemci yapılandırmaları](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Betik Eylem günlükleri görüntüleyin

HDInsight [betik eylemlerini](hdinsight-hadoop-customize-cluster-linux.md) komut dosyaları el ile veya ne zaman belirtilen bir kümede çalışır. Örneğin, betik eylemleri, küme üzerinde ek yazılım yüklemeniz veya varsayılan değerleri aracılığıyla yapılandırma ayarlarınızı değiştirmek için kullanılabilir. Betik Eylem günlükleri, Küme kurulumu sırasında oluşan hataları ve küme performansı ve kullanılabilirliği etkileyebilecek yapılandırma ayarları değişiklikleri öngörü sağlayabilir.  Bir betik eyleminin durumunu görmek için seçin **ops** Ambari UI veya erişim durumu varsayılan depolama hesabında oturum düğmesi. Depolama günlüklerini kullanılabilir `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>3. Adım: Küme iş yürütme günlük dosyalarını yönetme

Sonraki adım, çeşitli hizmetler için iş yürütme günlük dosyalarını'gözden geçirme.  Apache HBase, Apache Spark ve birçok diğer hizmetleri içerebilir. Hangi günlüklerin yararlıdır (ve desteklenmeyen) belirleyen zaman alıcı olabilir çok sayıda ayrıntılı günlükleri, bir Hadoop kümesi oluşturur.  Günlük sisteminin anlamak için hedeflenen yönetim günlük dosyalarının önemlidir.  Bir örnek günlük dosyası verilmiştir.

![HDInsight günlük dosyası örneği](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Hadoop günlük dosyalarına erişmek

HDInsight, küme dosya sistemi hem de Azure depolama, günlük dosyalarını depolar. Günlük dosyaları küme içindeki açarak inceleyebilirsiniz bir [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) bağlantı kümesi ve dosya sistemini tarama veya uzak bir baş düğüm sunucu üzerinde Hadoop YARN durumu portalını kullanarak. Günlük dosyaları erişebilir ve Azure Depolama'dan veri indirme araçlardan herhangi birini kullanarak Azure Depolama'daki inceleyebilirsiniz. Örnekler [AzCopy](../storage/common/storage-use-azcopy.md), [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer)ve Visual Studio Sunucu Gezgini. PowerShell ve Azure depolama istemcisi kitaplıklarını veya Azure .NET SDK'ları, Azure blob depolama alanındaki verilere erişmek için kullanabilirsiniz.

Hadoop işlerinin iş çalıştıran *görev denemesi* kümesinde çeşitli düğümler üzerinde. HDInsight, ilk tamamlamayın herhangi bir görev denemesi sonlandırma kurgusal görev denemesi başlatabilirsiniz. Bu denetleyici, stderr ve syslog günlük dosya çubuğunda halindeyken günlüğe önemli bir etkinlik oluşturur. Ayrıca, birden çok görev denemesi aynı anda çalışıyor, ancak bir günlük dosyası yalnızca sonuçlarını doğrusal olarak görüntüleyebilirsiniz.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Azure Blob depolama alanına yazılan HDInsight günlükleri

HDInsight kümeleri, Azure PowerShell cmdlet'lerini veya .NET iş gönderme API'leri kullanılarak gönderilen herhangi bir iş için bir Azure Blob Depolama hesabına görevi günlükleri yazmak için yapılandırılır.  Kümeye SSH üzerinden işleri göndermek, sonra yürütme günlüğü bilgilerini Azure tablolarında önceki bölümde açıklandığı gibi depolanır.

İş yürütme günlük dosyalarını YARN ayrıca oluşturmak gibi HDInsight tarafından oluşturulan çekirdek günlük dosyaları ek olarak, Hizmetleri yüklü.  Hizmetleri yüklü sayısı ve günlük dosyalarının türüne bağlıdır.  Apache HBase, Apache Spark ve benzeri Bunun yaygın hizmetleridir.  Kümenizde kullanılabilir genel günlük dosyalarını anlamak her bir hizmet için günlük iş yürütme dosyaları araştırın.  Her hizmet, günlük dosyalarını depolamak için günlüğe kaydetme konumları ve kendi benzersiz yöntemlerine sahiptir.  Örneğin, aşağıdaki bölümde en yaygın hizmeti günlük dosyalarında (YARN) erişmek için ayrıntıları ele alınmıştır.

### <a name="hdinsight-logs-generated-by-yarn"></a>HDInsight günlüklerini YARN tarafından oluşturulan

YARN, bir çalışan düğümü üzerindeki tüm kapsayıcılar arasında günlükleri toplar ve bu günlükleri çalışan düğümü başına bir toplu günlük dosyası olarak depolar. Bir uygulama tamamlandıktan sonra günlük varsayılan dosya sistemi üzerinde depolanır. Uygulamanız, yüzlerce veya binlerce kullanabilir, ancak günlükler için tek çalışan düğümü üzerinde çalışan tüm kapsayıcılar, her zaman tek bir dosyaya toplanır. Uygulamanız tarafından kullanılan çalışan düğümü başına yalnızca tek bir günlük yok. Günlük toplama, HDInsight kümeleri sürüm 3.0 ve sonraki varsayılan olarak etkindir. Toplanan günlükler kümenin varsayılan depolama alanı bulunur.

```
    /app-logs/<user>/logs/<applicationId>
```

Toplanan günlükler doğrudan okunabilir olmayan kapsayıcı tarafından dizine TFile ikili biçimde yazılır. Bu günlükler, uygulamalar veya ilgi kapsayıcılar için düz metin olarak görüntülemek için YARN ResourceManager günlükleri veya CLI araçlarını kullanın.

#### <a name="yarn-cli-tools"></a>YARN CLI araçları

YARN CLI araçları kullanmak için HDInsight kümesine SSH kullanarak bağlanmanız gerekir. Belirtin `<applicationId>`, `<user-who-started-the-application>`, `<containerId>`, ve `<worker-node-address>` şu komutları çalıştırarak bilgi. Aşağıdaki komutlardan birini düz metin olarak günlükleri görüntüleyebilirsiniz:

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi

YARN ResourceManager kullanıcı Arabirimi, küme baş düğümü üzerinde çalışan ve Ambari web kullanıcı Arabirimi erişilir. YARN günlükleri görüntülemek için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında gidin `https://CLUSTERNAME.azurehdinsight.net`. CLUSTERNAME HDInsight kümenizin adıyla değiştirin.
2. YARN Hizmetleri soldaki listeden seçin.
3. Hızlı bağlantılar açılan listeden, küme baş düğümleri seçin ve ardından **ResourceManager günlükleri**. YARN günlükleri yönelik bağlantıların bir listesi sunulur.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>4. Adım: Günlük birim depolama boyutları ve maliyetleri tahmin edin

Önceki adımları tamamladıktan sonra türleri bir anlayış ve birimler, HDInsight kümesi oluşturmayı günlük dosyalarının vardır.

Bir süre sonra anahtar günlük depolama konumları, günlük veri hacmi analiz edin. Örneğin, birim ve büyüme üzerinde 30-60-90 çözümleyebilirsiniz günlük dönemleri.  Bu bilgileri bir elektronik tabloya kaydedin veya Excel için Visual Studio, Azure Depolama Gezgini veya Power Query gibi diğer araçları kullanın. Daha fazla bilgi için [analiz HDInsight günlüklerini](hdinsight-debug-jobs.md).  

Artık bir günlük yönetimi stratejisi anahtar günlükleri için oluşturmak için yeterli bilgi vardır.  Her iki günlük boyut büyümesi tahmin ve bundan sonra depolama Azure hizmet maliyetlerini oturum, elektronik tabloyu (veya tercih ettiğiniz araç) kullanın.  Ayrıca günlük bekletme için tüm gereksinimleri İncelemekte olduğunuz günlükleri kümesini göz önünde bulundurun.  Artık hangi günlük dosyalarını (varsa) silinebilir ve hangi günlüklerin korunur verilecek ve daha az maliyetli bir Azure depolama alanına arşivlenmiş belirledikten sonra gelecek günlük depolama maliyetleri, reforecast.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>5. Adım: Günlük arşiv ilkeleri ve işlemlerini belirleyin

Hangi günlük dosyalarının silinebilir belirledikten sonra birçok Hadoop Hizmetleri günlük dosyaları belirli bir süre sonra otomatik olarak silmek için günlük parametreleri ayarlayabilirsiniz.

Bazı günlük dosyaları için bir yaklaşım arşivleme düşük fiyatlı günlük dosyası kullanabilirsiniz. Azure Resource Manager etkinlik günlükleri için bu yaklaşım, Azure portalını kullanarak keşfedebilirsiniz.  ARM günlüklerini seçerek arşivlerini ayarlama **etkinlik günlüğü**' HDInsight örneğinizin Azure portalındaki bağlantı.  Etkinlik günlüğü arama sayfasının seçin **dışarı** açmak için menü öğesi **Etkinlik günlüğünü dışarı aktar** bölmesi.  Abonelik, bölge, bir depolama hesabına dışarı aktarmak etkinleştirilip etkinleştirilmeyeceğini ve günlükleri tutmak için kaç gün içinde doldurun. Bu aynı bölmede de bir olay hub'ına dışarı aktarmak belirtebilirsiniz. 

![Günlük dosyalarını Dışarı Aktar](./media/hdinsight-log-management/archive.png)

Alternatif olarak, günlük arşivleme PowerShell ile betik oluşturabilirsiniz.  PowerShell Betiği örneği için bkz. [arşiv Azure Otomasyonu, Azure Blob depolama alanına kaydeder](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8).

### <a name="accessing-azure-storage-metrics"></a>Azure depolama ölçümlerini erişme

Azure depolama, günlük depolama işlemleri ve erişim için yapılandırılabilir. Bu çok ayrıntılı günlükler, izleme ve planlama kapasite ve depolama isteklerine denetim için kullanabilirsiniz. İzleme ve çözümlerinizin performansını ayarlamanıza olanak sağlayan, gecikme süresi ayrıntıları günlüğe kaydedilen bilgileri içerir.
Bir HDInsight kümesi için verileri tutan Azure depolama için oluşturulan günlük dosyalarını incelemek için Hadoop için .NET SDK'sını kullanabilirsiniz.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Eski günlük dosyaları için yedekleme dizinleri sayısı ve boyutu denetleme

Tutulan günlük dosyası sayısı ve boyutu denetlemek için aşağıdaki özellikleri ayarlayın. `RollingFileAppender`:

* `maxFileSize` kritik, yukarıda dosya alınır, dosya boyutudur. Varsayılan değer 10 MB ' dir.
* `maxBackupIndex` oluşturulması için varsayılan 1 yedekleme dosyalarının sayısını belirtir.

### <a name="other-log-management-techniques"></a>Diğer günlük yönetim teknikleri

Disk alanı yetersiz çalışmasını önlemek için bazı işletim sistemi araçları gibi kullanabilirsiniz [logrotate](https://linux.die.net/man/8/logrotate) işleme günlük dosyalarının yönetmek için. Yapılandırabileceğiniz `logrotate` günlük olarak çalıştırılacak sıkıştırma günlük dosyaları ve eskilerle kaldırılıyor. Yaklaşımınızı gereksinimlerinize göre gibi ne kadar süreyle yerel düğümlerinde logfiles tutmak bağlıdır.  

Ayrıca, çıkış günlük boyutunu önemli ölçüde artırır hata ayıklama günlüğü bir veya daha fazla hizmetler için etkin olup olmadığını kontrol edebilirsiniz.  

Günlükler için tek bir merkezi konumda tüm düğümleri toplamak için tüm günlük girişlerini Solr içine almak gibi bir veri akışı oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ve HDInsight için uygulama günlüğe kaydetme](https://msdn.microsoft.com/library/dn749790.aspx)
* [Linux tabanlı HDInsight içinde erişim Apache Hadoop YARN uygulama günlüklerine](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Çeşitli Apache Hadoop bileşenleri için günlük dosyalarının boyutunu kontrol etme](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
