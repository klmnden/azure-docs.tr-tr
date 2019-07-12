---
title: Yerleşik bir müşteri Azure kaynak yönetimi - Azure Deniz temsilcisi
description: Nasıl ekleme bir müşteri Azure kaynak yönetimi erişilebilecek ve kendi kiracınızı yönetilen kaynaklarını izin verme temsilcisi öğrenin.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 1885a6220f14de234710b6980b5d3b6a6172bb7e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809865"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Yerleşik bir müşteri Azure kaynak yönetimi temsilcisi

Bu makalede, nasıl, bir hizmet sağlayıcısı olarak temsil edilen Azure kaynak yönetimi, müşteriye temsilci kaynaklarını (abonelikler ve/veya kaynak grupları) erişilebilen ve kendi Azure Active Directory (yönetilen için izin verme ekleyebilir açıklanmaktadır. Azure AD) kiracısı. Hizmet sağlayıcıları ve burada müşterileri diyoruz, ancak birden çok kiracının yönetme kuruluşların yönetim deneyimlerinden birleştirmek için aynı işlem kullanabilirsiniz.

Kaynaklar için birden çok müşteriyi yönetiyorsanız, bu işlemi yineleyebilirsiniz. Kiracınız için yetkili bir kullanıcı oturum açtığında, daha sonra kullanıcı müşteri kiralama kapsamlarda her müşterinin kiracısına oturum açmak zorunda kalmadan yönetim işlemlerini gerçekleştirmek için yetkilendirilebilir.

Eklenen aboneliklerinizi, etki müşterilerle yaşadığımız izlemek için Microsoft iş ortağı ağı (MPN) Kimliğiniz ilişkilendirebilirsiniz. Daha fazla bilgi için bkz. [şekilde Azure hesaplarınızı için bir iş ortağı kimliği Bağla](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started).

> [!NOTE]
> Bunlar Azure Market'te yayımlanan yönetilen Hizmetleri teklifi (genel veya özel) satın aldığınızda müşteriler otomatik olarak eklenen olabilir. Daha fazla bilgi için bkz. [yayımlama yönetilen hizmetler sunan Azure Marketi'nde](publish-managed-services-offers.md). Azure Market'te yayımlanan bir teklifi ile burada açıklanan ekleme işlemi de kullanabilirsiniz.

Onboarding işlemi her iki hizmet sağlayıcısının Kiracı içinde ve müşteri kiracısında alınacağı işlemleri gerektirir. Bu makaledeki tüm adımları açıklanmaktadır.

> [!IMPORTANT]
> Şu anda ekleme yapamazsınız bir aboneliği (veya bir abonelik içindeki kaynak grubu) için Azure aboneliği, Azure Databricks kullanıyorsa kaynak yönetimi temsilcisi. Benzer şekilde, bir abonelik ile yetiştirme kaydedilmişse **Microsoft.ManagedServices** kaynak sağlayıcısı, olmayacaktır şu anda bu abonelik için bir Databricks çalışma alanı oluşturabilirsiniz.

## <a name="gather-tenant-and-subscription-details"></a>Kiracı ve abonelik ayrıntılarını toplama

İçin yerleşik bir müşterinin Kiracı olmalıdır bir etkin Azure aboneliği. Aşağıdakileri bilmeniz gerekir:

- Hizmet sağlayıcısının Kiracı (burada müşterinin kaynakları yöneteceğiniz) Kiracı kimliği
- (Hizmet sağlayıcısı tarafından yönetilen kaynaklar gerekir) müşterinin kiracısına Kiracı kimliği
- Hizmet sağlayıcısı tarafından yönetilecek (veya hizmet sağlayıcısı tarafından yönetilecek kaynak gruplarını içeren), müşterinin kiracısındaki belirli her abonelik için abonelik kimlikleri

Bu bilgiler yoksa, aşağıdaki yollardan biriyle alabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Kiracı Kimliğinizi Azure portalının sağ üst taraftaki hesap adınızın üzerine gelin veya seçerek görülebilir **dizini Değiştir**. Seçin ve Kiracı Kimliğinizi kopyalamak için portalın içinde aramak için "Azure Active Directory'den" ve ardından **özellikleri** ve gösterilen değerini kopyalayın **dizin kimliği** alan. Bir aboneliğin Kimliğini bulmak için "Abonelikler" için arama yapın ve ardından uygun bir abonelik kimliğini seçin

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```


## <a name="ensure-the-customers-subscription-is-registered-for-onboarding"></a>Müşterinin hizmet aboneliği ekleme için kayıtlı olduğundan emin olun

Her abonelik için onboarding el ile kaydederek yetkilendirilmelidir **Microsoft.ManagedServices** kaynak sağlayıcısı. Müşteri özetlenen adımları izleyerek bir abonelik kaydedebilirsiniz [Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md).

Müşteri, abonelik aşağıdaki yollardan biriyle hazırlanması için hazır olduğunu doğrulayabilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. Azure portalında aboneliği seçin.
1. Seçin **kaynak sağlayıcıları**.
1. Onaylayın **Microsoft.ManagedServices** olarak gösterir **kayıtlı**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Set-AzContext -Subscription <subscriptionId>
Get-AzResourceProvider -ProviderNameSpace 'Microsoft.ManagedServices'
```

