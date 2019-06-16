---
title: Bir Azure Service Fabric küme şablonu oluştur | Microsoft Docs
description: Service Fabric kümesi için bir Resource Manager şablonunun nasıl oluşturulacağını öğrenin. Azure Active Directory (Azure AD) güvenlik ve Azure anahtar kasası istemci kimlik doğrulaması için yapılandırın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/16/2018
ms.author: aljo
ms.openlocfilehash: 2fdea1f088dd6eabdf7d72342c837d976133a1bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60386894"
---
# <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Bir Service Fabric kümesine Resource Manager şablonu oluşturma

Bir [Azure Service Fabric kümesi](service-fabric-deploy-anywhere.md) bir ağa bağlı sanal makinelerin, mikro hizmetlerin dağıtılıp yönetildiği kümesidir. Azure'da çalışan bir Service Fabric kümesi, bir Azure kaynağıdır ve yönetilen ve izlenen Kaynak Yöneticisi kullanılarak dağıtılır.  Bu makalede Azure'da çalışan bir Service Fabric kümesi için bir Resource Manager şablonu nasıl oluşturulur.  Şablon tamamlandıktan sonra [azure'da kümeyi dağıtmak](service-fabric-cluster-creation-via-arm.md).

Küme güvenliği, kümenin ilk ayarlanır ve daha sonra değiştirilemez yapılandırılır. Salt okunur bir küme ayarı önce [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security]. Azure'da Service Fabric kullanımları x509 güvenli kümenize ve onun uç noktaları, istemcilerin kimliğini doğrulamak için sertifika ve şifrelersiniz. Ayrıca, Azure Active Directory Yönetim uç noktalarına erişimi güvenli hale getirmek için tavsiye edilir. Azure AD kiracılar ve kullanıcılar küme oluşturmadan önce oluşturulmalıdır.  Daha fazla bilgi için okuma [istemcilerin kimliğini doğrulamak için Azure AD'yi ayarlarken ayarlamak](service-fabric-cluster-creation-setup-aad.md).

