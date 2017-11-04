---
title: "Azure portalını kullanarak hdınsight'ta Hadoop Windows tabanlı kümeler yönetme | Microsoft Docs"
description: "Hdınsight hizmetini yönetme hakkında bilgi edinin. Hdınsight kümesi oluşturma, etkileşimli JavaScript konsolunu açın ve Hadoop komut konsolunu açın."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: ecaad702843a63bb82b781339d25fde10df0a0a4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure Portalı'nı kullanarak Windows tabanlı Hadoop kümeleri hdınsight'ta yönetme

Kullanarak [Azure portal][azure-portal], Azure Hdınsight'ta Windows tabanlı Hadoop kümeleri oluşturma, Hadoop kullanıcı parolasını değiştirmek ve Hadoop komut konsolundan kümede erişebilmesi için Uzak Masaüstü Protokolü (RDP) etkinleştirin.

Bu makaledeki bilgiler yalnızca Windows tabanlı Hdınsight kümeleri için geçerlidir. Linux tabanlı kümelerde yönetme hakkında daha fazla bilgi için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Ön koşullar

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure depolama hesabı** -bir Hdınsight kümesi bir Azure Blob storage kapsayıcısı varsayılan dosya sistemi olarak kullanır. Azure Blob storage Hdınsight kümeleri ile sorunsuz bir deneyim nasıl sağladığını hakkında daha fazla bilgi için bkz: [kullanım Azure Blob Storage Hdınsight ile](hdinsight-hadoop-use-blob-storage.md). Bir Azure Storage hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](../storage/common/storage-create-storage-account.md).

