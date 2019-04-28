---
title: Dinamik şemaları oluşturmak için parametreleri kullanın
description: Statik ve dinamik parametreleri ve bunları kullanarak dinamik bir blueprint'i nasıl oluşturduğunu öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: ac7b662bc9ef4f3ae675c4cbde18e159383d3d8e
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767042"
---
# <a name="creating-dynamic-blueprints-through-parameters"></a>Parametreler ile dinamik şemaları oluşturma

Çeşitli yapılar (örneğin, kaynak grupları, Resource Manager şablonları, ilkeleri veya rol atamaları) ile tam olarak tanımlı bir şema hızlı oluşturma ve Azure içindeki nesneleri tutarlı oluşturulmasını sağlar. Bu yeniden kullanılabilir tasarım desenleri ve kapsayıcıları esnek kullanımını etkinleştirmek için Azure şemaları parametrelerini destekler. Parametresi hem tanımı ve şema tarafından dağıtılan yapıtları özelliklerini değiştirmek için atama sırasında esneklik oluşturur.

Kaynak grubu yapıt buna basit bir örnektir. Bir kaynak grubu oluşturulurken sağlanmalıdır iki gerekli değeri içeriyor.: ad ve konum. Parametre varsa yaramadı kılavuzunuz için bir kaynak grubu eklerken, bu ad ve her kullanım için bir konum blueprint'in tanımlarsınız. Bu yineleme blueprint yapıtları aynı kaynak grubunda oluşturmak için her kullanılmasına neden olur. Kaynaklar bu kaynak grubu içinde yinelenen haline gelir ve çakışmaya neden.

> [!NOTE]
> Bu, aynı ada sahip bir kaynak grubu eklemek iki farklı planlar için bir sorun değildir.
> Bir şema içinde zaten bulunan bir kaynak grubu varsa, bu kaynak grubunda ilgili yapıtlar oluşturmak şema devam eder. Aynı ada sahip iki kaynak olarak bu çakışmaya neden olabilir ve kaynak türü, bir abonelikte var olamaz.

Bu sorunun çözümü parametreleri var. Blueprint yapıtın her bir özellik değeri bir aboneliğe ataması sırasında tanımlamanıza izin verir. Parametre bir kaynak grubu ve diğer kaynakları tek bir abonelik içinde çakışma olmadan oluşturan bir şema kullanmayı mümkün kılar.

## <a name="blueprint-parameters"></a>Şema parametreleri

