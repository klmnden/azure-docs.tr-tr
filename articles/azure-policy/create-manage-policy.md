---
title: Azure İlkesi kullanarak kurumsal uyumluluğu zorunlu tutacak ilkeleri oluşturma ve yönetme
description: Standartları zorunlu tutmak, yönetmeliklere uygunluğu ve denetim gereksinimlerini karşılamak, maliyetleri denetlemek, güvenlik ve performans tutarlılığını korumak, ayrıca kuruluş genelinde tasarım ilkeleri ayarlamak için Azure İlkesi'ni kullanın.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 2e04e08d22890246e2b68a55d79e82864201ef9d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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

   ![İlke arama](media/create-manage-policy/search-policy.png)

2. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.
3. **İlke - Atamalar** sayfasının üst kısmından **İlke Ata**'yı seçin.

   ![İlke tanımı atama](media/create-manage-policy/select-assign-policy.png)

4. **İlke Ata** sayfasında üç noktaya tıklayıp bir abonelik (gerekli) ve kaynak grubu (isteğe bağlı) seçerek **Kapsam**’ı belirleyin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler.  Ardından **Kapsam** sayfasının alt kısmından **Seç**’e tıklayın.

   Bu örnekte **Contoso Aboneliği** kullanılır. Sizin aboneliğiniz farklı olacaktır.

5. Bir veya daha fazla kaynak grubunu (kapsamı yalnızca belirli bir aboneliğe ayarladıysanız) veya bir kaynak grubundaki belirli kaynakları (her iki kapsam durumunda da) dışlamak istiyorsanız, ilke atama bölümünden **Dışlamalar** özelliğini kullanabilirsiniz. Şimdilik boş bırakın.

6. **İlke tanımı** üç nokta öğesini seçerek kullanılabilen tanımların listesini açın. **Tür** değeri *Yerleşik* olan ilke tanımlarını filtreleyerek bunların tümünü görüntüleyebilir ve açıklamalarını okuyabilirsiniz.

7. **SQL Server sürüm 12.0 gerektir**'i seçin. Bu seçeneği hemen bulamazsanız, arama kutusuna **sql server gerektir** yazıp ENTER tuşuna basın veya arama kutusunun dışına tıklayın. İlke tanımını bulup seçtikten sonra **Kullanılabilen Tanımlar** sayfasının en altından **Seç**’e tıklayın.

   ![İlkeyi bulma](media/create-manage-policy/select-available-definition.png)

8. **Atama adı** otomatik olarak seçtiğiniz ilke adıyla doldurulur, ancak bunu değiştirebilirsiniz. Bu örnekte, *SQL Server sürüm 12.0 gerektir* ayarını değiştirmeyin. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklama, bu ilke atamasıyla ilgili ayrıntıları sağlar.

9. **Ata**'ya tıklayın.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Artık bir yerleşik ilke tanımı atadığınıza göre, Azure İlkesi'yle daha fazlasını da yapabilirsiniz. Bundan sonra, ortamınızda oluşturulan sanal makinelerin G serisinden olamayacağını güvence altına alarak maliyet tasarrufu yapmak için yeni bir özel ilke oluşturun. Bu yolla, kuruluşunuzdaki kullanıcılar G serisinden bir sanal makine oluşturmayı her denediğinde istekleri reddedilir.

1. Azure İlkesi sayfasının sol tarafındaki **YAZMA** bölümünden **Tanımlar**’ı seçin.

   ![Yazma alanındaki Tanım](media/create-manage-policy/definition-under-authoring.png)

