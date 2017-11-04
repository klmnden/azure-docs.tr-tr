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
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: af75d63caca24f389345c964e2dc506a255bec19
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Etki alanına katılmış Hdınsight kümeleri (Önizleme) yapılandırma

Azure Active Directory (Azure AD) ile Azure Hdınsight kümesi ayarlama öğrenin ve [Apache bırakabilmenizi](http://hortonworks.com/apache/ranger/) güçlü kimlik doğrulaması ve zengin rol tabanlı erişim denetimi (RBAC) ilkelerini yararlanmak için.  Etki alanına katılmış Hdınsight yalnızca Linux tabanlı kümelerde yapılandırılabilir. Daha fazla bilgi için bkz: [tanıtmak etki alanına katılmış Hdınsight kümeleri](apache-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

Bu makalede, bir dizinin ilk öğreticide şöyledir:

* Apache bırakabilmenizi etkin (aracılığıyla Azure Directory etki alanı Hizmetleri yeteneği) Azure AD bağlı Hdınsight kümesi oluşturun.
* Oluşturun ve Apache bırakabilmenizi aracılığıyla Hive ilkeleri uygulanır ve kullanıcıların (örneğin, veri bilimcilerine) için Hive ODBC tabanlı araçlar, örneğin Excel, Tableau vb. kullanarak bağlanmasına izin vermek. Microsoft, HBase, Spark ve Storm, gibi diğer iş yüklerinin en kısa sürede etki alanına katılmış Hdınsight için ekleme çalışmaktadır.

Son topoloji örneği şu şekilde görünür:

![Etki alanına katılmış Hdınsight topolojisi](./media/apache-domain-joined-configure/hdinsight-domain-joined-topology.png)

Şu anda Azure AD Klasik sanal ağlar (Vnet'ler) yalnızca destekler ve yalnızca destek Linux tabanlı Hdınsight kümeleri için Azure Resource Manager tabanlı sanal ağlar, iki sanal ağlar ve bunlar arasında eşleme Hdınsight Azure AD tümleştirme gerektirir. Karşılaştırma için iki dağıtım modelleri arasında bilgi [Azure Resource Manager ve klasik dağıtım: dağıtım modelleri ve kaynaklarınızın durumunu anlamak](../../azure-resource-manager/resource-manager-deployment-model.md). İki Vnet Azure AD DS ile aynı bölgede olması gerekir.

Azure hizmet adları benzersiz olmalıdır. Aşağıdaki adlar, bu öğreticide kullanılır. Contoso adlı kurgusal bir addır. Değiştirmeniz gereken *contoso* öğreticide olduğunuzda farklı bir ada sahip. 

**Adları:**

| Özellik | Değer |
| --- | --- |
| Azure AD VNet |contosoaadvnet |
| Azure AD Vnet kaynak grubu |contosoaadrg |
| Azure AD dizini |contosoaaddirectory |
| Azure AD etki alanı adı |contoso (contoso.onmicrosoft.com) |
| Hdınsight VNet |contosohdivnet |
| Hdınsight VNet kaynak grubu |contosohdirg |
| Hdınsight kümesi |contosohdicluster |

Bu öğretici, bir etki alanına katılmış Hdınsight kümesi yapılandırmak için adımları sağlar. Her bölümde diğer makalelerinin bağlantıları ile ilgili daha fazla bilgi bulunur.

## <a name="prerequisite"></a>Önkoşul:
* İle öğrenmeniz [Azure AD etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) kendi [fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-ds/) yapısı.
* Aboneliğiniz bu genel Önizleme için Güvenilenler listesine olduğundan emin olun. E-posta göndererek yapabilirsiniz hdipreview@microsoft.com abonelik kimliğinizi içeren
* Etki alanınız için imzalama yetkilisi tarafından imzalanmış bir SSL sertifikası. Güvenli LDAP yapılandırarak sertifika gereklidir. Otomatik olarak imzalanan sertifikalar kullanılamaz.

## <a name="procedures"></a>Yordamları
1. Azure AD için Azure klasik bir VNet oluşturun.  
2. Oluşturun ve Azure AD ve Azure AD DS yapılandırın.
3. Bir Hdınsight VNet Azure kaynak yönetimi modunda oluşturun.
4. İki Vnet eş.
5. Hdınsight kümesi oluşturun.

> [!NOTE]
> Bu öğretici, Azure AD olmadığını varsayar. Varsa, 2. adımda bölümüne atlayabilirsiniz.
> 
> 

## <a name="create-an-azure-virtual-network-classic"></a>Bir Azure sanal ağ (Klasik) oluşturun
Bu bölümde, bir sanal ağ (Klasik) Azure Portalı'nı kullanarak da oluşturun. Sonraki bölümde, sanal ağda Azure AD için Azure AD DS etkinleştirin. Aşağıdaki yordamda ve diğer sanal ağ oluşturma yöntemlerini kullanma hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**Klasik bir VNet oluşturmak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın. 
2. Tıklatın **yeni** > **ağ** > **sanal ağ**.
3. İçinde **dağıtım modeli seçin**seçin **Klasik**ve ardından **oluşturma**.
4. Aşağıdaki değerleri yazın veya seçin:
   
   * **Ad**: contosoaadvnet
   * **Adres alanı**: 10.1.0.0/16
   * **Alt ağ adı**: Subnet1
   * **Alt ağ adres aralığı**: 10.1.0.0/24
   * **Abonelik**: (Bu VNet oluşturmak için kullanılan bir abonelik seçin.)
   * **Kaynak grubu**: contosoaadrg
   * **Konum**: (Hdınsight kümeniz için bir bölge seçin.)
     
     > [!IMPORTANT]
     > Azure AD DS destekleyen bir konum seçmeniz gerekir. Daha fazla bilgi için bkz. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Klasik VNet ve kaynak grubu VNet Azure AD DS ile aynı bölgede olması gerekir.
     > 
     > 
5. VNet oluşturmak için **Oluştur**’a tıklayın.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Oluşturma ve Azure AD için Azure AD DS yapılandırma
Bu bölümde şunları yapacaksınız:

1. Azure AD oluşturun.
2. Azure AD kullanıcıları oluşturun. Bu etki alanı kullanıcıyı görürsünüz. İlk kullanıcı, Azure AD ile Hdınsight kümesi yapılandırmak için kullanın.  Diğer iki kullanıcılar Bu öğretici için isteğe bağlıdır. İçinde kullanılacak [etki alanına katılmış Hdınsight kümeleri için ilkeler yapılandırma Hive](apache-domain-joined-run-hive.md) Apache bırakabilmenizi ilkeleri yapılandırırken.
3. AAD DC Yöneticiler grubu oluşturun ve Azure AD kullanıcı grubuna ekleyin. Bu kullanıcı, kuruluş birimi oluşturmak için kullanın.
4. Azure AD için Azure AD etki alanı Hizmetleri (Azure AD DS) etkinleştirin.
5. LDAPS Azure AD için yapılandırın. Basit Dizin Erişim Protokolü (LDAP) okuma ve Azure AD ile yazmak için kullanılır.

Var olan bir Azure AD kullanmayı tercih ederseniz, 1 ve 2. adımları atlayabilirsiniz.

**Azure AD oluşturmak için**

1. Gelen [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **yeni** > **uygulama hizmetleri** > **Active Directory**  >  **Directory** > **özel Oluştur**. 
2. Aşağıdaki değerleri yazın veya seçin:
   
   * **Ad**: contosoaaddirectory
   * **Etki alanı adı**: contoso.  Bu ad genel olarak benzersiz olmalıdır.
   * **Ülke veya bölge**: ülkenizi veya bölgenizi seçin.
3. **Tamamla**’ya tıklayın.

**Bir Azure AD kullanıcı oluşturun**

1. Gelen [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **Active Directory** -> **contosoaaddirectory**. 
2. Tıklatın **kullanıcılar** üstteki menüden.
3. Tıklatın **kullanıcı ekleme**.
4. Girin **kullanıcı adı**ve ardından **sonraki**. 
5. Kullanıcı profili yapılandırın; İçinde **rol**seçin **genel yönetici**; ve ardından **sonraki**.  Genel yönetici rolü, kuruluş birimleri oluşturmak için gereklidir.
6. Tıklatın **oluşturma** geçici bir parola almak için.
7. Parolanın bir kopyasını alın ve ardından **tam**. Daha sonra Bu öğreticide, Hdınsight kümesi oluşturmak için bu genel yönetici kullanıcı kullanır.

İle iki daha fazla kullanıcı oluşturmak için aynı yordamı izleyin **kullanıcı** rol, hiveuser1 ve hiveuser2. Şu kullanıcılar kullanılacak [etki alanına katılmış Hdınsight kümeleri için yapılandırma Hive ilkeleri](apache-domain-joined-run-hive.md).

**AAD DC Yöneticiler grubu oluşturmak ve bir Azure AD kullanıcı eklemek için**

1. Gelen [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **Active Directory** > **contosoaaddirectory**. 
2. Tıklatın **grupları** üstteki menüden.
3. Tıklatın **grup ekleme** veya **grubu eklemek**.
4. Aşağıdaki değerleri yazın veya seçin:
   
   * **Ad**: AAD DC yöneticileri.  Grup adı değişmez.
   * **Grup türü**: güvenlik.
5. **Tamamla**’ya tıklayın.
6. Tıklatın **AAD DC Yöneticiler** grubu açın.
7. Tıklatın **üye eklemek**.
8. Önceki adımda oluşturduğunuz ilk kullanıcıyı seçin ve ardından **tam**.
9. Adlı başka bir grup oluşturmak için aynı adımı tekrarlayın **HiveUsers**, ve iki Hive kullanıcı grubuna ekleyin.

Daha fazla bilgi için bkz: [Azure AD etki alanı Hizmetleri (Önizleme) - 'AAD DC Yöneticiler' Grup Oluştur](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Azure AD için Azure AD DS etkinleştirmek için**

1. Gelen [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **Active Directory** > **contosoaaddirectory**. 
2. Tıklatın **yapılandırma** üstteki menüden.
3. Ekranı aşağı kaydırarak **etki alanı Hizmetleri**ve aşağıdaki değerleri ayarlayın:
   
   * **Bu dizin için etki alanı Hizmetleri'ni etkinleştirme**: Evet.
   * **DNS etki alanı adı, etki alanı Hizmetleri'nin**: Bu Azure dizinin varsayılan DNS adını gösterir. Örneğin, contoso.onmicrosoft.com.
   * **Etki Alanı Hizmetleri bu sanal ağa bağlanmak**: daha önce oluşturduğunuz Klasik sanal ağı seçin yani **contosoaadvnet**.
4. Tıklatın **kaydetmek** sayfanın en altındaki. Göreceğiniz **bekleniyor...**  yanına **bu dizin için etki alanı Hizmetleri'ni etkinleştirme**.  
5. Bekle **bekleniyor...**  kaybolur, ve **IP adresi** doldurulmuş. İki IP adresi doldurulacak. Bu etki alanı Hizmetleri tarafından sağlanan etki alanı denetleyicilerinin IP adresleridir. Karşılık gelen etki alanı denetleyicisi sağlanan ve hazır olduktan sonra her IP adresi görünür. İki IP adresi yazın. Bunları daha sonra ihtiyacınız olacak.

Daha fazla bilgi için bkz: [etkinleştirmek Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Parola Eşitleme**

Kendi etki alanı kullanıyorsanız, parola eşitleme gerekir. Bkz: [için bir yalnızca bulut Azure Azure AD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme AD dizini](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**Azure AD için LDAPS yapılandırmak için**

1. Etki alanınız için imzalama yetkilisi tarafından imzalanmış bir SSL sertifikası alın. Kendinden imzalı bir sertifika kullanmak istiyorsanız, lütfen ulaşmak hdipreview@microsoft.com için bir özel durum.
2. Gelen [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **Active Directory** > **contosoaaddirectory**. 
3. Tıklatın **yapılandırma** üstteki menüden.
4. Kaydırma **etki alanı Hizmetleri**.
5. Tıklatın **yapılandırma sertifika**.
6. Sertifika dosyası ve parola belirtmek için yönergeleri izleyin. Göreceğiniz **bekleniyor...**  yanına **bu dizin için etki alanı Hizmetleri'ni etkinleştirme**.  
7. Bekle **bekleniyor...**  kaybolur, ve **güvenli LDAP sertifikası** doldurulmuş.  Bu, 10 dakika veya daha fazla alabilir.

> [!NOTE]
> Azure AD DS'de bazı arka plan görevleri çalıştırmak, sertifikası karşıya yükleniyor - çalışırken bir hata görebilirsiniz <i>bu Kiracı için gerçekleştirilen bir işlem yoktur. Lütfen daha sonra yeniden deneyin</i>.  Lütfen bu hatanın oluştuğu durumda süre sonra yeniden deneyin. İkinci etki alanı denetleyicisi IP sağlanacak 3 saat sürebilir.
> 
> 

Daha fazla bilgi için bkz: [yapılandırma güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Hdınsight kümesi için Resource Manager Vnet'i oluşturma
Bu bölümde, Hdınsight kümesi için kullanılacak bir Azure Resource Manager Vnet'i oluşturur. Azure başka yöntemler kullanarak VNET oluşturma hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Sanal ağ oluşturduktan sonra Azure AD VNet için aynı DNS sunucularını kullanmak için Resource Manager Vnet'i yapılandırır. Klasik VNet ve Azure AD oluşturmak için Bu öğreticide adımları izlediyseniz, DNS sunucularını 10.1.0.4 ve 10.1.0.5 ' dir.

**Resource Manager Vnet'i oluşturmak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **yeni**, **ağ**ve ardından **sanal ağ**. 
3. İçinde **dağıtım modeli seçin**seçin **Resource Manager**ve ardından **oluşturma**.
4. Aşağıdaki değerleri yazın veya seçin:
   
   * **Ad**: contosohdivnet
   * **Adres alanı**: 10.2.0.0/16. Adres aralığı Klasik VNet IP adresi aralığıyla çakışamaz emin olun.
   * **Alt ağ adı**: Subnet1
   * **Alt ağ adres aralığı**: 10.2.0.0/24
   * **Abonelik**: (Azure aboneliğinizi seçin.)
   * **Kaynak grubu**: contosohdirg
   * **Konum**: (Azure AD Vnet'iniz, yani contosoaadvnet aynı konumu seçin.)
5. **Oluştur**'a tıklayın.

**DNS için Resource Manager Vnet'i yapılandırmak için**

1. Gelen [Azure portal](https://portal.azure.com), tıklatın **daha fazla hizmet** -> **sanal ağlar**. Değil tıklattığınızdan emin **sanal ağları (Klasik)**.
2. Tıklatın **contosohdivnet**.
3. Tıklatın **DNS sunucuları** yeni dikey pencerenin sol taraftaki.
4. Tıklatın **özel**ve ardından aşağıdaki değerleri girin:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Bu DNS sunucusu IP adreslerini (Klasik VNet) Azure AD VNet DNS sunucuları için aynı olmalıdır.
5. **Kaydet** düğmesine tıklayın.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Azure AD VNet ve Hdınsight VNet eş
**İki VNet eş için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **daha fazla hizmet** sol menüden.
3. Tıklatın **sanal ağlar**. Tıklamayın **sanal ağları (Klasik)**.
4. Tıklatın **contosohdivnet**.  Hdınsight VNet budur.
5. Tıklatın **eşlemeler** dikey pencerenin sol menüde.
6. Tıklatın **Ekle** üstteki menüden. Bu açılır **eklemek eşliği** dikey.
7. Üzerinde **eklemek eşliği** dikey penceresinde, ayarlama veya aşağıdaki değerleri seçin:
   
   * **Ad**: ContosoAADHDIVNetPeering
   * **Sanal ağ dağıtım modeli**: Klasik
   * **Abonelik**: Klasik (Azure AD) vnet için kullanılan, abonelik adını seçin.
   * **Sanal ağ**: contosoaadvnet.
   * **Sanal ağ erişimine izin ver**: (denetleyin)
   * **İletilen trafiğe izin**: (denetleyin). Diğer iki onay işaretini kaldırın.
8. **Tamam** düğmesine tıklayın.

## <a name="create-hdinsight-cluster"></a>Hdınsight kümesi oluşturma
Bu bölümde Azure portalı kullanarak hdınsight'ta Linux tabanlı Hadoop kümesi oluşturma veya [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma yöntemleri ve ayarlarını anlama, bkz: [Hdınsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md). Resource Manager şablonu kullanarak Hdınsight'ta Hadoop kümeleri oluşturma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları kullanarak Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Azure Portalı'nı kullanarak bir etki alanına katılmış Hdınsight kümesi oluşturmak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **yeni**, **Intelligence + analiz**ve ardından **Hdınsight**.
3. Gelen **yeni Hdınsight kümesi** dikey penceresinde, aşağıdaki değerleri seçin veya girin:
   
   * **Küme adı**: etki alanına katılmış Hdınsight kümesi için yeni bir küme adı girin.
   * **Abonelik**: Bu küme oluşturmak için kullanılan Azure aboneliğini seçin.
   * **Küme Yapılandırması**:
     
     * **Küme türü**: Hadoop. Etki alanına katılmış Hdınsight şu anda yalnızca üzerinde desteklenen Hadoop kümeleri değil.
     * **İşletim sistemi**: Linux.  Etki alanına katılmış Hdınsight yalnızca Linux tabanlı Hdınsight kümelerinde desteklenir.
     * **Sürüm**: HDI 3.6. Etki alanına katılmış Hdınsight yalnızca Hdınsight kümesi sürüm 3.6 desteklenir.
     * **Küme türü**: PREMIUM
       
       Tıklatın **seçin** değişiklikleri kaydedin.
   * **Kimlik bilgileri**: küme kullanıcı ve SSH kullanıcı kimlik bilgilerini yapılandırın.
   * **Veri kaynağı**: yeni bir depolama hesabı oluşturun veya varolan bir depolama hesabı Hdınsight kümesi için varsayılan depolama hesabı olarak kullanın. Konumun iki sanal ağlar aynı olmalıdır.  Konum de Hdınsight kümesi konumdur.
   * **Fiyatlandırma**: kümenizin çalışan düğümü sayısını seçin.
   * **Yapılandırmaları Gelişmiş**: 
     
     * **Etki alanına katılma & sanal/alt**: 
       
       * **Etki alanı ayarları**: 
         
         * **Etki alanı adı**: contoso.onmicrosoft.com
         * **Etki alanı kullanıcı adı**: bir etki alanı kullanıcı adı girin. Bu etki alanı aşağıdaki ayrıcalıklara sahip olmalıdır: Makine etki alanına katılma ve küme oluşturma sırasında; belirttiğiniz kuruluş birimine yerleştirin Küme oluşturma sırasında belirttiğiniz kuruluş birimi içinde hizmet sorumluları oluşturmak; Geriye doğru DNS girdilerini oluşturun. Bu etki alanı kullanıcısı bu etki alanına katılmış Hdınsight küme yöneticisinin olur.
         * **Etki alanı parolası**: etki alanı kullanıcı parolası girin.
         * **Kuruluş birimi**: Hdınsight kümesi ile kullanmak istediğiniz kuruluş Biriminin ayırt edici adını girin. Örneğin: OU HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Bu OU mevcut değilse, Hdınsight kümesi bu OU oluşturmayı deneyecek. OU zaten var veya etki alanı hesabı yeni bir tane oluşturmak için izinlere sahip olduğundan emin olun. AADDC Yöneticiler parçası olan etki alanı hesabı kullanırsanız, OU oluşturmak için gerekli izinlere sahip.
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Access kullanıcı grubuna**: kullanıcıların kümeye eşitlemek istediğiniz güvenlik grubunu belirtin. Örneğin, HiveUsers.
           
           Tıklatın **seçin** değişiklikleri kaydedin.
           
           ![Etki alanına katılmış Hdınsight portal etki alanı ayarını yapılandırın](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Sanal ağ**: contosohdivnet
       * **Alt ağ**: Subnet1
         
         Tıklatın **seçin** değişiklikleri kaydedin.        
         Tıklatın **seçin** değişiklikleri kaydedin.
   * **Kaynak grubu**: Hdınsight VNet (contosohdirg) için kullanılan kaynak grubunu seçin.
4. **Oluştur**'a tıklayın.  

Etki alanına katılmış Hdınsight kümesi oluşturmak için başka bir seçenek, Azure kaynak yönetimi şablonu kullanmaktır. Aşağıdaki yordam, nasıl gösterir:

**Kaynak Yönetimi şablonunu kullanarak bir etki alanına katılmış Hdınsight kümesi oluşturmak için**

1. Azure portalında bir Resource Manager şablonunu açmak için aşağıdaki görüntüye tıklayın. Resource Manager şablonu bir ortak blob kapsayıcısında bulunur. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Gelen **parametreleri** dikey penceresinde, aşağıdaki değerleri girin:
   
   * **Abonelik**: (Azure aboneliğinizi seçin).
   * **Kaynak grubu**: tıklatın **var olanı kullan**, ve şimdiye aynı kaynak grubu belirtin.  Örneğin contosohdirg. 
   * **Konum**: bir kaynak grubu konumu belirtin.
   * **Küme Adı**: Oluşturacağınız Hadoop kümesi için bir ad girin. Örneğin contosohdicluster.
   * **Küme türü**: bir küme türü seçin.  Varsayılan değer **hadoop**.
   * **Konum**: küme için bir konum seçin.  Varsayılan depolama hesabı aynı konuma kullanır.
   * **Çalışan düğüm sayısı küme**: çalışan düğümü sayısını seçin.
   * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı **admin** şeklindedir.
   * **SSH kullanıcı adı ve parolası**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz. 
   * **Sanal ağ kimliği**: /subscriptions/&lt;Abonelikkimliği > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;vnetname adlı >
   * **Sanal ağ alt**: /subscriptions/&lt;Abonelikkimliği > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;vnetname adlı >/alt ağlar/Subnet1
   * **Etki alanı adı**: contoso.onmicrosoft.com
   * **Kuruluş birimi DN**: OU HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com =
   * **Küme Users grubunun DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (etki alanı yönetici kullanıcı adı girin)
   * **DomainAdminPassword**: (etki alanı yönetici kullanıcı parolasını girin)
   * **Hüküm ve koşulları yukarıda belirtildiği ediyorum**: (denetleyin)
   * **Panoya Sabitle**: (denetleyin)
3. **Satın al**’a tıklayın. **Şablon Dağıtımı’nı dağıtma** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer. Küme oluşturulduktan sonra küme dikey penceresini açmak için portalda tıklatabilirsiniz.

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Küme silme ilişkin yönergeler için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerine bağlanmak için SSH kullanmak için bkz: [Linux, Unix veya OS X Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