REST API aracılığıyla parametreleri şema üzerinde oluşturulabilir. Bu parametreler, desteklenen tüm öğelerle parametreleri farklıdır. Bir parametre üzerinde şema oluşturulduğunda, o şema içindeki yapıları tarafından kullanılabilir. Örnek kaynak grubunu adlandırma önek olabilir. Yapıt blueprint parametresi, "dinamik çoğunlukla" parametreyi oluşturmak üzere kullanabilirsiniz. Parametresi, atama sırasında tanımlanabilir gibi bu düzen adlandırma kurallarına bir tutarlılık sağlar. Adımlar için bkz: [ayarı statik parametreler - düzey parametre blueprint](#blueprint-level-parameter).

### <a name="using-securestring-and-secureobject-parameters"></a>SecureString ve secureObject parametrelerini kullanma

Resource Manager şablonu while _yapıt_ parametrelerini destekler **secureString** ve **secureObject** türleri, Azure şemaları ile bağlı her gerektirir bir Azure anahtar kasası.
Bu güvenlik önlemi şema birlikte gizli dizilerin depolanmasında güvenli olmayan uygulama engeller ve güvenli desenlerinin İstihdam teşvik eder. Azure bir Blueprint'i eklenmesi ya da güvenli bir parametre, bir Resource Manager şablonunda algılama bu güvenlik önlemi destekler _yapıt_. Hizmet daha sonra aşağıdaki anahtar kasası özelliklerin algılanan güvenli parametre başına ataması sırasında ister:

- Key Vault kaynak kimliği
- Key Vault gizli dizi adı
- Key Vault gizli dizi sürümü

Şema atamasını kullanıyorsa bir **yönetilen sistem tarafından atanan kimliği**, Key Vault başvurulan _gerekir_ şema tanımını atanır aynı abonelikte yok.

Şema atamasını kullanıyorsa bir **yönetilen kullanıcı tarafından atanan kimliği**, Key Vault başvurulan _olabilir_ merkezi bir abonelikte yok. Yönetilen kimlik, anahtar Kasası'şema atamasını önce uygun hakları verilmesi gerekir.

Her iki durumda da, anahtar kasası olmalıdır **şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir** yapılandırılan **erişim ilkeleri** sayfası. Bu özellik etkinleştirme hakkında yönergeler için bkz. [Key Vault - etkinleştir şablonu dağıtım](../../../managed-applications/key-vault-access.md#enable-template-deployment).

Azure Key Vault hakkında daha fazla bilgi için bkz. [anahtar kasasına genel bakış](../../../key-vault/key-vault-overview.md).

## <a name="parameter-types"></a>Parametre türleri

### <a name="static-parameters"></a>Statik Parametreler

Bir şema tanımı içinde tanımlanmış bir parametre değeri olarak adlandırılan bir **statik parametresinin**, her kullanım blueprint'in statik bu değeri kullanarak yapıt dağıtır. Bu kaynak grubunun adı için anlam ifade etmez ancak kaynak grubu örnekte bu konum için mantıklı olabilir. Kaynak grubu, her atama blueprint'in oluşturacak sonra ne olursa olsun aynı konumda ataması sırasında çağrılır. Bu esnekliğin hangi, atama sırasında değiştirilebilir gerekli vs tanımlamak seçmeli olmasını sağlar.

#### <a name="setting-static-parameters-in-the-portal"></a>Portalda static parametrelerini ayarlama

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** sol sayfasında.

1. Var olan bir şema üzerinde tıklayın ve ardından **düzenleme şema** veya **+ Oluştur şema** ve şirket bilgileri doldurun **Temelleri** sekmesi.

1. Tıklayın **sonraki: Yapıtları** veya tıkladığınızda **Yapıtları** sekmesi.

1. Sahip parametre seçeneklerini görüntülemek için şema eklenen yapıtları **X, Y dolduruldu** içinde **parametreleri** sütun. Yapıt parametrelerini düzenlemek için yapıt satırına tıklayın.

   ![Bir şema tanımı üzerinde şema parametreleri](../media/parameters/parameter-column.png)

1. **Düzenle Yapıt** tıkladığınız yapıt için uygun değeri seçenekleri sayfasında görüntülenir. Bir başlık ve değer kutusu bir onay kutusu her yapı parametresine sahiptir. Kutunun hale denetlenmeyen kümesine bir **statik parametresinin**. Yalnızca aşağıdaki örnekte _konumu_ olduğu bir **statik parametresinin** , içerdiğinden denetlenmeyen ve _kaynak grubu adı_ denetlenir.

   ![Blueprint yapıtı üzerinde şema statik Parametreler](../media/parameters/static-parameter.png)

#### <a name="setting-static-parameters-from-rest-api"></a>REST API'SİNDEN statik parametrelerini ayarlama

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourMG}` - Yönetim grubunuzun adıyla değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

##### <a name="blueprint-level-parameter"></a>Blueprint düzeyi parametresi

REST API aracılığıyla bir şema oluştururken, oluşturmak mümkün [blueprint parametreleri](#blueprint-parameters). Bunu yapmak için aşağıdaki REST API URI'sini ve gövde biçimi kullanın:

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "description": "This blueprint has blueprint level parameters.",
          "targetScope": "subscription",
          "parameters": {
              "owners": {
                  "type": "array",
                  "metadata": {
                      "description": "List of AAD object IDs that is assigned Owner role at the resource group"
                  }
              }
          },
          "resourceGroups": {
              "storageRG": {
                  "description": "Contains the resource template deployment and a role assignment."
              }
          }
      }
  }
  ```

Blueprint düzeyi parametresi oluşturulduktan sonra bu şema için eklenen yapıtları kullanılabilir.
Aşağıdaki REST API örnek, bir rol atama yapıtındaki üzerinde şema oluşturur ve şema düzeyinde parametresini kullanır.

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "kind": "roleAssignment",
      "properties": {
          "resourceGroup": "storageRG",
          "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
          "principalIds": "[parameters('owners')]"
      }
  }
  ```

Bu örnekte, **principalIds** özelliği kullanan **sahipleri** düzey parametre değerini kullanarak blueprint `[parameters('owners')]`. Blueprint düzeyi parametresi kullanılarak bir yapıt üzerinde bir parametre ayarı yine de bir örneği bir **statik parametresinin**. Blueprint düzeyi parametresi blueprint ataması sırasında ayarlanamaz ve her atamada aynı değer olacaktır.

##### <a name="artifact-level-parameter"></a>Yapı düzeyi parametresi

Oluşturma **statik parametreler** bir yapıt üzerinde benzer, ancak kullanmak yerine düz bir değeri alır `parameters()` işlevi. Aşağıdaki örnek, iki statik parametreler oluşturur **tagName** ve **tagValue**. Her değer doğrudan sağlanır ve bir işlev çağrısı kullanmaz.

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "kind": "policyAssignment",
      "properties": {
          "description": "Apply storage tag and the parameter also used by the template to resource groups",
          "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
          "parameters": {
              "tagName": {
                  "value": "StorageType"
              },
              "tagValue": {
                  "value": "Premium_LRS"
              }
          }
      }
  }
  ```

