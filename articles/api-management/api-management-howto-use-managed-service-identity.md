---
title: Kullanım Azure yönetilen hizmet kimliği Azure API Management | Microsoft Docs
description: Yönetilen hizmet kimliği Azure API Management'te kullanmayı öğrenin
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 98aa70935a3efbbe2edb07aade85fa3ea17ce786
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Azure yönetilen hizmet kimliği Azure API Management kullanma

Bu makalede, bir API Management hizmet örneği için bir yönetilen hizmet kimliği oluşturma ve diğer kaynaklarına erişmek nasıl gösterir. Azure Active Directory (Azure AD) tarafından oluşturulan bir yönetilen hizmet kimliği, API Management Örneğinize kolay ve güvenli bir şekilde Azure AD korumalı gibi başka kaynaklar Azure anahtar kasası erişim sağlar. Bu yönetilen hizmet kimliği, Azure tarafından yönetilen ve sağlamak veya tüm gizli döndürmek gerektirmez. Azure yönetilen hizmet kimliği hakkında daha fazla bilgi için bkz: [Azure kaynakları için Yönetilen hizmet kimliği](../active-directory/msi-overview.md).

## <a name="create-a-managed-service-identity-for-an-api-management-instance"></a>API Management örneği için bir yönetilen hizmet kimliği oluşturma

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Yönetilen hizmet kimliği portalında ayarlamak için ilk normal olarak API Management örneği oluşturur ve özelliği etkinleştirmek.

1. Normal olarak portalda API Management örneği oluşturma. İçin portalda gidin.
2. Seçin **yönetilen hizmet kimliği**.
3. Azure Active Directory ile kayıt için açık geçiş yapın. Kaydet'i tıklatın.

![MSI etkinleştir](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak

Kaynak tanımı'nda aşağıdaki özellikler dahil olmak üzere API Management örneği bir kimlikle oluşturabilirsiniz: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Bu oluşturup, API Management Örneğinize kimliğini yönetmek için Azure bildirir. 

Örneğin, tam bir Azure Resource Manager şablonu aşağıdakine benzeyebilir:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0"
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "contoso",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "Developer",
                "capacity": "1"
            },
            "properties": {
                "publisherEmail": "admin@contoso.com",
                "publisherName": "Contoso"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>Diğer kaynaklarına erişmek için Yönetilen hizmet kimliğini kullan

> [!NOTE]
> Şu anda, yönetilen hizmet kimliği sertifikaları Azure anahtar Kasası'API Management özel etki alanı adları elde etmek için kullanılabilir. Daha fazla senaryo yakında desteklenecek.
> 
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Azure anahtar Kasası'nı bir sertifika edinin

#### <a name="prerequisites"></a>Önkoşullar
1. Pfx sertifika içeren anahtar kasası aynı Azure aboneliği ve API Management hizmeti ile aynı kaynak grubunda olması gerekir. Bu Azure Resource Manager şablonu bir gereksinimdir. 
2. Gizli içerik türü olmalıdır *uygulama/x-pkcs12*. Sertifikayı karşıya yüklemek için aşağıdaki komut dosyasını kullanabilirsiniz:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Sertifikanın nesne sürümü sağlanmazsa, anahtar Kasası'na yüklendikten sonra API Management sertifikanın daha yeni sürümü otomatik olarak al. 

Aşağıdaki örnek, aşağıdaki adımları içeren bir Azure Resource Manager şablonu gösterir:

1. API Management örneği ile bir yönetilen hizmet kimliği oluşturma.
2. Bir Azure anahtar kasası örneğinin erişim ilkeleri güncelleştirmek ve ondan parolaları almak API Management örneği izin verin.
3. API Management örneği anahtar kasası örneğinden bir sertifika ile özel etki alanı adını ayarlayarak güncelleştirin.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure yönetilen hizmet kimliği hakkında daha fazla bilgi edinin:

* [Azure kaynakları için Yönetilen hizmet kimliği](../active-directory/msi-overview.md)
* [Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates)

