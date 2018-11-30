---
title: HDInsight Azure portalını kullanarak Windows tabanlı Apache Hadoop kümelerini yönetme
description: HDInsight hizmeti yönetmeyi öğrenin. Bir HDInsight kümesi oluşturma, etkileşimli JavaScript Konsolu'nu açın ve Apache Hadoop komut konsolunu açın.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 378f52f0418c8c99e9ce6ca393ca10a77504698d
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499592"
---
# <a name="manage-windows-based-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight Windows tabanlı Apache Hadoop kümelerini yönetme

Kullanarak [Azure portalında][azure-portal], Windows tabanlı oluşturabilirsiniz [Apache Hadoop](https://hadoop.apache.org/) kümeleri Azure HDInsight, Hadoop kullanıcı parolası değiştirme ve Uzak Masaüstü Protokolü (etkinleştir) Küme Hadoop komut konsolunda erişebilmesi için RDP).

Bu makaledeki bilgiler, yalnızca pencere tabanlı HDInsight kümeleri için geçerlidir. Linux tabanlı kümeler yönetme hakkında daha fazla bilgi için bkz. [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](hdinsight-administer-use-portal-linux.md).

[!INCLUDE [windows-retirement-notice](../../includes/windows-retirement-notice.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure depolama hesabı** -bir HDInsight kümesi, bir Azure Blob Depolama kapsayıcısı varsayılan dosya sistemi olarak kullanır. Azure Blob storage HDInsight kümeleri ile sorunsuz bir deneyimi nasıl sağladığını hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Blob Depolama kullanma](hdinsight-hadoop-use-blob-storage.md). Bir Azure depolama hesabı oluşturma hakkında daha fazla bilgi edinmek için bkz. [bir depolama hesabının nasıl oluşturulacağını](../storage/common/storage-create-storage-account.md).

## <a name="open-the-portal"></a>Portalını açın
1. [https://portal.azure.com](https://portal.azure.com) adresinde oturum açın.
2. Portal açtıktan sonra şunları yapabilirsiniz:

   * Tıklayın **kaynak Oluştur** sol menüden yeni bir küme oluşturmak için:

       ![Yeni HDInsight kümesi düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Tıklayın **HDInsight kümeleri** sol menüden.

       ![Azure portal HDInsight kümesi düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Varsa **HDInsight** değil, sol taraftaki menüde görünmesini tıklayın **Gözat**.

     ![Azure portal Gözat kümesi düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Küme oluşturma
Portalı kullanarak oluşturma yönergeleri için bkz: [oluşturma HDInsight kümeleri](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight, geniş aralığı Apache Hadoop bileşenleri ile çalışır. Doğrulandı ve desteklenen bileşenlerin listesi için bkz [hangi sürümüdür Azure HDInsight Apache hadoop'un](hdinsight-component-versioning.md). Aşağıdaki seçeneklerden birini kullanarak HDInsight özelleştirebilirsiniz:

* Bir kümenin küme yapılandırmasını değiştirin veya Giraph veya Solr gibi özel bileşenleri yüklemek için özelleştirebileceğiniz özel komut dosyaları çalıştırmak için betik eylemi kullanın. Daha fazla bilgi için [betik eylemi kullanarak özelleştirme HDInsight küme](hdinsight-hadoop-customize-cluster.md).
* Küme oluşturma sırasında küme özelleştirme parametreleri Azure PowerShell veya HDInsight .NET SDK'sını kullanın. Bu yapılandırma değişikliklerini sonra küme kullanım ömrü korunur ve gerçekleştiren Azure platformu düzenli bakım için küme düğümün görüntüsünü yeniden oluşturur etkilenmez. Küme özelleştirmesi parametreleri kullanma hakkında daha fazla bilgi için bkz. [oluşturma HDInsight kümeleri](hdinsight-hadoop-provision-linux-clusters.md).
* İster yerel bazı Java bileşenlerini [Apache Mahout](https://mahout.apache.org/) ve [basamaklama](https://www.cascading.org/), küme üzerinde JAR dosyaları olarak çalıştırılabilir. Bu JAR dosyaları Azure Blob depolama alanına dağıtılmış ve HDInsight kümelerine Hadoop işi gönderme mekanizmalar aracılığıyla gönderildi. Daha fazla bilgi için [gönderme Apache Hadoop işlerini program aracılığıyla](hadoop/submit-apache-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > HDInsight kümelerinde JAR dosyaları ile iletişime geçin veya JAR dosyalarını HDInsight kümelerine dağıtma sorunları varsa [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Geçişli HDInsight tarafından desteklenmiyor ve Microsoft Support uygun değil. Desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan küme sürümlerindeki yenilikler](hdinsight-component-versioning.md).
  >
  >

Uzak Masaüstü bağlantısı kullanarak küme üzerinde özel yazılım yüklemesi desteklenmez. Kümeler yeniden oluşturmanız gerekiyorsa, bunlar kaybolacak gibi tüm baş düğümün sürücülerindeki dosyaları depolamak kaçınmanız gerekir. Dosyaların Azure Blob Depolama'da depolanması önerilir. BLOB Depolama alanı kalıcıdır.

## <a name="list-and-show-clusters"></a>Kümeleri Listele ve Göster
1. [https://portal.azure.com](https://portal.azure.com) adresinde oturum açın.
2. Tıklayın **HDInsight kümeleri** sol menüden.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmındaki filtre kullanabilirsiniz.
4. Ayrıntıları göstermek için listedeki bir kümeden çift tıklayın.

    **Menü ve essentials**:

    ![Azure portal HDInsight küme temel bileşenleri](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Menüsünü özelleştirmek için menüde herhangi bir yere sağ tıklayın ve ardından **Özelleştir**.
   * **Ayarları** ve **tüm ayarlar**: görüntüler **ayarları** dikey penceresinde küme, küme için ayrıntılı yapılandırma bilgilerini erişmenize olanak sağlar.
   * **Pano**, **küme Panosu** ve **URL**: Linux tabanlı kümeler için Ambari Web olan küme panosuna erişmek için tüm yolları şunlardır.
   * **Secure Shell**: kümeye Secure Shell (SSH) bağlantısı kullanarak bağlanmak için yönergeleri gösterir.
   * **Küme ölçeklendirme**: Bu küme için alt düğüm sayısını değiştirmenize izin verir.
   * **Silme**: kümeyi siler.
   * **Hızlı Başlangıç**: yardımcı olacak bilgileri görüntüler, HDInsight kullanmaya başlama.
   * **Kullanıcılar**: izinlerini ayarlamanızı sağlar *portal Yönetim* Azure aboneliğinizde diğer kullanıcılar için bu kümenin.

     > [!IMPORTANT]
     > Bu *yalnızca* erişim ve izinleri Azure portalında bu kümeye etkiler ve kim bağlanın veya HDInsight kümesine göndermek hiçbir etkisi olmaz.
     >
     >
   * **Etiketleri**: etiketler cloud services'ın özel bir sınıflandırma tanımlamak için anahtar/değer çiftleri kümesi izin verir. Örneğin, adında bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.
   * **Ambari görünümleri**: Ambari Web bağlanır.

     > [!IMPORTANT]
     > HDInsight kümesi tarafından sağlanan hizmetleri yönetmek için Ambari Web veya Ambari REST API'sini kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Apache Ambari kullanarak HDInsight yönetme kümelerini](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Kullanım**:

     ![Azure portal HDInsight kümesi kullanımı](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Tıklayın **ayarları**.

    ![Azure portal HDInsight kümesi kullanımı](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Özellikler**: küme özelliklerini görüntüleyin.
   * **Küme, AAD kimlik**:
   * **Azure depolama anahtarları**: varsayılan depolama hesabını ve anahtarını görüntüleyin. Depolama hesabı küme oluşturma işlemi sırasında bir yapılandırmadır.
   * **Küme oturum açma**: HTTP küme kullanıcı adı ve parolasını değiştirin.
   * **Dış meta depolar**: Görünüm [Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/) meta depolar. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir.
   * **Küme ölçeklendirme**: artırma ve azaltma küme çalışan düğümü sayısı.
   * **Uzak Masaüstü**: etkinleştirme ve Uzak Masaüstü (RDP) erişimi devre dışı bırakın ve RDP kullanıcı adını yapılandırın.  RDP kullanıcı adını HTTP kullanıcı adından farklı olmalıdır.
   * **İş ortağı**:

     > [!NOTE]
     > Genel bir kullanılabilir ayarların listesi budur; Bunların hepsi, tüm küme türleri için mevcut olacaktır.
     >
     >
6. Tıklayın **özellikleri**:

    Özellikler bölümü aşağıda listelenmiştir:

   * **Ana bilgisayar adı**: küme adı.
   * **Küme URL**.
   * **Durum**: dahil, kabul iptal ClusterStorageProvisioned AzureVMConfiguration, çalıştığından, silme, hata, HDInsightConfiguration, işletimsel, silinmiş, zaman aşımına uğradı, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Bölge**: Azure konumu. Desteklenen Azure konumları listesi için bkz. **bölge** açılan liste kutusunu [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Oluşturulan veri**.
   * **İşletim sistemi**: ya da **Windows** veya **Linux**.
   * **Tür**: Hadoop, HBase, Storm, Spark.
   * **Sürüm**. Bkz: [HDInsight sürümleri](hdinsight-component-versioning.md)
   * **Abonelik**: Abonelik adı.
   * **Abonelik kimliği**.
   * **Birincil veri kaynağına**. Azure Blob Depolama hesabı, Hadoop dosya sistemi varsayılan olarak kullanılır.
   * **Çalışan düğümlerinin fiyatlandırma katmanını**.
   * **Baş düğüm fiyatlandırma katmanı**.

## <a name="delete-clusters"></a>Kümeleri Sil
Küme silme, varsayılan depolama hesabı veya bağlı tüm depolama hesaplarını silinmez. Kümenin aynı depolama hesapları ve aynı meta depolarla kullanarak yeniden oluşturabilirsiniz.

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **Sil** üstteki menüden ve ardından yönergeleri izleyin.

Ayrıca bkz: [duraklatma/kümeleri kapatma](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme kümesi Azure HDInsight kümesini yeniden oluşturmak zorunda kalmadan çalışan bir küme tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).
>
>

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi:

* Apache Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Bu tüm çalışan ve farklı bekleyen işleri ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* Apache HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, küme baş düğümüne günlüğe kaydetme ve bir komut istemi penceresinden aşağıdaki komutları çalıştırmadan tarafından el ile bölgesel sunucuları dengeleyebilirsiniz:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz]
* Apache Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Apache Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Lütfen [Apache Storm belgeleri](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Storm topolojiyi yeniden dengelemek için CLI komutunu kullanmak nasıl bir örnek aşağıdadır:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Kümeleri ölçeklendirme**

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **ayarları** üst menü ve ardından **ölçek kümesi**.
4. Girin **numarası, çalışan düğümleri**. Küme düğümü sayısını Azure abonelikleri arasında değişiklik gösterir. Sınırı artırmak için fatura desteğine başvurabilirsiniz.  Maliyet bilgileri, düğüm sayısını yapmış olduğunuz değişiklikleri yansıtır.

    ![HDInsight Hadoop HBase, Storm Spark ölçek](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Kümeleri duraklatma/Kapat
Hadoop işlerinin çoğu zaman zaman yalnızca batch işleri çalıştıran ' dir. Hadoop kümeleri için Küme işlemi için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programlama yapabileceğiniz birçok yolu vardır:

* Kullanıcı Azure veri fabrikası. Bkz: [Azure HDInsight bağlı hizmeti](../data-factory/compute-linked-services.md) ve [dönüştürmek ve Azure Data Factory kullanarak çözümlemek](../data-factory/transform-data.md) Hizmetleri için isteğe bağlı hem de şirket içinde tanımlanan HDInsight bağlı.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md).
* Klasik Azure CLI'yi kullanın. Bkz: [yönetme HDInsight kümeleri Klasik Azure CLI kullanarak](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK'sını kullanın. Bkz: [gönderme Apache Hadoop işlerini](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Fiyatlandırma bilgileri için bkz. [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/). Portaldan bir kümeyi silmek için bkz: [küme silme](#delete-clusters)

## <a name="change-cluster-username"></a>Küme kullanıcı adını değiştir
Bir HDInsight kümesi iki kullanıcı hesabı içerebilir. Oluşturma işlemi sırasında HDInsight küme kullanıcı hesabı oluşturulur. RDP aracılığıyla kümesine erişmek için bir RDP kullanıcı hesabı da oluşturabilirsiniz. Bkz: [Uzak Masaüstü'nü etkinleştirme](#connect-to-hdinsight-clusters-by-using-rdp).

**HDInsight küme kullanıcı adı ve parolayı değiştirmek için**

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **ayarları** üst menü ve ardından **küme oturum açma**.
4. Varsa **küme oturum açma** olmuştur etkin tıklamanız **devre dışı**ve ardından **etkinleştirme** kullanıcı adı ve parola değiştirmeden önce...
5. Değişiklik **küme oturum açma adı** ve/veya **küme oturum açma parolası**ve ardından **Kaydet**.

    ![HDInsight, küme, kullanıcı, kullanıcı adı, parola, http, kullanıcı değiştirme](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>GRANT/revoke-access
HDInsight kümeleri aşağıdaki HTTP web Hizmetleri (Bu hizmetlerin tümü, RESTful uç noktalarına sahip) sahip:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton da

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme / erişim Azure Portalı'ndan sağlayıcısını.

> [!NOTE]
> Verme/erişimini iptal ederek, küme kullanıcı adını ve parolasını sıfırlar.
>
>

**HTTP web Hizmetleri erişim vermek/iptal etmek için**

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **ayarları** üst menü ve ardından **küme oturum açma**.
4. Varsa **küme oturum açma** olmuştur etkin tıklamanız **devre dışı**ve ardından **etkinleştirme** kullanıcı adı ve parola değiştirmeden önce...
5. İçin **küme oturum açma kullanıcı** ve **küme oturum açma parolası**, küme için yeni kullanıcı adını ve parolasını (sırasıyla) girin.
6. **KAYDET**'e tıklayın.

    ![HDInsight genel http web hizmeti erişimi Kaldır](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Her HDInsight kümenin varsayılan depolama hesabı vardır. Varsayılan depolama hesabı ve küme için kendi anahtarları altında görünür **ayarları**/**özellikleri**/**Azure depolama anahtarları**. Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Resource Manager modunda her HDInsight kümesi ile bir Azure kaynak grubu oluşturulur. Bir kümeye ait Azure kaynak grubu görünür:

* Küme listesi olan bir **kaynak grubu** sütun.
* Küme **temel** Döşe.  

Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>HDInsight sorgu konsolu aç
HDInsight sorgu Konsolu aşağıdaki özellikleri içerir:

* **Hive Düzenleyicisi**: Hive işlerini göndermeye yönelik bir GUI web arabirimi.  Bkz: [sorgu Konsolu kullanarak Apache Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-query-console.md).

    ![HDInsight portal hive Düzenleyicisi](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **İş Geçmişi**: İzleyici Hadoop işlerini.  

    ![HDInsight portal iş geçmişi](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Tıklayın **sorgu adı** işi özellikleri dahil olmak üzere ayrıntıları göstermek için **iş sorgusu**, ve ** iş çıktısı. Sorgu hem de çıkış istasyonunuza indirebilirsiniz.
* **Tarayıcı dosyası**: varsayılan depolama hesabını ve bağlı depolama hesaplarına göz atın.

    ![HDInsight portal dosya tarayıcı Gözat](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Ekran üzerinde **<Account>** öğesi olup Azure depolama hesabı türü gösterir.  Dosyalara göz atmak için hesap adına tıklayın.
* **Hadoop UI**.

    ![HDInsight portalı Hadoop kullanıcı Arabirimi](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Gelen **Hadoop UI*, dosyalara göz atın ve günlüklerini kontrol edin.
* **Yarn UI**.

    ![HDInsight portalı YARN UI](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Hive sorguları çalıştırma
Portaldan Hive işlerini çalıştırmak için tıklayın **Hive Düzenleyicisi** HDInsight sorgu konsoluna. Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>İşleri izleme
Portaldan işleri izlemek için tıklayın **iş geçmişi** HDInsight sorgu konsoluna. Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

## <a name="browse-files"></a>Dosyalara göz atın
Varsayılan depolama hesabını ve bağlı depolama hesaplarına depolanan dosyalara göz atmak için tıklatın **dosya tarayıcı** HDInsight sorgu konsoluna. Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

Ayrıca **dosya sistemine Gözat** yardımcı programı'ndan **Hadoop UI** HDInsight konsolunda.  Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Küme kullanımı izleme
**Kullanım** HDInsight küme dikey penceresinde bölümünü nasıl ayrılacağını ve bu küme için ayrılmış çekirdek sayısının yanı sıra HDInsight ile kullanmak için aboneliğinizi kullanılabilir çekirdek sayısı hakkında daha fazla bilgi görüntüler Bu küme içindeki düğümler için. Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

> [!IMPORTANT]
> HDInsight kümesi tarafından sağlanan hizmetleri izlemek için Ambari Web veya Ambari REST API'sini kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Apache Ambari kullanarak HDInsight yönetme kümelerini](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Hadoop kullanıcı arabirimini açın
Kümesini izlemek için dosya sistemine Gözat ve günlükleri denetleme, tıklatın **Hadoop UI** HDInsight sorgu konsoluna. Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Yarn UI'ı açın
Yarn kullanıcı arabiriminde kullanmak için **Yarn UI** HDInsight sorgu konsoluna. Bkz: [açık HDInsight sorgu Konsolu](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>RDP kullanarak kümeye bağlanma
Kümenin, oluşturma sırasında sağladığınız kimlik bilgileri, kümedeki hizmetlerin, ancak Uzak Masaüstü aracılığıyla kümenin kendisi için erişim verin. Küme sağlama veya bir küme sağlandıktan sonra Uzak Masaüstü erişimi'ni kapatabilirsiniz. Oluşturma sırasında Uzak Masaüstü'nü etkinleştirme hakkında yönergeler için bkz. [oluşturma HDInsight küme](hdinsight-hadoop-provision-linux-clusters.md).

**Uzak Masaüstü'nü etkinleştirmek için**

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **ayarları** üst menü ve ardından **Uzak Masaüstü**.
4. Girin **sonu**, **Uzak Masaüstü kullanıcı adı** ve **Uzak Masaüstü parola**ve ardından **etkinleştirme**.

    ![HDInsight etkinleştirme devre dışı bırakma, Uzak Masaüstü'nü yapılandırma](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    Bir hafta sonu için varsayılan değerleri var.

   > [!NOTE]
   > HDInsight .NET SDK'sı, bir kümede uzak masaüstünü etkinleştirmek için de kullanabilirsiniz. Kullanım **EnableRdp** aşağıdaki şekilde HDInsight istemci nesnesi üzerinde yöntemi: **istemci. EnableRdp (küme adı, konumu, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Benzer şekilde, küme üzerinde Uzak Masaüstü'nü devre dışı bırakmak için kullanabileceğiniz **istemci. (Küme adı, konumu) DisableRdp**. Bu yöntemler hakkında daha fazla bilgi için bkz. [HDInsight .NET SDK başvurusu](https://go.microsoft.com/fwlink/?LinkId=529017). Bu, yalnızca Windows üzerinde çalışan HDInsight kümeleri için geçerlidir.
   >
   >

**RDP kullanarak bir kümeye bağlanmak için**

1. Oturum [portalı][azure-portal].
2. Tıklayın **tümüne Gözat** sol menüden **HDInsight kümeleri**, küme adınızı tıklatın.
3. Tıklayın **ayarları** üst menü ve ardından **Uzak Masaüstü**.
4. Tıklayın **Connect** ve yönergeleri izleyin. Bağlantı devre dışı ise, bunu önce etkinleştirmeniz gerekir. Uzak Masaüstü kullanıcı adı ve parola kullanarak emin olun.  Küme kullanıcı kimlik bilgilerini kullanamazsınız.

## <a name="open-hadoop-command-line"></a>Hadoop komut satırı açın
Uzak Masaüstü'nü kullanarak kümeye bağlanın ve Hadoop komut satırında kullanmak için önce Uzak Masaüstü erişimi kümeye önceki bölümde açıklandığı gibi etkinleştirmiş olmanız gerekir.

**Bir Hadoop komut satırı açın**

1. Uzak Masaüstü kullanarak kümeye bağlanın.
2. Masaüstü'nden çift **Hadoop komut satırı**.

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Hadoop komutları hakkında daha fazla bilgi için bkz. [Apache Hadoop komutlarını başvuru](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Önceki ekran görüntüsünde, klasör adı katıştırılmış Hadoop sürüm numarasına sahip. Sürüm numarası, kümeye yüklü Hadoop bileşenleri göre değiştirebilirsiniz. Bu klasörleri için başvuruda bulunmak için Hadoop ortam değişkenlerini kullanabilirsiniz. Örneğin:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, portalı kullanarak bir HDInsight kümesi oluşturma ve Apache Hadoop komut satırı aracını açmak nasıl öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure PowerShell kullanarak HDInsight'ı yönetme](hdinsight-administer-use-powershell.md)
* [Klasik Azure CLI kullanarak HDInsight'ı yönetme](hdinsight-administer-use-command-line.md)
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Program aracılığıyla Apache Hadoop işlerini gönderme](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight Apache Hadoop hangi sürümünü mi?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop komut satırı"