Üretim iş yüklerini çalıştırmak için bir üretim kümesini dağıtmadan önce öncelikle mutlaka okuyun [üretim hazırlık denetim](service-fabric-production-readiness-checklist.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma
Örnek Resource Manager şablonları kullanılabilir [github'daki Azure örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates). Bu şablonlar, küme şablonunuza için başlangıç noktası olarak kullanılabilir.

Bu makalede [güvenli beş düğümlü küme] [ service-fabric-secure-cluster-5-node-1-nodetype] örnek şablonu ve şablon parametreleri. İndirme *azuredeploy.json* ve *azuredeploy.parameters.json* bilgisayarınıza ve iki dosyayı da sık kullandığınız metin düzenleyicinizde açın.

> [!NOTE]
> (Azure kamu, Azure Çin'de, Azure Almanya) Ulusal Bulutlar için aşağıdakiler de eklemelisiniz `fabricSettings` şablonunuz için: `AADLoginEndpoint`, `AADTokenEndpointFormat` ve `AADCertEndpointFormat`.

## <a name="add-certificates"></a>Sertifika Ekle
Sertifika anahtarlarını içeren anahtar kasası başvurarak sertifikaları için Küme Kaynak Yöneticisi şablonu ekleyin. Resource Manager şablon parametreleri dosyasında bu anahtar kasası parametrelerini ve değerlerini ekleyin (*azuredeploy.parameters.json*).

### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Bir sanal makine ölçek kümesi osProfile tüm sertifikaları Ekle
Kümeye yüklü her sertifikanın yapılandırılmalıdır **osProfile** ölçek bölümünü kaynak (Microsoft.Compute/virtualMachineScaleSets) ayarlayın. Bu eylem Vm'lerde sertifikayı yüklemek için kaynak sağlayıcısı bildirir. Bu yükleme, küme sertifikası hem de uygulamalarınız için kullanmayı planladığınız herhangi bir uygulama güvenlik sertifikaları içerir:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

### <a name="configure-the-service-fabric-cluster-certificate"></a>Service Fabric küme sertifikası yapılandırma

Küme kimlik doğrulama sertifikası hem de Service Fabric küme kaynak (Microsoft.ServiceFabric/clusters) yapılandırılması gerekir ve sanal makine ölçek kümesi kaynak sanal makine ölçek için Service Fabric uzantısı ayarlar. Bu düzenleme, küme kimlik doğrulaması ve yönetim uç noktaları için sunucu kimlik doğrulaması kullanmak için yapılandırmak Service Fabric kaynak sağlayıcısı sağlar.

#### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Kaynak sertifika bilgilerini sanal makine ölçek kümesi ekleme

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "commonNames": ["[parameters('certificateCommonName')]"],
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

#### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağı için sertifika bilgilerini ekleyin

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
  }
}
```

## <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Azure AD istemci erişimi için kullanılacak Azure AD Yapılandırması Ekle

Küme Kaynak Yöneticisi şablonu için Azure AD yapılandırmasının sertifika anahtarlar içeren anahtar kasası başvurarak ekleyin. Resource Manager şablon parametreleri dosyasında bu Azure AD parametrelerini ve değerlerini ekleyin (*azuredeploy.parameters.json*). 

> [!NOTE]
> Azure AD kiracılar ve kullanıcılar küme oluşturmadan önce oluşturulmalıdır.  Daha fazla bilgi için okuma [istemcilerin kimliğini doğrulamak için Azure AD'yi ayarlarken ayarlamak](service-fabric-cluster-creation-setup-aad.md).

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
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

## <a name="populate-the-parameter-file-with-the-values"></a>Parametre dosyasını değerlerle doldurmak

Son olarak, parametre dosyasını doldurmak için çıkış değerleri anahtar kasası ve Azure AD PowerShell komutlarını kullanın.

Ardından Azure service fabric RM PowerShell modüllerini kullanmayı planlıyorsanız, küme sertifika bilgilerini doldurmak gerekmez. Sisteminin imzalı self oluşturmasını istiyorsanız, kümenin güvenliği için sertifika, null olarak kalmasını. 

> [!NOTE]
> RM modülleri almak ve bu boş parametre değerleri doldurmak parametre adları çok aşağıdaki adlarla eşleşir.

```json
"clusterCertificateThumbprint": {
    "value": ""
},
"certificateCommonName": {
    "value": ""
},
"clusterCertificateUrlValue": {
    "value": ""
},
"sourceVaultvalue": {
    "value": ""
},
```

Uygulama sertifikaları kullanarak ya da anahtar kasasına yüklenmiş mevcut bir kümeye kullanıyorsanız, bu bilgileri alın ve bunu doldurmak gerekir.

RM modülleri, Azure AD yapılandırmasının oluşturma yeteneği yoktur için istemci erişimi için Azure AD kullanmayı planlıyorsanız, bu doldurmanız gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```

## <a name="test-your-template"></a>Şablonunuzu test
Resource Manager şablonunuzu bir parametre dosyasıyla test etmek için aşağıdaki PowerShell komutunu kullanın:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Bir sorunla karşılaşırsanız ve şifreli iletileri alma durumunda, kullanın, ardından "-Debug" seçeneği olarak.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Aşağıdaki diyagram, Azure AD yapılandırma ve anahtar kasası Resource Manager şablonunuzu nerelerde gösterir.

![Resource Manager bağımlılık Haritası][cluster-security-arm-dependency-map]

## <a name="next-steps"></a>Sonraki adımlar
Kümeniz için bir şablon olduğuna göre bilgi nasıl [Azure'a küme dağıtma](service-fabric-cluster-creation-via-arm.md).  Henüz yapmadıysanız, okuma [üretim hazırlık denetim](service-fabric-production-readiness-checklist.md) bir üretim kümesini dağıtmadan önce.

Bu makalede dağıtılan kaynakların özelliklerini ve JSON söz dizimi hakkında bilgi edinmek için bkz:

* [Microsoft.ServiceFabric/clusters](/azure/templates/microsoft.servicefabric/clusters)
* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers)
* [Microsoft.Compute/virtualMachineScaleSets](/azure/templates/microsoft.compute/virtualmachinescalesets)

<!-- Links -->
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-create-template/cluster-security-arm-dependency-map.png