2. Sayfanın üst kısmındaki **+ İlke tanımı** seçeneğini belirleyin. Bunu yaptığınızda **İlke tanımı** sayfası açılır.
3. Aşağıdakileri girin:

   - İlke tanımının kayıtlı olduğu yönetim grubu veya abonelik. **Tanım konumu**’ndaki üç noktayı kullanarak seçin.

     > [!NOTE]
     > Bu ilke tanımını birden çok aboneliğe uygulamayı planlıyorsanız, konumun ilkeyi atayacağınız abonelikleri içeren yönetim grubu olması gerekir. Bir girişim tanımı için de aynısı geçerlidir.

   - İlke tanımının adı - *Sanal makine SKU'larının G serisinden küçük olmasını gerektir*
   - İlke tanımının yapması beklenen işlemin açıklaması – *Bu ilke tanımı, maliyeti düşürmek için bu kapsamda oluşturulan tüm sanal makinelerin SKU'larının G serisinden küçük olmasını zorunlu tutar.*
   - Mevcut seçenekler arasından seçim yapın veya bu ilke tanımı için yeni bir kategori oluşturun.
   - Aşağıdaki json kodunu kopyalayın ve ardından gereksinimlerinize uygun olarak aşağıdaki öğelerle güncelleştirin:
      - İlke parametreleri.
      - İlke kuralları/koşulları; bu örnekte, G serisine eşit VM SKU boyutu
      - İlkenin etkisi, bu örnekte **Reddet**.

    Json aşağıdakine benzer olmalıdır. Düzeltilmiş kodunuzu Azure Portal'a yapıştırın.

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

    İlke kuralındaki *alan* özelliğinin değeri şunlardan biri olmalıdır: Ad, Tür, Konum, Etiketler veya bir diğer ad. Diğer adlara örnek olarak `"Microsoft.Compute/VirtualMachines/Size"` verilebilir.

    Diğer Azure ilkesi örneklerini görüntülemek için bkz. [Azure İlkesi Şablonları](json-samples.md).

4. **Kaydet**’i seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API ile ilke tanımı oluşturma

İlke Tanımları için REST API ile ilke oluşturabilirsiniz. REST API, ilke tanımlarını oluşturmanıza ve silmenize, ayrıca mevcut tanımlar hakkında bilgi almanıza olanak tanır.
İlke tanımı oluşturmak için aşağıdaki örneği kullanın:

```http-interactive
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
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

PowerShell örneğine devam etmeden önce, Azure PowerShell'in en son sürümünü yüklediğinizden emin olun. İlke parametreleri 3.6.0 sürümünde eklenmiştir. Daha önceki bir sürümü kullanıyorsanız, örneklerde parametrenin bulunamadığını belirten bir hata döndürülür.

`New-AzureRmPolicyDefinition` cmdlet'ini kullanarak ilke tanımı oluşturabilirsiniz.

Bir dosyadan ilke tanımı oluşturmak için, dosyanın yolunu geçirin. Dış dosya için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Yerel dosya kullanımı için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
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
                "not": {
                    "field": "Microsoft.Storage/storageAccounts/accessTier",
                    "equals": "cool"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Çıkış bir `$definition` nesnesinde depolanır ve bu nesne ilke ataması sırasında kullanılır.
Aşağıdaki örnekte, parametreler içeren bir ilke tanımı oluşturulur:

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

$definition = New-AzureRmPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>İlke tanımlarını PowerShell ile görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzureRmPolicyDefinition
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

```
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

İlke tanımı komutuyla Azure CLI'yi kullanarak ilke tanımı oluşturabilirsiniz.
Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

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
                "not": {
                    "field": "Microsoft.Storage/storageAccounts/accessTier",
                    "equals": "cool"
                }
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

Girişim tanımıyla, çeşitli ilke tanımlarını gruplandırıp kapsamlı bir hedefe ulaşabilirsiniz. Girişim tanımı oluşturduğunuzda, tanım kapsamındaki kaynakların girişim tanımını oluşturan ilke tanımlarına uyumlu kalacağından emin olabilirsiniz.  Girişim tanımları hakkında daha fazla bilgi için bkz. [Azure İlkesine genel bakış](azure-policy-introduction.md).

### <a name="create-an-initiative-definition"></a>Girişim tanımı oluşturma

1. Azure İlkesi sayfasının sol tarafındaki **YAZMA** bölümünden **Tanımlar**’ı seçin.

   ![Tanımları seçin](media/create-manage-policy/select-definitions.png)

2. Sayfanın üst kısmından **+ Giriş Tanımı**'nı seçerek **Girişim Tanımı** sayfasını açın.

   ![Girişim tanımı](media/create-manage-policy/initiative-definition.png)

