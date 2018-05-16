---
title: Hdınsight kümesi - Azure Hdınsight için günlüklerini yönetme | Microsoft Docs
description: Türleri, boyutlar ve Hdınsight etkinliği günlük dosyaları için bekletme ilkeleri belirleyin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: d3ca9983eee4db09a68bf772b80c9ef841117872
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>HDInsight kümesi için günlükleri yönetme

Hdınsight kümesi çeşitli günlük dosyalarını oluşturur. Örneğin, Apache Hadoop ve Apache Spark gibi ilgili hizmetler ayrıntılı iş yürütme günlüklerini oluşturur. Günlük dosyası yönetimi, sağlıklı bir Hdınsight kümesi sürdürme parçasıdır. Günlük arşivlemek için yasal gereksinimleri olabilir.  Sayısı ve günlük dosyalarının boyutunu nedeniyle günlük depolama en iyi duruma getirme ve yardımcı olan arşivleme maliyet yönetim hizmeti.

Hdınsight küme günlüklerini yönetme korunuyor küme ortamında tüm yönleri hakkında bilgi içerir. Bu bilgiler, gerektiği gibi tüm ilişkili Azure hizmeti günlükleri, küme yapılandırması, iş yürütme bilgileri, tüm hata durumları ve diğer verileri içerir.

Hdınsight günlük yönetim genel adımlar şunlardır:

* 1. adım: Belirlemek günlük bekletme ilkeleri
* Adım 2: Küme hizmeti sürümleri yapılandırma günlükleri yönetme
* Adım 3: küme iş yürütme günlük dosyalarını yönetme
* 4. adım: günlük birimi depolama boyutlarına ve maliyetleri tahmin
* 5. adım: Günlük arşiv ilkelere ve süreçlere belirleme

## <a name="step-1-determine-log-retention-policies"></a>1. adım: Belirlemek günlük bekletme ilkeleri

Bir Hdınsight kümesi günlük yönetim stratejinizi oluşturmanın ilk adımı, iş senaryolarını ve iş yürütme geçmişi depolama gereksinimleri hakkında bilgi toplamaktır.

### <a name="cluster-details"></a>Küme ayrıntıları

Aşağıdaki küme ayrıntıları yardımcı olma, günlük yönetim stratejinizi bilgileri toplamak için yararlı olur. Bu bilgiler, belirli bir Azure hesabı oluşturduğunuz tüm Hdınsight kümeleri toplayın.

* Küme adı
* Bölge ve Azure kullanılabilirlik bölge küme
* Son durum değişikliği ayrıntılarını da dahil olmak üzere küme durumu
* Tür ve ana, çekirdek ve görev düğümler için belirtilen Hdınsight örnek sayısı

Azure Portalı'nı kullanarak bu üst düzey bilgilerin çoğunu elde edebilirsiniz.  Alternatif olarak, Hdınsight kümelere hakkında bilgi almak için Azure CLI kullanabilirsiniz:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

