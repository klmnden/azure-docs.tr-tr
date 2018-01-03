---
title: "Oluşturma ve bir Azure hizmet Kataloğu yönetilen uygulama yayımlama | Microsoft Docs"
description: "Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 46adcdf39625c85dc962a7541b68c5500cf920ee
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Dahili tüketim için yönetilen bir uygulama yayımlama

Oluşturma ve Azure yayımlama [yönetilen uygulamaları](overview.md) kuruluşunuzun üyeleri için yöneliktir. Örneğin, bir BT departmanının kuruluş standartlarıyla uyumluluğu sağlamak yönetilen uygulamaları yayımlayabilirsiniz. Bu yönetilen uygulamaların Azure Market hizmet Kataloğu kullanılabilir.

Hizmet kataloğu için yönetilen bir uygulamayı yayımlamak için yapmanız gerekir:

* İle yönetilen uygulamayı dağıtmak için gereken kaynakları tanımlayan bir şablon oluşturun.
* Portal için kullanıcı arabirimi öğeleri yönetilen uygulama dağıtırken tanımlayın.
* Gerekli şablon dosyalarını içeren bir .zip paketi oluşturun.
* Hangi kullanıcı, Grup veya uygulama kullanıcının abonelik kaynak grubunda erişmesi karar verin.
* .Zip paketi noktaları ve kimlik için erişim isteğinde yönetilen uygulama tanımı oluşturun.

Bu makalede, yönetilen uygulamanızın yalnızca bir depolama hesabını içerir. Yönetilen bir uygulama yayımlama adımları göstermeye yöneliktir. Tam örnek için bkz: [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).

## <a name="create-the-resource-template"></a>Kaynak şablonu oluşturma

Adlı bir dosyaya her yönetilen uygulama tanımını içeren **mainTemplate.json**. İçinde Azure kaynaklarını sağlamak tanımlar. Şablon normal bir Resource Manager şablonundan farklı değildir.

Adlı bir dosya oluşturun **mainTemplate.json**. Adı büyük/küçük harf duyarlıdır.

Aşağıdaki JSON dosyanıza ekleyin. Bir depolama hesabı oluşturmak için parametreler tanımlar ve depolama hesabının özelliklerini belirtir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString('storage'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

MainTemplate.json dosyasını kaydedin.

## <a name="create-the-user-interface-definition"></a>Kullanıcı arabirim tanımı oluştur

Azure Portalı'nı kullanan **createUiDefinition.json** yönetilen uygulamayı oluşturan kullanıcılar için kullanıcı arabirimi oluşturmak için dosya. Tanımladığınız nasıl kullanıcılar her parametre için değer girin. Bir açılan listesinden, metin kutusuna, parola kutusu ve diğer araçları giriş gibi seçeneklerini kullanabilirsiniz. Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.

Adlı bir dosya oluşturun **createUiDefinition.json**. Adı büyük/küçük harf duyarlıdır.

Aşağıdaki JSON dosyasına ekleyin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

CreateUIDefinition.json dosyasını kaydedin.

## <a name="package-the-files"></a>Paket dosyaları

İki dosya app.zip adlı bir .zip dosyasına ekleyin. İki dosya .zip dosyası kök düzeyinde olması gerekir. Bir klasöre yerleştirin, gerekli dosyaları mevcut olmayan durumları yönetilen uygulama tanımı oluşturulurken bir hata alırsınız. 

Paket, burada da tüketilebilir gelen erişilebilen bir konuma karşıya yükleyin. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Bir Azure Active Directory kullanıcı grubu veya uygulama oluşturma

Sonraki adım, bir kullanıcı grubu veya kaynakları müşteri adına yönetmek için uygulama seçmektir. Bu kullanıcı grubu veya uygulama atanan role göre yönetilen kaynak grubu üzerinde izinleri vardır. Rol sahibi veya katkıda gibi herhangi bir yerleşik rol tabanlı erişim denetimi (RBAC) rolüne olabilir. Ayrıca bir bireysel kullanıcı kaynakları yönetme izni verebilirsiniz, ancak genellikle bir kullanıcı grubuna bu iznini atayın. Yeni bir Active Directory kullanıcı grubu oluşturmak için bkz: [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../active-directory/active-directory-groups-create-azure-portal.md).

Kaynakları yönetmek için kullanılacak kullanıcı grubu nesne kimliği gerekir. 

![Grup Kimliği alma](./media/publish-service-catalog-app/get-group-id.png)

### <a name="get-the-role-definition-id"></a>Rol tanımı kimliği alma

Ardından, kullanıcı, kullanıcı grubu veya uygulama için erişim vermek istediğiniz RBAC yerleşik rolün rol tanımı kimliği gerekir. Genellikle, sahibi veya katkıda bulunan veya okuyucu rolü kullanın. Aşağıdaki komut, Sahip rolünün rol tanımı kimliğinin nasıl alınacağını gösterir:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Zaten bir kaynak grubu, yönetilen uygulama tanımını depolamak için yoksa, şimdi oluşturun:

```powershell
New-AzureRmResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Şimdi, yönetilen uygulama tanımı kaynağını oluşturun.

```powershell
$blob = Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzureRmManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "<group-id>:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="create-the-managed-application-by-using-the-portal"></a>Portalı kullanarak yönetilen uygulama oluşturma

Şimdi, şirketinizdeki yönetilen uygulamayı dağıtmak üzere portalı kullanın. Pakette oluşturulan kullanıcı arabirimi bakın.

1. Azure portalına gidin. Seçin **+ yeni** arayın ve **hizmet Kataloğu**.

   ![Arama hizmet Kataloğu](./media/publish-service-catalog-app/select-new.png)

1. Seçin **hizmet Kataloğu yönetilen uygulama**.

   ![Hizmet Kataloğu'nu seçin](./media/publish-service-catalog-app/select-service-catalog.png)

1. **Oluştur**’u seçin.

   ![Bu seçeneği belirleyin](./media/publish-service-catalog-app/select-create.png)

1. Kullanılabilir çözümleri listesinden oluşturmak istediğiniz yönetilen uygulamayı bulun ve seçin. **Oluştur**’u seçin.

   ![Yönetilen uygulama Bul](./media/publish-service-catalog-app/find-application.png)

1. Yönetilen uygulama için gerekli olan temel bilgiler sağlar. Abonelik ve yönetilen uygulamayı içeren yeni bir kaynak grubu belirtin. Seçin **Batı Orta ABD** konumu. İşiniz bittiğinde, seçin **Tamam**.

   ![Yönetilen uygulama parametreleri sağlayın](./media/publish-service-catalog-app/provide-basics.png)

1. Yönetilen uygulama kaynaklarında özgü değerleri sağlayın. İşiniz bittiğinde, seçin **Tamam**.

   ![Kaynak parametreleri sağlayın](./media/publish-service-catalog-app/provide-resource-values.png)

1. Şablon, sağlanan değerler doğrular. Doğrulama başarılı olursa seçin **Tamam** dağıtımı başlatmak için.

   ![Yönetilen uygulama doğrula](./media/publish-service-catalog-app/validate.png)

Dağıtım tamamlandıktan sonra yönetilen uygulama applicationGroup adlı bir kaynak grubunda mevcut. Depolama hesabı applicationGroup artı bir karma dize değeri adlı bir kaynak grubunda yok.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Projeleri, örneğin bkz [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.
