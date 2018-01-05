---
title: "Azure ilke oluşturmak ve kuruluş uyumluluğu zorlamak üzere ilkelerini yönetmek için kullanın | Microsoft Docs"
description: "Standartları zorlamak, yasal uyumluluk ve denetim gereksinimlerini karşılamak, maliyetleri denetim, güvenlik ve performans tutarlılık sağlamak ve kurumsal geniş tasarım ilkeleri zorunlu tuttukları için Azure ilkesini kullanın."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/03/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 882cf3cde71f5154efcd88f055984e72463b3099
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Uyumluluğu zorlamak üzere ilkeleri oluşturun ve yönetin

Azure'da ilkeleri oluşturun ve yönetin nasıl giderildiğini anlamak, Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu sağlama için önemlidir. Bu öğreticide, bazı oluşturma, atama ve ilkelerini yönetme, kuruluşunuz genelindeki gibi ilgili daha yaygın görevleri yapmak için Azure ilke kullanmayı öğreneceksiniz:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynakları için bir koşul uygulamak için bir ilke atama
> * Oluşturma ve birden çok kaynak için uyumluluğu izlemek amacıyla bir girişimi tanımı atama
> * Uyumlu olmayan veya reddedilen kaynak çözümleyin
> * Bir kuruluş genelinde yeni bir ilke uygulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="assign-a-policy"></a>Bir ilke atama

Azure ilkesiyle zorlamayı ilk adımı, bir ilke tanımı atamaktır. Bir ilke tanımı hangi koşul bir ilke uygulanır ve hangi eylemin yapılacağını altında tanımlar. Bu örnekte, biz adlı yerleşik ilke tanımı atamak *gerektiren SQL Server sürümü 12.0*, tüm SQL Server veritabanlarını uyumlu olmasını v12.0 olmalıdır koşul zorlamak için.

1. Arama ve seçerek Azure portalında Azure İlkesi Hizmeti başlatma **İlkesi** sol bölmede.

   ![İlke arama](media/assign-policy-definition/search-policy.png)

2. Azure İlkesi sayfasının sol bölmesinde **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanan bir ilkedir.
3. **Atamalar** bölmesinin üst kısmında **İlke Ata**'yı seçin.

   ![İlke tanımı atama](media/create-manage-policy/select-assign-policy.png)

4. **İlke Ata** sayfasında, kullanılabilir tanımlar listesini açmak için **İlke** alanının yanındaki ![İlke tanımı düğmesine](media/assign-policy-definition/definitions-button.png) tıklayın.

   ![Kullanılabilir ilke tanımlarını açma](media/create-manage-policy/open-policy-definitions.png)

5. Seçin **SQL Server sürümü 12.0 gerektiren**.

   ![Bir ilke bulun](media/create-manage-policy/select-available-definition.png)

6. İlke ataması için görünen **Ad** sağlayın. Bu durumda, kullanalım *gerektiren SQL Server sürümü 12.0*. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Bu ilke ataması bu ortamda oluşturulan tüm SQL sunucuları nasıl sağlar hakkında ayrıntılar sürüm 12.0 olan açıklama sağlar.
7. İlkenin mevcut kaynaklara uygulanmasını güvence altına almak için fiyatlandırma katmanını **Standart** olarak değiştirin.

   Azure İlkesi içinde iki fiyatlandırma katmanı vardır: *Ücretsiz* ve *Standart*. Ücretsiz katmanıyla, ilkeleri yalnızca gelecek kaynaklarda zorunlu tutabilirsiniz; Standart katmanıyla ise, uyumluluk durumunuzu daha iyi anlayabilmek için ilkeleri mevcut kaynaklarda da zorunlu tutarsınız. Sınırlı Önizleme aşamasında olduğumuzdan, henüz bir fiyatlandırma modeli yayımlamadık. Dolayısıyla *Standart*'ı seçtiğinizde fatura almayacaksınız. Fiyatlandırma hakkında daha fazla bilgi için şu konuya bakın: [Azure İlkesi fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-policy).

8. Seçin **kapsam** -abonelik (veya kaynak grubu), önceden kayıtlı. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir.

   Bu abonelik - kullanarak Biz bu örneğin **Azure Analytics kapasite geliştirme**. Aboneliğinizi farklılık gösterir.

10. **Ata**'yı seçin.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Biz ilke tanımı atadığınız, ortamınızda çoğaltmanın oluşturulan VM'ler G serisinde olamaz sağlayarak maliyetleri kaydetmek için yeni bir ilke oluşturmak için yapacağız. Kurumsal bir kullanıcı VM G serisinde oluşturmayı dener her zaman bu şekilde isteğini reddetti.

1. Seçin **tanımı** altında **yazma** sol bölmede.

   ![Yazma altında tanımı](media/create-manage-policy/definition-under-authoring.png)

