---
title: Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme
description: Azure portalını kullanarak HDInsight küme oluşturma ve yönetme hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: hrasheed
ms.openlocfilehash: c745fceca5efa66b1b23661001d93ddb287fe37b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460642"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak [Azure portalında](https://portal.azure.com), yönetebileceğiniz [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight kümeleri. Diğer araçları kullanarak HDInsight Hadoop kümelerini yönetme hakkında bilgi için yukarıdaki sekme seçicisini kullanın.

## <a name="prerequisites"></a>Önkoşullar

HDInsight, mevcut Apache Hadoop kümesi.  Bkz: [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).

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
    |Tags|Cloud services'ın özel bir sınıflandırma tanımlamak için anahtar/değer çiftleri ayarlamanıza olanak tanır. Örneğin, adında bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.|
    |Sorunları tanılayın ve çözün|Sorun giderme bilgilerini görüntüleyin.|
    |Hızlı başlangıç|Yardımcı olacak bilgileri görüntüler, HDInsight kullanmaya başlama.|
    |Araçlar|HDInsight için bilgiler yardımcı ilgili araçlar.|

  - **Ayarlar menüsü**  

    | Öğe| Açıklama |
    |---|---|
    |Küme boyutu|Denetleme, artırmak ve küme çalışan düğümü sayısını azaltın. Bkz: [ölçek kümeleri](hdinsight-administer-use-portal-linux.md#scale-clusters).|
    |Kota sınırları|Aboneliğiniz için kullanılan ve kullanılabilir çekirdek görüntüler.|
    |SSH + kümede oturum açma|Güvenli Kabuk (SSH) bağlantısı kullanarak kümeye bağlanmak için yönergeleri gösterir. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Data Lake Storage Gen1|Data Lake depolama Gen1 erişimi yapılandırın.  Bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).|
    |Depolama hesapları|Depolama hesapları ve anahtarları görüntüleyin. Depolama hesapları, küme oluşturma işlemi sırasında yapılandırılır.|
    |Uygulamalar|HDInsight uygulamaları ekleme/kaldırma.  Bkz: [özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).|
    |Betik eylemleri|Küme üzerinde Bash betiklerini çalıştırabilirsiniz. Bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).|
    |Dış meta depolar|Görünüm [Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/) meta depolar. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir.|
    |HDInsight iş ortağı|Geçerli HDInsight iş ortağı ekleme/kaldırma.|
    |Özellikler|Görünüm [küme özelliklerini](#properties).|
    |Kilitler|Kümenin silindi veya değiştirilmesini engellemek için bir kilit ekleyin.|
    |Şablonu dışarı aktarma|Görüntülemek ve küme için Azure Resource Manager şablonunu dışarı aktarın. Şu anda yalnızca bağımlı Azure depolama hesabına dışarı aktarabilirsiniz. Bkz: [oluşturma Linux tabanlı Apache Hadoop Azure Resource Manager şablonlarını kullanarak HDInsight kümelerinde](hdinsight-hadoop-create-linux-clusters-arm-templates.md).|

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
|ANA BİLGİSAYAR ADI|Küme adı.|
|KÜME URL'Sİ|Ambari web arabirimine URL'si.|
|Özel uç noktası|Küme için özel uç nokta.|
|Güvenli Kabuk (SSH)|SSH üzerinden kümeye erişirken kullanılacak kullanıcı adı ve ana bilgisayar adı.|
|DURUM|Biri: İptal, kabul ClusterStorageProvisioned AzureVMConfiguration, çalıştığından, silme, hata, HDInsightConfiguration, işletimsel, silinmiş, zaman aşımına uğradı, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, veya ClusterCustomization.|
|BÖLGE|Azure konumu. Desteklenen Azure konumları listesi için bkz. **bölge** aşağı açılan liste kutusunu [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).|
|OLUŞTURULMA TARİHİ|Bir tarih kümesi dağıtıldı.|
|İŞLETİM SİSTEMİ|Her iki **Windows** veya **Linux**.|
|TÜR|Hadoop, HBase, Storm, Spark.|
|Version|Bkz: [HDInsight sürümleri](hdinsight-component-versioning.md).|
|ABONELİK|Abonelik adı.|
|VARSAYILAN VERİ KAYNAĞI|Varsayılan Küme dosya sistemi.|
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

Bkz: [ölçek HDInsight kümeleri](./hdinsight-scaling-best-practices.md) eksiksiz bilgi.

## <a name="pauseshut-down-clusters"></a>Kümeleri duraklatma/Kapat

Hadoop işlerinin çoğu yalnızca zaman zaman çalışan toplu işler ' dir. Hadoop kümeleri için Küme işlemi için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programlama yapabileceğiniz birçok yolu vardır:

* Kullanıcı Azure veri fabrikası. Bkz: [oluşturma isteğe bağlı Linux tabanlı Apache Hadoop Azure Data Factory kullanarak HDInsight kümelerinde](hdinsight-hadoop-create-linux-clusters-adf.md) isteğe bağlı HDInsight'ı oluşturmak için bağlı hizmetler.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](./interactive-query/interactive-query-tutorial-analyze-flight-data.md).
* Azure CLI kullanın. Bkz: [yönetme Azure HDInsight kümeleri Azure CLI kullanarak](hdinsight-administer-use-command-line.md).
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
1. Küme kullanıcı adı ve parolayı girin.  Varsayılan Küme kullanıcı adı _yönetici_.

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
4. Gelen **betik eylemlerini** sayfasında **gönderme yeni**.
5. Gelen **betik eylemini Gönder** sayfasında, aşağıdaki bilgileri girin:

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

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme/kullanarak erişim verme [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Abonelik kimliği bulunamadı
Her kümenin bir Azure aboneliğine bağlıdır.  Kimliği görülebilir: Azure aboneliği [küme giriş sayfası](#homePage).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Resource Manager modunda bir Azure Resource Manager grubu ile her HDInsight kümesi oluşturulur. Resource Manager grubudur görülemediğinden [küme giriş sayfası](#homePage).

## <a name="find-the-storage-accounts"></a>Depolama hesaplarını bulma
HDInsight kümeleri verileri depolamak için bir Azure depolama hesabına veya Azure Data Lake Storage'ı kullanın. Bir varsayılan depolama hesabını ve bağlı depolama hesabı sayısı, her bir HDInsight kümesi olabilir. Gelen depolama hesaplarını listelemek için [küme giriş sayfası](#homePage) altında **ayarları**seçin **depolama hesapları**.

## <a name="monitor-jobs"></a>İşleri izleme
Bkz: [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="cluster-size"></a>Küme boyutu

**Küme boyutu** gelen döşeme [küme giriş sayfası](#homePage) bu küme ve bu küme içindeki düğümler için nasıl ayrılacağını ayrılan çekirdek sayısını görüntüler.

> [!IMPORTANT]  
> HDInsight kümesi tarafından sağlanan hizmetleri izlemek için Ambari Web veya Ambari REST API'sini kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Apache Ambari kullanarak HDInsight yönetme kümelerini](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Kümeye bağlanma

* [Apache Hive, HDInsight ile kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı temel yönetim işlevleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure PowerShell kullanarak HDInsight'ı yönetme](hdinsight-administer-use-powershell.md)
* [Azure CLI kullanarak HDInsight'ı yönetme](hdinsight-administer-use-command-line.md)
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Ambari REST API'SİNİN kullanımıyla ilgili ayrıntılar](hdinsight-hadoop-manage-ambari-rest-api.md)
* [HDInsight, Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight Apache Sqoop'u kullanma](hadoop/hdinsight-use-sqoop.md)
* [Apache Hive ve Apache Pig, HDInsight ile kullanmak Python kullanıcı tanımlı işlevler (UDF)](hadoop/python-udf-hdinsight.md)
* [Azure HDInsight Apache Hadoop hangi sürümünü mi?](hdinsight-component-versioning.md)