Bu sonuç aşağıdakine benzer döndürülmesi gerekir:

```output
ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationDefinitions}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationAssignments}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {}
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set –subscription <subscriptionId>
az provider show –namespace "Microsoft.ManagedServices" –-output table
```

Bu sonuç aşağıdakine benzer döndürülmesi gerekir:

```output
Namespace                  RegistrationState
-------------------------  -------------------
Microsoft.ManagedServices  Registered
```

## <a name="define-roles-and-permissions"></a>Rolleri ve izinleri tanımlama

Bir hizmet sağlayıcısı olarak, farklı erişmesi için farklı kapsamlar, tek bir müşteriye sahip birden çok teklife kullanmak isteyebilirsiniz.

Yönetimini kolaylaştırmak için her rol için Azure AD kullanıcı gruplarını kullanarak doğrudan o kullanıcıya izin atamak yerine, gruba bireysel kullanıcı eklemek veya kaldırmak izin verme öneririz. Rolleri bir hizmet sorumlusu atamak isteyebilirsiniz. Böylece yalnızca kullanıcıların yanlışlıkla hata olasılığını azaltmaya yardımcı olur, işi tamamlamak için gerekli izinlere sahip en az ayrıcalık ilkesini uygulayın emin olun. Daha fazla bilgi için bkz. [önerilen güvenlik uygulamaları](../concepts/recommended-security-practices.md).

> [!NOTE]
> Rol atamaları, rol tabanlı erişim denetimi (RBAC) kullanmalıdır [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles). Tüm yerleşik roller sahibi ve herhangi bir yerleşik rolü ile temsil edilen Azure kaynak yönetimi ile şu anda desteklenen [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions) izni. Yerleşik kullanıcı erişimi yöneticisi rolü, aşağıda açıklandığı gibi sınırlı kullanım için desteklenir. Özel roller ve [Klasik Abonelik Yöneticisi rolleri](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) da desteklenmez.

Yetkilendirmeleri tanımlamak için her bir kullanıcı, kullanıcı grubu veya erişim vermek istediğiniz bir hizmet sorumlusu kimliği değerlerini bilmeniz gerekir. Ayrıca, atamak istediğiniz yerleşik her rol için rol tanımı kimliği gerekir. Zaten yoksa, bunları aşağıdaki yollardan biriyle alabilirsiniz.



### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>').objectId

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list –-query "[?displayName == '<yourGroupName>'].objectId" –-output tsv

# To retrieve the objectId for an Azure AD user
az ad user show –-upn-or-object-id "<yourUPN>" –-query "objectId" –-output tsv

# To retrieve the objectId for an SPN
az ad sp list –-query "[?displayName == '<spDisplayName>'].objectId" –-output tsv