3. **Tanım konumu**’ndaki üç nokta simgesini kullanarak tanımın depolanacağı aboneliği seçin.

4. Girişimin **Adını** ve **Açıklamasını** girin.

   Bu örnekte, kaynakların güvenlikle ilgili ilke tanımlarıyla uyumlu olduğundan emin olursunuz. Bu nedenle, girişimin adı **Güvenliği Sağlama** olabilir ve şöyle bir açıklama yazılabilir: **Bu girişim kaynakların güvenliğini sağlamakla ilişkili tüm ilke tanımlarını işlemek için oluşturuldu**.

5. **Kategori** için mevcut seçenekler arasından seçim yapın veya yeni bir kategori oluşturun.

6. **Kullanılabilir Tanımlar** (**Girişim tanımı** sayfasının sağ yarısı) listesine göz atın ve bu girişime eklemek istediğiniz ilke tanımlarını seçin. **Güvenliği Sağlama** girişimi için ilke tanımı bilgilerinin yanındaki **+** seçeneğine veya bir ilke tanımı satırına tıklayıp ayrıntılar sayfasından **+ Ekle** seçeneğine tıklayarak aşağıdaki yerleşik ilke tanımlarını ekleyin:
   - SQL Server sürüm 12.0 gerektir
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   Listeden ilke tanımını seçtiğinizde tanım **İLKELER VE PARAMETRELER** bölümüne eklenir.

   ![Girişim tanımları](media/create-manage-policy/initiative-definition-2.png)

7. **Kaydet**’e tıklayın.

### <a name="assign-an-initiative-definition"></a>Girişim tanımını atama

1. Azure İlkesi sayfasının sol tarafındaki **YAZMA** bölümünden **Tanımlar**’ı seçin.
2. Daha önce oluşturduğunuz **Güvenliği Sağlama** adlı girişim tanımını bulup seçin.
3. Sayfanın üst kısmından **Ata**’yı seçerek **Güvenliği Sağlama: Girişimi Ata** sayfasını açın.

   ![Tanımı atama](media/create-manage-policy/assign-definition.png)

   Alternatif olarak, seçili satıra sağ tıklayarak ya da satırın sonundaki üç noktaya sol tıklayarak bir bağlam menüsünü açabilirsiniz.  Sonra da **Ata**’yı seçebilirsiniz.

   ![Bir satıra sağ tıklayın](media/create-manage-policy/select-right-click.png)

4. Aşağıdaki örnek bilgileri girerek **Güvenliği Sağlama: Girişimi Ata** sayfasını doldurun. Kendi bilgilerinizi de kullanabilirsiniz.

   - Kapsam: Girişimi kaydettiğiniz abonelik varsayılan kapsam olur.  Kapsamı değiştirerek girişimi abonelik kayıt konumundaki bir kaynak grubuna atayabilirsiniz.
   - Dışlamalar: Kapsam dahilinde yer alan ancak girişim atamasının uygulanmasını önlemek istediğiniz kaynaklar varsa bunları yapılandırın.
   - Girişim tanımı ve Atama adı: Güvenliği Sağlama (atanan girişimin adı olarak önceden oluşturulur).
   - Açıklama: Bu girişim ataması, bu ilke tanımları grubunu zorunlu tutmak için uyarlanmıştır.

5. **Ata**'ya tıklayın.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Dışlama'yı kullanarak uyumlu olmayan veya reddedilen kaynağı hariç tutma

Yukarıdaki örneği izleyerek, SQL Server sürüm 12.0'ı gerektirecek bir ilke tanımı atadıktan sonra 12.0’dan farklı bir sürümde oluşturulan SQL Server reddedilebilir. Bu bölümde, tek bir kaynak grubunda bir dışlama oluşturarak SQL Server oluşturma girişiminin reddedilmesi sorununu çözme işleminde size yol gösterilir. Dışlama, ilkenin (veya girişimin) ilgili kaynağa uygulanmasını engeller. Aşağıdaki örnekte, tek bir kaynak grubunda tüm SQL Server sürümlerine izin verilir. Kaynak grubuna dışlama uygulanabilir veya dışlamayı tek tek kaynakları içerecek şekilde daraltabilirsiniz.