Bu bilgileri görüntülemek için PowerShell de kullanabilirsiniz.  Daha fazla bilgi için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure PowerShell kullanarak](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Kümeleri üzerinde çalışan iş yüklerinin anlama

Her tür için stratejileri günlüğü uygun tasarımı, Hdınsight kümelere çalışan iş yükü türleri anlamak önemlidir.

* İş yükleri Deneysel (örneğin, geliştirme veya test) veya üretim kaliteli var mı?
* Ne sıklıkta üretim kaliteli iş yükleri normal olarak çalışıyor?
* Tüm iş yükleri, yoğun bir kaynak ve/veya uzun süre çalışan misiniz?
* Tüm iş yükleri, birden çok türde günlüklere üretilen Hadoop Hizmetleri karmaşık kümesi kullanıyor musunuz?
* Herhangi bir iş yükleri ilişkili yasal yürütme çizgileri gereksinimleri?

### <a name="example-log-retention-patterns-and-practices"></a>Örnek günlük bekletme modelleri ve yöntemleri

* Her günlük girişinin veya başka teknikler aracılığıyla bir tanımlayıcı ekleyerek izleme verileri çizgileri koruma göz önünde bulundurun. Bu, özgün veri kaynağı ve işlemi geri izleme ve tutarlılık ve geçerlilik anlamak için her aşama verilerine izleyin olanak sağlar.

* Nasıl, kümeden ya da birden fazla kümesinden günlükleri toplamak ve bunları denetim, izleme, planlama ve uyarı gibi amaçlarla collate göz önünde bulundurun. Erişim ve düzenli olarak, günlük dosyalarını indirmek ve birleştirmek ve bunları bir Pano görüntü sağlamak için analiz için özel bir çözüm kullanabilirsiniz. Güvenlik ve hata algılama için uyarı verme ek yetenekler de ekleyebilirsiniz. PowerShell, Hdınsight SDK veya Azure Klasik dağıtım modeli erişen kodu kullanarak bu yardımcı programları oluşturabilirsiniz.

* Bir izleme çözümü veya hizmet yararlı bir yararı olup göz önünde bulundurun. Microsoft System Center sağlayan bir [Hdınsight Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=42521). Toplamak ve günlükleri merkezileştirmek için Chukwa ve Ganglia gibi üçüncü taraf araçları da kullanabilirsiniz. Birçok şirket, örneğin Centerity, Compuware APM, Sematext SPM ve Zettaset Orchestrator Hadoop tabanlı büyük veri çözümleri izlemek için hizmetleri sunar.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>Adım 2: Küme hizmeti sürümleri yönetmek ve betik eylemi günlükleri görüntüleme

Tipik bir Hdınsight kümesi çeşitli hizmetler ve açık kaynaklı yazılım paketlerini (örneğin, Apache HBase, Apache Spark ve benzeri) kullanır. Bioinformatics gibi bazı iş yükleri için hizmet yapılandırması günlük geçmişi iş yürütme günlüklerini ek olarak korumak için gerekebilir.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ambari UI ile küme yapılandırma ayarlarını görüntüleme

Apache Ambari basitleştirir yönetimi, yapılandırması ve bir web sağlayarak bir Hdınsight kümesini izleme kullanıcı Arabirimi ve bir REST API'si. Linux tabanlı Hdınsight kümelerinde Ambari dahil edilir. Seçin **küme Panosu** bölmesini açmak için Azure portalı sayfasındaki Hdınsight **' küme panolarında** bağlantı sayfası.  Ardından, **Hdınsight küme Panosu** bölmesinde Ambari kullanıcı arabirimini açın.  Küme oturum açma kimlik bilgileriniz istenir.

Hizmet görünümlerini listesini açmak için seçin **Ambari görünümleri** Hdınsight için Azure portal sayfasında bölmesi.  Bu liste, yüklediğiniz hangi kitaplıkların bağlı olarak değişir.  Örneğin, YARN sıra yöneticisinin, Hive görünümü ve Tez görünümü görebilirsiniz.  Yapılandırma ve hizmet bilgileri görmek için herhangi bir hizmet bağlantıyı seçin.  Ambari UI **yığını ve sürüm** sayfası, Küme Hizmetleri Yapılandırma ve hizmet sürüm geçmişi hakkında bilgi sağlar. Ambari UI bu bölüme gidin seçin **yönetici** menüsünü ve sonra **yığınları ve sürümleri**.  Seçin **sürümleri** hizmeti sürüm bilgisini görmek için sekme.

![Yığın ve sürümleri](./media/hdinsight-log-management/stack-versions.png)

Ambari kullanıcı arabirimini kullanarak, belirli bir ana bilgisayar (veya düğüm) kümede çalışan (veya tüm) Hizmetleri için yapılandırma indirebilirsiniz.  Seçin **ana** menüsü, ardından ilgi ana bilgisayar için bağlantı. Bu ana bilgisayarın sayfasında seçin **konak Eylemler** düğmesine ve ardından **karşıdan istemci yapılandırmalar**. 

![Ana bilgisayar istemci yapılandırmalar](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Betik Eylem günlükleri görüntüleme

Hdınsight [betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md) el ile veya ne zaman belirtilen bir kümede betiği çalıştırın. Örneğin, betik eylemleri kümede ek yazılım yüklemesi veya varsayılan değerlerden yapılandırma ayarlarını değiştirmek için kullanılabilir. Betik Eylem günlükleri Küme kurulumu sırasında oluşan hataların ve ayrıca küme performansını ve kullanılabilirliğini etkileyebilecek yapılandırma ayarlarını değişiklikleri fikirler sağlayabilir.  Betik eylemi durumunu görmek için seçin **ops** Ambari UI veya erişim durumu, varsayılan depolama hesabında oturum düğmesinde. Depolama günlüklerine kullanılabilir `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Adım 3: küme iş yürütme günlük dosyalarını yönetme

Sonraki adım, çeşitli hizmetler için iş yürütme günlük dosyalarını gözden geçirme.  Hizmetleri, Apache HBase, Apache Spark ve diğer birçok dahil olabilir. Hangi günlükler yararlı (ve hangi değil) belirleme zaman alıcı olabilir ayrıntılı günlükleri, çok sayıda Hadoop kümesi oluşturur.  Günlük sistemi anlama günlük dosyalarını hedeflenen Yönetim için önemlidir.  Bir örnek günlük dosyası verilmiştir.

![Hdınsight günlük dosyası örneği](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Hadoop günlük dosyalarını erişim

Hdınsight küme dosya sistemi hem de Azure depolama, günlük dosyalarını depolar. Küme için bir SSH bağlantısı açarak ve dosya sistemi gözatma veya uzak baş düğüm sunucuda Hadoop YARN durum Portalı'nı kullanarak, günlük dosyaları küme içindeki inceleyebilirsiniz. Azure depolama erişebilir ve verileri Azure depolama biriminden karşıdan yükleme araçları birini kullanarak günlük dosyalarında inceleyebilirsiniz. AZCopy, CloudXplorer ve Visual Studio Sunucu Gezgini örnektir. Azure blob depolama alanındaki verilere erişmek için PowerShell ve Azure Storage istemcisi kitaplıklarını veya Azure .NET SDK'ları kullanabilirsiniz.

Hadoop çalışan işleri olarak iş *görev denemeleri* çeşitli düğümleri üzerinde. Hdınsight, ilk tamamlamayın diğer görev girişimleri sonlandırma kurgusal görev denemeleri başlatabilirsiniz. Bu denetleyici, stderr ve syslog günlük dosyaları üzerinde-çalışma sırasında oturum önemli etkinlik oluşturur. Ayrıca, birden çok görev denemeleri eşzamanlı olarak çalıştığında, ancak bir günlük dosyası yalnızca sonuçlarını doğrusal olarak görüntüleyebilirsiniz.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Azure Blob depolama alanına yazılan Hdınsight günlükleri

Hdınsight kümeleri, Azure PowerShell cmdlet'lerini veya .NET iş gönderme API'leri kullanılarak gönderilen herhangi bir iş için bir Azure Blob Depolama hesabına görev günlükleri yazmak için yapılandırılır.  Kümeye SSH üzerinden iş gönderirseniz, sonra yürütme günlük kaydı bilgileri Azure tablolarda önceki bölümde açıklandığı gibi depolanır.

YARN de oluşturmak iş yürütme günlük dosyaları gibi hizmetleri Hdınsight tarafından oluşturulan çekirdek günlük dosyaları ek olarak, yüklü.  Sayısı ve türü günlük dosyalarının yüklenmiş tüm hizmetlere bağlıdır.  Ortak Hizmetleri Apache HBase, Apache Spark ve benzeri ' dir.  Kümenizde genel günlük dosyalarının yüklenebilir anlamak her hizmet için iş günlüğü yürütme dosyaları araştırın.  Her hizmet günlük dosyalarını depolamak için günlüğe kaydetme ve konumları, kendi benzersiz yöntemlerine sahiptir.  Örnek olarak, aşağıdaki bölümde en yaygın hizmeti günlük dosyalarını (YARN) erişmek için ayrıntıları ele alınmıştır.

### <a name="hdinsight-logs-generated-by-yarn"></a>YARN tarafından oluşturulan Hdınsight günlükleri

YARN günlüklerini bir alt düğüm üzerindeki tüm kapsayıcıları arasında toplar ve bu günlükleri çalışan düğümü başına bir toplu günlük dosyası olarak depolar. Bir uygulama bittikten sonra günlük varsayılan dosya sistemi üzerinde depolanır. Uygulamanızın yüzlerce veya binlerce kapsayıcıların kullanabilir, ancak günlükler tek alt düğüm üzerinde çalışan tüm kapsayıcıları için her zaman tek bir dosyaya toplanır. Uygulamanız tarafından kullanılan çalışan düğümü başına yalnızca tek bir günlük yoktur. Günlük toplama Hdınsight kümeleri sürüm 3.0 ve üstü varsayılan olarak etkindir. Toplanan günlükleri kümenin varsayılan depolama bulunur.

```
    /app-logs/<user>/logs/<applicationId>
```

Kapsayıcı tarafından dizine TFile ikili biçimde yazıldığı şekilde toplanmış günlükleri doğrudan okunabilir değildir. Bu günlükler uygulamalar veya ilgi kapsayıcıları için düz metin olarak görüntülemek için YARN ResourceManager günlükleri veya CLI araçlarını kullanın.

#### <a name="yarn-cli-tools"></a>YARN CLI araçlarını

YARN CLI araçlarını kullanmak için önce SSH kullanarak Hdınsight kümesine bağlanmanız gerekir. Belirtin `<applicationId>`, `<user-who-started-the-application>`, `<containerId>`, ve `<worker-node-address>` Bu komutlar çalıştırıldığında, bilgi. Aşağıdaki komutlardan birini düz metin olarak günlükleri görüntüleyebilirsiniz:

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi

YARN ResourceManager UI küme baş düğüm üzerinde çalışır ve Ambari web kullanıcı Arabirimi erişilir. YARN günlüklerini görüntülemek için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında gidin `https://CLUSTERNAME.azurehdinsight.net`. CLUSTERNAME Hdınsight kümenizin adıyla değiştirin.
2. Sol taraftaki hizmetler listesinden YARN seçin.
3. Hızlı bağlantılar aşağı açılır listeden, küme baş düğümler birini seçin ve ardından **ResourceManager günlükleri**. YARN günlüklerini bağlantılar listesiyle birlikte sunulur.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>4. adım: günlük birimi depolama boyutlarına ve maliyetleri tahmin

Önceki adımları tamamladıktan sonra bir anlayış türleri ve birimler, Hdınsight kümelere oluşturan günlük dosyalarının vardır.

Ardından, anahtar günlük depolama konumları günlük veri birimi bir süre boyunca çözümleyin. Örneğin, birim ve büyümesi üzerindeki 30-60-90 çözümleyebilirsiniz günlük dönemleri.  Bu bilgileri bir elektronik tabloda kaydedin veya Excel için Visual Studio, Azure Depolama Gezgini veya Power Query gibi diğer araçları kullanın. Daha fazla bilgi için bkz: [Hdınsight analiz günlükleri](hdinsight-debug-jobs.md).  

Artık anahtar günlükleri için bir günlük yönetimi stratejisi oluşturmak için yeterli bilgi var.  Her iki günlük boyut büyümesi tahmin ve ileride depolama Azure hizmet maliyetleri oturum, elektronik (veya tercih aracı) kullanın.  Ayrıca tüm günlük bekletme gereksinimleri İncelemekte olduğunuz günlükleri kümesi için göz önünde bulundurun.  Şimdi (varsa) hangi günlük dosyalarının silinebilir ve hangi günlükleri korunur ve gerekir daha ucuz Azure depolama alanına arşivlenmiş belirledikten sonra gelecekteki günlük depolama maliyetleri reforecast.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>5. adım: Günlük arşiv ilkelere ve süreçlere belirleme

Hangi günlük dosyalarının silinebilir belirledikten sonra günlük dosyalarının belirtilen bir süre sonra otomatik olarak silmek için birçok Hadoop Hizmetleri günlük parametreleri ayarlayabilirsiniz.

Belirli günlük dosyaları için bir yaklaşım arşivleme daha düşük fiyatlı günlük dosyası kullanabilirsiniz. Azure Resource Manager etkinlik günlükleri için Azure Portalı'nı kullanarak bu yaklaşım keşfedebilirsiniz.  Seçerek ARM günlüklerini arşivlerini ayarlama **etkinlik günlüğü**' Hdınsight Örneğiniz için Azure Portalı'nda bağlantı.  Etkinlik günlüğü arama sayfanın üst kısmında seçin **verme** açmak için menü öğesini **verme etkinlik günlüğü** bölmesi.  Abonelik, bölge, bir depolama hesabına dışarı aktarmak kullanılıp ve günlükleri korumak için kaç gün içinde doldurun. Bu aynı bölmesinde, ayrıca bir olay hub'ına dışarı aktarmak belirtebilirsiniz. 

![Günlük dosyaları dışarı aktarma](./media/hdinsight-log-management/archive.png)

Alternatif olarak, PowerShell ile günlük arşivleme komut dosyası oluşturabilirsiniz.  Örnek PowerShell komut dosyası için bkz [arşiv Azure Otomasyonu, Azure Blob depolama alanına oturum](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8).

### <a name="accessing-azure-storage-metrics"></a>Azure storage ölçümleri erişme

Azure depolama günlük depolama işlemleri ve erişim için yapılandırılabilir. Bu çok ayrıntılı günlükler izleme ve planlama kapasite ve depolama isteklerine denetlemek için kullanabilirsiniz. Günlüğe kaydedilen bilgileri izlemenizi ve çözümlerinizi performansını ince ayar etkinleştirme gecikmesi ayrıntılarını içerir.
Hdınsight kümesi için verilerini tutan Azure depolama için oluşturulan günlük dosyalarını incelemek için Hadoop için .NET SDK'yı kullanabilirsiniz.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Eski günlük dosyaları için yedekleme dizinleri sayısı ve boyutu denetleme

Korunur günlük dosyalarının sayısı ve boyutu denetlemek için aşağıdaki özellik kümesinin `RollingFileAppender`:

* `maxFileSize` Kritik üzerinde dosya alınıyor dosya boyutudur. Varsayılan değer 10 MB'tır.
* `maxBackupIndex` oluşturulmuş olması için varsayılan 1 yedekleme dosyalarının sayısını belirtir.

### <a name="other-log-management-techniques"></a>Diğer günlük yönetim teknikleri

Disk alanı yetersiz çalışmasını önlemek için bazı işletim sistemi araçları gibi kullanabilir `logrotate` günlük dosyalarının işleme yönetmek için. Yapılandırabileceğiniz `logrotate` günlük olarak çalıştırmak için sıkıştırma günlük dosyaları ve kaldırma eskilerle. Durum yaklaşımınızı gereksinimlerinize göre gibi ne kadar süreyle yerel düğümlerinde günlük dosyalarını tutmak için bağlıdır. 

Ayrıca, hangi günlük boyutu büyük ölçüde artırır hata ayıklama günlüğünü bir veya daha fazla hizmet için etkinleştirilip etkinleştirilmediğini kontrol edebilirsiniz. 

Merkezi bir konuma tüm düğümlerdeki günlükleri toplamak için Solr içine tüm günlük girişlerini alma gibi bir veri akışı oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ve Hdınsight için uygulama günlüğü](https://msdn.microsoft.com/library/dn749790.aspx)
* [Linux tabanlı Hdınsight üzerindeki erişim YARN uygulama günlüğü](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Çeşitli Hadoop bileşenleri için günlük dosyalarının boyutunu kontrol etme](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
