---
title: Azure yönetilen uygulamayla yönetilen kimlik
description: Yönetilen bir uygulama yönetilen bir kimlik ile yapılandırmayı öğrenin. Yönetilen kimlik, yönetilen mevcut kaynaklara bağlı uygulamaları dağıtmak için kullanılabilir yönetilen kaynak grubu dışında Azure kaynaklarını yönetmek ve etkinlik günlüğü için yönetilen uygulamalar, işletimsel bir kimlik sağlamak için yönetilen uygulamalar vermek ve diğer Azure Hizmetleri.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: jobreen
author: jjbfour
ms.date: 05/13/2019
ms.openlocfilehash: 5ef653e825a5f1eb0f5df52f9c2544a5224b34cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66003454"
---
# <a name="azure-managed-application-with-managed-identity"></a>Azure yönetilen uygulamayla yönetilen kimlik

> [!NOTE]
> Yönetilen uygulamalar için yönetilen kimlik desteği şu anda Önizleme aşamasındadır. Yönetilen kimlik kullanmak için lütfen 2018-09-01-Önizleme API sürümü kullanın.

Yönetilen bir kimlik içeren yönetilen uygulamayı yapılandırmayı öğrenin. Yönetilen kimlik müşteri yönetilen uygulamayı ek mevcut kaynaklara erişim izin vermek için kullanılabilir. Kimlik Azure platformu tarafından yönetilir ve sağlama veya herhangi bir gizli anahtar döndürme gerektirmez. Yönetilen kimlikleri de Azure Active Directory (AAD) hakkında daha fazla bilgi için bkz. [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md).

Uygulamanızı kimlikler iki tür verilebilir:

- A **sistem tarafından atanan kimlik** uygulamanıza bağlıdır ve uygulamanızı silinirse silinir. Bir uygulama yalnızca tek bir sistem tarafından atanan kimlik olabilir.
- A **kullanıcı tarafından atanan kimlik** tek başına uygulamanıza atanan Azure kaynağı olduğundan. Bir uygulamanın birden çok kullanıcı tarafından atanan kimlik olabilir.

## <a name="how-to-use-managed-identity"></a>Yönetilen kimliği kullanma

Yönetilen kimlik birçok senaryo için yönetilen uygulamalar sağlar. Çözülebileceğini bazı yaygın senaryolar şunlardır:

- Yönetilen bir uygulama dağıtma, mevcut Azure kaynaklarına bağlı. Örnek bir Azure sanal makinesi (VM) bağlı yönetilen uygulama içinde dağıtma bir [var olan ağ arabirimini](../virtual-network/virtual-network-network-interface-vm.md).
- Yönetilen uygulama ve yayımcıya dışında Azure kaynaklarına erişim **yönetilen kaynak grubu**.
- Etkinlik günlüğü ve diğer Azure Hizmetleri için yönetilen uygulamalar, işletimsel bir kimlik sağlama.

## <a name="adding-managed-identity"></a>Yönetilen kimlik ekleniyor

Yönetilen bir kimlik ile yönetilen bir uygulama oluşturmak için Azure kaynağında ayarlamak için ek bir özellik gerekir. Aşağıdaki örnek, bir örneği gösterilmektedir. **kimlik** özelliği:

```json
{
"identity": {
    "type": "SystemAssigned, UserAssigned",
    "userAssignedIdentities": {
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity": {}
    }
}
```

İle yönetilen bir uygulama oluşturmak için iki ortak yolla **kimlik**: [CreateUIDefinition.json](./create-uidefinition-overview.md) ve [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md). Senaryoları için basit tek oluşturun, yönetilen kimliği etkinleştirmek için daha zengin bir deneyim sağlar çünkü CreateUIDefinition kullanılmalıdır. Ancak, Gelişmiş veya karmaşık ile ilgilenirken gerektiren sistemler otomatik veya birden fazla yönetilen uygulama dağıtımları, şablonlar kullanılabilir.

### <a name="using-createuidefinition"></a>CreateUIDefinition kullanma

