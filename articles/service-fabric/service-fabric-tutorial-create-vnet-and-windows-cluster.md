---
title: Azure'da Windows Service Fabric Kümesi oluşturma | Microsoft Docs
description: Bu öğreticide, PowerShell kullanarak bir Azure sanal ağına ve ağ güvenlik grubuna Windows Service Fabric kümesi dağıtmayı öğrenirsiniz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/19/2019
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 590e1e5853ccf4a525477f194c78f1fd8ce679ed
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453078"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Öğretici: Azure sanal ağına Service Fabric Windows kümesi dağıtma

Bu öğretici, bir serinin birinci bölümüdür. PowerShell ile bir şablon kullanarak bir [Azure sanal ağına (VNET)](../virtual-network/virtual-networks-overview.md) ve [ağ güvenlik grubuna](../virtual-network/virtual-networks-nsg.md) Windows çalıştıran bir Service Fabric kümesi dağıtmayı öğrenirsiniz. Öğreticiyi tamamladığınızda, bulutta çalışan ve uygulama dağıtabileceğiniz bir kümeniz olur.  Azure CLI kullanarak Linux kümesi oluşturmak için bkz. [Azure’da güvenli bir Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Bu öğreticide bir üretim senaryosu açıklanır.  İsterseniz hızlı bir şekilde test amacıyla daha küçük bir küme oluşturun, bkz: [test kümesi oluşturma](./scripts/service-fabric-powershell-create-secure-cluster-cert.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure’da güvenli bir küme oluşturma
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Service Fabric SDK’sını ve PowerShell modülünü](service-fabric-get-started.md) yükleyin
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) yükleyin
* Temel kavramlarını gözden [Azure kümeleri](service-fabric-azure-clusters-overview.md)

Aşağıdaki yordamlar yedi düğümlü bir Service Fabric küme oluşturun. Azure’da Service Fabric kümesi çalıştırmaktan kaynaklanan maliyetleri hesaplamak için [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)’nı kullanın.

## <a name="download-and-explore-the-template"></a>Şablonu indirin ve keşfedin

Aşağıdaki Resource Manager şablonu dosyalarını indirin:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Bu şablon, bir sanal ağ ve ağ güvenlik grubu yedi sanal makinelerin ve üç düğüm türleri güvenli bir küme dağıtır.  Diğer örnek şablonlar [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates)'da bulunabilir.  [azuredeploy.json][template] aşağıdakiler dahil bir grup kaynak dağıtır.

### <a name="service-fabric-cluster"></a>Service Fabric kümesi

**Microsoft.ServiceFabric/clusters** kaynağında şu özelliklere sahip bir Windows kümesi yapılandırılır:

* üç düğüm türleri
* (şablon parametrelerinde yapılandırılabilir) birincil düğüm türünde beş düğüm, bir düğümdeki her iki düğüm türleri
* İşletim Sistemi: Windows Server 2016 Datacenter kapsayıcılarla (şablon parametrelerinde yapılandırılabilir)
* sertifikanın güvenliğinin sağlanması (şablon parametrelerinde yapılandırılabilir)
* [ters proxy](service-fabric-reverseproxy.md) etkin
* [DNS hizmeti](service-fabric-dnsservice.md) etkin
* Bronz [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (şablon parametrelerinde yapılandırılabilir)
* Gümüş [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (şablon parametrelerinden yapılandırılabilir)
* istemci bağlantısı uç noktası: 19000 (şablon parametrelerinde yapılandırılabilir)
* HTTP ağ geçidi uç noktası: 19080 (şablon parametrelerinde yapılandırılabilir)

### <a name="azure-load-balancer"></a>Azure yük dengeleyici

**Microsoft.Network/loadBalancers** kaynağında bir yük dengeleyici yapılandırılır ve şu bağlantı noktaları için yoklamalar ve kurallar ayarlanır:

* istemci bağlantısı uç noktası: 19000
* HTTP ağ geçidi uç noktası: 19080
* Uygulama bağlantı noktası: 80
* Uygulama bağlantı noktası: 443
* Service Fabric ters proxy'si: 19081

Başka bir uygulama bağlantı noktası gerekiyorsa **Microsoft.Network/loadBalancers** kaynağı ile **Microsoft.Network/networkSecurityGroups** kaynağını trafiğin geçmesine izin verecek şekilde ayarlamanız gerekir.

### <a name="virtual-network-subnet-and-network-security-group"></a>Sanal ağ, alt ağ ve ağ güvenlik grubu

Sanal ağ, alt ağ ve ağ güvenlik grubunun adları şablon parametrelerinde bildirilir.  Sanal ağın ve alt ağın adres alanları da şablon parametrelerinde bildirilir ve **Microsoft.Network/virtualNetworks** kaynağında yapılandırılır:

* sanal ağ adres alanı: 172.16.0.0/20
* Service Fabric alt ağ adres alanı: 172.16.2.0/23

**Microsoft.Network/networkSecurityGroups** kaynağında aşağıdaki gelen trafik kuralları etkinleştirilir. Şablon değişkenlerini değiştirerek bağlantı noktası değerlerini değiştirebilirsiniz.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication - 1025, 1026, 1027
* Kısa ömürlü bağlantı noktası aralığı – 49152-65534 (en az 256 bağlantı noktası gerekir)
* Uygulamanın kullanımına yönelik bağlantı noktaları: 80 ve 443
* Uygulama bağlantı noktası aralığı – 49152-65534 (hizmetler arası iletişim için kullanılır ve diğerlerinden farklı olarak Yük dengeleyicide açılmaz)
* Diğer tüm bağlantı noktalarını engelleyin

Başka bir uygulama bağlantı noktası gerekiyorsa **Microsoft.Network/loadBalancers** kaynağı ile **Microsoft.Network/networkSecurityGroups** kaynağını trafiğin geçmesine izin verecek şekilde ayarlamanız gerekir.

### <a name="windows-defender"></a>Windows Defender
Varsayılan olarak, [Windows Defender virüsten koruma](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) yüklü ve Windows Server 2016 işlev. Kullanıcı arabirimi bazı sku'larda varsayılan olarak yüklenir, ancak gerekli değildir.  Şablonda bildirilen her düğüm türü/sanal makine ölçek kümesi için [Azure VM kötü amaçlı yazılımdan koruma uzantısını](/azure/virtual-machines/extensions/iaas-antimalware-windows) işlemleri ve Service Fabric dizinleri hariç tutmak için kullanılır:

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>Şablon parametrelerini ayarlama

[azuredeploy.parameters.json][parameters] parametre dosyası, kümenin ve ilişkili kaynakların dağıtılması için kullanılan birçok değeri bildirir. Dağıtımınız için değiştirmeniz gerekebilecek bazı parametreler:

|Parametre|Örnek değer|Notlar|
|---|---||
|adminUserName|vmadmin| Küme VM'leri için yönetici kullanıcı adı.[VM için kullanıcı adı gereksinimleri](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Küme VM’leri için yönetici parolası. [VM için parola gereksinimleri](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| Kümenin adı. Yalnızca harf ve sayı içerebilir. Uzunluğu 3 ile 23 karakter arasında olmalıdır.|
|location|southcentralus| Kümenin konumu. |
|certificateThumbprint|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika SHA1 parmak izi değerini girin. Örneğin: "6190390162C988701DB5676EB81083EA608DCCF3"</p>. |
|certificateUrlValue|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır. </p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika URL’sini girin. Örneğin, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için kaynak kasa değerini girin. Örneğin: "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Azure Active Directory istemci kimlik doğrulamasını ayarlama
Azure üzerinde barındırılan ortak bir ağda dağıtılmış Service Fabric kümeleri için istemci düğümü karşılıklı kimlik doğrulaması için önerilir:
* Azure Active Directory istemci kimliği için kullan
* Sunucu kimliği ve http iletişim SSL şifrelemesi için bir sertifika

Önce bir Service Fabric kümesi yapılması için istemcilerin kimliğini doğrulamak için Azure AD'yi ayarlarken ayarlama [kümeyi oluştururken](#createvaultandcert).  Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. 

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ve [Visual Studio](service-fabric-manage-application-in-visual-studio.md). Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulamaları oluşturduğunuz: bir web uygulaması ve bir yerel uygulama.  Uygulama oluşturulduktan sonra kullanıcıları salt okunur olarak atadığınız ve yönetici rolleri.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerleri planlanmalıdır ve, zaten oluşturduğunuz değerleri değil.

Bu makalede, zaten bir kiracı oluşturmuş varsayıyoruz. Tamamlamadıysanız, okuyarak başlamanız [bir Azure Active Directory kiracısı edinme](../active-directory/develop/quickstart-create-new-tenant.md).

Bazı yapılandırma Azure AD'de bir Service Fabric kümesi ile yer alan adımların basitleştirmek için Windows PowerShell komutları kümesi oluşturduk. [Betiklerini indirme](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) bilgisayarınıza.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure AD uygulamaları oluşturmak ve kullanıcıları rollere atama
Küme erişimi denetlemek için iki Azure AD uygulaması oluştur: bir web uygulaması ve bir yerel uygulama. Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınıza atama [Service Fabric tarafından desteklenen roller](service-fabric-cluster-security-roles.md): salt okunur ve yönetici

Çalıştırma `SetupApplications.ps1`ve parametrelere Kiracı kimliği, küme adı ve web uygulamasının yanıt URL'si girin.  Ayrıca, kullanıcı adları ve kullanıcılar için parola belirtin.  Örneğin:

```PowerShell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> (Örneğin Azure devlet kurumları, Azure Çin'de, Azure Almanya) Ulusal Bulutlar için de belirtmeniz `-Location` parametresi.

Bulabilirsiniz, *Tenantıd*, ya da dizin kimliği, içinde [Azure portalında](https://portal.azure.com). Seçin **Azure Active Directory -> Özellikler** ve kopyalama **dizin kimliği** değeri.

*ClusterName* betiği tarafından oluşturulan Azure AD uygulamaları önek olarak eklemek için kullanılır. Gerçek bir küme adı tam olarak eşleşmesi gerekmez. Yalnızca bunlar ile kullanılan Service Fabric kümesine Azure AD'ye yapıtları eşlemek kolaylaştırmak için tasarlanmıştır.

*WebApplicationReplyUrl* varsayılan uç nokta oturum açma işlemini tamamladıktan sonra kullanıcılarınız için Azure AD'ye verir. Bu uç nokta olan varsayılan olarak, kümenizin Service Fabric Explorer uç nokta olarak ayarlayın:

https://&lt;cluster_domain&gt;: 19080/Explorer

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesap için oturum açmanız istenir. Oturum açtıktan sonra Service Fabric kümenizi temsil etmek için yerel uygulamalar ve web betik oluşturur. Kiracının uygulamaları bakarsanız [Azure portalında](https://portal.azure.com), iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

PowerShell penceresini açık tutmak için iyi bir fikirdir, bu nedenle, kümeyi oluşturduğunuzda Azure Resource Manager şablon tarafından gereken JSON betiği yazdırır.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD istemci erişimi için kullanılacak Azure AD Yapılandırması Ekle
İçinde [azuredeploy.json][template], Azure AD'de yapılandırırken **Microsoft.ServiceFabric/clusters** bölümü.  Kiracı kimliği, küme uygulama kimliği ve istemci uygulama kimliği parametreleri ekleme  

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

Parametre değerlerini eklemek [azuredeploy.parameters.json] [ parameters] parametre dosyası.  Örneğin:

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Sanal ağı ve kümeyi dağıtma

Ardından, ağ topolojisini ayarlayın ve Service Fabric kümesini dağıtın. [azuredeploy.json][template] Resource Manager şablonu bir sanal ağın (VNET) yanı sıra Service Fabric için bir alt ağ ve ağ güvenlik grubu (NSG) da oluşturur. Şablon tarafından sertifika güvenliği etkin bir küme de dağıtılır.  Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden (CA) alınan bir sertifika kullanın. Test kümelerinin güvenliğinin sağlanması için otomatik olarak imzalanan bir sertifika kullanılabilir.

Bu makalede şablonda küme sertifikayı belirlemek için sertifika parmak izini kullanan bir küme dağıtır.  İki sertifika, sertifika yönetimi zorlaştırır aynı parmak olabilir. Sertifika ortak adları kullanarak sertifika parmak izleri kullanarak dağıtılan bir kümenin geçiş sertifika yönetimi çok daha kolay hale getirir.  Sertifika Yönetim sertifikası ortak adlarının kullanmak için küme güncelleştirme hakkında bilgi edinmek için okumaya devam [küme sertifikası ortak adı yönetim değiştirme](service-fabric-cluster-change-cert-thumbprint-to-cn.md).

### <a name="create-a-cluster-using-an-existing-certificate"></a>Mevcut bir sertifikayı kullanarak küme oluşturma

Aşağıdaki betik, [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet’ini ve bir şablonu kullanarak Azure’da yeni bir küme dağıtır. Ayrıca, cmdlet tarafından Azure’da yeni bir anahtar kasası oluşturulur ve sertifikanız karşıya yüklenir.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Yeni ve otomatik olarak imzalanan bir sertifika ile küme oluşturma

Aşağıdaki betik, [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) cmdlet’ini ve bir şablonu kullanarak Azure’da yeni bir küme dağıtır. Ayrıca, cmdlet tarafından Azure’da yeni bir anahtar kasası oluşturulup bu kasaya otomatik olarak imzalanan yeni bir sertifika eklenir ve sertifika dosyası yerel olarak indirilir.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanma

Service Fabric SDK’sı ile birlikte yüklenen Service Fabric PowerShell modülünü kullanarak kümeye bağlanın.  İlk olarak sertifikayı bilgisayarınızda geçerli kullanıcının Kişisel (My) deposuna yükleyin.  Aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

**Service Fabric** PowerShell modülü, Service Fabric kümelerini, uygulamalarını ve hizmetlerini yönetmek için birçok cmdlet sağlar.  Güvenli kümeye bağlanmak için [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini kullanın. Sertifika SHA1 parmak izi ve bağlantı uç noktası ayrıntıları önceki adımda elde edilen çıktıdan bulunabilir.

AAD istemci kimlik doğrulamasını önceden ayarlama, aşağıdaki komutu çalıştırın: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

AAD istemci kimlik doğrulaması Kurulum belirtilmedi ise aşağıdaki komutu çalıştırın:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

[Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'ini kullanarak bağlı olduğunuzu ve kümenin iyi durumda olduğunu doğrulayın.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici serisindeki diğer makalelerde, az önce oluşturduğunuz küme kullanılır. Hemen bir sonraki makaleye geçmeyecekseniz ücret alınmaması için [kümeyi silmeniz](service-fabric-cluster-delete.md) iyi olur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * PowerShell kullanarak Azure’da güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Ardından, kümenizi nasıl ölçeklendirebileceğinizi öğrenmek üzere aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Küme Ölçeklendirme](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
