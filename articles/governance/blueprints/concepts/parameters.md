---
title: Azure planlar içinde dinamik bir blueprint'i parametreler aracılığıyla oluşturma
description: Statik ve dinamik parametreleri ve bunları kullanarak dinamik bir blueprint'i nasıl oluşturduğunu öğrenin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e1a1d736eb9b22cd444a75b94d112abfe3fbe5af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993587"
---
# <a name="creating-dynamic-blueprints-through-parameters"></a>Parametreler ile dinamik şemaları oluşturma

Çeşitli yapılar (örneğin, kaynak grupları, Resource Manager şablonları, ilkeleri veya rol atamaları) ile tam olarak tanımlı bir şema hızlı oluşturma ve Azure nesnelerin tutarlı sağlama sunar. Bu yeniden kullanılabilir tasarım desenleri ve kapsayıcıları esnek kullanımını etkinleştirmek için Azure şemaları parametrelerini destekler. Parametresi hem tanımı ve şema tarafından dağıtılan yapıtları özelliklerini değiştirmek için atama sırasında esneklik oluşturur.

Kaynak grubu yapıt buna basit bir örnektir. Bir kaynak grubu oluşturulurken sağlanmalıdır iki gerekli değeri içeriyor.: ad ve konum. Parametre varsa yaramadı kılavuzunuz için bir kaynak grubu eklerken, bu ad ve her kullanım için bir konum blueprint'in tanımlarsınız. Bu, blueprint yapıtları aynı kaynak grubunda oluşturmak için her kullanılmasına neden olur. Kaynak grubunu, kaynakları bu kaynak grubu içinde olmayan bir sorun yinelenen haline gelir ve çakışmaya neden olsa da.

> [!NOTE]
> Bu, aynı ada sahip bir kaynak grubu eklemek iki farklı planlar için bir sorun değildir.
> Bir şema içinde zaten bulunan bir kaynak grubu varsa, bu kaynak grubunda ilgili yapıtlar oluşturmak şema devam eder. Aynı ada sahip iki kaynak olarak bu çakışmaya neden olabilir ve kaynak türü, bir abonelikte var olamaz.

Bu şekilde, parametrelerini nerelerde olur. Bu özellikler, kaynak grubu adı söz konusu olduğunda ve location özelliği için değer, şemalar sağlar şema tanımı sırasında tanımlamadan değil ancak bunun yerine bir aboneliğe ataması sırasında değerlerini tanımlayın. Bu, bir kaynak grubu ve diğer kaynakları tek bir abonelik içinde çakışma olmadan oluşturan bir şema yeniden mümkün kılar.

## <a name="blueprint-parameters"></a>Şema parametreleri

