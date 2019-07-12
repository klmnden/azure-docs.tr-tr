---
title: Key Vault başvuru - Azure App Service | Microsoft Docs
description: Azure App Service ve Azure işlevleri içindeki Azure Key Vault başvurular için kavramsal başvurusu ve Kurulum Kılavuzu
services: app-service
author: mattchenderson
manager: jeconnoc
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e7a049c8def0a5014aeb8a0e7a16aaa8def28009
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705690"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions-preview"></a>App Service ve Azure işlevleri'ni (Önizleme) için Key Vault başvuruları kullanın

> [!NOTE] 
> Key Vault başvurular şu anda Önizleme aşamasındadır.

Bu konuda, Azure Key vault'tan bir gizli App Service veya Azure işlevleri uygulamanızda hiçbir kod değişikliği gerekmeden iş işlemini göstermektedir. [Azure Key Vault](../key-vault/key-vault-overview.md) merkezi gizli dizileri yönetimi, erişim ilkeleri ve denetim geçmişi üzerinde tam denetim sağlayan bir hizmettir.

## <a name="granting-your-app-access-to-key-vault"></a>Anahtar Kasası'na erişim verme

Anahtar Kasasından gizli anahtarları okumak için oluşturulan bir kasası varsa ve uygulama erişim izni vermeniz gerekir.

1. Bir anahtar kasası oluşturma [Key Vault Hızlı Başlangıç](../key-vault/quick-create-cli.md).

1. Oluşturma bir [yönetilen sistem tarafından atanan kimliği](overview-managed-identity.md) uygulamanız için.

   > [!NOTE] 
   > Key Vault şu anda yalnızca destek sistem tarafından atanan yönetilen kimlikleri başvuruyor. Kullanıcı tarafından atanan kimlikleri kullanılamaz.

1. Oluşturma bir [erişim ilkesi anahtar Kasası'nda](../key-vault/key-vault-secure-your-key-vault.md#key-vault-access-policies) daha önce oluşturduğunuz uygulama kimliği için. Bu ilke "Get" gizli izni etkinleştirin. "Uygulama yetkili" yapılandırmayın veya `applicationId` ayarları, bu olarak yönetilen bir kimlik ile uyumlu değil.

    Erişim verme uygulamaya key vault'ta kimlik tek seferlik bir işlemdir ve tüm Azure abonelikleri için aynı kalır. İstediğiniz sayıda sertifikaları dağıtmak için kullanabilirsiniz. 

## <a name="reference-syntax"></a>Başvuru söz dizimi

Key Vault başvurusunu biçimindedir `@Microsoft.KeyVault({referenceString})`burada `{referenceString}` aşağıdaki seçeneklerden birini değiştirilir:

> [!div class="mx-tdBreakAll"]
> | Başvuru dizesi                                                            | Açıklama                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | **SecretUri** veri düzlemi tam URI gibi bir sürümünü, örneğin, anahtar Kasası'nda gizli dizi olmalıdır https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931  |
> | VaultName=_vaultName_;SecretName=_secretName_;SecretVersion=_secretVersion_ | **VaultName** anahtar kasası kaynağınızın adını gerekir. **SecretName** hedef gizli dizi adı olmalıdır. **SecretVersion** kullanmak için gizli dizi sürümü olmalıdır. |

> [!NOTE] 
> Geçerli Önizleme sürümleri gerekli değildir. Gizli dizileri döndürürken güncelleştirme sürümü, uygulama yapılandırmasında gerekecektir.

Örneğin, tam bir başvuru aşağıdaki gibi görünür:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```

Alternatif olarak:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Anahtar Kasası'ndaki kaynak uygulama ayarları

Key Vault başvuruları için değerler olarak kullanılabilir [uygulama ayarları](configure-common.md#configure-app-settings), gizli anahtar Kasası'nda yerine site yapılandırmasında devam etmenize imkan sağlar. Uygulama ayarları, güvenli bir şekilde bekleme durumundayken şifrelenir, ancak bu Key Vault'a gizli dizi yönetimi özelliklerine ihtiyacınız varsa, bunlar tamamlamalıdır.

Bir uygulama ayarı için bir anahtar kasası başvurusu kullanmak için ayar değeri olarak başvuru ayarlayın. Uygulamanızı gizli anahtarıyla normal olarak aracılığıyla başvurabilirsiniz. Hiçbir kod değişikliği gerekli değildir.

> [!TIP]
> Key Vault başvurular kullanarak çoğu uygulama ayarları, her ortam için ayrı kasaları olması gerektiği kadar yuva ayarları olarak işaretlenmelidir.

### <a name="azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımı

Azure Resource Manager şablonları aracılığıyla kaynak dağıtımlarını otomatikleştirme, bu özellik iş yapmak için belirli bir sırada bağımlılıklarınızı sıra gerekebilir. Not, kendi kaynak olarak uygulama ayarlarınızı tanımlayın gerekecektir kullanmak yerine bir `siteConfig` site tanımı özelliği. Bu durum, sitesinin sistem tarafından atanan kimliği ile oluşturulur ve erişim ilkesinde kullanılabilir ilk tanımlanması gerekir çünkü.

Bir örnek psuedo şablonu bir işlev uygulaması için aşağıdakine benzeyebilir:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> Bu örnekte, kaynak denetimi dağıtım uygulama ayarlarına bağlıdır. Uygulama ayarını güncelleştirme zaman uyumsuz olarak davranır gibi normal olarak güvenli olmayan davranış budur. Ancak, ekledik çünkü `WEBSITE_ENABLE_SYNC_UPDATE_SITE` uygulama ayarını güncelleştirme zaman uyumlu. Bu, uygulama ayarları tam olarak güncelleştirilip güncelleştirilmediğini sonra kaynak denetimi dağıtım yalnızca başlayacak anlamına gelir.