Yönetilen bir uygulama yönetilen bir kimlik yapılandırılabilir [CreateUIDefinition.json](./create-uidefinition-overview.md). İçinde [çıkarır bölüm](./create-uidefinition-overview.md#outputs), anahtar `managedIdentity` yönetilen uygulama şablonu Identity özelliği geçersiz kılmak için kullanılabilir. Örnek kaçırırsanız etkinleştirecek **sistem tarafından atanan** yönetilen uygulama kimliği. Daha karmaşık kimlik nesneleri için girişler tüketici istemeniz CreateUIDefinition öğelerini kullanarak biçimlendirilmiş olmalıdır. İle yönetilen uygulamalar oluşturmak için bu girişlerin kullanılabilir **kullanıcı tarafından atanan kimlik**.

```json
"outputs": {
    "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
}
```

#### <a name="when-to-use-createuidefinition-for-managed-identity"></a>CreateUIDefinition için yönetilen kimliği kullanma zamanı

Aşağıda bazı öneriler CreateUIDefinition yönetilen kimliği yönetilen uygulamaları etkinleştirmek için kullanıldığı durumlar da mevcuttur.

- Yönetilen uygulama oluşturma, Azure portalı veya Market gider.
- Yönetilen kimlik karmaşık tüketici giriş gerektirir.
- Yönetilen uygulama oluşturma yönetilen Kimliği gereklidir.

#### <a name="systemassigned-createuidefinition"></a>SystemAssigned CreateUIDefinition

Yönetilen uygulama için SystemAssigned kimliğini sağlayan bir temel CreateUIDefinition.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
        ],
        "outputs": {
            "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
        }
    }
}
```

#### <a name="userassigned-createuidefinition"></a>UserAssigned CreateUIDefinition

Alan bir temel CreateUIDefinition bir **kullanıcı tarafından atanan kimlik** kaynak olarak giriş ve yönetilen uygulama için UserAssigned kimlik sağlar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "manageIdentity",
                "label": "Identity",
                "subLabel": {
                    "preValidation": "Manage Identities",
                    "postValidation": "Done"
                },
                "bladeTitle": "Identity",
                "elements": [
                    {
                        "name": "userAssignedText",
                        "type": "Microsoft.Common.TextBox",
                        "label": "User assigned managed identity",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('manageIdentity').userAssignedText),':{}}}'))]"
        }
    }
}
```

Yukarıdaki CreateUIDefinition.json girmek bir tüketici için bir metin kutusu olan bir oluşturma kullanıcı deneyimi oluşturur **kullanıcı tarafından atanan kimlik** Azure kaynak kimliği. Oluşturulan deneyimi gibi görünür:

![Örnek kullanıcı tarafından atanan kimlik CreateUIDefinition](./media/publish-managed-identity/user-assigned-identity.png)

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

> [!NOTE]
> Market'te yönetilen uygulama şablonları, müşterilerin Azure portalı üzerinden giderek deneyimi oluşturmak için otomatik olarak oluşturulur.
> Bu senaryolar için `managedIdentity` CreateUIDefinition çıkış anahtar kullanılan, etkin olarak kimlik.

Azure Resource Manager şablonları yönetilen kimliği etkinleştirilebilir. Örnek kaçırırsanız etkinleştirecek **sistem tarafından atanan** yönetilen uygulama kimliği. Daha karmaşık kimlik nesneleri girişleri sağlamak için Azure Resource Manager şablon parametreleri kullanarak biçimlendirilmiş olmalıdır. İle yönetilen uygulamalar oluşturmak için bu girişlerin kullanılabilir **kullanıcı tarafından atanan kimlik**.

#### <a name="when-to-use-azure-resource-manager-templates-for-managed-identity"></a>Azure Resource Manager şablonları için yönetilen kimliği kullanma zamanı

Yönetilen uygulamaların yönetilen kimliği etkinleştirmek için Azure Resource Manager şablonlarını kullanma zamanı ilgili bazı öneriler aşağıda verilmiştir.

- Yönetilen uygulamalar, bir şablonu temel alan programlı olarak dağıtılabilir.
- Yönetilen kimliği için özel rol atamaları, yönetilen uygulama sağlamak için gereklidir.
- Yönetilen uygulama, Azure portalı ve Market oluşturma akışı gerekmez.

#### <a name="systemassigned-template"></a>SystemAssigned şablonu

İle yönetilen bir uygulama dağıtır temel bir Azure Resource Manager şablonu **sistem tarafından atanan** kimlik.

```json
"resources": [
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

### <a name="userassigned-template"></a>UserAssigned şablonu

İle yönetilen bir uygulama dağıtır temel bir Azure Resource Manager şablonu bir **kullanıcı tarafından atanan kimlik**.

```json
"resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('managedIdentityName')]",
      "apiVersion": "2018-11-30",
      "location": "[parameters('location')]"
    },
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',parameters('managedIdentityName'))]": {}
            }
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

## <a name="granting-access-to-azure-resources"></a>Azure kaynaklarına erişim izni verme

Yönetilen bir uygulamanın bir kimlik verildiğinde, mevcut azure kaynaklarına erişim verilebilir. Bu işlem, Azure portalında erişim denetimi (IAM) arabirimi aracılığıyla yapılabilir. Yönetilen uygulama adını veya **kullanıcı tarafından atanan kimlik** bir rol ataması eklemek için arama yapılabilir.

![Yönetilen uygulama rolü ataması ekleme](./media/publish-managed-identity/identity-role-assignment.png)

## <a name="linking-existing-azure-resources"></a>Mevcut Azure kaynaklarına bağlama

> [!NOTE]
> A **kullanıcı tarafından atanan kimlik** olmalıdır [yapılandırılmış](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) yönetilen uygulamayı dağıtmadan önce. Ayrıca, yönetilen uygulamaların bağlantılı kaynak dağıtımını yalnızca desteklenen **Market** türü.

