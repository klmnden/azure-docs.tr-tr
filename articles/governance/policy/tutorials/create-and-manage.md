---
title: Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme
description: Standartları zorunlu tutmak, yönetmeliklere uygunluğu ve denetim gereksinimlerini karşılamak, maliyetleri denetlemek, güvenlik ve performans tutarlılığını korumak, ayrıca kuruluş genelinde tasarım ilkeleri ayarlamak için Azure İlkesi'ni kullanın.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/04/2019
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: ae4be75a4030db9afb02c5696a427b321f9f16b3
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802618"
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme

Kurumsal standartlarınıza ve hizmet düzeyi sözleşmelerinize uyumluluğu korumak için Azure'da ilkelerin nasıl oluşturulduğunu ve yönetildiğini anlamak önemlidir. Bu öğreticide, Azure İlkesi'ni kullanarak kuruluşunuz genelinde ilkeler oluşturma, atama ve yönetmeyle ilgili en yaygın görevlerden bazılarını yerine getirmeyi öğreneceksiniz; örneğin:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atama
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturma ve atama
> - Uyumlu olmayan veya reddedilen kaynakları çözümleme
> - Kuruluş genelinde yeni bir ilke uygulama

Mevcut kaynaklarınızın geçerli uyumluluk durumunu tanımlamak için bir ilke atamak isterseniz, hızlı başlangıç makalelerinde bunun nasıl yapıldığı açıklanır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="assign-a-policy"></a>İlke atama

Azure İlkesi ile uyumluluğu zorlamanın ilk adımı bir ilke tanımı atamaktır. İlke tanımında, bir ilkenin hangi koşullar altında zorlanacağını ve hangi etkinin uygulanacağı tanımlanır. Bu örnekte, uyumluluk için tüm SQL Server veritabanlarının v12.0 sürümünde olması koşulunu zorunlu tutan *SQL Server sürüm 12.0 gerektir* adlı yerleşik bir ilke tanımı atayacaksınız.

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![Tüm hizmetler ilkesinde arayın](../media/create-and-manage/search-policy.png)

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.

   ![İlkeye genel bakış sayfasından atamaları seçin](../media/create-and-manage/select-assignments.png)

1. **İlke - Atamalar** sayfasının üst kısmından **İlke Ata**'yı seçin.

   ![Atamalar sayfasından bir ilke tanımı atama](../media/create-and-manage/select-assign-policy.png)

1. **İlke Ata** sayfasında üç noktaya tıklayıp bir yönetim grubu veya abonelik belirleyerek **Kapsam**’ı seçin. İsterseniz bir kaynak grubu seçin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler.  Ardından **Kapsam** sayfasının alt kısmından **Seç**’e tıklayın.

   Bu örnekte **Contoso** aboneliği kullanılır. Sizin aboneliğiniz farklı olacaktır.

1. Kaynaklar **Kapsam**’a göre dışlanabilir.  **Dışlamalar**, **Kapsam**’dan bir düzey aşağıda başlatılır. **Dışlamalar** isteğe bağlıdır, bu yüzden şimdilik boş bırakın.

1. **İlke tanımı** üç nokta öğesini seçerek kullanılabilen tanımların listesini açın. **Tür** değeri *Yerleşik* olan ilke tanımlarını filtreleyerek bunların tümünü görüntüleyebilir ve açıklamalarını okuyabilirsiniz.

1. **SQL Server sürüm 12.0 gerektir**'i seçin. Hemen bulamıyorsanız, yazın **sql server gerektiren** Ara kutusuna ve ardından ENTER tuşuna basın veya dışında arama kutusuna tıklayın. İlke tanımını bulup seçtikten sonra **Kullanılabilen Tanımlar** sayfasının en altından **Seç**’e tıklayın.

   ![Bir ilke bulmak için arama filtresi kullanın.](../media/create-and-manage/select-available-definition.png)