### <a name="dynamic-parameters"></a>Dinamik parametreler

Statik bir parametrenin tersidir bir **dinamik parametre**. Bu parametre, şema üzerinde tanımlı değil, ancak bunun yerine her blueprint ataması sırasında tanımlanır. Kaynak grubu örnekte kullanımı bir **dinamik parametre** anlamlı kaynak grubu adı için. Bu blueprint'in her atama için farklı bir ad sağlar. Blueprint işlevlerin bir listesi için bkz. [blueprint işlevleri](../reference/blueprint-functions.md) başvuru.

#### <a name="setting-dynamic-parameters-in-the-portal"></a>Portalda dinamik parametreleri ayarlanıyor

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** sol sayfasında.

1. Atamak istediğiniz şema üzerinde sağ tıklayın. Seçin **Ata şema** veya atamak istediğiniz şema üzerinde tıklayın ve ardından tıklayın **Ata şema** düğmesi.

1. Üzerinde **Ata şema** sayfasında, bulmak **Yapıt parametreleri** bölümü. En az birine sahip her bir yapıt **dinamik parametre** yapıt ve yapılandırma seçenekleri görüntüler. Blueprint atamadan önce parametreleri için gerekli değerleri sağlayın. Aşağıdaki örnekte _adı_ olduğu bir **dinamik parametre** şema atamasını tamamlamak için tanımlanmalıdır.

   ![Blueprint ataması sırasında şema dinamik parametre](../media/parameters/dynamic-parameter.png)

#### <a name="setting-dynamic-parameters-from-rest-api"></a>REST API'SİNDEN dinamik parametreleri ayarlanıyor

Ayarı **dinamik parametreleri** değer girerek doğrudan atama sırasında gerçekleştirilir. Gibi bir işlevi yerine [parameters()](../reference/blueprint-functions.md#parameters), sağlanan değer uygun bir dizedir. Yapıtlar bir kaynak grubu için bir "şablonu adıyla" tanımlanmış **adı**, ve **konumu** özellikleri. Dahil edilen bir yapıt için diğer tüm parametreleri bölümünde tanımlanan **parametreleri** ile bir **\<adı\>** ve **değer** anahtar çifti. Blueprint ataması sırasında sağlanmayan dinamik bir parametre için yapılandırılmışsa, atama başarısız olur.

- REST API URI'si

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}  /providers/Microsoft.Blueprint/blueprints/MyBlueprint",
          "resourceGroups": {
              "storageRG": {
                  "name": "StorageAccount",
                  "location": "eastus2"
              }
          },
          "parameters": {
              "storageAccountType": {
                  "value": "Standard_GRS"
              },
              "tagName": {
                  "value": "CostCenter"
              },
              "tagValue": {
                  "value": "ContosoIT"
              },
              "contributors": {
                  "value": [
                      "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                      "38833b56-194d-420b-90ce-cff578296714"
                  ]
                },
              "owners": {
                  "value": [
                      "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                      "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                  ]
              }
          }
      },
      "identity": {
          "type": "systemAssigned"
      },
      "location": "westus"
  }
  ```

## <a name="next-steps"></a>Sonraki adımlar

- Listesine bakın [blueprint işlevleri](../reference/blueprint-functions.md).
- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.