REST API aracılığıyla desteklenen tüm öğelerle yanı sıra şema kendisi üzerinde parametreleri oluşturulabilir. Bir parametre üzerinde şema oluşturulduğunda, o şema içindeki yapıları tarafından kullanılabilir. Örnek kaynak grubunu adlandırma önek olabilir. Yapıt blueprint parametresi, parametre ataması sırasında yine de tanımlanabilir, ancak kuruluşunuzun adlandırma kurallarına uyması bir tutarlılık sahip bir "çoğunlukla dinamik" parametresi oluşturmak için sonra kullanabilirsiniz. Adımlar için bkz: [ayarı statik parametreler - düzey parametre blueprint](#blueprint-level-parameter).

### <a name="using-securestring-and-secureobject-parameters"></a>SecureString ve secureObject parametrelerini kullanma

Resource Manager şablonu while _yapıt_ parametrelerini destekler **secureString** ve **secureObject** türleri, Azure şemaları ile bağlı her gerektirir bir Azure anahtar kasası.
Bu şema birlikte gizli dizilerin depolanmasında güvenli olmayan uygulama engeller ve güvenli desenlerinin İstihdam teşvik eder. Azure bir Blueprint'i kolaylaştıran bu eklenmesi ya da güvenli bir parametre, bir Resource Manager şablonunda algılayarak _yapıt_ ve aşağıdaki Key Vault için şema atamasını sırasında isteyen özellikleri her güvenli algılandı Parametre:

- Key Vault kaynak kimliği
- Key Vault gizli dizi adı
- Key Vault gizli dizi sürümü

Başvurulan Key Vault için şema atandığı gibi aynı abonelikte bulunması gerekir ve ayrıca olmalıdır **şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir** Key Vault'un üzerinde yapılandırılmış **erişim ilkeleri** sayfası. Bu özellik etkinleştirme hakkında yönergeler için bkz. [Key Vault - etkinleştir şablonu dağıtım](../../../managed-applications/key-vault-access.md#enable-template-deployment).
Azure Key Vault hakkında daha fazla bilgi için bkz. [anahtar kasasına genel bakış](../../../key-vault/key-vault-overview.md).

## <a name="parameter-types"></a>Parametre türleri

### <a name="static-parameters"></a>Statik Parametreler

Bir şema tanımı içinde tanımlanmış bir parametre değeri olarak adlandırılan bir **statik parametresinin**. Her kullanım blueprint'in statik bu değeri kullanarak yapıt dağıtacağınız olmasıdır. Bu kaynak grubunun adı için anlamsız olmasını sırasında kaynak grubu örnekte, bu konum için mantıklı olabilir. Kaynak grubu, her atama blueprint'in oluşturacak sonra ne olursa olsun aynı konumda ataması sırasında çağrılır. Bu, seçmeli olarak hangi içinde gerekli vs atama sırasında değiştirilebilir tanımladığınız sağlar.

#### <a name="setting-static-parameters-in-the-portal"></a>Portalda static parametrelerini ayarlama

1. Tıklayarak Azure portalında Azure şemaları hizmet başlatma **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında, tıklayarak **şemaları**.

1. Seçin **şema tanımları** sol sayfasında.

1. Var olan bir şema üzerinde tıklayın ve ardından **Düzenle şema** veya **+ Oluştur şema** ve şirket bilgileri doldurun **Temelleri** sekmesi.

1. Tıklayın **sonraki: Yapıtlar** veya tıkladığınızda **Yapıtları** sekmesi.

1. Sahip parametre seçeneklerini görüntülemek için şema eklenen yapıtları **X, Y dolduruldu** içinde **parametreleri** sütun. Yapıt parametrelerini düzenlemek için yapıt satırına tıklayın.

   ![Şema parametreleri](../media/parameters/parameter-column.png)

1. **Düzenle Yapıt** tıkladığınız yapıt için uygun değeri seçenekleri sayfasında görüntülenir. Bir başlık ve değer kutusu bir onay kutusu her yapı parametresine sahiptir. Kutunun hale denetlenmeyen kümesine bir **statik parametresinin**. Yalnızca aşağıdaki örnekte _konumu_ olduğu bir **statik parametresinin** olarak işaretli değildir ve _kaynak grubu adı_ denetlenir.

   ![Blueprint statik Parametreler](../media/parameters/static-parameter.png)

#### <a name="setting-static-parameters-from-rest-api"></a>REST API'SİNDEN statik parametrelerini ayarlama

REST API URI, kendi değerlerinizle değiştirin gerektiğini kullanılan değişkenleri vardır:

- `{YourMG}` -Yönetim grubunuzun adıyla değiştirin
- `{subscriptionId}` -Abonelik Kimliğinizle değiştirin.

##### <a name="blueprint-level-parameter"></a>Blueprint düzeyi parametresi

REST API aracılığıyla bir şema oluştururken, oluşturmak mümkün [blueprint parametreleri](#blueprint-parameters). Bunu yapmak için aşağıdaki REST API URI'sini ve gövde biçimi kullanın:

- REST API URI'Sİ

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2017-11-11-preview
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

- REST API URI'Sİ

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2017-11-11-preview
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

Bu örnekte, **principalIds** yapılan özellik kullanımı **sahipleri** düzey parametre değeri sağlayarak blueprint `[parameters('owners')]`. Blueprint düzeyi parametresi kullanılarak bir yapıt üzerinde bir parametre ayarı yine de bir örneği bir **statik parametresinin** blueprint ataması sırasında ayarlanamaz ve bu değer her atamada olacaktır.

##### <a name="artifact-level-parameter"></a>Yapı düzeyi parametresi

Oluşturma **statik parametreler** bir yapıt üzerinde benzer, ancak kullanmak yerine düz bir değeri alır `parameters()` işlevi. Aşağıdaki örnek, iki statik parametreler oluşturur **tagName** ve **tagValue**. Her değer doğrudan sağlanır ve bir işlev çağrısı kullanmaz.

- REST API URI'Sİ

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2017-11-11-preview
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

Statik bir parametrenin tersidir bir **dinamik parametre**. Bu şema üzerinde tanımlı değil, ancak bunun yerine her blueprint ataması sırasında tanımlanmış bir parametredir. Kaynak grubu örnekte bu blueprint'in her atama için farklı bir ad sağlamayı kaynak grubu adı için anlamlıdır.

#### <a name="setting-dynamic-parameters-in-the-portal"></a>Portalda dinamik parametreleri ayarlanıyor

1. Tıklayarak Azure portalında Azure şemaları hizmet başlatma **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında, tıklayarak **şemaları**.

1. Seçin **şema tanımları** sol sayfasında.

1. Sağ tıklatın, atamak ve seçmek için istediğiniz şema üzerinde **atama şema** veya atamak istediğiniz şema üzerinde'ı tıklatın **atama şema** düğmesi.

1. Üzerinde **atama şema** sayfasında, bulmak **Yapıt parametreleri** bölümü. En az birine sahip her bir yapıt **dinamik parametre** yapıt ve yapılandırma seçenekleri görüntüler. Blueprint atamadan önce parametreleri için gerekli değerleri sağlayın. Aşağıdaki örnekte _adı_ olduğu bir **dinamik parametre** şema atamasını tamamlamak için tanımlanmalıdır.

   ![Blueprint dinamik parametre](../media/parameters/dynamic-parameter.png)

#### <a name="setting-dynamic-parameters-from-rest-api"></a>REST API'SİNDEN dinamik parametreleri ayarlanıyor

Ayarı **dinamik parametreleri** atama sırasında istediğiniz değeri doğrudan sağlayarak gerçekleştirilir. Gibi bir işlevi yerine `parameters()`, sağlanan değer uygun bir dizedir. Yapıtlar bir kaynak grubu için bir "şablon adına sahip" olarak tanımlanır ve **adı** ve **konumu** özellikleri. Diğer tüm parametreler için dahil edilen her bir yapıt tanımlanan altında **parametreleri** ile bir **\<adı\>** ve **değer** anahtar çifti. Blueprint ataması sırasında sağlanmadı dinamik bir parametre için yapılandırılmışsa, atama başarısız olur.

- REST API URI'Sİ

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2017-11-11-preview
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

- Hakkında bilgi edinin [blueprint yaşam döngüsü](lifecycle.md)
- Özelleştirme öğrenin [blueprint sıralama sırası](sequencing-order.md)
- Öğrenin yapmak kullanım [blueprint kaynak kilitleme](resource-locking.md)
- Bilgi edinmek için nasıl [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md)
- İle bir blueprint ataması sırasında sorunları gidermek [genel sorun giderme](../troubleshoot/general.md)