1. **Atama adı** otomatik olarak seçtiğiniz ilke adıyla doldurulur, ancak bunu değiştirebilirsiniz. Bu örnekte, *SQL Server sürüm 12.0 gerektir* ayarını değiştirmeyin. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklama, bu ilke atamasıyla ilgili ayrıntıları sağlar.  **Tarafından atanan** açan temel alınarak otomatik olarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. Bu kutuyu _gerekir_ işaretli olduğunda ilke veya girişim atandıktan sahip bir ilke içerir [Deployıfnotexists](../concepts/effects.md#deployifnotexists) efekt. Bu öğreticide kullanılan ilke bulunmadığından, boş bırakın. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. **Ata**'ya tıklayın.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Artık bir yerleşik ilke tanımı atadığınıza göre, Azure İlkesi'yle daha fazlasını da yapabilirsiniz. Ardından, ortamınızda oluşturulan sanal makinelerin G serisinden olamayacağını doğrulayarak maliyetlerden tasarruf etmek için yeni bir özel ilke oluşturun. Bu yolla, kuruluşunuzdaki kullanıcılar G serisinden bir sanal makine oluşturmayı her denediğinde istekleri reddedilir.

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

   ![Tanım Sayfası yazma grubu altında](../media/create-and-manage/definition-under-authoring.png)

1. Sayfanın üst kısmındaki **+ İlke tanımı** seçeneğini belirleyin. Bu düğme açılır **ilke tanımı** sayfası.

1. Aşağıdaki bilgileri girin:

   - İlke tanımının kayıtlı olduğu yönetim grubu veya abonelik. **Tanım konumu**’ndaki üç noktayı kullanarak seçin.

     > [!NOTE]
     > Bu ilke tanımını birden çok aboneliğe uygulamayı planlıyorsanız, konumun ilkeyi atayacağınız abonelikleri içeren yönetim grubu olması gerekir. Bir girişim tanımı için de aynısı geçerlidir.

   - İlke tanımının adı - *Sanal makine SKU'larının G serisinden küçük olmasını gerektir*
   - İlke tanımının yapması beklenen işlemin açıklaması – *Bu ilke tanımı, maliyeti düşürmek için bu kapsamda oluşturulan tüm sanal makinelerin SKU'larının G serisinden küçük olmasını zorunlu tutar.*
   - Mevcut seçenekler arasından seçim yapın (_İşlem_ gibi) veya bu ilke tanımı için yeni bir kategori oluşturun.
   - Aşağıdaki JSON kodunu kopyalayın ve ardından gereksinimlerinize uygun olarak aşağıdaki öğelerle güncelleştirin:
      - İlke parametreleri.
      - İlke kuralları/koşulları; bu örnekte, G serisine eşit VM SKU boyutu
      - İlkenin etkisi, bu örnekte **Reddet**.

   JSON aşağıdakine benzer olmalıdır. Düzeltilmiş kodunuzu Azure Portal'a yapıştırın.

   ```json
   {
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Compute/virtualMachines"
                   },
                   {
                       "field": "Microsoft.Compute/virtualMachines/sku.name",
                       "like": "Standard_G*"
                   }
               ]
           },
           "then": {
               "effect": "deny"
           }
       }
   }
   ```

   *Alan* ilke kuralı özelliği şu değerlerden biri olmalıdır: Adı, türü, konum, etiketler veya bir diğer ad. Diğer adlara örnek olarak `"Microsoft.Compute/VirtualMachines/Size"` verilebilir.

   Diğer Azure ilkesi örneklerini görüntülemek için bkz. [Azure İlkesi örnekleri](../samples/index.md).

1. **Kaydet**’i seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API ile ilke tanımı oluşturma

İlke Tanımları için REST API ile ilke oluşturabilirsiniz. REST API, ilke tanımlarını oluşturmanıza ve silmenize, ayrıca mevcut tanımlar hakkında bilgi almanıza olanak tanır. İlke tanımı oluşturmak için aşağıdaki örneği kullanın:

```http-interactive
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Aşağıdaki örnekte gösterilene benzer bir istek gövdesi ekleyin:

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>PowerShell ile ilke tanımı oluşturma

PowerShell örneğine devam etmeden önce Azure PowerShell Az modülünün en son sürümünü yüklediğiniz emin olun. 

`New-AzPolicyDefinition` cmdlet'ini kullanarak ilke tanımı oluşturabilirsiniz.

Bir dosyadan ilke tanımı oluşturmak için, dosyanın yolunu geçirin. Dış dosya için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Yerel dosya kullanımı için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Çıkış bir `$definition` nesnesinde depolanır ve bu nesne ilke ataması sırasında kullanılır. Aşağıdaki örnekte, parametreler içeren bir ilke tanımı oluşturulur:

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>İlke tanımlarını PowerShell ile görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzPolicyDefinition
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

```output
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Azure CLI ile ilke tanımı oluşturma

İlke tanımı komutuyla Azure CLI'yi kullanarak ilke tanımı oluşturabilirsiniz. Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>İlke tanımlarını Azure CLI ile görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az policy definition list
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Girişim tanımı oluşturma ve atama

Girişim tanımıyla, çeşitli ilke tanımlarını gruplandırıp kapsamlı bir hedefe ulaşabilirsiniz. Dahil edilen ilkelere uyum için atama kapsamındaki kaynakların girişim değerlendirir. Girişim tanımları hakkında daha fazla bilgi için bkz. [Azure İlkesine genel bakış](../overview.md).

### <a name="create-an-initiative-definition"></a>Girişim tanımı oluşturma

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

   ![Tanımları sayfasında tanımı seçin](../media/create-and-manage/definition-under-authoring.png)

1. Sayfanın üst kısmından **+ Giriş Tanımı**'nı seçerek **Girişim Tanımı** sayfasını açın.

   ![Girişim tanımı sayfasını gözden geçirin](../media/create-and-manage/initiative-definition.png)

1. Tanımın depolanacağı bir yönetim grubu veya abonelik seçmek için **Tanım konumu** üç nokta simgesini kullanın. Önceki sayfanın kapsamı tek bir yönetim grubu veya abonelik olduğunda, **Tanım konumu** otomatik olarak doldurulur.

1. Girişimin **Adını** ve **Açıklamasını** girin.

   Bu örnekte, kaynakların ilgili ilke tanımlarıyla uyumlu olduğunu doğrular. Girişimin adını **güvenliği sağlama** ve açıklama olarak ayarlayın: **Bu girişim kaynakların güvenliğini sağlamakla ilişkili tüm ilke tanımlarını işlemek için oluşturulan**.

1. **Kategori** için mevcut seçenekler arasından seçim yapın veya yeni bir kategori oluşturun.

1. **Kullanılabilir Tanımlar** (**Girişim tanımı** sayfasının sağ yarısı) listesine göz atın ve bu girişime eklemek istediğiniz ilke tanımlarını seçin. **Güvenliği Sağlama** girişimi için ilke tanımı bilgilerinin yanındaki **+** seçeneğine veya bir ilke tanımı satırına tıklayıp ayrıntılar sayfasından **+ Ekle** seçeneğine tıklayarak aşağıdaki yerleşik ilke tanımlarını ekleyin:

   - SQL Server sürüm 12.0 gerektir
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   İlke tanımı listeden seçtikten sonra bu seçeneğin altına eklenir **ilkeler ve parametreler**.

   ![Girişim tanımı parametreleri gözden geçir](../media/create-and-manage/initiative-definition-2.png)

1. Girişim için eklenen bir ilke tanımı parametrelere sahipse, bunlar ilke adı altında gösterilen **ilkeler ve parametreler** alan. _Değer_'i, "Değer ata" (bu girişimin tüm atamaları için sabit kodlanmıştır) veya "Girişim Parametresini Kullan" (her girişim ataması sırasında ayarlanır) olarak ayarlayabilirsiniz. 'Set değeri' seçilirse sağındaki açılan _değerleri_ girerek veya değerleri seçerek sağlar. 'Girişim Parametresini Kullan' seçildiğinde ise girişim ataması sırasında ayarlanan parametreyi tanımlamanıza olanak sağlayan yeni bir **Giriş parametreleri** bölümü görüntülenir. Bu girişim parametresinde izin verilen değerler, girişim ataması sırasında ayarlanabilecek değerleri daha fazla kısıtlayabilir.

   ![Girişim tanımı parametrelerini izin verilen değerleri Değiştir](../media/create-and-manage/initiative-definition-3.png)

   > [!NOTE]
   > Bazı `strongType` parametrelerinde değer listesi otomatik olarak belirlenebilir. Böyle durumlarda parametre satırının sağ tarafında üç nokta simgesi görünür. Bu simgeye tıklandığında 'Parametre kapsamı (&lt;parametre adı&gt;)' sayfası açılır. Bu sayfada, değer seçeneklerini sağlamak için kullanılacak aboneliği seçin. Bu parametre kapsamı yalnızca girişim tanımı oluşturma işlemi sırasında kullanılır ve atandığında, ilke değerlendirmesi veya girişim kapsamı üzerinde herhangi bir etkisi olmaz.

1. **Kaydet**’e tıklayın.

### <a name="assign-an-initiative-definition"></a>Girişim tanımını atama

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

1. Daha önce oluşturduğunuz **Güvenliği Sağlama** adlı girişim tanımını bulup tıklayın. Seçin **atama** açmak için sayfanın üst kısmındaki **güvenliği sağlama: Girişim Ata** sayfası.

   ![Girişim tanımı sayfasından bir tanımı atama](../media/create-and-manage/assign-definition.png)

   Ayrıca, seçilen satıra sağ tıklayın veya bağlamsal menü satırın sonundaki üç noktaya üzerinde sol.  Sonra da **Ata**’yı seçebilirsiniz.

   ![Girişim için diğer seçenekleri](../media/create-and-manage/select-right-click.png)

1. Doldurun **güvenliği sağlama: Girişim Ata** aşağıdaki örnek bilgileri girerek sayfası. Kendi bilgilerinizi de kullanabilirsiniz.

   - Kapsam: Yönetim grubuna veya aboneliğe girişime kaydettiğiniz varsayılan olur.  Kapsamı değiştirerek girişimi kayıt konumundaki bir aboneliğe veya kaynak grubuna atayabilirsiniz.
   - Dışlamaları: Bunları uygulanmasını girişim atamasını önlemek için kapsam içi herhangi bir kaynağa yapılandırın.
   - Girişim tanımını ve atama adı: Güvenli (atanan girişim adıyla önceden doldurulmuş) alın.
   - Açıklama: Bu girişim ataması, bu ilke tanımları grubunu zorunlu tutmak için uyarlanmıştır.
   - Atanan: Otomatik olarak açan temel alınarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. Bu kutuyu _gerekir_ işaretli olduğunda ilke veya girişim atandıktan sahip bir ilke içerir [Deployıfnotexists](../concepts/effects.md#deployifnotexists) efekt. Bu öğreticide kullanılan ilke bulunmadığından, boş bırakın. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. **Ata**'ya tıklayın.

## <a name="check-initial-compliance"></a>İlk uyumluluğu denetleme

1. Azure İlkesi sayfasının sol tarafından **Uyumluluklar**'ı seçin.

1. **Kaynak Alma** girişimini bulun. Yine de olasıdır _uyumluluk durumu_ , **başlatılmadı**. Atamanın ilerleme durumunun tüm ayrıntılarını almak için girişime tıklayın.

   ![Girişim uyumluluğu sayfası - değerlendirmeleri başlatılmadı](../media/create-and-manage/compliance-status-not-started.png)

1. Girişim ataması tamamlandıktan sonra, uyumluluk sayfası güncelleştirilerek _Uyumluluk durumu_ değeri **Uyumlu** olur.

   ![Girişim uyumluluğu sayfası-uyumlu kaynakları](../media/create-and-manage/compliance-status-compliant.png)

1. Girişim uyumluluk sayfasında herhangi bir ilke tıklandığında, ilkenin uyumluluk ayrıntıları sayfası açılır. Bu sayfada uyumluluk için kaynak düzeyinde ayrıntılar sağlanır.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Dışlama'yı kullanarak uyumlu olmayan veya reddedilen kaynağı hariç tutma

Yukarıdaki örneği izleyerek, SQL Server sürüm 12.0'ı gerektirecek bir ilke tanımı atadıktan sonra 12.0’dan farklı bir sürümde oluşturulan SQL Server reddedilebilir. Bu bölümde, reddedilen SQL server tek bir kaynak grubu üzerinde bir dışlama oluşturarak oluşturma isteğine çözümünde size yol gösterir. Dışlama, ilkenin (veya girişimin) ilgili kaynağa uygulanmasını engeller.
Aşağıdaki örnekte, tek bir kaynak grubunda tüm SQL Server sürümlerine izin verilir. Aboneliğe, kaynak grubuna dışlama uygulanabilir veya dışlamayı tek tek kaynakları içerecek şekilde daraltabilirsiniz.

Bir atanan ilke veya girişim engelleyen bir dağıtıma iki konumda görüntülenebilir:

- Dağıtım tarafından hedeflenen kaynak grubu üzerinde: Seçin **dağıtımları** sayfasında, ardından sol tarafındaki ve tıklayarak **dağıtım adı** başarısız dağıtım. Reddedilen kaynak, _Yasaklandı_ durum bilgisiyle listelenir. Kaynağı reddeden ilkeyi veya girişimi belirlemek için Dağıtıma Genel Bakış sayfasında **Başarısız oldu. Ayrıntılar için buraya tıklayın ->** seçeneğine tıklayın. Sayfanın sağ tarafında hata bilgilerini içeren bir pencere açılır. Altında **hata ayrıntılarını** ilgili ilke nesnelerin guıd'lerdir.

  ![Dağıtım ilke ataması tarafından reddedildi](../media/create-and-manage/rg-deployment-denied.png)

- Azure İlkesi sayfasında: Seçin **Uyumluluk** tıklayın ve sayfanın sol tarafındaki **SQL Server sürüm 12.0 gerektir** ilkesi. Açılan sayfada **Ret** sayısında bir artış görürsünüz. Altında **olayları** sekmesinde de gördüğünüz kimin ilke tarafından reddedildi dağıtım çalıştı.

  ![Atanan bir ilkenin uyumluluğuna genel bakış](../media/create-and-manage/compliance-overview.png)

Bu örnekte, Contoso'nun üst düzey sanallaştırma uzmanları birini Trent Etikan gerekli işi yapmakta olduğu. Bir özel durum Trent vermek ihtiyacımız ancak biz yalnızca bir kaynak grubunda sürüm 12.0 SQL Server'lar istemiyorsanız. **SQLServers_Excluded** adlı yeni bir kaynak grubu oluşturduk ve bu gruba bu ilke ataması için özel durum izni vereceğiz.

### <a name="update-assignment-with-exclusion"></a>Atamayı özel durumla güncelleştirme

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Atamalar**’ı seçin.

1. Tüm ilke atamalarına göz atın ve *SQL Server sürüm 12.0 gerektir* atamasını açın.

1. Üç noktaya tıklayıp dışlanacak kaynak grubunu (bu örnekte *SQLServers_Excluded*) seçerek **Dışlama**’yı ayarlayın.

   ![Hariç tutulan bir kaynak grubu için ilke ataması ekleme](../media/create-and-manage/request-exclusion.png)

   > [!NOTE]
   > İlkeye ve sahip olduğu etkiye bağlı olarak atama kapsamı içindeki bir kaynak grubunda yer alan belirli kaynaklara da dışlama izni verilebilir. Bu öğreticide bir **Ret** etkisi kullanıldığından, zaten mevcut olan belirli bir kaynakta dışlama ayarlanması mantıklı değildir.

1. **Seç**'e ve ardından **Kaydet**'e tıklayın.

Bu bölümde, reddedilen istek tek bir kaynak grubu üzerinde bir dışlama oluşturarak çözüldü.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, bu öğreticiden kaynaklarla çalışmak, atamaları veya tanımları yukarıda oluşturulan silmek için aşağıdaki adımları kullanın:

1. Seçin **tanımları** (veya **atamaları** atamayı silmeye çalışıyorsanız) altında **yazma** Azure İlkesi sayfasının sol tarafında.

1. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.

1. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atandı
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturuldu ve atandı
> - Uyumlu olmayan veya reddedilen kaynak sorunu çözüldü
> - Kuruluş genelinde yeni bir ilke uygulandı

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için şu makaleyi gözden geçirin:

> [!div class="nextstepaction"]
> [Azure İlkesi tanım yapısı](../concepts/definition-structure.md)