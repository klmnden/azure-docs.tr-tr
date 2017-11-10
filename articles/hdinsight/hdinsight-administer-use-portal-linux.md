---
title: "Azure portalını kullanarak hdınsight'ta Hadoop kümelerini yönetme | Microsoft Docs"
description: "Oluşturma ve Azure portalını kullanarak Hdınsight kümelerini yönetme hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: jgao
ms.openlocfilehash: 7d5534649595a3109442619e0adf13c0b354cc0f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak hdınsight'ta Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak [Azure portal][azure-portal], Azure hdınsight'ta Hadoop kümelerini yönetebilirsiniz. Diğer araçları kullanarak hdınsight'ta Hadoop kümelerini yönetme hakkında bilgi için yukarıdaki sekme seçicisini kullanın.

**Önkoşul**

Bu makaledeki adımları tamamlayabilmeniz için ihtiyacınız bir **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-azure-portal"></a>Azure portalını açın
1. Oturum [https://portal.azure.com](https://portal.azure.com).
2. Portal açtıktan sonra şunları yapabilirsiniz:

   * Tıklatın **yeni** sol menüden yeni bir küme oluşturmak için:

       ![Yeni Hdınsight küme düğmesi](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Tıklatın **Hdınsight kümeleri** var olan kümeleri listelemek için sol menüden:

       ![Azure portal Hdınsight küme düğmesi](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Görmüyorsanız, **Hdınsight kümeleri** düğmesini tıklatın, **daha fazla hizmet** listesi ve ardından altındaki **Hdınsight kümeleri** altında  **Intelligence + analiz** bölümü.


## <a name="create-clusters"></a>Küme oluşturma
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Hdınsight geniş Hadoop bileşenleri ile çalışır. Doğrulandı ve desteklenen bileşenlerin listesi için bkz: [hangi sürümüdür Azure Hdınsight'ta hadoop?](hdinsight-component-versioning.md) Genel küme oluşturma için bilgi [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Erişim denetimi gereksinimleri

Bir Hdınsight kümesi oluştururken, bir Azure aboneliği belirtmeniz gerekir. Küme, yeni bir Azure kaynak grubu veya varolan bir kaynak grubunda oluşturulabilir. Hdınsight kümeleri oluşturmak için izinlerinizi doğrulamak için aşağıdaki adımları kullanın:

- Yeni bir kaynak grubu oluşturmak için:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Tıklatın **abonelik** sol menüden. Sarı bir anahtar simgesi vardır. Aboneliklerin listesini göreceksiniz.
    3. Kümeleri oluşturmak için kullandığınız aboneliğe tıklayın. 
    4. Tıklatın **izinlerimi**.  Bunu gösterir, [rol](../active-directory/role-based-access-control-what-is.md#built-in-roles) abonelikte. En az gereksinim duyduğunuz Hdınsight kümesi oluşturmak için katkıda bulunan erişim.

- Var olan bir kaynak grubunu kullanmak için:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Tıklatın **kaynak grupları** kaynak gruplarını listelemek için sol menüden.
    3. Hdınsight kümesi oluşturmak için kullanmak istediğiniz kaynak grubunu tıklatın.
    4. Tıklatın **erişim denetimi (IAM)**, doğrulayın sizin (veya size ait bir grubu) en az katkıda bulunan kaynak grubu erişimi.

NoRegisteredProviderFound hatası veya MissingSubscriptionRegistration hatası alırsanız, bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Liste ve kümeleri Göster
1. Oturum [https://portal.azure.com](https://portal.azure.com).
2. Tıklatın **Hdınsight kümeleri** var olan kümeleri listelemek için sol menüden. Görmüyorsanız, **Hdınsight kümeleri**, tıklatın **daha fazla hizmet** ilk.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmında filtresini kullanabilirsiniz.
4. Genel bakış sayfasında görmek için listeden bir kümeden tıklatın:

    ![Azure portal Hdınsight küme essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png) **genel bakış menüsü:**
    * **Pano**: Ambari Web Linux tabanlı kümeler için olan küme panosu açılır.
    * **Güvenli Kabuk**: Güvenli Kabuk (SSH) bağlantısı kullanarak kümeye bağlanmak için yönergeleri gösterir.
    * **Küme ölçeklendirme**: Bu küme için alt düğüm sayısını değiştirmenize izin verir.
    * **Silme**: kümeyi siler.

    **Sol menü:**
    * **Etkinlik günlükleri**: Göster ve sorgu etkinlik günlükleri.
    * **Erişim denetimi (IAM)**: rol atamalarını kullanın.  Bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../active-directory/role-based-access-control-configure.md).
    * **Etiketler**: Bulut hizmetlerinizi, özel bir sınıflandırma tanımlamak için anahtar/değer çiftlerinin ayarlamanıza olanak tanır. Örneğin, adlı bir anahtar oluşturabilir **proje**ve ardından belirli bir projeyle ilişkili tüm hizmetler için ortak bir değer kullanın.
    * **Sorunları tanılamak ve**: sorun giderme bilgileri görüntüler.
    * **Kilitler**: değiştirilmiş veya silinmiş olan küme önlemek için bir kilit ekleyin.
    * **Otomasyon betiğini**: görüntü ve küme için Azure Resource Manager şablonunu dışarı aktarma. Şu anda, yalnızca bağımlı Azure depolama hesabı dışarı aktarabilirsiniz. Bkz: [oluşturma Linux tabanlı Hadoop kümeleri Azure Resource Manager şablonları kullanarak Hdınsight'ta](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Hızlı Başlangıç**: yardımcı olacak bilgileri görüntüler, Hdınsight kullanarak başlayın.
    * **Hdınsight Araçları**: Hdınsight için Yardım bilgileri ilgili araçlar.
    * **Oturum açma küme**: küme oturum açma bilgileri görüntüler.
    * **Abonelik çekirdek kullanım**: aboneliğiniz için kullanılan ve kullanılabilir çekirdekler görüntüler.
    * **Küme ölçeklendirme**: artırma ve azaltma küme çalışan düğüm sayısı. Bkz:[ölçek kümeleri](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Güvenli Kabuk**: Güvenli Kabuk (SSH) bağlantısı kullanarak kümeye bağlanmak için yönergeleri gösterir. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **Hdınsight iş ortağı**: geçerli Hdınsight iş ortağı Ekle/Kaldır.
    * **Dış meta deponuz**: Hive ve Oozie meta deponuz görüntüleyin. Meta depolar, yalnızca küme oluşturma işlemi sırasında yapılandırılabilir. Bkz: [Hive/Oozie meta depo kullanmak](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Betik eylemleri**: çalıştırmak Bash betikleri küme üzerinde. Bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).
    * **Uygulamaları**: Ekle/Kaldır Hdınsight uygulamaları.  Bkz: [özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).
    * **Özellikleri**: küme özelliklerini görüntüleyin.
    * **Depolama hesapları**: depolama hesaplarını ve anahtarlarını görüntülemek. Depolama hesapları küme oluşturma işlemi sırasında yapılandırılır.
    * **Küme AAD kimlik**:
    * **Yeni destek isteği**: Microsoft desteği ile bir destek bileti oluşturmanızı sağlar.
    
6. Tıklatın **özellikleri**:

    Özellikler şunlardır:

   * **Ana bilgisayar adı**: küme adı.
   * **Küme URL**: Ambari web arabirimi için URL.
   * **Güvenli Kabuk (SSH)**: SSH aracılığıyla küme erişirken kullanılacak kullanıcı adı ve ana bilgisayar adı.
   * **Durum**: biri: iptal edildi, kabul ClusterStorageProvisioned, AzureVMConfiguration, çalıştığından, silme, hatası, HDInsightConfiguration, işletimsel, silinen, süresi sona erdi, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued veya ClusterCustomization.
   * **Bölge**: Azure konumu. Desteklenen Azure konumlar listesi için bkz: **bölge** açılan liste kutusunu [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Oluşturulma tarihi**: küme dağıtılan tarih.
   * **İşletim sistemi**: ya da **Windows** veya **Linux**.
   * **Tür**: Hadoop, HBase, Storm, Spark.
   * **Sürüm**. Bkz: [Hdınsight sürümleri](hdinsight-component-versioning.md).
   * **Abonelik**: Abonelik adı.
   * **Varsayılan veri kaynağı**: varsayılan küme dosya sistemi.
   * **Çalışan düğüm boyutu**: çalışan düğümleri seçili VM boyutu.
   * **Baş düğüm boyutu**: baş düğümler seçili VM boyutu.
   * **Sanal ağ**: sanal ağ ve alt küme dağıtıldığı, dağıtım sırasında seçildiyse adı.

## <a name="delete-clusters"></a>Küme silme
Küme silme ya da bağlı tüm depolama hesaplarını varsayılan depolama hesabını silmez. Küme aynı depolama hesapları ve aynı meta deponuz kullanarak yeniden oluşturabilirsiniz. Kümeyi yeniden oluşturduğunuzda, yeni varsayılan Blob kapsayıcısını kullanmanızı öneririz.

1. Oturum [Portal][azure-portal].
2. Tıklatın **Hdınsight kümeleri** sol menüden. Görmüyorsanız, **Hdınsight kümeleri**, tıklatın **daha fazla hizmet** ilk.
3. Silmek istediğiniz kümesine tıklayın.
4. Tıklatın **silmek** üstteki menüden ve yönergeleri izleyin.

Ayrıca bkz. [Duraklat/kümeleri kapatma](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Başka depolama hesapları ekleme

Bir küme oluşturulduktan sonra ek Azure Storage ve Azure Data Lake Store hesapları ekleyebilirsiniz. Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme küme kümeye yeniden oluşturmak zorunda kalmadan bir Azure Hdınsight kümesi tarafından kullanılan çalışan düğümü sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, Hdınsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümünü emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).
>
>

Her tür Hdınsight tarafından desteklenen küme için veri düğüm sayısını değiştirme etkisini değişir:

* Hadoop

    Sorunsuz bir şekilde tüm bekleyen veya çalışan işler etkilemeden çalıştıran bir Hadoop kümesinde çalışan düğümü sayısını da artırabilirsiniz. İşlemi devam ederken yeni işleri da gönderilebilir. Kümenin her zaman işlevsel bir durumda bırakılır böylece bir ölçeklendirme işlemi hatalar düzgün bir şekilde ele alınır.

    Bir Hadoop kümesine veri düğüm sayısını azaltarak ölçeklendirilir, bazı kümedeki hizmetleri yeniden başlatılır. Bu davranış tüm çalışan ve işleri bekleyen ölçeklendirme işlemi tamamlandığında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, sunmaları olabilir.
* HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken, HBase kümesi düğümleri kaldırın. Bölgesel sunucular otomatik olarak ölçeklendirme işlemi tamamladıktan birkaç dakika içinde dengeli. Ancak, küme headnode için oturum açma ve bir komut istemi penceresinden aşağıdaki komutları çalıştırarak el ile de bölgesel sunucular dengeleyebilirsiniz:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

    HBase kabuğunu kullanma hakkında daha fazla bilgi için bkz: [hdınsight'ta Apache HBase örneği kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md).

* Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümeniz veri düğümleri kaldırın. Ancak, ölçekleme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    İki yolla yeniden dengelenmesi gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgelerine](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    Hdınsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![Hdınsight Storm ölçek yeniden dengeleyin](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Storm topolojisini yeniden dengelemeniz CLI komutu örnek aşağıda verilmiştir:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

**Küme ölçeklendirme**

1. Oturum [Portal][azure-portal].
2. Tıklatın **Hdınsight kümeleri** sol menüden.
3. Ölçeklendirmek istediğiniz kümesine tıklayın.
3. Tıklatın **ölçeklendirme kümesi**.
4. Girin **sayı, alt düğümleri**. Küme düğümleri sayısı Azure abonelikleri arasında değişiklik gösterir. Sınırını artırmak için fatura desteğine başvurabilirsiniz.  Maliyet bilgileri düğüm sayısını yapmış olduğunuz değişiklikleri yansıtır.

    ![Hdınsight, hadoop, hbase, storm, spark, Ölçek](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Kümeleri Duraklat/kapatma

Hadoop işlerinin çoğu yalnızca zaman zaman çalıştırmak toplu işleri ' dir. Çoğu Hadoop kümeleri için küme işleme için kullanılmayan süreyi büyük nokta vardır. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz.
Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

İşlem programı birçok yolu vardır:

* Kullanıcı Azure Data Factory. Bkz: [oluşturma isteğe bağlı Linux tabanlı Hadoop kümeleri Azure Data Factory kullanarak Hdınsight'ta](hdinsight-hadoop-create-linux-clusters-adf.md) isteğe bağlı Hdınsight oluşturmak için bağlantılı Hizmetleri.
* Azure PowerShell kullanın.  Bkz: [uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md).
* Azure CLI kullanın. Bkz: [Azure CLI kullanarak Hdınsight kümelerini yönetme](hdinsight-administer-use-command-line.md).
* Hdınsight .NET SDK'yı kullanın. Bkz: [gönderme Hadoop işlerini](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Fiyatlandırma bilgileri için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/). Portaldan bir kümeyi silmek için bkz: [küme silme](#delete-clusters)

## <a name="move-cluster"></a>Küme taşıma

Başka bir Azure kaynak grubu veya başka bir abonelik için bir Hdınsight kümesi taşıyabilirsiniz.  Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

## <a name="upgrade-clusters"></a>Küme yükseltme

Bkz: [daha yeni bir sürüme yükseltme Hdınsight kümesi](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Parolaları değiştirme
Hdınsight kümesi, iki kullanıcı hesapları olabilir. Hdınsight küme kullanıcı hesabı (paketini HTTP kullanıcı hesabı) ve SSH kullanıcı hesabı oluşturma işlemi sırasında oluşturulur. Küme kullanıcı hesabı kullanıcı adı ve parola ve SSH kullanıcı hesabını değiştirmek için betik eylemleri değiştirmek için Ambari web kullanıcı Arabirimi kullanabilirsiniz

### <a name="change-the-cluster-user-password"></a>Küme kullanıcı parolasını değiştirme
Küme kullanıcı parolasını değiştirmek için Ambari Web kullanıcı arabirimini kullanın. Ambarı'na oturum açmak için mevcut küme kullanıcı adı ve parola kullanmanız gerekir.

> [!NOTE]
> Küme kullanıcı (Yönetici) parolasını değiştirme başarısız bu kümeye karşı çalıştırmak betik eylemleri neden olabilir. Kalıcı betik eylemleri, hedef alt düğümleri varsa, bu komut dosyaları küme düğümlerine operations yeniden boyutlandırma eklediğinizde başarısız olabilir. Betik eylemleri hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemleri kullanılarak](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Ambari Web Hdınsight küme kullanıcı kimlik bilgilerini kullanarak kullanıcı Arabirimi için oturum açın. Varsayılan kullanıcı adı **admin** şeklindedir. URL **https://&lt;Hdınsight küme adı > azurehdinsight.net**.
2. Tıklatın **yönetici** üstteki menüden "Ambarı Yönet"'a tıklayın.
3. Sol menüden **kullanıcılar**.
4. Tıklatın **yönetici**.
5. Tıklatın **Parola Değiştir**.

Ambari sonra kümedeki tüm düğümlerde parolasını değiştirir.

### <a name="change-the-ssh-user-password"></a>SSH kullanıcı parolasını değiştirme
1. Bir metin düzenleyicisi kullanarak, aşağıdaki metni adlı bir dosya Kaydet **changepassword.sh**.

    > [!IMPORTANT]
    > Satır bitiş olarak LF kullanan bir Düzenleyicisi'ni kullanmanız gerekir. Düzenleyici CRLF kullanıyorsa, komut çalışmaz.

    ```bash
    #! /bin/bash
    USER=$1
    PASS=$2
    usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
    ```

2. Bir HTTP veya HTTPS adresi kullanarak Hdınsight'ta erişilebilen bir depolama konumuna dosyası yükleyin. Örneğin, ortak bir dosyanın gibi OneDrive veya Azure Blob Depolama depolayın. Bu URI sonraki adımda gerektiğinde URI (HTTP veya HTTPS adresi) dosyasına kaydedin.
3. Azure portalından tıklatın **Hdınsight kümeleri**.
4. Hdınsight kümesine tıklayın.
4. Tıklatın **betik eylemleri**.
4. Gelen **betik eylemleri** dikey penceresinde, select **gönderme yeni**. Zaman **betik eylemi Gönder** dikey penceresi görünür, aşağıdaki bilgileri girin:

   | Alan | Değer |
   | --- | --- |
   | Ad |SSH parolasını değiştirme |
   | Bash betik URI |Changepassword.sh dosyasına URI |
   | Düğümler (Head, çalışan, Nimbus, yönetici, Zookeeper, vb.) |✓ listelenen tüm düğüm türleri |
   | Parametreler |SSH kullanıcı adı ve yeni parolayı girin. Kullanıcı adı ve parola arasında bir boşluk olması gerekir. |
   | Bu betik eylemini Sürdür... |Bu alan işaretli bırakın. |
5. Seçin **oluşturma** komut dosyasını uygulamak için. Komut dosyası tamamlandıktan sonra yeni bir parola ile SSH kullanarak kümeye bağlanabilir.

## <a name="grantrevoke-access"></a>GRANT/revoke erişim
Hdınsight kümeleri (Bu hizmetlerin tümü için RESTful uç noktaları vardır) aşağıdaki HTTP web hizmetleri vardır:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Varsayılan olarak, bu hizmetleri için erişim verilir. Revoke/kullanarak erişim ver [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) ve [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Abonelik kimliği bulunamadı

**Azure aboneliğiniz kimliklerini bulmak için**

1. Oturum [Portal][azure-portal].
2. Tıklatın **abonelikleri**. Her abonelik bir ada ve bir kimliğe sahip

Her küme bir Azure aboneliğine bağlıdır. Kimliği kümede gösterilen abonelik **temel** döşeme. Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Azure Kaynak Yöneticisi modunda her Hdınsight kümesi içeren bir Azure Resource Manager grubu oluşturulur. Bir kümeye ait olduğu kaynak yöneticisi grubu görünür:

* Küme listesi olan bir **kaynak grubu** sütun.
* Küme **temel** döşeme.  

Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

## <a name="find-the-storage-accounts"></a>Depolama hesapları bulun

Hdınsight kümeleri, verileri depolamak için bir Azure Storage hesabı veya bir Azure Data Lake Store kullanın. Her Hdınsight kümesi, bir varsayılan depolama hesabı ve bağlantılı depolama hesabı sayısı sahip olabilir. Depolama hesaplarını listelemek için önce portaldan kümeyi açın ve ardından **depolama hesapları**:

![Hdınsight küme depolama hesapları](./media/hdinsight-administer-use-portal-linux/hdinsight-storage-accounts.png)

Önceki ekran görüntüsüne üzerinde var olan bir __varsayılan__ hesap varsayılan depolama hesabı olup olmadığını gösteren sütun.

Data Lake Store hesaplarını listelemek için tıklatın **Data Lake Store erişim** önceki ekran görüntüsü.

## <a name="run-hive-queries"></a>Hive sorguları çalıştırma
Doğrudan Azure Portalı'ndan Hive işi çalıştırılamıyor. ancak Ambari Web kullanıcı arabirimini Hive görünümde kullanabilirsiniz.

**Ambari Hive görünümünü kullanarak Hive sorguları çalıştırmak için**

1. Ambari Web Hdınsight küme kullanıcı kimlik bilgilerini kullanarak kullanıcı Arabirimi için oturum açın. Varsayılan kullanıcı adı **admin** şeklindedir. URL **https://&lt;Hdınsight küme adı > azurehdinsight.net**.
2. Aşağıdaki ekran görüntüsünde gösterildiği gibi Hive görünümü açın:  

    ![Hdınsight hive görünümü](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)

3. Tıklatın **sorgu** üstteki menüden.
4. Bir Hive sorgusu girin **sorgu Düzenleyicisi'ni**ve ardından **yürütme**.

## <a name="monitor-jobs"></a>İşleri izleme
Bkz: [Hdınsight kümelerini yönetme Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Gözatma dosyaları
Azure Portalı'nı kullanarak, varsayılan kapsayıcı içeriğini göz atabilirsiniz.

1. Oturum [https://portal.azure.com](https://portal.azure.com).
2. Tıklatın **Hdınsight kümeleri** var olan kümeleri listelemek için sol menüden.
3. Küme adına tıklayın. Küme listesi uzunsa, sayfanın üst kısmında filtresini kullanabilirsiniz.
4. Tıklatın **depolama hesapları** küme sol menüden.
5. Bir depolama hesabı'nı tıklatın.
7. Tıklatın **BLOB'lar** döşeme.
8. Varsayılan kapsayıcı adına tıklayın.

## <a name="monitor-cluster-usage"></a>Küme kullanımını izleme
**Kullanım** Hdınsight küme dikey bölümde aboneliğinize Hdınsight ile kullanmak için kullanılabilir çekirdek sayısı gibi bu küme ve bu küme içindeki düğümler için nasıl ayrılacağını ayrılan çekirdek sayısı hakkında bilgileri görüntüler. Bkz: [listesi ve Göster kümeleri](#list-and-show-clusters).

> [!IMPORTANT]
> Hdınsight küme tarafından sağlanan hizmetlerin izlemek için Ambari Web veya Ambari REST API kullanmanız gerekir. Ambari kullanarak daha fazla bilgi için bkz: [Ambari kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)

## <a name="connect-to-a-cluster"></a>Bir kümeye bağlanın

* [HDInsight ile Hive kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı temel yönetim işlevleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight Azure PowerShell kullanarak yönetme](hdinsight-administer-use-powershell.md)
* [Hdınsight Azure CLI kullanarak yönetme](hdinsight-administer-use-command-line.md)
* [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Ambari Web kullanıcı arabirimini kullanma hakkında daha fazla bilgi için](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API kullanımıyla ilgili ayrıntılar](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Hdınsight'ta Hive kullanma](hadoop/hdinsight-use-hive.md)
* [Hdınsight'ta pig kullanma](hadoop/hdinsight-use-pig.md)
* [Hdınsight'ta Sqoop kullanma](hadoop/hdinsight-use-sqoop.md)
* [Azure Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Hdınsight'ta Hadoop hangi sürümünün mi?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop komut satırı"