2. Seçin **+ ilke tanımı**.
3. Aşağıdakileri girin:

   - İlke tanımı - adını *VM SKU G serisi küçük gerektirir*
   - İlke tanımı yapmak için – yöneliktir açıklaması Bu ilke tanımı bu kapsamda oluşturulan tüm sanal makineleri SKU'ları maliyetini azaltmak G serisi daha küçük olmasını zorlar.
   - İlke tanımı Canlı – bu durumda, bizim ilke tanımı Live'da abonelik **Danışmanı Analytics kapasite geliştirme**. Abonelik listeniz farklılık gösterir.
   - Aşağıdaki json kodunu kopyalayın ve gereksinimlerinize ile güncelleştirin:
      - İlke parametreleri.
      - İlke kuralları /, bu durumda – VM SKU boyutunu G seriye eşit koşulları
      - Bu durumda – ilke etkili **reddetme**.

    İşte json aşağıdaki gibi görünmelidir. Düzenlenen kodunuzu Azure portalında yapıştırın.

    ```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
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

    Json kodunu örneklerini görüntülemek için okuma [Azure ilke şablonları](json-samples.md) makalesi.

4. **Kaydet**’i seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API'si ile bir ilke tanımı oluşturun

İlke tanımları için REST API ile bir ilke oluşturabilirsiniz. REST API oluşturmak ve ilke tanımları silmek ve varolan tanımları hakkında bilgi almak etkinleştirir.
Bir ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Aşağıdaki örneğe benzer bir istek gövdesi şunları içerir:

```
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

## <a name="create-a-policy-definition-with-powershell"></a>PowerShell ile bir ilke tanımı oluşturma

PowerShell örnek işlemine devam etmeden önce Azure PowerShell'in en son sürümünü yüklediğinizden emin olun. İlke parametreleri sürüm 3.6.0 eklendi. Önceki bir sürümü varsa, örnekleri parametresi bulunamıyor belirten bir hata döndürür.

Kullanarak bir ilke tanımı oluşturabilirsiniz `New-AzureRmPolicyDefinition` cmdlet'i.

Bir dosyadan bir ilke tanımı oluşturmak için dosya yolu geçirin. Dış bir dosya için aşağıdaki örneği kullanın:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Bir yerel dosya, aşağıdaki örneği kullanın:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Bir ilke tanımı sahip bir satır içi kuralı oluşturmak için aşağıdaki örneği kullanın:

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
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

Çıktı depolanan bir `$definition` ilke ataması sırasında kullanılan nesne.
Aşağıdaki örnek, parametreleri içeren bir ilke tanımı oluşturur:

```
$policy = '{
    "if": {
        "allOf": [
            {
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

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>Görünüm ilke tanımları

Aboneliğinizdeki tüm ilke tanımları görmek için aşağıdaki komutu kullanın:

```
Get-AzureRmPolicyDefinition
```

Yerleşik ilkeleri de dahil olmak üzere tüm kullanılabilir ilke tanımları döndürür. Her ilke şu biçimde verilir:

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

## <a name="create-a-policy-definition-with-azure-cli"></a>Azure CLI ile bir ilke tanımı oluşturun

İlke tanımı komutu ile Azure CLI kullanarak bir ilke tanımı oluşturabilirsiniz.
Bir ilke tanımı sahip bir satır içi kuralı oluşturmak için aşağıdaki örneği kullanın:

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
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

## <a name="view-policy-definitions"></a>Görünüm ilke tanımları

Aboneliğinizdeki tüm ilke tanımları görmek için aşağıdaki komutu kullanın:

```
az policy definition list
```

Yerleşik ilkeleri de dahil olmak üzere tüm kullanılabilir ilke tanımları döndürür. Her ilke şu biçimde verilir:

```
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

## <a name="create-and-assign-an-initiative-definition"></a>Oluşturun ve girişimi tanımı atayın

Bir girişimi tanımıyla bir değerlendiriyoruz elde etmek için birkaç ilke tanımları gruplandırabilirsiniz. Kaynakları tanımı kapsamında girişimi tanımını oluşturan ilke tanımları uyumlu kalmasını sağlamak için bir girişimi tanımı oluşturun.  Bkz: [Azure ilkesine genel bakış](./azure-policy-introduction.md) girişimi tanımları hakkında daha fazla bilgi için.

### <a name="create-an-initiative-definition"></a>Girişimi tanımı oluşturun

1. Seçin **tanımları** altında **yazma** sol bölmede.

   ![Tanımları seçin](media/create-manage-policy/select-definitions.png)

2. Seçin **Initiative tanımı** bu seçim, sayfanın en üstünde alır **Initiative tanımı** formu.
3. Ad ve açıklama girişiminin girin.

   Kaynaklardır güvenli alma hakkında ilke tanımları uyumlu, Initiative adı olacaktır olmak istiyoruz Bu örnekte, **alma güvenli**, açıklama olacaktır: **Bu girişim yapıldı kaynakların güvenliğini sağlama ile ilişkili tüm ilke tanımları işlemek için oluşturulan**.

   ![Girişim tanımı](media/create-manage-policy/initiative-definition.png)