# To retrieve role definition IDs
az role definition list –-name "<roleName>" | grep name
```

## <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma

İçin yerleşik müşterinizin oluşturmak ihtiyacınız bir [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/) şablon şunları içerir:

|Alan  |Tanım  |
|---------|---------|
|**mspName**     |Hizmet sağlayıcısı adı         |
|**mspOfferDescription**     |Teklifinizi (örneğin, "Contoso VM Yönetimi teklifi") kısa bir açıklaması      |
|**managedByTenantId**     |Kiracı Kimliğiniz         |
|**Yetkilendirmeleri**     |**Principalıd** kullanıcılar/gruplar/SPN'ler kiracınızdan, her biri için değerler bir **principalIdDisplayName** müşterinizin yetkilendirme amacını anlamalarına yardımcı olmak için ve bir yerleşik eşlendi**Roledefinitionıd** erişim düzeyini belirtmek için değer         |

Eklenecek bir müşterinin aboneliğini içinde sağlarız uygun bir Azure Resource Manager şablonu kullanma bizim [depo örnekleri](https://github.com/Azure/Azure-Lighthouse-samples/), yapılandırmanızla eşleşecek ve tanımlamak için değiştirme karşılık gelen bir parametre dosyasıyla birlikte, Yetkilendirme. Ayrı şablonları ekleme olmanıza bağlı olarak bir aboneliğin tümü, bir kaynak grubu veya abonelik içinde birden fazla kaynak grubu sağlanır. Ayrıca, yerleşik kendi aboneliklerinizi bu şekilde tercih ederseniz, Azure Market'te yayımlanmış bir yönetilen hizmet teklifi satın alan müşteriler için kullanılabilir bir şablonu sunuyoruz.

|**Eklemek için bu**  |**Bu Azure Resource Manager şablonu kullanma**  |**Ve bu parametre dosyasını değiştirme** |
|---------|---------|---------|
|Subscription   |[delegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Resource group   |[rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Bir abonelik içindeki birden fazla kaynak grubu   |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|(Bir teklif kullanarak Azure Marketi'nde yayımlandığında) aboneliği   |[marketplaceDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[marketplaceDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> Burada açıklanan işlemi eklenen olan her abonelik için ayrı bir dağıtım gerektirir.
> 
> Ayrı dağıtımları da farklı Aboneliklerde içinde birden çok kaynak gruplarını ekleme kullanıyorsanız gereklidir. Ancak, ekleme, bir dağıtımda birden çok kaynak grupları tek bir abonelik içinde yapılabilir.

Aşağıdaki örnek, bir değişiklik gösterir. **resourceProjection.parameters.json** olacak bir dosya eklemek için kullanılan bir abonelik. Kaynak grubu parametresi dosyaları (bulunan [rg temsilci kaynak yönetimi](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) klasör) benzerdir, ancak de dahil bir **rgName** parametresi olmasını belirli kaynak gruplarını tanımlayın eklenmedi.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```
Yukarıdaki örnekte son yetkilendirme ekler bir **Principalıd** kullanıcı erişimi yöneticisi rolü (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9). Bu rol atamasını yaparken içermelidir **delegatedRoleDefinitionIds** özelliği ve bir veya daha fazla yerleşik roller. Bu yetkilendirme oluşturulan kullanıcılar bu yerleşik rollerde yönetilen kimlikleri atayın. Normalde kullanıcı erişimi yöneticisi rolü ile ilişkili hiçbir diğer izinler bu kullanıcı için geçerli olduğunu unutmayın.

## <a name="deploy-the-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını dağıtma

Müşteri, parametre dosyanıza güncelleştirdikten sonra kaynak yönetimi şablonu, müşteri kiracısında bir abonelik düzeyinde dağıtım dağıtmanız gerekir. Ayrı bir dağıtım temsil edilen Azure kaynak yönetimi (veya eklemek istediğiniz kaynak gruplarını içeren her bir abonelik için) eklemek istediğiniz her bir abonelik için gereklidir.

> [!IMPORTANT]
> Dağıtımı olan bir müşterinin Kiracı Konuk içi hesabında gerçekleştirilmelidir [sahip yerleşik rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) eklenen olan abonelik (veya eklenen olan kaynak grupları içeriyor).

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment create –-name <deploymentName> \
                     --location <AzureRegion> \
                     --template-file <pathToTemplateFile> \
                     --parameters <parameters/parameterFile> \
                     --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment create –-name <deploymentName \
                     –-location <AzureRegion> \
                     --template-uri <templateUri> \
                     --parameters <parameterFile> \
                     --verbose
```

## <a name="confirm-successful-onboarding"></a>Başarıyla Hazırlanmanız onaylayın

Bir müşteri aboneliğinde başarıyla temsil edilen Azure kaynak yönetimi için eklendi, hizmet sağlayıcısının kiracısındaki kullanıcılar (bunlar yukarıdaki sürecinde ona erişim izniniz abonelik ve kaynaklarını görebilirsiniz. tek tek veya uygun izinlere sahip bir Azure AD grubunun bir üyesi olarak). Bunu doğrulamak için aşağıdaki yollardan birinde abonelik göründüğünden emin olmak için denetleyin.  

### <a name="azure-portal"></a>Azure portal

Hizmet sağlayıcısının Kiracı:

1. Gidin [müşteriler sayfam](view-manage-customers.md).
2. Seçin **müşteriler**.
3. Resource Manager şablonunda belirtilen teklif adı ile yapılan abonelik(ler) gördüğünüzü onaylayın.

Müşteri kiracısında:

1. Gidin [hizmet sağlayıcıları sayfası](view-manage-service-providers.md).
2. Seçin **hizmet sağlayıcısı teklifler**.
3. Resource Manager şablonunda belirtilen teklif adı ile yapılan abonelik(ler) gördüğünüzü onaylayın.

> [!NOTE]
> Bu güncelleştirmeler, Azure portalında yansıtılır önce dağıtımınız tamamlandıktan sonra birkaç dakika sürebilir.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md).
- [Görüntüleme ve yönetme müşteriler](view-manage-customers.md) giderek **müşterilerimin** Azure portalında.
