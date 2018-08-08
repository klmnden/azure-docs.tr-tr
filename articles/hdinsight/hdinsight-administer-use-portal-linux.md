---
title: Azure portalını kullanarak bir HDInsight Hadoop kümelerini yönetme
description: Azure portalını kullanarak HDInsight küme oluşturma ve yönetme hakkında bilgi edinin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: jasonh
ms.openlocfilehash: 20a48dcd4a9c3dd4c89390c1048ec4fd5f5783ae
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597217"
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak [Azure portalında][azure-portal], Azure HDInsight Hadoop kümelerini yönetebilirsiniz. Diğer araçları kullanarak HDInsight Hadoop kümelerini yönetme hakkında bilgi için yukarıdaki sekme seçicisini kullanın.

**Önkoşul**

Bu makaledeki adımları için ihtiyacınız olacak bir **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-azure-portal"></a>Azure portalını açın
1. Oturum [ https://portal.azure.com ](https://portal.azure.com).
2. Portal açtıktan sonra şunları yapabilirsiniz:

   * Tıklayın **kaynak Oluştur** sol menüden yeni bir küme oluşturmak için:

       ![Yeni HDInsight kümesi düğmesi](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)

       Girin **HDInsight** içinde **markette Ara**, tıklayın **HDInsight**ve ardından **Oluştur**.

   * Tıklayın **HDInsight kümeleri** sol menüden var olan kümeleri listelemek için:

       ![Azure portal HDInsight kümesi düğmesi](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Görmüyorsanız **HDInsight kümeleri** düğmesine ve ardından **HDInsight kümeleri** altında **zeka + analiz** bölümü.


## <a name="create-clusters"></a>Küme oluşturma
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight, geniş Hadoop bileşenleri ile çalışır. Doğrulandı ve desteklenen bileşenlerin listesi için bkz: [hangi sürümüdür Azure HDInsight hadoop?](hdinsight-component-versioning.md) Genel bir küme oluşturma için bilgi [Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Erişim denetimi gereksinimleri

Bir HDInsight kümesi oluşturduğunuzda, bir Azure aboneliği belirtmeniz gerekir. Kümeye yeni bir Azure kaynak grubu veya mevcut bir kaynak grubunda oluşturulabilir. HDInsight kümelerini oluşturmak için izinlerinizi doğrulamak için aşağıdaki adımları kullanabilirsiniz:

- Yeni bir kaynak grubu oluşturmak için:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Tıklayın **abonelik** sol menüden. Sarı bir anahtar simgesi var. Aboneliklerin listesini görürsünüz.
    3. Kümeleri oluşturmak için kullandığınız bir aboneliğe tıklayın. 
    4. Tıklayın **İzinlerim**.  Bu gösterir, [rol](../role-based-access-control/built-in-roles.md) abonelik. En az HDInsight kümesi oluşturmak için katkıda bulunan erişimi.

- Mevcut bir kaynak grubunu kullanmak için:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Tıklayın **kaynak grupları** kaynak gruplarını listelemek için sol menüden.
    3. HDInsight kümenizi oluşturmak için kullanmak istediğiniz kaynak grubuna tıklayın.
    4. Tıklayın **erişim denetimi (IAM)**, doğrulayın, (veya bir gruba ait olmanız) en az katkıda bulunan kaynak grubuna erişebilir.

NoRegisteredProviderFound hata veya MissingSubscriptionRegistration hata alırsanız bkz [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Kümeleri Listele ve Göster
1. Oturum [ https://portal.azure.com ](https://portal.azure.com).
2. Tıklayın **HDInsight kümeleri** sol menüden var olan kümeleri listesi. Görmüyorsanız **HDInsight kümeleri**, tıklayın **tüm hizmetleri** ilk.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmındaki filtre kullanabilirsiniz.
4. Genel Bakış sayfasını görmek için listenin bir kümeden tıklayın:

    ![Azure portal HDInsight küme temel bileşenleri](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png) **genel bakış menüsü:**
    * **Pano**: küme için Ambari web kullanıcı arabirimini açar.
    * **Secure Shell**: kümeye Secure Shell (SSH) bağlantısı kullanarak bağlanmak için yönergeleri gösterir.
    * **Küme ölçeklendirme**: Bu küme için alt düğüm sayısını değiştirmenize izin verir.
    * **Taşıma**: küme başka bir kaynak grubuna ya da başka bir aboneliğe taşınır.
    * **Silme**: kümeyi siler.

    **Sol menü:**
    * **Etkinlik günlükleri**: etkinlik günlüklerini göster ve sorgu.
    * **Erişim denetimi (IAM)**: rol atamalarını kullanın.  Bkz: [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md).
    * **Etiketleri**: özel bir taksonomi, bulut hizmetleri tanımlamak için anahtar/değer çiftleri ayarlamanıza olanak tanır. Örneğin, adında bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.
    * **Sorunları tanılama ve çözme**: sorun giderme bilgilerini görüntüleyin.
    * **Kilitler**: değiştirilmiş veya silinmiş olan küme önlemek için bir kilit ekleyin.
    * **Otomasyon betiği**: görüntü ve küme için Azure Resource Manager şablonunu dışarı aktarma. Şu anda yalnızca bağımlı Azure depolama hesabına dışarı aktarabilirsiniz. Bkz: [oluşturma Linux tabanlı Hadoop kümeleri Azure Resource Manager şablonlarını kullanarak HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Hızlı Başlangıç**: yardımcı olacak bilgileri görüntüler, HDInsight kullanmaya başlama.
    * **HDInsight Araçları**: HDInsight yönelik yardım bilgilerini ilgili araçlar.
    * **Abonelik çekirdek kullanımı**: aboneliğiniz için kullanılan ve kullanılabilir çekirdek görüntüler.
    * **Küme ölçeklendirme**: artırma ve azaltma küme çalışan düğümü sayısı. Bkz:[ölçek kümeleri](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **SSH + küme girişi**: kümeye Secure Shell (SSH) bağlantısı kullanarak bağlanmak için yönergeleri gösterir. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **HDInsight iş ortağı**: geçerli HDInsight iş ortağı ekleme/kaldırma.
    * **Dış meta depolar**: Hive ve Oozie meta depolar görüntüleyin. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir. Bkz: [Hive/Oozie meta veri deposu kullanmak](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Betik eylemleri**: çalıştırma Bash komut dosyaları küme üzerinde. Bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).
    * **Uygulamaları**: Ekle/Kaldır HDInsight uygulamaları.  Bkz: [özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).
    * **İzleme**: Azure Log analytics'te kümesini izleme.
    * **Özellikler**: küme özelliklerini görüntüleyin.
    * **Depolama hesapları**: depolama hesapları ve anahtarları görüntüleyin. Depolama hesapları, küme oluşturma işlemi sırasında yapılandırılır.
    * **Data Lake Store erişimi**: Data Lake depolar erişimi yapılandırın.  Bkz: [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
    * **Kaynak durumu**: bkz [Azure kaynak durumu genel bakış](../service-health/resource-health-overview.md).
    * **Yeni destek isteği**: Microsoft desteği ile bir destek bileti oluşturmanızı sağlar.
    
6. Tıklayın **özellikleri**:

    Özellikler şunlardır:

   * **Ana bilgisayar adı**: küme adı.
   * **Küme URL**: Ambari web arabirimine yönelik URL.
   * **Güvenli Kabuk (SSH)**: SSH üzerinden kümeye erişirken kullanılacak kullanıcı adı ve ana bilgisayar adı.
   * **Durum**: şunlardan biri: durduruldu, kabul ClusterStorageProvisioned AzureVMConfiguration, çalıştığından, silme, hata, HDInsightConfiguration, işletimsel, silinmiş, zaman aşımına uğradı, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued veya ClusterCustomization.
   * **Bölge**: Azure konumu. Desteklenen Azure konumları listesi için bkz. **bölge** açılan liste kutusunu [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Oluşturulma tarihi**: kümenin dağıtıldığı tarih.
   * **İşletim sistemi**: ya da **Windows** veya **Linux**.
   * **Tür**: Hadoop, HBase, Storm, Spark.
   * **Sürüm**. Bkz: [HDInsight sürümleri](hdinsight-component-versioning.md).
   * **Abonelik**: Abonelik adı.
   * **Varsayılan veri kaynağı**: varsayılan küme dosya sistemi.
   * **Çalışan düğümlerinin boyutu**: çalışan düğümleri seçilen sanal makine boyutu.
   * **Baş düğüm boyutu**: baş düğümlerine seçilen sanal makine boyutu.
   * **Sanal ağ**: dağıtım sırasında seçildiyse, kümeye dağıtılan, sanal ağın adı.

## <a name="delete-clusters"></a>Kümeleri Sil
Küme silme ya da bağlı tüm depolama hesaplarını varsayılan depolama hesabını silmez. Kümenin aynı depolama hesapları ve aynı meta depolarla kullanarak yeniden oluşturabilirsiniz. Kümeyi yeniden oluşturduğunuzda yeni bir varsayılan Blob kapsayıcısını kullanmanızı öneririz.

1. Oturum [portalı][azure-portal].
2. Tıklayın **HDInsight kümeleri** sol menüden. Görmüyorsanız **HDInsight kümeleri**, tıklayın **tüm hizmetleri** ilk.
3. Silmek istediğiniz kümeye tıklayın.
4. Tıklayın **Sil** üstteki menüden ve ardından yönergeleri izleyin.

Ayrıca bkz: [duraklatma/kümeleri kapatma](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Bir küme oluşturulduktan sonra ek Azure depolama hesapları ve Azure Data Lake Store hesapları ekleyebilirsiniz. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme kümesi küme yeniden oluşturmak zorunda kalmadan bir Azure HDInsight kümesi tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).
>
>

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi değişir:

* Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Bu davranış tüm çalışan ve farklı bekleyen işleri ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, küme baş düğümüne oturum açma ve bir komut istemi penceresinden aşağıdaki komutları çalıştırmadan tarafından el ile bölgesel sunucuları dengeleyebilirsiniz:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz. [HDInsight, Apache HBase örneğiyle çalışmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md).

* Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topolojiyi yeniden dengelemek gerekecektir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgeleri](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight Storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Bir örnek Storm topolojiyi yeniden dengelemek için CLI komutu aşağıda verilmiştir:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

**Kümeleri ölçeklendirme**

1. Oturum [portalı][azure-portal].
2. Tıklayın **HDInsight kümeleri** sol menüden.
3. Ölçeklendirmek istediğiniz kümeye tıklayın.
3. Tıklayın **küme ölçeklendirme**.
4. Girin **numarası, çalışan düğümleri**. Küme düğümleri sayısı Azure abonelikleri arasında değişiklik gösterir. Sınırı artırmak için fatura desteğine başvurabilirsiniz.  Maliyet bilgileri, düğüm sayısını yapmış olduğunuz değişiklikleri yansıtır.

    ![HDInsight, hadoop, hbase, storm, spark, Ölçek](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Kümeleri duraklatma/Kapat

Hadoop işlerinin çoğu yalnızca zaman zaman çalışan toplu işler ' dir. Hadoop kümeleri için Küme işlemi için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programlama yapabileceğiniz birçok yolu vardır:

* Kullanıcı Azure veri fabrikası. Bkz: [oluşturma isteğe bağlı Linux tabanlı Hadoop kümeleri Azure Data Factory kullanarak HDInsight](hdinsight-hadoop-create-linux-clusters-adf.md) isteğe bağlı HDInsight'ı oluşturmak için bağlı hizmetler.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md).
* Azure CLI kullanın. Bkz: [yönetme HDInsight kümeleri Azure CLI kullanarak](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK'sını kullanın. Bkz: [gönderme Hadoop işlerini](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Fiyatlandırma bilgileri için bkz. [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/). Portaldan bir kümeyi silmek için bkz: [küme silme](#delete-clusters)

## <a name="move-cluster"></a>Küme Taşı

Bir HDInsight kümesi, başka bir Azure kaynak grubu veya başka bir aboneliğe taşıyabilirsiniz.  Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

## <a name="upgrade-clusters"></a>Küme yükseltme

Bkz: [daha yeni bir sürüme yükseltme HDInsight küme](./hdinsight-upgrade-cluster.md).

## <a name="open-the-ambari-web-ui"></a>Ambari web kullanıcı arabirimini açın

Ambari bir sezgisel, kullanımı kolay Hadoop Yönetim web kullanıcı Arabirimi, RESTful API'ları tarafından desteklenen sağlar. Ambari, sistem yöneticilerinin yönetme ve Hadoop kümelerini izleme sağlar.

1. Bir HDInsight kümesi, Azure portalından açın.  Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).
2. Tıklayın **küme Panosu**.

    ![HDInsight Hadoop kümesi menüsü](./media/hdinsight-administer-use-portal-linux/hdinsight-azure-portal-cluster-menu.png)

1. Küme kullanıcı adı ve parolayı girin.  Varsayılan Küme kullanıcı adı _yönetici_. Ambari web kullanıcı Arabirimi gibi görünür:

    ![HDInsight Hadoop Ambari Web kullanıcı Arabirimi](./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-ambari-web-ui.png)

Daha fazla bilgi için [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md).

## <a name="change-passwords"></a>Parolaları değiştirme
Bir HDInsight kümesi iki kullanıcı hesabı içerebilir. HDInsight küme kullanıcı hesabı (yani) HTTP kullanıcı hesabı) ve SSH kullanıcı hesabı oluşturma işlemi sırasında oluşturulur. Küme kullanıcı hesabı kullanıcı adı ve parola ve SSH kullanıcı hesabı değiştirmek için betik eylemleri değiştirmek için Ambari web kullanıcı arabirimini kullanın

### <a name="change-the-cluster-user-password"></a>Küme kullanıcı parolasını değiştirme
Küme kullanıcı parolasını değiştirmek için Ambari Web kullanıcı arabirimini kullanabilirsiniz. Ambari için oturum açmak için mevcut küme kullanıcı adı ve parolayı kullanmanız gerekir.

> [!NOTE]
> Küme kullanıcı (Yönetici) parolası değiştirme başarısız olmasına karşı bu küme çalışacak bir betik eylemleri neden olabilir. Kalıcı betik eylemleri, hedef alt düğümleri varsa, bu betikleri aracılığıyla küme düğümlerine operations yeniden boyutlandırma eklediğinizde başarısız olabilir. Betik eylemleri hakkında daha fazla bilgi için bkz. [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Ambari Web Arabirimine HDInsight küme kullanıcı kimlik bilgilerini kullanarak oturum açın. Varsayılan kullanıcı adı **admin** şeklindedir. URL **https://&lt;HDInsight küme adı > azurehdinsight.net**.
2. Tıklayın **yönetici** üstteki menüden ve "Ambari yönetme"'ye tıklayın.
3. Sol menüden **kullanıcılar**.
4. Tıklayın **yönetici**.
5. Tıklayın **Parola Değiştir**.

Ambari, daha sonra kümedeki tüm düğümlerde parolayı değiştirir.

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
3. Azure portalından tıklayın **HDInsight kümeleri**.
4. HDInsight kümenizi tıklayın.
4. Tıklayın **betik eylemlerini**.
4. Gelen **betik eylemleri** dikey penceresinde **yeni Gönder**. Zaman **betik eylemini Gönder** dikey penceresi görünür, aşağıdaki bilgileri girin:

   | Alan | Değer |
   | --- | --- |
   | Ad |SSH parolasını değiştirme |
   | Bash betiği URI'si |Changepassword.sh dosya URI'si |
   | Düğümleri (baş, çalışan, Nimbus, Gözetmen, Zookeeper, vb.) |✓ listelenen tüm düğüm türleri |
   | Parametreler |SSH kullanıcı adı ve yeni parolayı girin. Kullanıcı adı ve parola arasına bir boşluk olması gerekir. |
   | Bu betik eylemini Sürdür... |Bu alan işaretli bırakın. |
5. Seçin **Oluştur** betik uygulamak için. Betik bittikten sonra yeni bir parola ile SSH kullanarak kümeye bağlanabilir.

## <a name="grantrevoke-access"></a>GRANT/revoke-access
HDInsight kümeleri aşağıdaki HTTP web Hizmetleri (Bu hizmetlerin tümü, RESTful uç noktalarına sahip) sahip:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton da

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme/kullanarak erişim verme [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) ve [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Abonelik kimliği bulunamadı

**Azure aboneliğinizi kimliklerini bulmak için**

1. Oturum [portalı][azure-portal].
2. Tıklayın **abonelikleri**. Her abonelik bir ad ve bir kimliği vardır.

Her kümenin bir Azure aboneliğine bağlıdır. Abonelik kimliği küme üzerinde gösterilen **temel** Döşe. Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Resource Manager modunda bir Azure Resource Manager grubu ile her HDInsight kümesi oluşturulur. Bir kümeye ait Resource Manager grubu görünür:

* Küme listesi olan bir **kaynak grubu** sütun.
* Küme **temel** Döşe.  

Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

## <a name="find-the-storage-accounts"></a>Depolama hesaplarını bulma

HDInsight kümeleri, verileri depolamak için bir Azure depolama hesabını veya bir Azure Data Lake Store kullanın. Bir varsayılan depolama hesabını ve bağlı depolama hesabı sayısı, her bir HDInsight kümesi olabilir. Depolama hesaplarını listelemek için önce portaldan kümeyi açın ve ardından **depolama hesapları**:

![HDInsight küme depolama hesapları](./media/hdinsight-administer-use-portal-linux/hdinsight-storage-accounts.png)

Önceki ekran görüntüsünde, var olan bir __varsayılan__ hesabın varsayılan depolama hesabı olup olmadığını gösteren bir sütun.

Data Lake Store hesaplarını listelemek için tıklatın **Data Lake Store erişimi** önceki ekran görüntüsünde.

## <a name="run-hive-queries"></a>Hive sorguları çalıştırma
Doğrudan Azure portalından Hive işi çalıştırılamıyor ancak Ambari Web kullanıcı arabiriminde Hive görünümü kullanabilirsiniz.

**Ambari Hive görünümünü kullanarak Hive sorguları çalıştırmak için**

1. Ambari Web Arabirimine HDInsight küme kullanıcı kimlik bilgilerini kullanarak oturum açın. Varsayılan kullanıcı adı **admin** şeklindedir. URL **https://&lt;HDInsight küme adı > azurehdinsight.net**.
2. Hive görünümü aşağıdaki ekran görüntüsünde gösterildiği gibi açın:  

    ![HDInsight hive görünümü](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)

3. Tıklayın **sorgu** üstteki menüden.
4. Bir Hive sorgusu girin **sorgu Düzenleyicisi**ve ardından **yürütme**.

## <a name="monitor-jobs"></a>İşleri izleme
Bkz: [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Dosyalara göz atın
Azure portalını kullanarak, varsayılan kapsayıcı içeriğini göz atabilirsiniz.

1. Oturum [ https://portal.azure.com ](https://portal.azure.com).
2. Tıklayın **HDInsight kümeleri** sol menüden var olan kümeleri listesi.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmındaki filtre kullanabilirsiniz.
4. Tıklayın **depolama hesapları** küme sol menüden.
5. Bir depolama hesabına tıklayın.
7. Tıklayın **Blobları** Döşe.
8. Varsayılan kapsayıcı adına tıklayın.

## <a name="monitor-cluster-usage"></a>Küme kullanımı izleme
**Kullanım** HDInsight küme dikey penceresinde bölümünü nasıl ayrılacağını ve bu küme için ayrılmış çekirdek sayısının yanı sıra HDInsight ile kullanmak için aboneliğinizi kullanılabilir çekirdek sayısı hakkında daha fazla bilgi görüntüler Bu küme içindeki düğümler için. Bkz: [kümeleri Listele ve Göster](#list-and-show-clusters).

> [!IMPORTANT]
> HDInsight kümesi tarafından sağlanan hizmetleri izlemek için Ambari Web veya Ambari REST API'sini kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [yönetme HDInsight kümeleri Ambari kullanarak](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Kümeye bağlanma

* [HDInsight ile Hive kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı temel yönetim işlevleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure PowerShell kullanarak HDInsight'ı yönetme](hdinsight-administer-use-powershell.md)
* [Azure CLI kullanarak HDInsight'ı yönetme](hdinsight-administer-use-command-line.md)
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Ambari Web kullanıcı arabirimini kullanma hakkında daha fazla bilgi edinin](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API'sini kullanarak ayrıntıları](hdinsight-hadoop-manage-ambari-rest-api.md)
* [HDInsight Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight Sqoop kullanma](hadoop/hdinsight-use-sqoop.md)
* [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight, Hadoop hangi sürümünü mi?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop komut satırı"
