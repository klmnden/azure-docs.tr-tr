---
title: Azure Resource Manager şablonu oluşturma
description: Bir Azure Resource Manager şablon yazma işlemi açıklanır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: tomfitz
ms.openlocfilehash: bbe891aa584423d64531ae4b23bb8a6ead38c3da
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205569"
---
# <a name="create-azure-resource-manager-template"></a>Azure Resource Manager şablonu oluşturma

Bu makalede, Azure Resource Manager şablonu oluştururken kararları ve işlem açıklanır. Bu örnekler, şablonunuzu yazarken yardımcı ve genel bir bakış sağlar. Kaynakları bir kaynak grubuna dağıtıyorsanız varsayılır. Azure ilkeleri veya rol tabanlı erişim denetimi atamalarını oluşturma gibi hesabınızı Azure aboneliğinize kaynakları dağıtma gerekiyorsa bkz [oluşturma kaynak grubu ve bir Azure aboneliği için kaynak](deploy-to-subscription.md).

## <a name="select-json-editor"></a>JSON Düzenleyicisi'ni seçin

Resource Manager şablonu bir JSON dosyasıdır. JSON dosyası üzerinde çalışmak için iyi bir geliştirme aracı ihtiyacınız vardır. Birçok seçeneğiniz vardır, ancak tercih ettiğiniz bir düzenleyici yoksa, yükleme [Visual Studio Code (VS Code)](https://code.visualstudio.com/). 

VS Code'u yükledikten sonra ekleme [Azure Resource Manager araçları uzantısını](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Bu uzantı, şablon yazma kolaylaştıran birçok özellik ekler.

![Şablonu Visual Studio kodu](./media/how-to-create-template/template-visual-studio-code.png)

Ekran görüntüsünde, Visual Studio Code'da açık bir Resource Manager şablonu gösterilmektedir. 

Resource Manager araçları uzantısını ve VS Code kullanma hakkında döşenmesine ilişkin bir öğretici için bkz [hızlı başlangıç: Visual Studio Code kullanarak Azure Resource Manager şablonları oluşturma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md).

## <a name="understand-the-template-structure"></a>Şablon yapısını anlama

Şablonun nasıl çalıştığını anlamak için şablonunun bölümlerine gözden geçirelim. Şablonunuzu her bölüm olmayabilir. Odaklanmak için istediğiniz bölümleri şunlardır:

* [Parametreleri](resource-group-authoring-templates.md#parameters) bölümünde, dağıtılan altyapıyı özelleştirmek için dağıtım sırasında belirtebileceğiniz değerleri gösterir. 

* [Değişkenleri](resource-group-authoring-templates.md#variables) bölümü şablonun tamamında yeniden kullanılan değerleri gösterir.

* [İşlevleri](resource-group-authoring-templates.md#functions) bölümünde, özelleştirilmiş şablonunuzda kullanılan şablon ifadeleri hangi gösterir.

* [Kaynakları](resource-group-authoring-templates.md#resources) bölümünde, aboneliğinizde dağıtılmış Azure kaynaklarını gösterir.

* [Çıkarır](resource-group-authoring-templates.md#outputs) bölümünde, dağıtım tamamlandıktan sonra döndürülen değerleri gösterir.

## <a name="look-for-similar-templates"></a>Benzer şablonları arayın

Genellikle, tam olarak ihtiyaç duyduğunuz benzer bir çözüm dağıtılan mevcut bir şablonu bulabilirsiniz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/) topluluğa katkıda bulunanlar şablonlardan yüzlerce sahiptir.

![Şablon hızlı deposu](./media/how-to-create-template/template-quickstart-repo.png)

Bu depo için tam olarak ihtiyaç duyduğunuz benzer bir şablon arayın. Şablonu tam olarak ihtiyacınız yapmaz, özelleştirebilirsiniz uygundur.

Bir şablon bulduktan sonra seçin **Github'da göz at**ve ardından kopyalama **azuredeploy.json** depodan dosya. VS Code'da adlı yeni bir dosya oluşturmak **azuredeploy.json** ve hızlı başlangıç depodan kopyaladığınız şablon dosyasının içeriği ekleyin.

## <a name="add-resources"></a>Kaynak Ekle

Büyük olasılıkla tam olarak aradığınız şeyi desteklediğinden emin olmak için şablonu özelleştirmek istiyorsanız. İlk olarak, dağıtılan kaynakları gözden geçirin. Eklemek, kaldırmak veya şablondaki kaynaklarda değiştirmeniz gerekebilir. Açıklamalar ve söz dizimi kaynakları için bkz. [Azure Resource Manager şablon başvurusu](/azure/templates/).

![Şablon başvurusu](./media/how-to-create-template/template-reference.png)

Bu özellikleri gözden geçirdikten sonra tüm gerekli değişiklikleri yapın. Kaynakları tanımlama hakkında daha fazla öneri için bkz. [kaynaklar için önerilen uygulamalar -](template-best-practices.md#resources).

## <a name="add-or-remove-parameters"></a>Parametre Ekle Kaldır

Şablon parametrelerini ayarlamak gerekebilir. Parametreleri temel ekleyip dağıtım sırasında etkinleştirmek istediğiniz üzerinde ne kadar özelleştirme. Parametreleri kullanma hakkında daha fazla öneri için bkz. [parametreleri - önerilen uygulamalar](template-best-practices.md#parameters).

## <a name="add-tags"></a>Etiket ekleme

Mantıksal olarak kategorilere göre düzenlemek için kaynaklarınıza etiketler ekleyin ve faturalandırma maliyetlerini bölün. Etiket eklemek kolaydır, kaynak için JSON uygulamadan. Örneğin, aşağıdaki depolama hesabı iki etiket vardır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
      {
        "apiVersion": "2016-01-01",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[concat('storage', uniqueString(resourceGroup().id))]",
        "location": "[resourceGroup().location]",
        "tags": {
          "Dept": "Finance",
          "Environment": "Production"
        },
        "sku": {
          "name": "Standard_LRS"
        },
        "kind": "Storage",
        "properties": { }
      }
    ]
}
```

Ayrıca dinamik parametreler etiketler ekleyebilirsiniz. Daha fazla bilgi için [şablon etiketleri](resource-group-using-tags.md#templates).

## <a name="review-template-functions"></a>Şablon işlevleri gözden geçirin

Köşeli parantez içine alınmış gibi ifadeler şablonunuzdaki fark edebilirsiniz `"[some-expression]"`. Bu ifadeler, şablon işlevleri değerleri dağıtım sırasında dinamik olarak oluşturmak için kullanın.

Örneğin, genellikle gibi bir ifade bakın:

```json
"name": "[parameters('siteName')]"
```

Bu ifade, bir parametre değerini alır. Name özelliği için değeri atanır.

Veya kullanan çeşitli işlevler gibi daha karmaşık bir ifade görebilirsiniz:

```json
"[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Bu ifade, bir depolama hesabının özelliklerine sahip bir nesne alır.

Ne olduğunu anlamak için işlevleri inceleyin [şablonu işlev başvurusu](resource-group-template-functions.md) belgeleri.

## <a name="create-more-than-one-instance"></a>Birden fazla örneği oluşturma

Bazen birden fazla örneğini bir kaynak oluşturmak istiyorsunuz. Örneğin, çeşitli depolama hesapları gerekebilir. Bunun yerine, şablon aracılığıyla kaynak yineleyin daha kullanabilirsiniz `copy` birden fazla örneğini belirtmek için sözdizimi.

Aşağıdaki örnek, üç depolama hesabı oluşturur:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Dinamik bir parametre örneklerinin sayısını da belirtebilirsiniz. Daha fazla bilgi için [bir kaynağa veya Azure Resource Manager şablonları özelliğinde birden fazla örneğini dağıtma](resource-group-create-multiple.md).

## <a name="conditionally-deploy-a-resource"></a>Koşullu olarak kaynak dağıtma

Bazen, dağıtım sırasında bir şablon kaynağında dağıtılıp dağıtılmadığını belirtmeniz gerekir. Örneğin, yeni bir kaynak dağıtın veya mevcut bir kaynağı kullanma esnekliği isteyebilirsiniz. `condition` Öğesi Aç veya kapat bir kaynak için dağıtım olanağı sağlar. Koşul öğe ifadesinde true olduğunda, kaynak dağıtılır. Yanlış olduğunda, kaynak dağıtım sırasında atlanır.

Aşağıdaki örnek, koşullu olarak bir depolama hesabı dağıtır:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Daha fazla bilgi için [koşul öğesi](resource-group-authoring-templates.md#condition).

## <a name="review-dependencies"></a>Bağımlılıkları gözden geçirin

Şablonunuzdaki bazı kaynaklar diğer kaynaklara önce dağıtılması gerekiyor. Örneğin, SQL server SQL veritabanı oluşturmadan önce bulunması gerekir. Kaynak Yöneticisi, örtük olarak kaynaklar için dağıtım sırasını belirler, [başvuru işlevi](resource-group-template-functions-resource.md#reference) kullanılır. Ancak, bazı durumlarda, kullanarak açıkça tanımlamak ihtiyacınız `dependsOn` öğesi. Herhangi bir bağımlılığın eklenmesi gerekip gerekmediğini görmek için şablonunuzun gözden geçirin. Dağıtım yavaş veya döngüsel başvurular oluşturmak gibi gereksiz bağımlılıkları ekleme konusunda dikkatli olun.

Aşağıdaki görüntüde, bağımlılık sırası çeşitli App Service kaynaklarını gösterilmektedir:

![Web uygulama bağımlılıkları](./media/how-to-create-template/web-dependencies.png)

Aşağıdaki örnek, bağımlılıkları tanımlayan bir şablon bölümünü gösterir.

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki kaynakları dağıtma sırasını belirleme](resource-group-define-dependencies.md).

## <a name="review-recommended-practices"></a>Önerilen yöntemleri gözden geçirin

Şablon dağıtımı öncesinde gözden geçirmeniz [Azure Resource Manager şablonu iyi](template-best-practices.md) olmadığını görmek için şablonunuzda uygulamak istediğiniz yaklaşım önerilir.

Şablonunuz, farklı Azure bulut ortamlarında kullanmak istiyorsanız bkz [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablonu dağıtmak için bkz. [Azure CLI ile dağıtma](resource-group-template-deploy-cli.md) veya [PowerShell ile dağıtma](resource-group-template-deploy.md).
* Adım adım bir hızlı bir şablon oluşturma hakkında bilgi için bkz: [Visual Studio Code kullanarak oluşturun, Azure Resource Manager şablonları](resource-manager-quickstart-create-templates-use-visual-studio-code.md).
* Bir şablonda kullanabileceğiniz işlevler listesi için bkz. [şablon işlevleri](resource-group-template-functions.md).
