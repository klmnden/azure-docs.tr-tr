---
title: "Azure İlkesi kullanarak kurumsal uyumluluğu zorunlu tutacak ilkeleri oluşturma ve yönetme | Microsoft Docs"
description: "Standartları zorunlu tutmak, yönetmeliklere uygunluğu ve denetim gereksinimlerini karşılamak, maliyetleri denetlemek, güvenlik ve performans tutarlılığını korumak, ayrıca kuruluş genelinde tasarım ilkeleri ayarlamak için Azure İlkesi'ni kullanın."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/18/2018
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: a3d47abcbf41133b9bc7194fd97f9b66a70003ff
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme

Kurumsal standartlarınıza ve hizmet düzeyi sözleşmelerinize uyumluluğu korumak için Azure'da ilkelerin nasıl oluşturulduğunu ve yönetildiğini anlamak önemlidir. Bu öğreticide, Azure İlkesi'ni kullanarak kuruluşunuz genelinde ilkeler oluşturma, atama ve yönetmeyle ilgili en yaygın görevlerden bazılarını yerine getirmeyi öğreneceksiniz; örneğin:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atama
> * Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturma ve atama
> * Uyumlu olmayan veya reddedilen kaynakları çözümleme
> * Kuruluş genelinde yeni bir ilke uygulama

Mevcut kaynaklarınızın geçerli uyumluluk durumunu tanımlamak için bir ilke atamak isterseniz, hızlı başlangıç makalelerinde bunun nasıl yapıldığı açıklanır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="assign-a-policy"></a>İlke atama

Azure İlkesi ile uyumluluğu zorlamanın ilk adımı bir ilke tanımı atamaktır. İlke tanımında, bir ilkenin hangi koşullar altında zorlanacağını ve hangi eylemin gerçekleştirileceği tanımlanır. Bu örnekte, uyumluluk için tüm SQL Server veritabanlarının v12.0 sürümünde olması koşulunu zorunlu tutan *SQL Server Sürüm 12.0 Gerektirme* adlı yerleşik bir ilke tanımı atayacaksınız.

1. Azure Portal'da sol bölmede **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![İlke arama](media/assign-policy-definition/search-policy.png)

2. Azure İlkesi sayfasının sol bölmesinde **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.
3. **Atamalar** bölmesinin üst kısmında **İlke Ata**'yı seçin.

   ![İlke tanımı atama](media/create-manage-policy/select-assign-policy.png)

4. **İlke Ata** sayfasında, kullanılabilir tanımlar listesini açmak için **İlke** alanının yanındaki ![İlke tanımı düğmesine](media/assign-policy-definition/definitions-button.png) tıklayın. Tümünü görüntülemek ve açıklamalarını okumak için **Tür** değeri *Yerleşik* olan ilke tanımlarını filtreleyebilirsiniz.

   ![Kullanılabilir ilke tanımlarını açma](media/create-manage-policy/open-policy-definitions.png)

5. **SQL Server Sürüm 12.0 Gerektir**'i seçin. Bu seçeneği hemen bulamazsanız, arama kutusuna **SQL Server Sürüm 12.0 Gerektir** yazın ve ENTER tuşuna basın.

   ![İlkeyi bulma](media/create-manage-policy/select-available-definition.png)

6. Görüntülenen **Ad** otomatik olarak doldurulur ama bu adı değiştirebilirsiniz. Bu örnekte, *SQL Server sürüm 12.0 gerektir* kullanın. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklamada bu ilke atamasının bu ortamda oluşturulan tüm SQL sunucularının 12.0 sürümünde olmasını nasıl sağlayacağıyla ilgili ayrıntılar sağlanır.

7. İlkenin mevcut kaynaklara uygulanmasını güvence altına almak için fiyatlandırma katmanını **Standart** olarak değiştirin.

   Azure İlkesi içinde iki fiyatlandırma katmanı vardır: *Ücretsiz* ve *Standart*. Ücretsiz katmanıyla, ilkeleri yalnızca gelecek kaynaklarda zorunlu tutabilirsiniz; Standart katmanıyla ise, uyumluluk durumunuzu daha iyi anlayabilmek için ilkeleri mevcut kaynaklarda da zorunlu tutarsınız. Azure İlkesi Önizleme aşamasında olduğundan, henüz bir fiyatlandırma modeli yayımlanmadı. Dolayısıyla *Standart*'ı seçtiğinizde fatura almayacaksınız. Fiyatlandırma hakkında daha fazla bilgi için şu konuya bakın: [Azure İlkesi fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-policy).

8. **Kapsam** olarak önceden kaydolduğunuz aboneliği (veya kaynak grubunu) seçin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir.

   Bu örnekte, **Azure Analytics Capacity Dev** aboneliği kullanılır. Sizin aboneliğiniz farklı olacaktır.