## <a name="open-the-portal"></a>Portalını açın
1. Oturum [https://portal.azure.com](https://portal.azure.com).
2. Portal açtıktan sonra şunları yapabilirsiniz:

   * Tıklatın **yeni** sol menüden yeni bir küme oluşturmak için:

       ![Yeni Hdınsight küme düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Tıklatın **Hdınsight kümeleri** sol menüden.

       ![Azure portal Hdınsight küme düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Varsa **Hdınsight** değil, soldaki menüde yer tıklatın **Gözat**.

     ![Azure portal gözatma küme düğmesi](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Küme oluşturma
Portalı kullanarak oluşturma yönergeleri için bkz: [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

Hdınsight geniş Hadoop bileşenleri ile çalışır. Doğrulandı ve desteklenen bileşenlerin listesi için bkz: [hangi sürümüdür Azure Hdınsight'ta hadoop](hdinsight-component-versioning.md). Aşağıdaki seçeneklerden birini kullanarak Hdınsight özelleştirebilirsiniz:

* Küme yapılandırmasını değiştirmek veya Giraph veya Solr gibi özel bileşenleri yüklemek için bir küme özelleştirebilirsiniz özel komut dosyaları çalıştırmak için betik eylemi kullanın. Daha fazla bilgi için bkz: [betik eylemi kullanarak özelleştirme Hdınsight kümesi](hdinsight-hadoop-customize-cluster.md).
* Küme oluşturma sırasında Hdınsight .NET SDK veya Azure PowerShell küme özelleştirme parametrelerini kullanın. Bu yapılandırma değişikliklerini kümenin kullanım ömrü sonra korunur ve Azure platformu düzenli olarak bakımını gerçekleştirir küme düğümü reimages etkilenmez. Küme özelleştirme parametreleri kullanma hakkında daha fazla bilgi için bkz: [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).
* Mahout ve basamaklama, gibi yerel bazı Java bileşenleri kümede JAR dosyaları olarak çalıştırabilirsiniz. Bu JAR dosyalarını Azure Blob depolama alanına dağıtılmış ve Hdınsight kümeleri için Hadoop iş gönderme mekanizmalar aracılığıyla gönderildi. Daha fazla bilgi için bkz: [gönderme Hadoop işleri program aracılığıyla](hadoop/submit-apache-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Hdınsight kümelerinde JAR dosyalarını çağırma başvurun veya Hdınsight kümelerine JAR dosyalarını dağıtma sorunları varsa [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Geçişli Hdınsight tarafından desteklenmiyor ve Microsoft Support uygun değil. Desteklenen bileşenlerin bir listesi için bkz: [Hdınsight tarafından sağlanan küme sürümlerindeki yenilikler](hdinsight-component-versioning.md).
  >
  >

Uzak Masaüstü bağlantısı kullanarak küme üzerinde özel yazılım yüklemesi desteklenmez. Kümeleri yeniden oluşturmanız gerekiyorsa, bunlar kaybolur gibi herhangi bir dosya baş düğümü sürücülerde depolamak kaçınmalısınız. Azure Blob Depolama dosyalarda depolamanızı öneririz. BLOB Depolama kalıcıdır.

## <a name="list-and-show-clusters"></a>Liste ve kümeleri Göster
1. Oturum [https://portal.azure.com](https://portal.azure.com).
2. Tıklatın **Hdınsight kümeleri** sol menüden.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmında filtresini kullanabilirsiniz.
4. Ayrıntıları göstermek için listedeki bir kümeden çift tıklayın.

    **Menü ve essentials**:

    ![Azure portal Hdınsight küme temelleri](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Menüsünü özelleştirmek için herhangi bir yere menüsünde sağ tıklayın ve ardından **Özelleştir**.
   * **Ayarları** ve **tüm ayarları**: görüntüler **ayarları** küme için ayrıntılı yapılandırma bilgilerini erişmenize olanak sağlayan küme dikey penceresinde.
   * **Pano**, **küme Panosu** ve **URL: Ambari Web Linux tabanlı kümeler için olan küme panoya erişmek için tüm yolları bunlar. -**Güvenli Kabuk **: Güvenli Kabuk (SSH) bağlantısı kullanarak kümeye bağlanmak için yönergeleri gösterir.
   * **Küme ölçeklendirme**: Bu küme için alt düğüm sayısını değiştirmenize izin verir.
   * **Silme**: kümeyi siler.
   * **Hızlı Başlangıç**: yardımcı olacak bilgileri görüntüler, Hdınsight kullanarak başlayın.
   * ** Kullanıcıları: izinlerini ayarlamanızı sağlar *portal Yönetim* diğer kullanıcılar için bu kümenin, Azure aboneliğinizin üzerinde.

     > [!IMPORTANT]
     > Bu *yalnızca* erişim ve izinleri bu kümeye Azure portalında etkiler ve kim bağlanmak veya Hdınsight kümesi göndermek herhangi bir etkisi olmaz.
     >
     >
   * **Etiketler**: etiketleri bulut hizmetlerinizi, özel bir sınıflandırma tanımlamak için anahtar/değer çiftleri kümesi izin verir. Örneğin, adlı bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.
   * **Ambari görünümleri**: Ambari Web bağlantıları.

     > [!IMPORTANT]
     > Hdınsight küme tarafından sağlanan hizmetleri yönetmek için Ambari Web veya Ambari REST API kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Ambari kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Kullanım**:

     ![Azure portal Hdınsight küme kullanımı](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Tıklatın **ayarları**.

    ![Azure portal Hdınsight küme kullanımı](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Özellikleri**: küme özelliklerini görüntüleyin.
   * **Küme AAD kimlik**:
   * **Azure depolama anahtarları**: varsayılan depolama hesabı ve anahtarını görüntüleyin. Depolama hesabı küme oluşturma işlemi sırasında bir yapılandırmadır.
   * **Oturum açma küme**: küme HTTP kullanıcı adını ve parolayı değiştirin.
   * **Dış meta deponuz**: Hive ve Oozie meta deponuz görüntüleyin. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir.
   * **Küme ölçeklendirme**: artırma ve azaltma küme çalışan düğüm sayısı.
   * **Uzak Masaüstü**: etkinleştirmek ve Uzak Masaüstü (RDP) erişimi devre dışı bırakın ve RDP kullanıcı adı yapılandırın.  RDP kullanıcı adı HTTP kullanıcı adından farklı olmalıdır.
   * **Kayıtlı iş ortağı**:

     > [!NOTE]
     > Bu, kullanılabilir ayarlar genel bir listesi verilmiştir; tümünü değil, tüm küme türleri için mevcut olacaktır.
     >
     >
6. Tıklatın **özellikleri**:

    Özellikler bölümü aşağıda listelenmiştir:

   * **Ana bilgisayar adı**: küme adı.
   * **Küme URL**.
   * **Durum**: dahil, kabul iptal ClusterStorageProvisioned, AzureVMConfiguration, çalıştığından, silme, hatası, HDInsightConfiguration, işletimsel, silinen, süresi sona erdi, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Bölge**: Azure konumu. Desteklenen Azure konumlar listesi için bkz: **bölge** açılan liste kutusunu [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Oluşturulan veri**.
   * **İşletim sistemi**: ya da **Windows** veya **Linux**.
   * **Tür**: Hadoop, HBase, Storm, Spark.
   * **Sürüm**. Bkz: [Hdınsight sürümleri](hdinsight-component-versioning.md)
   * **Abonelik**: Abonelik adı.
   * **Abonelik kimliği**.
   * **Birincil veri kaynağını**. Azure Blob Depolama hesabı Hadoop dosya sistemi varsayılan olarak kullanılır.
   * **Çalışan düğüm fiyatlandırma katmanı**.
   * **Baş düğüm fiyatlandırma katmanı**.

## <a name="delete-clusters"></a>Küme silme
Bir küme silmek, varsayılan depolama hesabı ya da bağlı tüm depolama hesaplarını silmez. Küme aynı depolama hesapları ve aynı meta deponuz kullanarak yeniden oluşturabilirsiniz.

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **silmek** üstteki menüden ve yönergeleri izleyin.

Ayrıca bkz. [Duraklat/kümeleri kapatma](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme küme kümeye yeniden oluşturmak zorunda kalmadan Azure Hdınsight'ta çalıştıran bir küme tarafından kullanılan çalışan düğümü sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, Hdınsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümünü emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).
>
>

Her tür Hdınsight tarafından desteklenen küme için veri düğüm sayısını değiştirme etkisi:

* Hadoop

    Sorunsuz bir şekilde tüm bekleyen veya çalışan işler etkilemeden çalıştıran bir Hadoop kümesinde çalışan düğümü sayısını da artırabilirsiniz. İşlemi devam ederken yeni işleri da gönderilebilir. Kümenin her zaman işlevsel bir durumda bırakılır böylece bir ölçeklendirme işlemi hatalar düzgün bir şekilde ele alınır.

    Bir Hadoop kümesine veri düğüm sayısını azaltarak ölçeklendirilir, bazı kümedeki hizmetleri yeniden başlatılır. Bu işleri bekleyen tüm çalışan ve ölçeklendirme işlemi tamamlandığında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, sunmaları olabilir.
* HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken, HBase kümesi düğümleri kaldırın. Bölgesel sunucular otomatik olarak ölçeklendirme işlemi tamamladıktan birkaç dakika içinde dengeli. Ancak, küme headnode günlüğe kaydetme ve bir komut istemi penceresinden aşağıdaki komutları çalıştırarak el ile de bölgesel sunucular dengeleyebilirsiniz:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz:]
* Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümeniz veri düğümleri kaldırın. Ancak ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    İki yolla yeniden dengelenmesi gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Lütfen [Apache Storm belgelerine](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    Hdınsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![Hdınsight storm ölçek yeniden dengeleyin](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Storm topolojisini yeniden dengelemeniz CLI komutunu kullanma örneği şöyledir:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Küme ölçeklendirme**

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **ayarları** üst menüsünden ve ardından **ölçekte küme**.
4. Girin **sayı, alt düğümleri**. Küme düğümü sayısını Azure abonelikleri arasında değişiklik gösterir. Sınırını artırmak için fatura desteğine başvurabilirsiniz.  Maliyet bilgileri düğüm sayısını yapmış olduğunuz değişiklikleri yansıtır.

    ![Hdınsight Hadoop HBase Storm Spark ölçek](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Kümeleri Duraklat/kapatma
Hadoop işlerinin çoğu yalnızca toplu işleri bazen çalışan ' dir. Çoğu Hadoop kümeleri için küme işleme için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programı birçok yolu vardır:

* Kullanıcı Azure Data Factory. Bkz: [Azure Hdınsight bağlı hizmeti](../data-factory/compute-linked-services.md) ve [dönüştürme ve Azure Data Factory kullanarak Analiz](../data-factory/transform-data.md) Hizmetleri için isteğe bağlı ve otomatik olarak tanımlanan Hdınsight bağlı.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md).
* Azure CLI kullanın. Bkz: [Azure CLI kullanarak Hdınsight kümelerini yönetme](hdinsight-administer-use-command-line.md).
* Hdınsight .NET SDK'yı kullanın. Bkz: [gönderme Hadoop işlerini](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Fiyatlandırma bilgileri için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/). Portaldan bir kümeyi silmek için bkz: [küme silme](#delete-clusters)

## <a name="change-cluster-username"></a>Değişiklik kümesi kullanıcı adı
Hdınsight kümesi, iki kullanıcı hesapları olabilir. Hdınsight küme kullanıcı hesabı oluşturma işlemi sırasında oluşturulur. RDP aracılığıyla küme erişmek için bir RDP kullanıcı hesabı da oluşturabilirsiniz. Bkz: [etkinleştir Uzak Masaüstü](#connect-to-hdinsight-clusters-by-using-rdp).

**Hdınsight küme kullanıcı adı ve parolayı değiştirmek için**

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **ayarları** üst menüsünden ve ardından **küme oturum açma**.
4. Varsa **küme oturum açma** bırakıldı etkin'ı tıklatmalısınız **devre dışı**ve ardından **etkinleştirmek** kullanıcı adı ve parola değiştirmeden önce...
5. Değişiklik **küme oturum açma adı** ve/veya **küme oturum açma parolasını**ve ardından **kaydetmek**.

    ![Hdınsight küme, kullanıcı, kullanıcı adı, parola, http, kullanıcı değiştirme](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>GRANT/revoke erişim
Hdınsight kümeleri (Bu hizmetlerin tümü için RESTful uç noktaları vardır) aşağıdaki HTTP web hizmetleri vardır:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Varsayılan olarak, bu hizmetleri için erişim verilir. İptal etme / erişim Azure portalından sağlayıcısını.

> [!NOTE]
> Verme/erişimi iptal ederek, küme kullanıcı adı ve parola sıfırlanır.
>
>

**HTTP web hizmetleri erişimi vermek/iptal etmek için**

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **ayarları** üst menüsünden ve ardından **küme oturum açma**.
4. Varsa **küme oturum açma** bırakıldı etkin'ı tıklatmalısınız **devre dışı**ve ardından **etkinleştirmek** kullanıcı adı ve parola değiştirmeden önce...
5. İçin **küme oturum açma kullanıcı** ve **küme oturum açma parolasını**, küme için yeni bir kullanıcı adı ve parola (sırasıyla) girin.
6. **KAYDET**'e tıklayın.

    ![Hdınsight genel http web hizmeti erişimi Kaldır](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Her Hdınsight kümesinde bir varsayılan depolama hesabı vardır. Varsayılan depolama hesabı ve kendi anahtarları bir küme için görünür altında **ayarları**/**özellikleri**/**Azure depolama anahtarları**. Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Kaynak Yöneticisi modunda her Hdınsight kümesi içeren bir Azure kaynak grubu oluşturulur. Bir kümeye ait olan Azure kaynak grubu görünür:

* Küme listesi olan bir **kaynak grubu** sütun.
* Küme **temel** döşeme.  

Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Hdınsight sorgu Konsolu'nu Aç
Hdınsight sorgu Konsolu aşağıdaki özellikleri içerir:

* **Düzenleyici hive**: Hive işlerini göndermenin bir GUI web arabirimi.  Bkz: [sorgu konsolunu kullanarak Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-query-console.md).

    ![Hdınsight portal hive Düzenleyicisi](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **İş Geçmişi**: İzleyici Hadoop işler.  

    ![Hdınsight portal iş geçmişi](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Tıklatın **sorgu adı** iş özellikleri dahil olmak üzere ayrıntılarını göstermek için **sorgu iş**, ve ** iş çıktısı. Sorgu ve çıktı istasyonunuza indirebilirsiniz.
* **Tarayıcı dosya**: varsayılan depolama hesabı ve bağlantılı depolama hesaplarını göz atın.

    ![Hdınsight portal dosya tarayıcısı Gözat](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Ekran üzerindeki  **<Account>**  öğesi olan bir Azure depolama hesabı türünü belirtir.  Dosyalara göz atmak için hesap adına tıklayın.
* **Hadoop UI**.

    ![Hdınsight portal Hadoop kullanıcı Arabirimi](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Gelen **Hadoop UI*, dosyalara gözatın ve günlüklerini denetleyin.
* **Yarn UI**.

    ![Hdınsight portal YARN kullanıcı Arabirimi](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Hive sorguları çalıştırma
Çalıştırdığınız için Hive işleri portaldan tıklatın **Hive Düzenleyicisi** Hdınsight sorgu konsolunda. Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>İşleri izleme
İşlerini portalından izlemek için tıklatın **iş geçmişi** Hdınsight sorgu konsolunda. Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

## <a name="browse-files"></a>Gözatma dosyaları
Varsayılan depolama hesabı ve bağlantılı depolama hesaplarını depolanan dosyaları göz atmak için tıklatın **dosya tarayıcısı** Hdınsight sorgu konsolunda. Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

Aynı zamanda **dosya sistemi Gözat** yardımcı programı'ndan **Hadoop UI** Hdınsight konsolunda.  Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Küme kullanımını izleme
**Kullanım** Hdınsight küme dikey bölümde aboneliğinize Hdınsight ile kullanmak için kullanılabilir çekirdek sayısı gibi bu küme ve bu küme içindeki düğümler için nasıl ayrılacağını ayrılan çekirdek sayısı hakkında bilgileri görüntüler. Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

> [!IMPORTANT]
> Hdınsight küme tarafından sağlanan hizmetlerin izlemek için Ambari Web veya Ambari REST API kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Ambari kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Hadoop kullanıcı arabirimini açın
Kümeyi izlemek için dosya sistemi göz atın ve günlükleri denetleyin, tıklatın **Hadoop UI** Hdınsight sorgu konsolunda. Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Yarn kullanıcı arabirimini açın
Yarn kullanıcı arabirimi kullanmak için tıklatın **Yarn kullanıcı Arabiriminde** Hdınsight sorgu konsolunda. Bkz: [açık Hdınsight sorgu konsol](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>RDP kullanarak kümelerine bağlanmak
Kimlik bilgileri, oluşturma sırasında sağlanan küme için küme üzerinde hizmetlerine ancak Uzak Masaüstü aracılığıyla kümenin kendisi için erişim verin. Bir küme sağladığınızda veya bir küme sağlandıktan sonra Uzak Masaüstü erişimi kapatabilirsiniz. Oluşturma sırasında Uzak Masaüstü'nü etkinleştirme hakkında yönergeler için bkz: [oluşturma Hdınsight kümesi](hdinsight-hadoop-provision-linux-clusters.md).

**Uzak Masaüstü'nü etkinleştirmek için**

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **ayarları** üst menüsünden ve ardından **Uzak Masaüstü**.
4. Girin **süresi**, **Uzak Masaüstü kullanıcı adı** ve **Uzak Masaüstü parola**ve ardından **etkinleştirmek**.

    ![Hdınsight etkinleştirme devre dışı bırakma, Uzak Masaüstü'nü yapılandırma](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    Bir hafta süresi için varsayılan değerleri var.

   > [!NOTE]
   > Bir kümede Uzak Masaüstü'nü etkinleştirmek için Hdınsight .NET SDK'yı da kullanabilirsiniz. Kullanım **EnableRdp** yöntemini aşağıdaki şekilde Hdınsight istemci nesnesinde: **istemci. EnableRdp (clustername, konum, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Benzer şekilde, küme üzerinde Uzak Masaüstü devre dışı bırakmak için kullanabileceğiniz **istemci. DisableRdp (clustername, konum)**. Bu yöntemleri hakkında daha fazla bilgi için bkz: [Hdınsight .NET SDK'sı başvurusu](http://go.microsoft.com/fwlink/?LinkId=529017). Bu, yalnızca Windows üzerinde çalışan Hdınsight kümeleri için geçerlidir.
   >
   >

**RDP kullanarak bir kümeye bağlanmak için**

1. Oturum [Portal][azure-portal].
2. Tıklatın **tümüne Gözat** sol menüden **Hdınsight kümeleri**, küme adına tıklayın.
3. Tıklatın **ayarları** üst menüsünden ve ardından **Uzak Masaüstü**.
4. Tıklatın **Bağlan** ve yönergeleri izleyin. Bağlantı devre dışı ise, önce etkinleştirmelisiniz. Uzak Masaüstü kullanıcı adı ve parola kullanarak emin olun.  Küme kullanıcı kimlik bilgilerini kullanamaz.

## <a name="open-hadoop-command-line"></a>Hadoop komut satırı açın
Uzak Masaüstü'nü kullanarak kümeye bağlanın ve Hadoop komut satırı kullanmak için önce Uzak Masaüstü erişimi kümeye önceki bölümde açıklandığı gibi etkinleştirmiş olmanız gerekir.

**Bir Hadoop komut satırı açın**

1. Uzak Masaüstü'nü kullanarak kümeye bağlanın.
2. Masaüstünden çift **Hadoop komut satırı**.

    ![HDI. HadoopCommandLine][image-hadoopcommandline]

    Hadoop komutları hakkında daha fazla bilgi için bkz: [Hadoop komutları başvuru](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Önceki ekran görüntüsünde, klasör adı katıştırılmış Hadoop sürüm numarasına sahip. Sürüm numarası değiştirilebilir kümeye yüklü Hadoop bileşenleri göre. Bu klasörleri başvurmak için Hadoop ortamı değişkenlerini kullanabilirsiniz. Örneğin:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Portal kullanarak bir Hdınsight kümesi oluşturma ve Hadoop komut satırı aracını açmak nasıl öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight Azure PowerShell kullanarak yönetme](hdinsight-administer-use-powershell.md)
* [Hdınsight Azure CLI kullanarak yönetme](hdinsight-administer-use-command-line.md)
* [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Hadoop işlerini programlı olarak gönderme](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Azure Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Hdınsight'ta Hadoop hangi sürümünün mi?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop komut satırı"
