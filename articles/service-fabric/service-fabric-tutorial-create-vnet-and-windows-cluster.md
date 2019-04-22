---
title: Windows Azure'da çalışan bir Service Fabric kümesi oluşturma | Microsoft Docs
description: Bu öğreticide, PowerShell kullanarak bir Azure sanal ağı ve ağ güvenlik grubu içinde bir Windows Service Fabric kümesi dağıtmayı öğrenirsiniz.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/13/2019
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: dabbefa8ca2073e30948f1c70782f730bceae030
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59050015"
---
# <a name="tutorial-deploy-a-service-fabric-cluster-running-windows-into-an-azure-virtual-network"></a>Öğretici: Windows çalıştıran bir Azure sanal ağına Service Fabric kümesine dağıtma

Bu öğretici, bir dizinin birinci bölümüdür. Uygulamasına Windows çalıştıran bir Azure Service Fabric kümesi dağıtmayı öğrenirsiniz bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ve [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) PowerShell ile bir şablon kullanarak. İşlemi tamamladığınızda, uygulamaları dağıtabileceğiniz bulutta çalışan bir kümeniz varsa. Azure CLI'yı kullanan bir Linux kümesi oluşturmak için bkz [Azure'da güvenli bir Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Bu öğreticide bir üretim senaryosu açıklanır. Sınama amacıyla daha küçük bir küme oluşturmak istiyorsanız bkz [test kümesi oluşturma](./scripts/service-fabric-powershell-create-secure-cluster-cert.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * Tanı koleksiyonu yapılandırma
> * Eventstore'a hizmetini ayarlama
> * Azure İzleyici günlüklerini ayarlayın
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure’da güvenli bir küme oluşturma
> * [Bir kümesini izleme](service-fabric-tutorial-monitor-cluster.md)
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Yükleme [Service Fabric SDK'sını ve PowerShell Modülü](service-fabric-get-started.md).
* Yükleme [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps).
* Temel kavramlarını gözden [Azure kümeleri](service-fabric-azure-clusters-overview.md).
* [Plan ve hazırlık](service-fabric-cluster-azure-deployment-preparation.md) bir küme dağıtımları için.

Aşağıdaki yordamlar yedi düğümlü bir Service Fabric küme oluşturun. Kullanım [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) Azure'da bir Service Fabric kümesi çalıştırmaktan kaynaklanan maliyetleri hesaplamak için.

## <a name="download-and-explore-the-template"></a>Şablonu indirin ve keşfedin

Aşağıdaki Azure Resource Manager şablonu dosyalarını indirin:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Bu şablon, bir sanal ağ ve ağ güvenlik grubu yedi sanal makinelerin ve üç düğüm türleri güvenli bir küme dağıtır.  Diğer örnek şablonlar [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates)'da bulunabilir. [Azuredeploy.json] [ template] kaynakları, aşağıdakiler dahil birçok dağıtır.

### <a name="service-fabric-cluster"></a>Service Fabric kümesi

**Microsoft.ServiceFabric/clusters** kaynağında şu özelliklere sahip bir Windows kümesi yapılandırılır:

* Üç düğüm türleri.
* (Şablon parametrelerinde yapılandırılabilir) birincil düğüm türündeki beş düğüme ve diğer iki düğüm türlerinin her bir düğümü.
* İşletim Sistemi: Kapsayıcılar (şablon parametrelerinde yapılandırılabilir) ile Windows Server 2016 Datacenter.
* Sertifika (şablon parametrelerinde yapılandırılabilir) güvenli.
* [Ters proxy](service-fabric-reverseproxy.md) etkinleştirilir.
* [DNS hizmeti](service-fabric-dnsservice.md) etkinleştirilir.
* [Dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Bronz (şablon parametrelerinde yapılandırılabilir).
* [Güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) Gümüş (şablon parametrelerinde yapılandırılabilir).
* istemci bağlantısı uç noktası: 19000 (şablon parametrelerinde yapılandırılabilir).
* HTTP ağ geçidi uç noktası: 19080 (şablon parametrelerinde yapılandırılabilir).

### <a name="azure-load-balancer"></a>Azure Load Balancer

İçinde **Microsoft.Network/loadBalancers** kaynak, bir yük dengeleyici yapılandırılır. Aşağıdaki bağlantı noktaları için araştırmalarla kurallar ayarlanır:

* istemci bağlantısı uç noktası: 19000
* HTTP ağ geçidi uç noktası: 19080
* Uygulama bağlantı noktası: 80
* Uygulama bağlantı noktası: 443
* Service Fabric ters proxy'si: 19081

Diğer uygulama bağlantı noktası gerekiyorsa, ayarlamak gerekir **Microsoft.Network/loadBalancers** kaynak ve **Microsoft.Network/networkSecurityGroups** içinde trafiğine izin verecek şekilde kaynak.

### <a name="virtual-network-subnet-and-network-security-group"></a>Sanal ağ, alt ağ ve ağ güvenlik grubu

Sanal ağ, alt ağ ve ağ güvenlik grubunun adları şablon parametrelerinde bildirilir. Sanal ağın ve alt ağın adres alanları da şablon parametrelerinde bildirilir ve **Microsoft.Network/virtualNetworks** kaynağında yapılandırılır:

* sanal ağ adres alanı: 172.16.0.0/20
* Service Fabric alt ağ adres alanı: 172.16.2.0/23

**Microsoft.Network/networkSecurityGroups** kaynağında aşağıdaki gelen trafik kuralları etkinleştirilir. Şablon değişkenlerini değiştirerek bağlantı noktası değerlerini değiştirebilirsiniz.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication: 1025, 1026, 1027
* Kısa ömürlü bağlantı noktası aralığı: 49152 için (en az 256 bağlantı noktası gerekir) 65534.
* Uygulamanın kullanımına yönelik bağlantı noktaları: 80 ve 443
* Uygulama bağlantı noktası aralığı: 49152 için (hizmet iletişimi için kullanılır. 65534 Diğer bağlantı noktaları için yük dengeleyicide açık değildir).
* Diğer tüm bağlantı noktalarını engelleyin

Diğer uygulama bağlantı noktası gerekiyorsa, ayarlamak gerekir **Microsoft.Network/loadBalancers** kaynak ve **Microsoft.Network/networkSecurityGroups** içinde trafiğine izin verecek şekilde kaynak.

### <a name="windows-defender"></a>Windows Defender
Varsayılan olarak, [Windows Defender virüsten koruma programı](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) yüklü ve Windows Server 2016 işlev. Kullanıcı arabirimi bazı sku'larda varsayılan olarak yüklenir, ancak gerekli değildir. Şablonda bildirilen her düğüm türü/sanal makine ölçek kümesi için [Azure VM kötü amaçlı yazılımdan koruma uzantısını](/azure/virtual-machines/extensions/iaas-antimalware-windows) işlemleri ve Service Fabric dizinleri hariç tutmak için kullanılır:

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

[azuredeploy.parameters.json][parameters] parametre dosyası, kümenin ve ilişkili kaynakların dağıtılması için kullanılan birçok değeri bildirir. Dağıtımınız için değiştirmek için Parametreler şunlardır:

**Parametre** | **Örnek değer** | **Notlar** 
|---|---|---|
|adminUserName|vmadmin| Küme VM’leri için yönetici kullanıcı adı. [VM kullanıcı adı gereksinimleri](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm). |
|adminPassword|Password#1234| Küme VM’leri için yönetici parolası. [VM için parola gereksinimlerini](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).|
|clusterName|mysfcluster123| Kümenin adı. Yalnızca harf ve sayı içerebilir. Uzunluğu 3 ile 23 karakter arasında olmalıdır.|
|location|southcentralus| Kümenin konumu. |
|certificateThumbprint|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika SHA1 parmak izi değerini girin. Örneğin: "6190390162C988701DB5676EB81083EA608DCCF3".</p> |
|certificateUrlValue|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır. </p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika URL’sini girin. Örneğin, "https:\//mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için kaynak kasa değerini girin. Örneğin: "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Azure Active Directory istemci kimlik doğrulamasını ayarlama
Azure üzerinde barındırılan ortak bir ağda dağıtılmış Service Fabric kümeleri için istemci düğümü karşılıklı kimlik doğrulaması için önerilir:
* Azure Active Directory istemci kimliği için kullanın.
* Sunucu kimliği ve HTTP iletişim SSL şifrelemesi için bir sertifika kullanın.

Önce bir Service Fabric kümesi için istemcilerin kimliğini doğrulamak için Azure Active Directory (Azure AD) ayarı yapılmalıdır [kümeyi oluştururken](#createvaultandcert). Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. 

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ve [Visual Studio](service-fabric-manage-application-in-visual-studio.md). Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulamaları oluşturduğunuz: bir web uygulaması ve bir yerel uygulama.  Uygulama oluşturulduktan sonra kullanıcıları salt okunur olarak atadığınız ve yönetici rolleri.

> [!NOTE]
> Kümeyi oluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Küme adları ve uç noktaları betikleri beklediğiniz çünkü değerleri planlanmalıdır ve, zaten oluşturduğunuz değerleri değil.

Bu makalede, bir kiracı zaten oluşturduğunuzu varsayar. Siz yapmadıysanız okuyarak başlamanız [bir Azure Active Directory kiracısı edinme](../active-directory/develop/quickstart-create-new-tenant.md).

Yapılandırma Azure AD'de bir Service Fabric kümesi ile yer alan adımların basitleştirmek için Windows PowerShell komutları kümesi oluşturduk. [Betiklerini indirme](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) bilgisayarınıza.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure AD uygulamaları oluşturmak ve kullanıcıları rollere atama
Küme erişimi denetlemek için iki Azure AD uygulaması oluştur: bir web uygulaması ve bir yerel uygulama. Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınıza atama [Service Fabric tarafından desteklenen roller](service-fabric-cluster-security-roles.md): salt okunur ve yönetici

Çalıştırma `SetupApplications.ps1`ve parametrelere Kiracı kimliği, küme adı ve web uygulamasının yanıt URL'si girin. Kullanıcı adları ve kullanıcılar için parola belirtin. Örneğin:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Belirtin (örneğin Azure devlet kurumları, Azure Çin'de, Azure Almanya) Ulusal Bulutlar için `-Location` parametresi.

Bulabilirsiniz, *Tenantıd*, ya da dizin kimliği, içinde [Azure portalında](https://portal.azure.com). Seçin **Azure Active Directory** > **özellikleri** ve kopyalama **dizin kimliği** değeri.

*ClusterName* betiği tarafından oluşturulan Azure AD uygulamaları önek olarak eklemek için kullanılır. Gerçek bir küme adı tam olarak eşleşmesi gerekmez. Yalnızca, Azure AD yapıtlar kullanımda Service Fabric kümesine eşlemek kolaylaştırır.

*WebApplicationReplyUrl* varsayılan uç nokta oturum açma işlemini tamamladıktan sonra kullanıcılarınız için Azure AD'ye verir. Bu uç nokta olan varsayılan olarak, kümenizin Service Fabric Explorer uç nokta olarak ayarlayın:

https://&lt;cluster_domain&gt;: 19080/Explorer

Azure AD kiracısı için yönetici ayrıcalıklarına sahip bir hesap için oturum açmanız istenir. Oturum açtıktan sonra Service Fabric kümenizi temsil etmek için yerel uygulamalar ve web betik oluşturur. Kiracının uygulamalarda [Azure portalında](https://portal.azure.com), iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_küme
   * *ClusterName*\_istemci

PowerShell penceresini açık tutmak için iyi bir fikirdir, bu nedenle, kümeyi oluşturduğunuzda Resource Manager şablon tarafından gereken JSON betiği yazdırır.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD istemci erişimi için kullanılacak Azure AD Yapılandırması Ekle
İçinde [azuredeploy.json][template], Azure AD'de yapılandırırken **Microsoft.ServiceFabric/clusters** bölümü. Kiracı kimliği, küme uygulama kimliği ve istemci uygulama kimliği parametreleri ekleme  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
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

Parametre değerlerini eklemek [azuredeploy.parameters.json] [ parameters] parametre dosyası. Örneğin:

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
<a id="configurediagnostics" name="configurediagnostics_anchor"></a>

## <a name="configure-diagnostics-collection-on-the-cluster"></a>Kümede tanılama koleksiyonunu yapılandırma
Service Fabric kümesi çalıştırırken, merkezi bir konumda tüm düğümlerden günlükleri toplamak için iyi bir fikirdir. Günlükleri sahip merkezi bir konumda, kümenizdeki sorunları veya uygulamalar ve hizmetler, kümede çalışan sorunları gidermek ve çözümlemenize yardımcı olur.

Karşıya yükleme ve günlükleri toplamak için bir yolu, günlükleri, Azure Depolama'ya yükler ve ayrıca Azure Application Insights veya olay hub'larına günlükleri gönderme seçeneği olan Azure tanılama (WAD) uzantısı kullanmaktır. Bir dış işlem, olayları depolamadan okuyun ve bunları Azure İzleyici günlükleri gibi bir analiz platformu ürün veya başka bir günlük ayrıştırma çözümde yerleştirmek için de kullanabilirsiniz.

Bu öğreticide izliyorsanız, tanılama koleksiyonu zaten yapılandırılan [şablon][template].

Dağıtılan tanılama sahip olmayan var olan bir kümeniz varsa ekleyin veya küme şablonu güncelleştirebilir. Portaldan şablonunu indirin veya var olan kümeyi oluşturmak için kullanılan Resource Manager şablonunu değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin:

Yeni bir depolama kaynağı, şablonu kaynaklar bölümüne ekleyin:
```json
"resources": [
...
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
...
]
```

Ardından, depolama hesabı adı ve tür parametrelerini şablon parametreleri bölümünü ekleyin. Burada depolama ile hesap adı, yer tutucu metin depolama hesabı adı geçer Değiştir istersiniz.

```json
"parameters": {
...
"applicationDiagnosticsStorageAccountType": {
    "type": "string",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
    "description": "Replication option for the application diagnostics storage account"
    }
},
"applicationDiagnosticsStorageAccountName": {
    "type": "string",
    "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
    "metadata": {
    "description": "Name for the storage account that contains application diagnostics data from the cluster"
    }
},
...
}
```

Ardından, ekleme **IaaSDiagnostics** uzantılar dizisi uzantısı **VirtualMachineProfile** her özellik **Microsoft.Compute/virtualMachineScaleSets** Kümedeki kaynak.  Kullanıyorsanız [örnek şablonu][template], üç sanal makine ölçek kümeleri (kümedeki her düğüm türü için bir tane) vardır.

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
                    "properties": {
                        "type": "IaaSDiagnostics",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                        "storageAccountEndPoint": "https://core.windows.net/"
                        },
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "settings": {
                        "WadCfg": {
                            "DiagnosticMonitorConfiguration": {
                            "overallQuotaInMB": "50000",
                            "EtwProviders": {
                                "EtwEventSourceProviderConfiguration": [
                                {
                                    "provider": "Microsoft-ServiceFabric-Actors",
                                    "scheduledTransferKeywordFilter": "1",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableActorEventTable"
                                    }
                                },
                                {
                                    "provider": "Microsoft-ServiceFabric-Services",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                                    }
                                }
                                ],
                                "EtwManifestProviderConfiguration": [
                                {
                                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                    "scheduledTransferLogLevelFilter": "Information",
                                    "scheduledTransferKeywordFilter": "4611686018427387904",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricSystemEventTable"
                                    }
                                }
                                ]
                            }
                            }
                        },
                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                        },
                        "typeHandlerVersion": "1.5"
                    }
                }
            ...
            ]
        }
    }
}
```
<a id="configureeventstore" name="configureeventstore_anchor"></a>

## <a name="configure-the-eventstore-service"></a>Eventstore'a hizmetini yapılandırma
Eventstore'a service, Service fabric'te izleme bir seçenektir. Eventstore'a zaman küme veya iş yüklerini belirli bir noktada durumunu anlamak için bir yol sağlar. Eventstore'a kümeden olayları tutar durum bilgisi olan bir Service Fabric hizmeti ' dir. Olayı Service Fabric Explorer, REST ve API'ler aracılığıyla sunulur. Eventstore'a doğrudan herhangi bir varlık, kümenizdeki ilgili tanılama verilerini almak için küme sorgular ve yardımcı olmak için kullanılmalıdır:

* Geliştirme veya test sorunları tanılayın ve burada izleme bir işlem hattı kullanıyor olabilir
* Yönetim eylemleri kümenizde sürüp işlenen doğru onaylayın
* "Service Fabric belirli bir varlık ile nasıl etkileşim kurmanın anlık" Al



Kümenizde Eventstore'a hizmetini etkinleştirmek için aşağıdaki ekleyin **fabricSettings** özelliği **Microsoft.ServiceFabric/clusters** kaynak:

```json
"apiVersion": "2018-02-01",
"type": "Microsoft.ServiceFabric/clusters",
"name": "[parameters('clusterName')]",
"properties": {
    ...
    "fabricSettings": [
        ...
        {
            "name": "EventStoreService",
            "parameters": [
                {
                "name": "TargetReplicaSetSize",
                "value": "3"
                },
                {
                "name": "MinReplicaSetSize",
                "value": "1"
                }
            ]
        }
    ]
}
```
<a id="configureloganalytics" name="configureloganalytics_anchor"></a>

## <a name="set-up-azure-monitor-logs-for-the-cluster"></a>Azure İzleyici günlüklerine kümesi için ayarlayın

Azure İzleyici günlüklerine küme düzeyi olayları izlemek için Bizim önerimiz olur. Kümenizi izlemek için Azure İzleyici günlüklerini ayarlamak için ihtiyacınız [küme düzeyi olayları görüntülemek için tanılamayı](#configure-diagnostics-collection-on-the-cluster).  

Çalışma alanı kümenizden gelen Tanılama verileri bağlanması gerekir.  Bu günlük veriler *applicationDiagnosticsStorageAccountName* depolama hesabında WADServiceFabric * EventTable WADWindowsEventLogsTable ve WADETWEventTable tablolar.

Azure Log Analytics çalışma alanı ve çözüm çalışma alanına ekleyin:

```json
"resources": [
    ...
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Memory",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Available MBytes"
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter2",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Service Fabric Service",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Average milliseconds per request"
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
]
```

Ardından, parametre ekleme
```json
"parameters": {
    ...
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "mysfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "West Europe",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
}
```

Ardından, değişkenleri ekleyin:
```json
"variables": {
    ...
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
}
```

Log Analytics Aracısı uzantısı her sanal makine ölçek kümesinde ve aracının Log Analytics çalışma alanına bağlamak ekleyin. Bu kapsayıcı, uygulamaları ve performans izleme hakkında tanılama veri toplama sağlar. Bu uzantı olarak sanal makine ölçek kümesi kaynağına ekleyerek, Azure Resource Manager her düğümde yükleneceğini bile kümenin ne zaman ölçeklendirme sağlar.

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
                    "properties": {
                        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                        "type": "MicrosoftMonitoringAgent",
                        "typeHandlerVersion": "1.0",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                        },
                        "protectedSettings": {
                            "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                        }
                    }
                }
            ...
            ]
        }
    }
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Sanal ağı ve kümeyi dağıtma

Ardından, ağ topolojisini ayarlayın ve Service Fabric kümesini dağıtın. [Azuredeploy.json] [ template] Resource Manager şablonu, Service Fabric için bir sanal ağ, alt ağ ve ağ güvenlik grubu oluşturur. Şablon tarafından sertifika güvenliği etkin bir küme de dağıtılır. Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden bir sertifika kullanın. Test kümelerinin güvenliğinin sağlanması için otomatik olarak imzalanan bir sertifika kullanılabilir.

Bu makalede şablonda küme sertifikayı belirlemek için sertifika parmak izini kullanan bir küme dağıtır. İki sertifika, sertifika yönetimi zorlaştırır aynı parmak olabilir. Sertifika ortak adları için sertifika parmak izleri dağıtılan bir kümeden geçiş sertifika yönetimini kolaylaştırır. Sertifika Yönetim sertifikası ortak adlarının kullanmak için küme güncelleştirme hakkında bilgi edinmek için okumaya devam [sertifika ortak adına yönetim değişiklik kümesine](service-fabric-cluster-change-cert-thumbprint-to-cn.md).

### <a name="create-a-cluster-by-using-an-existing-certificate"></a>Mevcut bir sertifikayı kullanarak küme oluşturma

Aşağıdaki betik [yeni AzServiceFabricCluster](/powershell/module/az.servicefabric/New-azServiceFabricCluster) cmdlet'i ve azure'da yeni bir kümeye dağıtmak için bir şablon. Cmdlet tarafından Azure'da yeni bir anahtar kasası oluşturulur ve sertifikanız karşıya yüklenir.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # Must match the clustername parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-by-using-a-new-self-signed-certificate"></a>Yeni, otomatik olarak imzalanan bir sertifika kullanarak küme oluşturma

Aşağıdaki betik [yeni AzServiceFabricCluster](/powershell/module/az.servicefabric/New-azServiceFabricCluster) cmdlet'i ve azure'da yeni bir kümeye dağıtmak için bir şablon. Cmdlet tarafından Azure'da yeni bir anahtar kasası oluşturulur, yeni bir otomatik olarak imzalanan sertifika anahtar Kasası'na ekler ve yerel olarak sertifika dosyasını indirir.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanma

Service Fabric SDK'sıyla yüklü Service Fabric PowerShell modülünü kullanarak kümeye bağlanın.  İlk olarak sertifikayı bilgisayarınızda geçerli kullanıcının Kişisel (My) deposuna yükleyin. Aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Şimdi güvenli kümenize bağlanmaya hazırsınız.

**Service Fabric** PowerShell modülü, Service Fabric kümelerini, uygulamalarını ve hizmetlerini yönetmek için birçok cmdlet sağlar. Güvenli kümeye bağlanmak için [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini kullanın. Sertifika SHA1 parmak izi ve bağlantı uç noktası ayrıntıları önceki adımda elde edilen çıktıdan bulunabilir.

Daha önce Azure AD istemci kimlik doğrulaması ayarlarsanız, aşağıdaki komutu çalıştırın: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

Azure AD istemci kimlik doğrulaması ayarlamadınız ise aşağıdaki komutu çalıştırın:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Sizin bağlı olduğunuzu ve kullanarak, kümenin iyi durumda olduğunu denetleyin [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'i.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici serisindeki diğer makalelerde, oluşturduğunuz küme kullanılır. Hemen bir sonraki makaleye geçmeyecekseniz ücret alınmaması için [kümeyi silmeniz](service-fabric-cluster-delete.md) iyi olur.

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi ölçeklendirme konusunda bilgi almak için aşağıdaki öğreticiye geçin.

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * Tanı koleksiyonu yapılandırma
> * Eventstore'a hizmetini ayarlama
> * Azure İzleyici günlüklerini ayarlayın
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Ardından, kümenizi izlemek hakkında bilgi edinmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir kümesini izleme](service-fabric-tutorial-monitor-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
