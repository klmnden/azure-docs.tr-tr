---
title: Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme
description: Azure portalını kullanarak HDInsight küme oluşturma ve yönetme hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: hrasheed
ms.openlocfilehash: 003aeadba1f4683af40f390d40dd3bbe32e02a83
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096375"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak [Azure portalında][azure-portal], yönetebileceğiniz [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight kümeleri. Diğer araçları kullanarak HDInsight Hadoop kümelerini yönetme hakkında bilgi için yukarıdaki sekme seçicisini kullanın.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- HDInsight, mevcut Apache Hadoop kümesi.  Bkz: [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="getting-started"></a>Başlarken
[https://portal.azure.com](https://portal.azure.com) adresinde oturum açın.

## <a name="showClusters"></a> Kümeleri Listele ve Göster
**HDInsight kümeleri** sayfası var olan kümeleri listeler.  Portaldan:
1. Seçin **tüm hizmetleri** sol menüden.
2. Seçin **HDInsight kümeleri** altında **ANALYTICS**.

## <a name="homePage"></a> Küme giriş sayfası 
Küme adınızı seçin [ **HDInsight kümeleri** ](#showClusters) sayfası.  Bu açılır **genel bakış** görünümü, aşağıdaki görüntüye benzer olacaktır:

![Azure portal HDInsight küme temel bileşenleri](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials2.png)

**Üst menü:**  

| Öğe| Açıklama |
|---|---|
|Taşı|Kümeyi, başka bir kaynak grubuna ya da başka bir aboneliğe taşınır.|
|Sil|Kümeyi siler. |
|Yenile|Görünümü yeniler.|

**Sol menü:**  
  - **Sol üst köşedeki menüyü**

    | Öğe| Açıklama |
    |---|---|
    |Genel Bakış|Kümeniz için genel bilgiler sağlar.|
    |Etkinlik günlüğü|Etkinlik günlükleri sorgulamak ve görüntüleyin.|
    |Erişim denetimi (IAM)|Rol atamalarını kullanın.  Bkz: [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md).|
    |Etiketler|Cloud services'ın özel bir sınıflandırma tanımlamak için anahtar/değer çiftleri ayarlamanıza olanak tanır. Örneğin, adında bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.|
    |Sorunları tanılama ve çözme|Sorun giderme bilgilerini görüntüleyin.|
    |Hızlı Başlangıç|Yardımcı olacak bilgileri görüntüler, HDInsight kullanmaya başlama.|
    |Araçlar|HDInsight için bilgiler yardımcı ilgili araçlar.|

  - **Ayarlar menüsü**  

    | Öğe| Açıklama |
    |---|---|
    |Küme boyutu|Denetleme, artırmak ve küme çalışan düğümü sayısını azaltın. Bkz: [ölçek kümeleri](hdinsight-administer-use-portal-linux.md#scale-clusters).|
    |Kota sınırları|Aboneliğiniz için kullanılan ve kullanılabilir çekirdek görüntüler.|
    |SSH + Kümede oturum açma|Güvenli Kabuk (SSH) bağlantısı kullanarak kümeye bağlanmak için yönergeleri gösterir. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Data Lake Storage Gen1|Data Lake depolama Gen1 erişimi yapılandırın.  Bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).|
    |Depolama hesapları|Depolama hesapları ve anahtarları görüntüleyin. Depolama hesapları, küme oluşturma işlemi sırasında yapılandırılır.|
    |Uygulamalar|HDInsight uygulamaları ekleme/kaldırma.  Bkz: [özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).|
    |Betik eylemleri|Küme üzerinde Bash betiklerini çalıştırabilirsiniz. Bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).|
    |Dış meta depolar|Görünüm [Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/) meta depolar. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir.|
    |HDInsight iş ortağı|Geçerli HDInsight iş ortağı ekleme/kaldırma.|
    |Özellikler|Görünüm [küme özelliklerini](#properties).|
    |Kilitler|Kümenin silindi veya değiştirilmesini engellemek için bir kilit ekleyin.|
    |Otomasyon betiği|Görüntülemek ve küme için Azure Resource Manager şablonunu dışarı aktarın. Şu anda yalnızca bağımlı Azure depolama hesabına dışarı aktarabilirsiniz. Bkz: [oluşturma Linux tabanlı Apache Hadoop Azure Resource Manager şablonlarını kullanarak HDInsight kümelerinde](hdinsight-hadoop-create-linux-clusters-arm-templates.md).|

  - **Menü izleme**

    | Öğe| Açıklama |
    |---|---|
    |Uyarılar|Uyarıları ve eylemleri yönetme.|
    |Ölçümler|Azure İzleyici günlüklerine küme ölçümlerini izleyin.|
    |Tanılama ayarları|Tanılama ölçümlerini depolanacağı ayarları.|
    |Operations Management Suite|Kümenizi Azure Operations Management Suite (OMS) ve Azure İzleyici günlüklerine izleyin.|

  - **Destek + sorun giderme menüsü**

    | Öğe| Açıklama |
    |---|---|
    |Kaynak durumu|Bkz: [Azure kaynak durumu genel bakış](../service-health/resource-health-overview.md).|
    |Yeni destek isteği|Microsoft desteği ile bir destek bileti oluşturmanızı sağlar.|

## <a name="properties"></a> Küme Özellikleri

Gelen [küme giriş sayfası](#homePage)altında **ayarları** seçin **özellikleri**.

|Öğe | Açıklama |
|---|---|
|Ana Bilgisayar Adı|Küme adı.|
|Küme URL'si|Ambari web arabirimine URL'si.|
|Güvenli Kabuk (SSH)|SSH üzerinden kümeye erişirken kullanılacak kullanıcı adı ve ana bilgisayar adı.|
|Durum|Biri: İptal, kabul ClusterStorageProvisioned AzureVMConfiguration, çalıştığından, silme, hata, HDInsightConfiguration, işletimsel, silinmiş, zaman aşımına uğradı, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, veya ClusterCustomization.|
|Bölge|Azure konumu. Desteklenen Azure konumları listesi için bkz. **bölge** aşağı açılan liste kutusunu [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).|
|Oluşturma tarihi|Bir tarih kümesi dağıtıldı.|
|İşletim sistemi|Her iki **Windows** veya **Linux**.|
|Tür|Hadoop, HBase, Storm, Spark.|
|Version|Bkz: [HDInsight sürümleri](hdinsight-component-versioning.md).|
|Abonelik|Abonelik adı.|
|Varsayılan veri kaynağı|Varsayılan Küme dosya sistemi.|
|Çalışan düğümlerinin boyutu|Çalışan düğümlerinin seçilen sanal makine boyutu.|
|Baş düğüm boyutu|Baş düğümler seçilen sanal makine boyutu.|
|Sanal ağ|Sanal bir dağıtım sırasında seçildiyse, kümeye dağıtılan, ağ adı.|

## <a name="move-clusters"></a>Kümeleri Taşı

Bir HDInsight kümesi, başka bir Azure kaynak grubu veya başka bir aboneliğe taşıyabilirsiniz.

Gelen [küme giriş sayfası](#homePage):

1. Seçin **taşıma** üstteki menüden.
2. Seçin **başka bir kaynak grubuna taşımak** veya **taşımak, başka bir aboneliğe**.
3. Yeni sayfadaki yönergeleri izleyin.

## <a name="delete-clusters"></a>Kümeleri Sil
Küme silme ya da bağlı tüm depolama hesaplarını varsayılan depolama hesabını silmez. Kümenin aynı depolama hesapları ve aynı meta depolarla kullanarak yeniden oluşturabilirsiniz. Kümeyi yeniden oluşturduğunuzda yeni bir varsayılan Blob kapsayıcısını kullanmanızı öneririz.

Gelen [küme giriş sayfası](#homePage):

1. Seçin **Sil** üstteki menüden.
2. Yeni sayfadaki yönergeleri izleyin.

Ayrıca bkz: [duraklatma/kümeleri kapatma](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Bir küme oluşturulduktan sonra ek Azure depolama hesapları ve Azure Data Lake Storage hesapları ekleyebilirsiniz. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme kümesi küme yeniden oluşturmak zorunda kalmadan bir Azure HDInsight kümesi tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]  
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Listesini görmek ve kümelerini gösterir.

Gelen [küme giriş sayfası](#homePage):

1. Altında **ayarları**seçin **küme boyutu**.
2. Girin **numarası, çalışan düğümleri** sayısal metin kutusuna. Küme düğümleri sayısı Azure abonelikleri arasında değişiklik gösterir. Sınırı artırmak için fatura desteğine başvurabilirsiniz.  Maliyet bilgileri, düğüm sayısını yapmış olduğunuz değişiklikleri yansıtır.
3. **Kaydet**’i seçin.

    ![HDInsight, hadoop, hbase, storm, spark, Ölçek](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster2.png)

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi değişir:

* Apache Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Bu davranış tüm çalışan ve farklı bekleyen işleri ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* Apache HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, küme baş düğümüne oturum açma ve bir komut istemi penceresinden aşağıdaki komutları çalıştırmadan tarafından el ile bölgesel sunucuları dengeleyebilirsiniz:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz. [HDInsight, Apache HBase örneğiyle çalışmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topolojiyi yeniden dengelemek gerekecektir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgeleri](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight Storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Bir örnek Storm topolojiyi yeniden dengelemek için CLI komutu aşağıda verilmiştir:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```


## <a name="pauseshut-down-clusters"></a>Kümeleri duraklatma/Kapat

Hadoop işlerinin çoğu yalnızca zaman zaman çalışan toplu işler ' dir. Hadoop kümeleri için Küme işlemi için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programlama yapabileceğiniz birçok yolu vardır:

* Kullanıcı Azure veri fabrikası. Bkz: [oluşturma isteğe bağlı Linux tabanlı Apache Hadoop Azure Data Factory kullanarak HDInsight kümelerinde](hdinsight-hadoop-create-linux-clusters-adf.md) isteğe bağlı HDInsight'ı oluşturmak için bağlı hizmetler.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data-linux.md).
* Klasik Azure CLI'yi kullanın. Bkz: [yönetme HDInsight kümeleri Klasik Azure CLI kullanarak](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK'sını kullanın. Bkz: [gönderme Apache Hadoop işlerini](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Fiyatlandırma bilgileri için bkz. [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/). Portaldan bir kümeyi silmek için bkz: [küme silme](#delete-clusters)



## <a name="upgrade-clusters"></a>Küme yükseltme

Bkz: [daha yeni bir sürüme yükseltme HDInsight küme](./hdinsight-upgrade-cluster.md).

## <a name="open-the-apache-ambari-web-ui"></a>Apache Ambari web kullanıcı arabirimini açın

Ambari bir sezgisel, kullanımı kolay Hadoop Yönetim web kullanıcı Arabirimi, RESTful API'ları tarafından desteklenen sağlar. Ambari, sistem yöneticilerinin yönetme ve Hadoop kümelerini izleme sağlar.

Gelen [küme giriş sayfası](#homePage):

1. Seçin **küme panoları**.

    ![HDInsight Hadoop kümesi menüsü](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu2.png)

1. Seçin **Ambari giriş** yeni sayfasından.
2. Küme kullanıcı adı ve parolayı girin.  Varsayılan Küme kullanıcı adı _yönetici_. Ambari web kullanıcı Arabirimi gibi görünür:

Daha fazla bilgi için [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Parolaları değiştirme
Bir HDInsight kümesi iki kullanıcı hesabı içerebilir. Oluşturma işlemi sırasında HDInsight küme kullanıcı hesabı (HTTP kullanıcı hesabı) ve SSH kullanıcı hesabı oluşturulur. SSH kullanıcı hesabı değiştirmek için betik eylemleri ve küme kullanıcı hesabı parolasını değiştirmek için portalı kullanabilirsiniz.

### <a name="change-the-cluster-user-password"></a>Küme kullanıcı parolasını değiştirme

> [!NOTE]  
> Küme kullanıcı (Yönetici) parolası değiştirme başarısız olmasına karşı bu küme çalışacak bir betik eylemleri neden olabilir. Kalıcı betik eylemleri, hedef alt düğümleri varsa, bu betikleri aracılığıyla küme düğümlerine operations yeniden boyutlandırma eklediğinizde başarısız olabilir. Betik eylemleri hakkında daha fazla bilgi için bkz. [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

Gelen [küme giriş sayfası](#homePage):
1. Seçin **SSH + küme oturum açma** altında **ayarları**.
2. Seçin **sıfırlama kimlik bilgisi**.
3. Girin ve metin kutularını yeni parolayı onaylayın.
4. **Tamam**’ı seçin.

Kümedeki tüm düğümlerde parolası değiştirildi.

### <a name="change-the-ssh-user-password"></a>SSH kullanıcı parolasını değiştirme
1. Bir metin düzenleyicisi kullanarak, aşağıdaki metni adlı bir dosya Kaydet **changepassword.sh**.

    > [!IMPORTANT]  
    > Satır sonu olarak LF kullanan bir düzenleyici kullanmanız gerekir. Düzenleyici CRLF kullanıyorsa, komut çalışmaz.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Bir HTTP veya HTTPS adresi kullanarak HDInsight ' erişilebilen bir depolama konumuna dosya yükleyin. Örneğin, genel dosya OneDrive veya Azure Blob Depolama gibi depolayın. Bu URI sonraki adımda gerektiğinde URI (HTTP veya HTTPS adresi) dosyasına kaydedin.
3. Gelen [küme giriş sayfası](#homePage)seçin **betik eylemlerini** altında **ayarları**.
4. Gelen **betik eylemleri** dikey penceresinde **yeni Gönder**. 
5. Gelen **betik eylemini Gönder** dikey penceresinde aşağıdaki bilgileri girin:

   | Alan | Değer |
   | --- | --- |
   | Betik türü | Seçin **- özel** aşağı açılan listeden.|
   | Ad |"Ssh Parola Değiştir" |
   | Bash betiği URI'si |Changepassword.sh dosya URI'si |
   | Düğüm türleri: (Head, çalışan, Nimbus, Gözetmen, Zookeeper, vb.) |✓ listelenen tüm düğüm türleri |
   | Parametreler |SSH kullanıcı adı ve yeni parolayı girin. Kullanıcı adı ve parola arasına bir boşluk olması gerekir. |
   | Bu betik eylemini Sürdür... |Bu alan işaretli bırakın. |

6. Seçin **Oluştur** betik uygulamak için. Betik bittikten sonra yeni bir parola ile SSH kullanarak kümeye bağlanabilir.

## <a name="grantrevoke-access"></a>GRANT/revoke-access
HDInsight kümeleri aşağıdaki HTTP web Hizmetleri (Bu hizmetlerin tümü, RESTful uç noktalarına sahip) sahip:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton da

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme/kullanarak erişim verme [Klasik Azure CLI'yı](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) ve [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Abonelik kimliği bulunamadı
Her kümenin bir Azure aboneliğine bağlıdır.  Kimliği görülebilir: Azure aboneliği [küme giriş sayfası](#homePage).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Resource Manager modunda bir Azure Resource Manager grubu ile her HDInsight kümesi oluşturulur. Resource Manager grubudur görülemediğinden [küme giriş sayfası](#homePage).

## <a name="find-the-storage-accounts"></a>Depolama hesaplarını bulma
HDInsight kümeleri verileri depolamak için bir Azure depolama hesabına veya Azure Data Lake Storage'ı kullanın. Bir varsayılan depolama hesabını ve bağlı depolama hesabı sayısı, her bir HDInsight kümesi olabilir. Gelen depolama hesaplarını listelemek için [küme giriş sayfası](#homePage) altında **ayarları**seçin **depolama hesapları**.


## <a name="monitor-jobs"></a>İşleri izleme
Bkz: [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md#monitoring).


## <a name="monitor-cluster-usage"></a>Küme kullanımı izleme
**Kullanım** HDInsight küme dikey penceresinde bölümünü nasıl ayrılacağını ve bu küme için ayrılmış çekirdek sayısının yanı sıra HDInsight ile kullanmak için aboneliğinizi kullanılabilir çekirdek sayısı hakkında daha fazla bilgi görüntüler Bu küme içindeki düğümler için. Listesini görmek ve kümelerini gösterir.

> [!IMPORTANT]  
> HDInsight kümesi tarafından sağlanan hizmetleri izlemek için Ambari Web veya Ambari REST API'sini kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Apache Ambari kullanarak HDInsight yönetme kümelerini](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Kümeye bağlanma

* [Apache Hive, HDInsight ile kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı temel yönetim işlevleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure PowerShell kullanarak HDInsight'ı yönetme](hdinsight-administer-use-powershell.md)
* [Klasik Azure CLI kullanarak HDInsight'ı yönetme](hdinsight-administer-use-command-line.md)
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Ambari Web kullanıcı arabirimini kullanma hakkında daha fazla bilgi edinin](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API'SİNİN kullanımıyla ilgili ayrıntılar](hdinsight-hadoop-manage-ambari-rest-api.md)
* [HDInsight, Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight Apache Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight Apache Sqoop'u kullanma](hadoop/hdinsight-use-sqoop.md)
* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight Apache Hadoop hangi sürümünü mi?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop komut satırı"