10. **Ata**'yı seçin.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Artık bir yerleşik ilke tanımı atadığınıza göre, Azure İlkesi'yle daha fazlasını da yapabilirsiniz. Bundan sonra, ortamınızda oluşturulan sanal makinelerin G serisinden olamayacağını güvence altına alarak maliyet tasarrufu yapmak için yeni bir özel ilke oluşturun. Bu yolla, kuruluşunuzdaki kullanıcılar G serisinden bir sanal makine oluşturmayı her denediğinde istekleri reddedilir.

1. Sol bölmedeki **Yazma**'nın altında **Tanım**'ı seçin.

   ![Yazma alanındaki Tanım](media/create-manage-policy/definition-under-authoring.png)

2. **+ İlke Tanımı**'nı seçin.
3. Aşağıdakileri girin:

   - İlke tanımının adı - *Sanal makine SKU'larının G serisinden küçük olmasını gerektir*
   - İlke tanımının yapması beklenen işlemin açıklaması – İlke tanımı maliyeti düşürmek için bu kapsamda oluşturulan tüm sanal makinelerin SKU'larının G serisinden küçük olmasını zorunlu tutar.
   - İlke tanımının bulunduğu abonelik. Bu örnekte, ilke tanımı **Advisor Analytics Capacity Dev** aboneliğindedir. Sizin abonelik listeniz farklı olacaktır.
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

    İlke kuralındaki *alan özelliğinin* değeri şunlardan biri olmalıdır: Ad, Tür, Konum, Etiketler veya bir diğer ad. Örneğin, `"Microsoft.Compute/VirtualMachines/Size"`.

    Başka json kodu örnekleri görmek için, [Azure İlkesi şablonları](json-samples.md) makalesini okuyun.

4. **Kaydet**’i seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API ile ilke tanımı oluşturma

İlke Tanımları için REST API ile ilke oluşturabilirsiniz. REST API, ilke tanımlarını oluşturmanıza ve silmenize, ayrıca mevcut tanımlar hakkında bilgi almanıza olanak tanır.
İlke tanımı oluşturmak için aşağıdaki örneği kullanın:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
Aşağıdaki örnekte gösterilene benzer bir istek gövdesi ekleyin:

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

## <a name="create-a-policy-definition-with-powershell"></a>PowerShell ile ilke tanımı oluşturma

PowerShell örneğine devam etmeden önce, Azure PowerShell'in en son sürümünü yüklediğinizden emin olun. İlke parametreleri 3.6.0 sürümünde eklenmiştir. Daha önceki bir sürümü kullanıyorsanız, örneklerde parametrenin bulunamadığını belirten bir hata döndürülür.

`New-AzureRmPolicyDefinition` cmdlet'ini kullanarak ilke tanımı oluşturabilirsiniz.

Bir dosyadan ilke tanımı oluşturmak için, dosyanın yolunu geçirin. Dış dosya için aşağıdaki örneği kullanın:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Yerel dosya kullanımı için aşağıdaki örneği kullanın:

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

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

Çıkış bir `$definition` nesnesinde depolanır ve bu nesne ilke ataması sırasında kullanılır.
Aşağıdaki örnekte, parametreler içeren bir ilke tanımı oluşturulur:

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

## <a name="view-policy-definitions"></a>İlke tanımlarını görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```
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

## <a name="view-policy-definitions"></a>İlke tanımlarını görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```
az policy definition list
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

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

## <a name="create-and-assign-an-initiative-definition"></a>Girişim tanımı oluşturma ve atama

Girişim tanımıyla, çeşitli ilke tanımlarını gruplandırıp kapsamlı bir hedefe ulaşabilirsiniz. Girişim tanımı oluşturduğunuzda, tanım kapsamındaki kaynakların girişim tanımını oluşturan ilke tanımlarına uyumlu kalacağından emin olabilirsiniz.  Girişim tanımları hakkında daha fazla bilgi için bkz. [Azure İlkesi genel bakış](./azure-policy-introduction.md).

### <a name="create-an-initiative-definition"></a>Girişim tanımı oluşturma

1. Sol bölmedeki **Yazma**'nın altında **Tanımlar**'ı seçin.

   ![Tanımları seçin](media/create-manage-policy/select-definitions.png)

2. Sayfanın üst kısmında **Giriş Tanımı**'nı seçin; bu seçim size **Girişim Tanımı** formuna götürür.
3. Girişimin adını ve açıklamasını girin.

   Bu örnekte, kaynakların güvenlikle ilgili ilke tanımlarıyla uyumlu olduğundan emin olacaksınız. Bu nedenle, girişimin adı **Güvenliği Sağlama** olabilir ve şöyle bir açıklama yazılabilir: **Bu girişim kaynakların güvenliğini sağlamakla ilişkili tüm ilke tanımlarını işlemek için oluşturuldu**.

   ![Girişim tanımı](media/create-manage-policy/initiative-definition.png)

