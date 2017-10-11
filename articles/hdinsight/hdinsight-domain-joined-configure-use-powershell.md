---
title: "PowerShell - Azure kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma | Microsoft Docs"
description: "Ayarlama ve Azure PowerShell kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma konusunda bilgi edinin"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 9da76bb5f649817cd2f027f3d0eb46d58a996b4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Azure PowerShell kullanarak etki alanına katılmış Hdınsight kümeleri (Önizleme) yapılandırma
Azure Active Directory (Azure AD) ile Azure Hdınsight kümesi ayarlama öğrenin ve [Apache bırakabilmenizi](http://hortonworks.com/apache/ranger/) Azure PowerShell kullanarak. Bir Azure PowerShell Betiği yapılandırmanın hızlı ve daha az hata potansiyeli yapmak için sağlanır. Etki alanına katılmış Hdınsight yalnızca Linux tabanlı kümelerde yapılandırılabilir. Daha fazla bilgi için bkz: [tanıtmak etki alanına katılmış Hdınsight kümeleri](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

Tipik bir etki alanına katılmış Hdınsight küme yapılandırması, aşağıdaki adımları içerir:

1. Azure AD için Azure klasik bir VNet oluşturun.  
2. Oluşturun ve Azure AD ve Azure AD DS yapılandırın.
3. Kuruluş birimi oluşturmak için Klasik VNet VM ekleyin. 
4. Azure AD DS için bir kuruluş birimi oluşturun.
5. Bir Hdınsight VNet Azure kaynak yönetimi modunda oluşturun.
6. Geriye doğru DNS bölgeleri için Azure AD DS ayarlayın.
7. İki Vnet eş.
8. Hdınsight kümesi oluşturun.

Sağlanan PowerShell komut dosyası, 3 ile 7 arasındaki adımları gerçekleştirir. 1 ve 2. adım el ile gitmeniz gerekir.  Azure PowerShell kullanmayı tercih ederseniz, bkz. [yapılandırma etki alanına katılmış Hdınsight kümeleri](hdinsight-domain-joined-configure.md). 

Son topoloji örneği şu şekilde görünür:

![Etki alanına katılmış Hdınsight topolojisi](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Şu anda Azure AD Klasik sanal ağlar (Vnet'ler) yalnızca destekler ve yalnızca destek Linux tabanlı Hdınsight kümeleri için Azure Resource Manager tabanlı sanal ağlar, iki sanal ağlar ve bunlar arasında eşleme Hdınsight Azure AD tümleştirme gerektirir. Karşılaştırma için iki dağıtım modelleri arasında bilgi [Azure Resource Manager ve klasik dağıtım: dağıtım modelleri ve kaynaklarınızın durumunu anlamak](../azure-resource-manager/resource-manager-deployment-model.md). İki Vnet Azure AD DS ile aynı bölgede olması gerekir.

> [!NOTE]
> Bu öğretici, Azure AD olmadığını varsayar. Varsa, 2. adımda bölümüne atlayabilirsiniz.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi incelemek için aşağıdaki öğeleri sahip olmanız gerekir:

* İle öğrenmeniz [Azure AD etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) kendi [fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-ds/) yapısı.
* Aboneliğiniz bu genel Önizleme için Güvenilenler listesine olduğundan emin olun. E-posta göndererek yapabilirsiniz hdipreview@microsoft.com abonelik kimliğinizi içeren
* Etki alanınız için imzalama yetkilisi tarafından imzalanmış bir SSL sertifikası. Güvenli LDAP yapılandırarak sertifika gereklidir. Otomatik olarak imzalanan sertifikalar kullanılamaz.
* Azure PowerShell.  Bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Azure AD için Azure klasik bir VNet oluşturun.
Yönergeler için bkz: [burada](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Oluşturun ve Azure AD ve Azure AD DS yapılandırın.
Yönergeler için bkz: [burada](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma
PowerShell Betiği yüklenebilir [GitHub](https://github.com/hdinsight/DomainJoinedHDInsight). Zip dosyasını ayıklayın ve dosyalarını yerel olarak kaydedin.

**PowerShell komut dosyasını düzenlemek için**

1. Windows PowerShell ISE veya herhangi bir metin düzenleyicisi kullanarak run.ps1 açın.
2. Aşağıdaki değişkenleri için değerleri doldurun:
   
   * **$SubscriptionName** – Hdınsight kümenizle oluşturmak istediğiniz Azure abonelik adı. Bu abonelikte Klasik sanal ağda zaten oluşturmuş ve abonelik altında Hdınsight kümesi için bir Azure Resource Manager sanal ağ oluşturma.
   * **$ClassicVNetName** -Azure AD DS içeren klasik sanal ağı. Bu sanal ağ, yukarıda sağlanan aynı abonelik olmalıdır. Bu sanal ağ Azure Portalı'nı kullanarak ve klasik portal kullanmayan oluşturulması gerekir. Yönerge izlerseniz [yapılandırma etki alanına katılmış Hdınsight kümeleri (Önizleme)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), contosoaadvnet varsayılan addır.
   * **$ClassicResourceGroupName** – yukarıda belirtilen Klasik sanal ağı için Resource Manager grup adı. Örneğin contosoaadrg. 
   * **$ArmResourceGroupName** – Hdınsight kümesi oluşturmak istediğiniz kaynak grubu adı, içinde. Aynı kaynak grubunda $ArmResourceGroupName kullanabilirsiniz.  Kaynak grubu mevcut değilse, komut dosyası kaynak grubu oluşturur.
   * **$ArmVNetName** -içinde Hdınsight kümesi oluşturmak istediğiniz kaynak yöneticisi sanal ağ adı. Bu sanal ağ $ArmResourceGroupName yerleştirilecek.  VNet mevcut değilse, PowerShell Betiği oluşturur. Mevcut değilse, yukarıdaki sağladığınız kaynak grubunun parçası olmalıdır.
   * **$AddressVnetAddressSpace** – Resource Manager sanal ağın ağ adres alanı. Bu adres alanı kullanılabilir olduğundan emin olun. Bu adres alanı Klasik sanal ağın adres alanı çakışamaz. Örneğin, "10.1.0.0/16"
   * **$ArmVnetSubnetName** -içinde Hdınsight kümesi VM'ler yerleştirmek istediğiniz kaynak yöneticisi sanal ağ alt ağ adı. Alt ağ yoksa, PowerShell Betiği oluşturur. Mevcut değilse, yukarıdaki sağlayan sanal ağın parçası olması gerekir.
   * **$AddressSubnetAddressSpace** – Resource Manager sanal ağ alt ağ adres aralığı. VM IP adresleri Hdınsight kümesi bu alt ağ adres aralığından olacaktır. Örneğin, "10.1.0.0/24".
   * **$ActiveDirectoryDomainName** – Hdınsight katılmasını istediğiniz Azure AD etki alanı adı için sanal makineleri küme. Örneğin, "contoso.onmicrosoft.com"
   * **$ClusterUsersGroups** – Hdınsight kümesine eşitlemek istediğiniz AD güvenlik grupları'nın ortak adı. Bu güvenlik grubundaki kullanıcıların, active directory etki alanı kimlik bilgilerini kullanarak küme Panosu oturum açabilmesi olacaktır. Bu güvenlik grupları active Directory'de mevcut olması gerekir. Örneğin, "hiveusers" veya "clusteroperatorusers".
   * **$OrganizationalUnitName** -içinde Hdınsight yerleştirmek istediğiniz etki alanındaki kuruluş birimi küme sanal makineleri ve küme tarafından kullanılan hizmet asıl adı. Henüz yoksa, PowerShell Betiği Bu OU oluşturursunuz. Örneğin, "HDInsightOU".
3. Değişiklikleri kaydedin.

**Komut dosyasını çalıştırmak için**

1. Çalıştırma **Windows PowerShell** yönetici olarak.
2. Run.ps1 klasöre göz atın. 
3. Dosya adını yazarak komut dosyasını çalıştırın ve isabet **ENTER**.  Oturum açma 3 iletişim açılır:
   
   1. **Klasik Azure portalında oturum** – Klasik Azure portalında oturum açmak için kullandığınız kimlik bilgilerinizi girin. Azure AD ve Azure AD DS'yi bu kimlik bilgilerini kullanarak oluşturmuş olmanız gerekir.
   2. **Azure Resource Manager portalı oturum açma** – Azure Resource Manager portalında oturum açmak için kullandığınız kimlik bilgilerinizi girin.
   3. **Etki alanı kullanıcı adı** – Hdınsight kümesinde bir yönetici olmasını istediğiniz etki alanı kullanıcı adı kimlik bilgilerini girin. Azure AD sıfırdan oluşturduysanız bu belgelerini kullanarak bu kullanıcı oluşturmuş olmanız gerekir. 
      
      > [!IMPORTANT]
      > Kullanıcı adı şu biçimde girin: 
      > 
      > EtkiAlanıAdı\KullanıcıAdı (örneğin contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Bu kullanıcı 3 ayrıcalıklarına sahip olmalıdır: Belirtilen Active Directory etki alanı için; makinelere katılmak için Hizmet sorumluları ve makine nesnelerinde sağlanan kuruluş birimi oluşturmak için; ve geriye doğru DNS proxy kurallar ekleyin.

Geriye doğru DNS bölgeleri oluşturulurken, komut dosyası bir ağ kimliği girin isteyip istemediğinizi sorar Bu ağ kimliği Resource Manager sanal ağın adres ön eki olmalıdır. Resource Manager sanal ağ alt ağ adresi alanınızı 10.2.0.0/24 ise, aracı ağ kimliği için istediğinde 10.2.0.0/24 Örneğin 

## <a name="create-hdinsight-cluster"></a>Hdınsight kümesi oluşturma
Bu bölümde Azure portalı kullanarak hdınsight'ta Linux tabanlı Hadoop kümesi oluşturma veya [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-template-deploy.md). Diğer küme oluşturma yöntemleri ve ayarlarını anlama, bkz: [Hdınsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Resource Manager şablonu kullanarak Hdınsight'ta Hadoop kümeleri oluşturma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları kullanarak Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Azure Portalı'nı kullanarak bir etki alanına katılmış Hdınsight kümesi oluşturmak için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **yeni**, **Intelligence + analiz**ve ardından **Hdınsight**.
3. Gelen **yeni Hdınsight kümesi** dikey penceresinde, aşağıdaki değerleri seçin veya girin:
   
   * **Küme adı**: etki alanına katılmış Hdınsight kümesi için yeni bir küme adı girin.
   * **Abonelik**: Bu küme oluşturmak için kullanılan Azure aboneliğini seçin.
   * **Küme Yapılandırması**:
     
     * **Küme türü**: Hadoop. Etki alanına katılmış Hdınsight şu anda yalnızca üzerinde desteklenen Hadoop kümeleri değil.
     * **İşletim sistemi**: Linux.  Etki alanına katılmış Hdınsight yalnızca Linux tabanlı Hdınsight kümelerinde desteklenir.
     * **Sürüm**: Hadoop 2.7.3 (HDI 3.5). Etki alanına katılmış Hdınsight yalnızca Hdınsight kümesi sürüm 3.5 desteklenir.
     * **Küme türü**: PREMIUM
       
       Tıklatın **seçin** değişiklikleri kaydedin.
   * **Kimlik bilgileri**: küme kullanıcı ve SSH kullanıcı kimlik bilgilerini yapılandırın.
   * **Veri kaynağı**: yeni bir depolama hesabı oluşturun veya varolan bir depolama hesabı Hdınsight kümesi için varsayılan depolama hesabı olarak kullanın. Konumun iki sanal ağlar aynı olmalıdır.  Konum de Hdınsight kümesi konumdur.
   * **Fiyatlandırma**: kümenizin çalışan düğümü sayısını seçin.
   * **Yapılandırmaları Gelişmiş**: 
     
     * **Etki alanına katılma & sanal/alt**: 
       
       * **Etki alanı ayarları**: 
         
         * **Etki alanı adı**: contoso.onmicrosoft.com
         * **Etki alanı kullanıcı adı**: bir etki alanı kullanıcı adı girin. Bu etki alanı aşağıdaki ayrıcalıklara sahip olmalıdır: makineler etki alanına katılma ve önceki; yapılandırdığınız kuruluş birimine yerleştirin Hizmet sorumluları içinde daha önce yapılandırılmış kuruluş birimi oluşturun; Geriye doğru DNS girdilerini oluşturun. Bu etki alanı kullanıcısı bu etki alanına katılmış Hdınsight küme yöneticisinin olur.
         * **Etki alanı parolası**: etki alanı kullanıcı parolası girin.
         * **Kuruluş birimi**: daha önce yapılandırılmış OU'nun ayırt edici adını girin. Örneğin: OU HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com =
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Access kullanıcı grubuna**: güvenlik grubuna, wan kümeye eşitleme, kullanıcıları belirtin. Örneğin, HiveUsers.
           
           Tıklatın **seçin** değişiklikleri kaydedin.
           
           ![Etki alanına katılmış Hdınsight portal etki alanı ayarını yapılandırın](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Sanal ağ**: contosohdivnet
       * **Alt ağ**: Subnet1
         
         Tıklatın **seçin** değişiklikleri kaydedin.        
         Tıklatın **seçin** değişiklikleri kaydedin.
   * **Kaynak grubu**: Hdınsight VNet (contosohdirg) için kullanılan kaynak grubunu seçin.
4. **Oluştur**'a tıklayın.  

Etki alanına katılmış Hdınsight kümesi oluşturmak için başka bir seçenek, Azure kaynak yönetimi şablonu kullanmaktır. Aşağıdaki yordam, nasıl gösterir:

**Kaynak Yönetimi şablonunu kullanarak bir etki alanına katılmış Hdınsight kümesi oluşturmak için**

1. Azure portalında bir Resource Manager şablonunu açmak için aşağıdaki görüntüye tıklayın. Resource Manager şablonu bir ortak blob kapsayıcısında bulunur. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy to Azure"></a>
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
   * **Küme Users grubunun D Ns**: "\"CN HiveUsers, OU = AADDC kullanıcılar, DC = =<DomainName>, DC onmicrosoft, DC = com =\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (etki alanı yönetici kullanıcı adı girin)
   * **DomainAdminPassword**: (etki alanı yönetici kullanıcı parolasını girin)
   * **Hüküm ve koşulları yukarıda belirtildiği ediyorum**: (denetleyin)
   * **Panoya Sabitle**: (denetleyin)
3. **Satın al**’a tıklayın. **Şablon Dağıtımı’nı dağıtma** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer. Küme oluşturulduktan sonra küme dikey penceresini açmak için portalda tıklatabilirsiniz.

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Küme silme ilişkin yönergeler için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure portalını kullanarak](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Sonraki adımlar

* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerine bağlanmak için SSH kullanmak için bkz: [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

