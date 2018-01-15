---
title: "Etki alanına katılmış Hdınsight kümeleri - Azure yapılandırma | Microsoft Docs"
description: "Ayarlama ve etki alanına katılmış Hdınsight kümeleri yapılandırma hakkında bilgi edinin"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: saurinsh
ms.openlocfilehash: 4921e329c2ec8ce3d5bbf8a0851146e13d5f6cd3
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Etki alanına katılmış Hdınsight sandbox ortamını yapılandırma

Tek başına Active Directory ile Azure Hdınsight kümesi ayarlama öğrenin ve [Apache bırakabilmenizi](http://hortonworks.com/apache/ranger/) güçlü kimlik doğrulaması ve zengin rol tabanlı erişim denetimi (RBAC) ilkelerini yararlanmak için. Daha fazla bilgi için bkz: [tanıtmak etki alanına katılmış Hdınsight kümeleri](apache-domain-joined-introduction.md).

Hdınsight kümesi etki alanına katılmış olmadan her küme yalnızca bir Hadoop HTTP kullanıcı hesabı ve bir SSH kullanıcı hesabı olabilir.  Birden çok kullanıcı kimlik doğrulaması kullanılarak elde edilir:

-   Tek başına bir Active Directory Azure Iaas üzerinde çalışıyor.
-   Azure Active Directory.
-   Müşterinin şirket içi ortamda çalışan active Directory.

Tek başına bir Active Directory kullanarak Azure Iaas üzerinde çalışan bu makalede ele alınmıştır. Bu, bir müşteri Hdınsight'ta çok kullanıcılı destek almak için izleyebileceğiniz basit mimarisidir. Bu makalede, bu yapılandırma için iki yaklaşım kapsar:

- Seçenek 1: tek başına active directory ve Hdınsight kümesi oluşturmak için bir Azure kaynak yönetimi şablonu kullanın.
- Seçenek 2: Tüm işlem aşağıdaki adımları ayrılır:
    - Bir şablonu kullanarak bir Active Directory oluşturun.
    - LDAPS ayarlayın.
    - AD kullanıcıları ve grupları oluşturma
    - Hdınsight kümesi oluşturma

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

## <a name="prerequisite"></a>Önkoşul
* Azure aboneliği

## <a name="option-1-one-step-approach"></a>Seçenek 1: tek adımlı yaklaşımı
Bu bölümde, Azure portalından bir Azure kaynak yönetimi şablonunu açın. Şablon tek başına bir Active Directory oluşturmak için kullanılır ve Hdınsight kümesi. Şu anda etki alanına katılmış Hadoop kümesi, Spark kümesi ve etkileşimli sorgu kümesi oluşturabilirsiniz.

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın. Şablon bulunan [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/).
   
    Spark kümesi oluşturmak için:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Etkileşimli sorgu küme oluşturmak için:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Hadoop kümesi oluşturmak için:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Değerleri girin, seçin **hüküm ve koşulları yukarıda belirtildiği ediyorum**seçin **panoya Sabitle**ve ardından **satın alma**. Fare imlecinizi açıklamaları görmek için bir açıklama işareti alanlarının yanındaki getirin. Değerleri çoğunu doldurulduğunu. Varsayılan değerleri veya kendi değerlerinizi kullanabilirsiniz.

    - **Kaynak grubu**: bir Azure kaynak grubu adı girin.
    - **Konum**: yakın olan bir konum seçin.
    - **Yeni depolama hesabı adı**: bir Azure depolama hesabı adı girin. Bu yeni depolama hesabı varsayılan depolama hesabı olarak PDC, BDC ve Hdınsight kümesi tarafından kullanılır.
    - **Yönetici kullanıcı adı**: etki alanı yönetici kullanıcı adı girin.
    - **Yönetici parolası**: etki alanı yönetici parolası girin.
    - **Etki alanı adı**: varsayılan ad *contoso.com*.  Etki alanı adını değiştirirseniz, aynı zamanda güncelleştirmelisiniz **güvenli LDAP sertifikası** alan ve **kuruluş birimi DN** alan.
    - **Küme adı**: Hdınsight küme adını girin.
    - **Küme türü**: Bu değeri değiştirmeyin. Küme türü değiştirmek istiyorsanız, son adımda belirli bir şablon kullanın.

    Bazı değerleri şablonda sabit kodlanmış, örneğin, çalışan düğümü örnek sayısı iki.  Sabit kodlanmış değerler değiştirmek için tıklatın. **Düzen şablonu**.

    ![Hdınsight küme etki alanına katılmış Düzen şablonu](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Şablonu başarıyla tamamlandıktan sonra kaynak grubunda oluşturduğunuz 23 kaynak yok.

## <a name="option-2-multi-step-approach"></a>Seçenek 2: çok adımlı yaklaşımı

Bu bölümde dört adım vardır:

1. Bir şablonu kullanarak bir Active Directory oluşturun.
2. LDAPS ayarlayın.
3. AD kullanıcıları ve grupları oluşturma
4. Hdınsight kümesi oluşturma

### <a name="create-an-active-directory"></a>Bir Active Directory oluşturun

Azure Resource Manager şablonu, Azure kaynak oluşturmak kolaylaştırır. Bu bölümde, kullandığınız bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) yeni bir orman ve etki alanı ile iki sanal makine oluşturmak için. İki sanal makine, bir birincil etki alanı denetleyicisi ve bir yedek etki alanı denetleyicisi olarak görev yapar.

**Bir etki alanı ile iki etki alanı denetleyicisi oluşturmak için**

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Şablon şuna benzer:

    ![Hdınsight etki alanına katılmış ormanın etki alanı sanal makineler oluşturun](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Aşağıdaki değerleri girin:

    - **Abonelik**: Bir Azure aboneliği seçin.
    - **Kaynak grubu adı**: bir kaynak grubu adı yazın.  Bir kaynak grubu bir projeyle ilgili Azure kaynaklarınızı yönetmek için kullanılır.
    - **Konum**: yakın olan bir Azure konumu seçin.
    - **Yönetici kullanıcı adı**: Bu, etki alanı yönetici kullanıcı adınızdır. Bu kullanıcı Hdınsight kümenize HTTP kullanıcı hesabı değil. Bu öğreticiyi kullanın hesabıdır.
    - **Yönetici parolası**: etki alanı yöneticisi parolasını girin.
    - **Etki alanı adı**: iki parçalı ad etki alanı adı olmalıdır. Örneğin: contoso.com veya contoso.local ya da hdinsight.test.
    - **DNS öneki**: bir DNS öneki yazın
    - **PDC RDP bağlantı noktası**: (Bu öğretici için varsayılan değeri kullanın)
    - **BDC RDP bağlantı noktası**: (Bu öğretici için varsayılan değeri kullanın)
    - **yapıları konumu**: (Bu öğretici için varsayılan değeri kullanın)
    - **yapıları konumu SAS belirteci**: (Bu alanı boş Bu öğretici için bırakın.)

Kaynak oluşturmak için yaklaşık 20 dakika sürer.

### <a name="setup-ldaps"></a>LDAPS Kurulumu

Basit Dizin Erişim Protokolü (LDAP) okuma ve AD için yazmak için kullanılır.

**Uzak Masaüstü kullanarak PDC bağlanmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kaynak grubunu açın ve ardından birincil etki alanı denetleyicisi (PDC) sanal makine açın. Varsayılan PDC adPDC adıdır. 
3. Tıklatın **Bağlan** Uzak Masaüstü kullanarak PDC bağlanmak için.

    ![Hdınsight etki alanına katılmış PDC Uzak Masaüstü bağlantı](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Active Directory Sertifika Hizmetleri eklemek için**

4. Açık **Sunucu Yöneticisi'ni** değil açılırsa.
5. Tıklatın **Yönet**ve ardından **rol ve Özellik Ekle**.

    ![Hdınsight etki alanına katılmış rol ve Özellik Ekle](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. "Başlamadan önce", tıklatın **sonraki**.
6. Seçin **rol tabanlı veya özellik tabanlı yükleme**ve ardından **sonraki**.
7. PDC seçin ve ardından **sonraki**.  Varsayılan PDC adPDC adıdır.
8. Seçin **Active Directory Sertifika Hizmetleri**.
9. Tıklatın **Özellik Ekle** açılan iletişim kutusundan.
10. Sihirbazı izleyin, yordamın geri kalanını için varsayılan ayarları kullanın.
11. Sihirbazı kapatmak için **Kapat**'a tıklayın.

**AD sertifikası yapılandırmak için**

1. Sunucu Yöneticisi'nden, sarı bildirim simgesine tıklayın ve ardından **yapılandırma Active Directory Sertifika Hizmetleri**.

    ![Hdınsight etki alanına katılmış AD sertifikası yapılandırma](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Tıklatın **rol hizmetlerini** sol tarafta seçin **sertifika yetkilisi**ve ardından **sonraki**.
3. Sihirbazı izleyin, yordamın geri kalanını için varsayılan ayarları kullanın (tıklatın **yapılandırma** son adımı sırasında).
4. Sihirbazı kapatmak için **Kapat**'a tıklayın.

### <a name="optional-create-ad-users-and-groups"></a>(İsteğe bağlı) AD kullanıcıları ve grupları oluşturma

**AD kullanıcıları ve grupları oluşturma**
1. Uzak Masaüstü kullanarak PDC Bağlan
1. Açık **Active Directory Kullanıcıları ve Bilgisayarları**.
2. Sol bölmede, etki alanı adı seçin.
3. Tıklatın **Geçerli kapsayıcıda yeni bir kullanıcı oluşturmak** üst menü çubuğunda simge.

    ![Hdınsight etki alanına katılmış kullanıcılar oluşturma](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Birkaç kullanıcı oluşturmak için yönergeleri izleyin. Örneğin, hiveuser1 ve hiveuser2.
5. Tıklatın **Geçerli kapsayıcıda yeni bir grup oluşturun** üst menü çubuğunda simge.
6. Adlı bir grup oluşturmak için yönergeleri izleyin **HDInsightUsers**.  Daha sonra Bu öğreticide bir Hdınsight kümesi oluştururken bu grubu kullanılır.

> [!IMPORTANT]
> Bir etki alanına katılmış Hdınsight küme oluşturmadan önce PDC sanal makinenin yeniden başlatılması gerekir.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Hdınsight kümesi VNet oluşturma

Bu bölümde, öğreticinin önceki bölümlerinde Resource Manager şablonu kullanılarak oluşturulan sanal ağ bir Hdınsight kümesine eklemek için Azure Portalı'nı kullanın. Bu makalede, yalnızca etki alanına katılmış kümesi yapılandırması için belirli bilgiler yer almaktadır.  Genel bilgi için bkz: [Azure portalını kullanarak Hdınsight oluşturma Linux tabanlı kümelerde](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**Bir etki alanına katılmış Hdınsight kümesi oluşturmak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Öğreticinin önceki bölümlerinde Resource Manager şablonunu kullanarak oluşturduğunuz kaynak grubunu açın.
3. Hdınsight kümesi kaynak grubuna ekleyin.
4. Seçin **özel** seçeneği:

    ![Hdınsight etki alanına katılmış özel oluştur seçeneği](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Özel yapılandırma seçeneğini kullanarak altı bölümleri vardır: temelleri, depolama, uygulama, küme boyutu, Gelişmiş ayarları ve özeti.
5. İçinde **Temelleri** bölümü:

    - Küme türü: seçin **Kurumsal güvenlik paketi**. Şu anda Kurumsal güvenlik paketi yalnızca aşağıdaki küme türleri için etkin hale getirilebilir: Hadoop, etkileşimli sorgu ve Spark.

        ![Hdınsight etki alanına katılmış Kurumsal güvenlik paketi](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Oturum açma kullanıcı küme: Hadoop HTTP kullanıcı budur. Bu hesap, etki alanı yönetici hesabıyla farklıdır.
    - Kaynak grubu: daha önce Resource Manager şablonunu kullanarak oluşturduğunuz kaynak grubunu seçin.
    - Konumu: Konumu sanal ağ oluştururken kullandığınız adla aynı olması ve Resource Manager şablonu kullanarak DC'leri gerekir.

6. İçinde **Gelişmiş ayarları** bölümü:

    - Etki alanı ayarları:

        ![Hdınsight etki alanı Gelişmiş ayarları etki alanına katılmış](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Etki alanı adı: içinde kullanılan etki alanı adı girin [bir Active Directory oluşturma](#create-an-active-directory).
        - Etki alanı kullanıcı adı: içinde kullanılan AD yönetici kullanıcı adı girin [bir Active Directory oluşturma](#create-an-active-directory).
        - Kuruluş birimi: bir örnek ekran görüntüsüne bakın.
        - LDAPS URL'si: ekran görüntüsü bir örnek için bkz.
        - Access kullanıcı grubuna: oluşturduğunuz kullanıcı grubu adı girin [oluşturma AD kullanıcıları ve grupları](#optionally-createad-users-and-groups)
    - Sanal ağ: oluşturduğunuz sanal ağı seçin [bir Active Directory oluşturma](#create-an-active-directory). Şablonda kullanılan varsayılan addır **adVNET**.
    - Alt ağ: şablonda kullanılan varsayılan addır **adSubnet**.



Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Küme silme ilişkin yönergeler için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerine bağlanmak için SSH kullanmak için bkz: [Linux, Unix veya OS X Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