4. **Kullanılabilir Tanımlar** listesine göz atın ve bu girişime eklemek istediğiniz ilke tanımlarını seçin. **Güvenliği sağlama** girişimimiz için, şu yerleşik ilke tanımlarını **ekleyin**:
   - SQL Server sürüm 12.0 gerektir
   - Güvenlik Merkezi'nde korumasız web uygulamalarını izle.
   - Güvenlik Merkezi'nde esnek ağın genelini izle.
   - Güvenlik Merkezi'nde olası uygulama Beyaz Listesini izle.
   - Güvenlik Merkezi'nde şifrelenmemiş VM Disklerini izle.

   ![Girişim tanımları](media/create-manage-policy/initiative-definition-2.png)

   Listeden ilke tanımlarını seçtikten sonra, önceki resimde gösterildiği gibi bunları **İlkeler ve parametreler** altında görürsünüz.

5. **Tanım konumu**'nu kullanarak tanımın depolanacağı aboneliği seçin. **Kaydet**’i seçin.

### <a name="assign-an-initiative-definition"></a>Girişim tanımını atama

1. **Yazma**'nın altında **Tanımlar** sekmesine gidin.
2. Oluşturduğunuz **Güvenliği sağlama** girişim tanımını arayın.
3. Girişim tanımını seçin ve sonra da **Ata**'yı seçin.

   ![Tanımı atama](media/create-manage-policy/assign-definition.png)

4. Aşağıdaki örnek bilgileri girerek **Atama** formunu doldurun. Kendi bilgilerinizi de kullanabilirsiniz.
   - Ad: Güvenliği sağlama ataması
   - Açıklama: Bu girişim ataması **Azure Advisor Capacity Dev** aboneliğinde bu ilke tanımları grubunu zorunlu tutmak için uyarlanmıştır.
   - Fiyatlandırma Katmanı: Standart
   - Bu atamanın uygulanmasını istediğiniz kapsam: **Azure Advisor Capacity Dev**. Kendi aboneliğinizi ve kaynak grubunuzu seçebilirsiniz.

5. **Ata**'yı seçin.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Dışlama'yı kullanarak uyumlu olmayan veya reddedilen kaynağı hariç tutma

Yukarıdaki örneği izleyerek, SQL Server sürüm 12.0'ı gerektirecek bir ilke tanımı atadıktan sonra farklı bir sürümde oluşturulan SQL Server reddedilebilir. Bu bölümde, reddedilen SQL Server oluşturma girişimini çözmek için bir dışlama isteğinde bulunma işleminde size yol gösterilir. Dışlama temelde ilkenin zorlanmasını engeller. Kaynak grubuna dışlama uygulanabilir veya dışlamayı tek tek kaynakları içerecek şekilde daraltabilirsiniz.

1. Sol bölmede **Atamalar**'ı seçin.
2. Tüm ilke atamalarına göz atın ve *SQL Server sürüm 12.0 gerektir* atamasını açın.
3. SQL Server oluşturmaya çalıştığınız kaynak gruplarındaki kaynaklar için bir dışlama **seçin**. Bu örnekte, Microsoft.Sql/servers/databases: *azuremetrictest/testdb* ve *azuremetrictest/testdb2*'yi dışlarsınız.

   ![Dışlama isteğinde bulunma](media/create-manage-policy/request-exclusion.png)

   Reddedilen kaynak sorununu çözmenin diğer yolları: Kesinlikle haklı bir gerekçeyle SQL Server'ın oluşturulmasına ihtiyacınız varsa ilkeyle ilişkilendirilmiş kişiye ulaşabilir ve erişiminiz varsa ilkeyi doğrudan düzenleyebilirsiniz.

4. **Ata**'ya tıklayın.

Bu bölümde, kaynaklarda bir dışlama isteğine bulunarak sürümü 12.0 olan bir SQL Server oluşturma girişiminizin reddedilmesi sorununu çözmüş oldunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerle çalışmaya devam etmeyi planlıyorsanız bu kılavuzda oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki adımları kullanarak daha önce oluşturulan tüm atamaları veya tanımları silin:

1. Sol bölmede **Tanımlar**'ı (veya atamayı silmeye çalışıyorsanız **Atamalar**'ı) seçin.
2. Az önce oluşturduğunuz yeni girişim veya tanımını (ya da atamayı) arayın.
3. Tanım veya atamanın sonundaki üç noktayı seçin ve sonra da **Tanımı Sil** (veya **Atamayı Sil**) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki işlemleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atandı
> * Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturuldu ve atandı
> * Uyumlu olmayan veya reddedilen kaynak sorunu çözüldü
> * Kuruluş genelinde yeni bir ilke uygulandı

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için şu makaleyi gözden geçirin:

> [!div class="nextstepaction"]
> [Azure İlkesi tanım yapısı](policy-definition.md)