Yönetilen kimlik, dağıtım sırasında var olan kaynaklara erişim gerektiren yönetilen uygulamayı dağıtmak için de kullanılabilir. Müşteri tarafından yönetilen uygulama sağlandığında **kullanıcı tarafından atanan kimlikleri** için ek yetkilendirme sağlamak için eklenen **mainTemplate** dağıtım.

### <a name="authoring-the-createuidefinition-with-a-linked-resource"></a>CreateUIDefinition ile bağlantılı bir kaynak yazma

Yönetilen uygulama mevcut kaynaklara, hem var olan Azure kaynak dağıtımını bağlanırken ve **kullanıcı tarafından atanan kimlik** geçerli rolüyle, kaynak atamaya sağlanmalıdır.

 İki giriş gerektiriyor CreateUIDefinition örnek: bir ağ arabirimi kaynak kimliği ve kullanıcı tarafından atanan kaynak kimliği.

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
                "name": "managedApplicationSetting",
                "label": "Managed Application Settings",
                "subLabel": {
                    "preValidation": "Managed Application Settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Managed Application Settings",
                "elements": [
                    {
                        "name": "networkInterfaceId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "network interface resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Network/networkInterfaces/existingnetworkinterface",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.Network/networkInterfaces/networkinterface1",
                        "visible": true
                    },
                    {
                        "name": "userAssignedId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "user assigned identity resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/identity1",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "existingNetworkInterfaceId": "[steps('managedApplicationSetting').networkInterfaceId]",
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('managedApplicationSetting').userAssignedId),':{}}}'))]"
        }
    }
}
```

Bu CreateUIDefinition.json iki alanı olan bir oluşturma kullanıcı deneyimi oluşturur. İlk alan için yönetilen uygulama dağıtımına bağlı kaynak Azure kaynak Kimliğini girmesini sağlar. İkincisi ise bir tüketici girmek **kullanıcı tarafından atanan kimlik** bağlı Azure kaynağına erişimi olan Azure kaynak kimliği. Oluşturulan deneyimi gibi görünür:

![CreateUIDefinition ile iki giriş örnek: bir ağ arabirimi kaynak kimliği ve kullanıcı tarafından atanan kaynak kimliği](./media/publish-managed-identity/network-interface-cuid.png)

### <a name="authoring-the-maintemplate-with-a-linked-resource"></a>Bağlantılı bir kaynağı ile mainTemplate yazma

CreateUIDefinition güncelleştirmeye ek olarak, ana şablon ayrıca bağlı kaynak kimliği geçirilen kabul edecek şekilde güncelleştirilmesi gerekiyor Ana şablon yeni bir parametre ekleyerek yeni çıkış kabul etmek için güncelleştirilebilir. Bu yana `managedIdentity` çıkış oluşturulan yönetilen uygulama şablonu değerini geçersiz kılar, ana şablona geçirilir ve Parametreler bölümünde eklenmemelidir.

CreateUIDefinition tarafından sağlanan mevcut bir ağ arabirimi için ağ profilinde ayarlar bir örnek ana şablonu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "existingNetworkInterfaceId": { "type": "string" }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2016-04-30-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "myLinkedResourceVM",
            "location": "[resourceGroup().location]",
            "properties": {
                …,
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[parameters('existingNetworkInterfaceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="consuming-the-managed-application-with-a-linked-resource"></a>Yönetilen uygulama ile bağlantılı bir kaynağı tüketen

Yönetilen uygulama paketi oluşturduktan sonra Azure Portalı aracılığıyla yönetilen uygulama tarafından kullanılabilir. Kullanılabilir önce önkoşul birkaç adım vardır.

- Gerekli bağlı Azure kaynak örneği oluşturulmalıdır.
- **Kullanıcı tarafından atanan kimlik** olmalıdır [oluşturuldu ve rol atamalarını belirtilen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) bağlı kaynağı.
- Varolan kaynak kimliği bağlantılı ve **kullanıcı tarafından atanan kimlik** kimliği için CreateUIDefinition sağlanır.

## <a name="accessing-the-managed-identity-token"></a>Yönetilen kimlik belirteci erişme

Yönetilen uygulama belirteci aracılığıyla artık erişilebilir `listTokens` yayımcı Kiracı API'si. Bir örnek istek gibi görünebilir:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}?api-version=2018-09-01-preview HTTP/1.1
```

Örnek yanıt aşağıdaki gibi görünmelidir:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "value": [
        {
            "access_token": "eyJ0eXAi…",
            "expires_in": "2…",
            "expires_on": "1557…",
            "not_before": "1557…",
            "authorizationAudience": "https://management.azure.com/",
            "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}",
            "token_type": "Bearer"
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel sağlayıcı ile bir yönetilen uygulama yapılandırma](./custom-providers-overview.md)