Atanan bir ilke ya da girişim nedeniyle engellenen bir atama iki konumda görüntülenebilir:

- Dağıtım tarafından hedeflenen kaynak grubu üzerinde: Sayfanın sol tarafından **Dağıtımları**'ı seçip başarısız olan dağıtımın **Dağıtım Adı**’na tıklayın. Reddedilen kaynak, _Yasaklandı_ durum bilgisiyle listelenir. Kaynağı reddeden ilkeyi veya girişimi belirlemek için Dağıtıma Genel Bakış sayfasında **Başarısız oldu. Ayrıntılar için buraya tıklayın ->** seçeneğine tıklayın. Sayfanın sağ tarafında hata bilgilerini içeren bir pencere açılır. **Hata Ayrıntıları** bölümünde ilgili ilke nesnelerinin GUID’leri yer alır.

   ![Dağıtım ilke ataması tarafından reddedildi](media/create-manage-policy/rg-deployment-denied.png)

- Azure İlkesi sayfasında: Sayfanın sol tarafından **Uyumluluk**’u seçin ve **SQL Server sürüm 12.0 gerektir** ilkesine tıklayın. Açılan sayfada **Ret** sayısında bir artış görürsünüz. **Olaylar** sekmesinde, ilke tarafından reddedilen dağıtımı kimin gerçekleştirmeye çalıştığını da görebilirsiniz.

   ![Atanan bir ilkenin uyumluluğuna genel bakış](media/create-manage-policy/compliance-overview.png)

Bu örnekte, Contoso'nun kıdemli Sanallaştırma uzmanlarından biri olan Taner Keskin yapması gereken işleri yapıyordu. Tamer’e özel durum izni vermemiz gerekiyor, ancak sürümü 12.0 olmayan SQL Server’ların her kaynak grubunda olmasını istemiyoruz. **SQLServers_Excluded** adlı yeni bir kaynak grubu oluşturduk ve bu gruba bu ilke ataması için özel durum izni vereceğiz.

### <a name="update-assignment-with-exclusion"></a>Atamayı özel durumla güncelleştirme

1. Azure İlkesi sayfasının sol tarafındaki **YAZMA** bölümünden **Atamalar**’ı seçin.
2. Tüm ilke atamalarına göz atın ve *SQL Server sürüm 12.0 gerektir* atamasını açın.
3. Üç noktaya tıklayıp dışlanacak kaynak grubunu (bu örnekte *SQLServers_Excluded*) seçerek **Dışlama**’yı ayarlayın.

   ![Dışlama isteğinde bulunma](media/create-manage-policy/request-exclusion.png)

   > [!NOTE]
   > İlkeye ve sahip olduğu etkiye bağlı olarak atama kapsamı içindeki bir kaynak grubunda yer alan belirli kaynaklara da dışlama izni verilebilir. Bu öğreticide bir **Ret** etkisi kullanıldığından, zaten mevcut olan belirli bir kaynakta dışlama ayarlanması mantıklı değildir.

4. **Seç**'e ve ardından **Kaydet**'e tıklayın.

Bu bölümde, tek bir kaynak grubunda bir dışlama oluşturarak SQL Server’ın yasaklanan bir sürümünü oluşturma girişiminin reddedilmesi sorununu çözdünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki kaynaklarla çalışmayı tamamladıysanız, aşağıdaki adımları kullanarak daha önce oluşturulan tüm atamaları veya tanımları silin:

1. Azure İlkesi sayfasının sol tarafındaki **YAZMA** bölümünden **Tanımlar**’ı (veya bir atamayı silmeye çalışıyorsanız Sol bölmede Tanımlar'ı (veya atamayı silmeye çalışıyorsanız **Atamalar**'ı) seçin.
2. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.
3. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki işlemleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atandı
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturuldu ve atandı
> - Uyumlu olmayan veya reddedilen kaynak sorunu çözüldü
> - Kuruluş genelinde yeni bir ilke uygulandı

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için şu makaleyi gözden geçirin:

> [!div class="nextstepaction"]
> [Azure İlkesi tanım yapısı](policy-definition.md)