4. Listesine göz **kullanılabilir tanımları** ve o Initiative eklemek istediğiniz ilke tanımlarını seçin. İçin bizim **güvenli alma** girişimi, ilke tanımları yerleşik aşağıdakileri ekleyin:
   - SQL Server sürüm 12.0 gerektirir
   - Güvenlik Merkezi'nde korumasız web uygulamalarını izleyin.
   - Güvenlik Merkezi'nde izin veren ağ üzerinden izleyin.
   - Olası uygulama uygulamaları güvenilir listeye almayı Güvenlik Merkezi'nde izleyin.
   - Güvenlik Merkezi'nde şifrelenmemiş VM diskleri izleyin.

   ![Girişimi tanımları](media/create-manage-policy/initiative-definition-2.png)

   İlke tanımları listeden seçtikten sonra altında görürsünüz **ilkeleri ve parametreleri**, yukarıda gösterildiği gibi.

5. **Oluştur**’u seçin.

### <a name="assign-an-initiative-definition"></a>Bir girişimi tanımı atayın

1. Git **tanımları** altında sekmesinde **yazma**.
2. Arama **güvenli alma** oluşturduğunuz girişimi tanımı.
3. Girişimi tanımı'nı seçin ve ardından **atamak**.

   ![Bir tanımı atayın](media/create-manage-policy/assign-definition.png)

4. Doldurmak **atama** girerek form:
   - Ad: Get güvenli atama
   - Açıklama: Bu Grup İlkesi tanımlarında zorlamayı doğrultusunda bu girişimi atama uyarlanmış **Azure Danışmanı kapasite geliştirme** abonelik
   - Fiyatlandırma katmanı: standart
   - uygulanan bu atama gibi kapsamı: **Azure Danışmanı kapasite geliştirme**

5. **Ata**'yı seçin.

## <a name="resolve-a-non-compliant-or-denied-resource"></a>Uyumlu olmayan veya reddedilen kaynak çözümleyin

SQL server sürümü 12.0 gerektirecek şekilde ilke tanımı atama sonra yukarıdaki örnekte,, farklı bir sürümü ile oluşturulmuş bir SQL server reddedildi. Bu bölümde, biz reddedilen bir dışlama isteyerek farklı sürümün bulunduğu bir SQL server oluşturma girişimi çözümünde size taramasını.

1. Sol bölmede **Atamalar**'ı seçin.
2. Tüm ilke atamalarını göz atın ve başlatma *gerektiren SQL Server sürümü 12.0* atama.
3. SQL server oluşturmaya çalıştığınız kaynak grupları için bir dışlama isteyin. Bu durumda, biz Microsoft.Sql/servers/databases hariç: *baconandbeer/Cheetos* ve *baconandbeer/Chorizo*.

   ![İstek dışlama](media/create-manage-policy/request-exclusion.png)

   Reddedilen bir kaynak çözmek diğer yolları içerir: SQL server tarafından oluşturulan gerek ve erişimi varsa ilkeyi doğrudan düzenlemek için güçlü bir gerekçe varsa, ilkeyle ilişkili kişiye ulaşmasını.

4. **Kaydet**’i seçin.

Bu bölümde, bir SQL server sürümüyle 12.0, dışlama kaynaklara isteyerek oluşturma girişimi, reddi çözümlendi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerde ile çalışmaya devam etmeyi planlıyorsanız, temiz bu Kılavuzu'nda oluşturulan kaynakları yukarı değil. Devam etmek düşünmüyorsanız atamaları ya da yukarıda oluşturduğunuz tanımları silmek için aşağıdaki adımları kullanın:

1. Seçin **tanımları** (veya **atamaları** atama silmeyi denediğiniz varsa) sol bölmedeki.
2. Yeni Initiative veya ilke tanımı (veya atama) yeni oluşturulan arayın.
3. Üç nokta tanımı veya atama ucundaki ve seçin **silmek tanımı** (veya **silmek atama**).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, başarılı bir şekilde aşağıdaki gerçekleştirdiniz:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynakları için bir koşul uygulamak için bir ilke atanan
> * Oluşturulan ve birden çok kaynak için uyumluluğu izlemek amacıyla bir girişimi tanımı atayın
> * Uyumlu olmayan veya reddedilen kaynak çözümlendi
> * Yeni bir ilke bir kuruluş genelinde uygulanmadı

İlke tanımları yapılar hakkında daha fazla bilgi için bu makalenin bakın:

> [!div class="nextstepaction"]
> [Azure ilke tanımı yapısı](policy-definition.md)
