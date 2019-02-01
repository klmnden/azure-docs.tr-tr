---
title: Azure Resource Manager şablonu kaynakları | Microsoft Docs
description: Bildirim temelli JSON söz dizimini kullanarak Azure Resource Manager şablonları, kaynaklar bölümü açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: 2f850c25250c59a5fd62964d53b6b9d37ff4cf49
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55491408"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları, kaynaklar bölümü

Kaynaklar bölümünde, dağıtılan veya güncelleştirilen kaynakları tanımlayın. Bu bölümde, doğru değerleri sağlamak üzere dağıtıyorsanız türlerini anlamanız gerekir çünkü karmaşık alabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki yapıya sahip kaynakları tanımlarsınız:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| koşul | Hayır | Bu dağıtım sırasında kaynak sağlanan olup olmadığını gösteren Boole değeri. Zaman `true`, kaynak dağıtım sırasında oluşturulur. Zaman `false`, bu dağıtım için kaynak atlandı. |
| apiVersion |Evet |Kaynak oluşturmak için REST API sürümü. |
| type |Evet |Kaynak türü. Kaynak sağlayıcıya ve kaynak türü için ad alanı, bu değer oluşur (gibi **Microsoft.Storage/storageAccounts**). |
| ad |Evet |Kaynağın adı. Ad URI bileşeni kısıtlamaları RFC3986 içinde tanımlanan izlemelidir. Ayrıca, kaynak adı dışında tarafların emin olmak için adını doğrulamak için kullanıma sunan Azure Hizmetleri başka bir kimlik sızmasını girişimi değildir. |
| location |Değişir |Sağlanan kaynak coğrafi konumda desteklenmiyor. Mevcut konumlardan birini seçebilirsiniz, ancak genellikle kullanıcılarınıza yakın olan bir çekme mantıklıdır. Genellikle, da aynı bölgede birbiriyle etkileşim kaynakları yerleştirin mantıklıdır. Çoğu kaynak türleri bir konum gerektirme, ancak bazı türleri (örneğin, bir rol ataması) bir konuma gerektirmez. |
| etiketler |Hayır |Kaynakla ilişkili etiketler. Kaynakları aboneliğiniz arasında mantıksal olarak düzenlemek için etiketler. |
| Açıklamaları |Hayır |Şablonunuzda kaynaklar belgelemek için Notlar |
| kopyala |Hayır |Birden fazla örneği gerekiyorsa oluşturmak için kaynak sayısı. Paralel varsayılan moddur. Tüm istemediğinizde seri modu veya aynı anda dağıtmak amacıyla kaynaklarınızı belirtin. Daha fazla bilgi için [Azure Resource Manager'da kaynakları çeşitli örneklerini oluşturmak](resource-group-create-multiple.md). |
| dependsOn |Hayır |Bu kaynak dağıtılmadan önce dağıtılmalıdır kaynaklar. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları doğru sırayla dağıtır. Kaynakları birbirlerine bağımlı olmayan, paralel olarak dağıtılan. Değer bir kaynağa virgülle ayrılmış bir listesini olabilir adlarına veya kaynak benzersiz tanımlayıcıları. Yalnızca bu şablon dağıtılan kaynakları listeler. Bu şablonda tanımlı olmayan kaynakları önceden var olmalıdır. Dağıtımınızı yavaş ve döngüsel bağımlılıklar oluşturma gibi gereksiz bağımlılıkları eklemekten kaçının. Bağımlılıklarını ayarlama hakkında yönergeler için bkz [Azure Resource Manager şablonlarında bağımlılık tanımlama](resource-group-define-dependencies.md). |
| properties |Hayır |Kaynağa özgü yapılandırma ayarları. Özellikleri için değer, istek gövdesinde bir kaynak oluşturmak REST API işlemi için (PUT yöntemini) sağladığınız değerler ile aynıdır. Ayrıca, bir özelliği birden çok örneği oluşturmak için bir kopya dizisi belirtebilirsiniz. |
| sku | Hayır | Bazı kaynaklar dağıtmak için SKU tanımlama değerlerini sağlar. Örneğin, bir depolama hesabı için yedeklilik türünü belirtebilirsiniz. |
| tür | Hayır | Bazı kaynaklar dağıttığınız kaynak türünü tanımlayan bir değeri sağlar. Örneğin, Cosmos DB, oluşturulacak türünü belirtebilirsiniz. |
| planı | Hayır | Bazı kaynaklar dağıtmayı planlıyorsunuz tanımlayan değerleri sağlar. Örneğin, bir sanal makine için Market görüntüsüne belirtebilirsiniz. | 
| kaynaklar |Hayır |Tanımlanan kaynağına bağımlı alt kaynakları. Yalnızca üst kaynak şema tarafından izin verilen kaynak türleri sağlar. Üst kaynak türü gibi tam olarak nitelenmiş tür alt kaynak içerir **Microsoft.Web/sites/extensions**. Üst Kaynak bağımlılığı kapsanan değil. Ayrıca, bu bağımlılık açıkça tanımlamanız gerekir. |

## <a name="condition"></a>Koşul

Kaynak Oluştur gerekip gerekmediğini dağıtım sırasında karar vermelisiniz kullanırsanız `condition` öğesi. Bu öğenin değeri true veya false olarak çözümler. Değer true ise, bir kaynak oluşturulur. Kaynak değeri false olduğunda oluşturulmadı. Değer yalnızca kaynağın tamamını uygulanabilir.

Genellikle, yeni bir kaynak oluşturmak veya mevcut bir istediğinizde bu değeri kullanın. Örneğin, yeni bir depolama hesabı dağıtıldığına veya mevcut bir depolama hesabını belirtmek için kullanılan, kullanın:

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

Kullanan bir tam örnek şablonu için `condition` öğesi bkz [yeni veya mevcut bir sanal ağ, depolama ve genel IP ile VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="resource-specific-values"></a>Kaynağa özgü değer

**ApiVersion**, **türü**, ve **özellikleri** öğeleri her kaynak türü için farklıdır. **Sku**, **tür**, ve **planı** bazı kaynak türleri için kullanılabilir, ancak tüm öğeler. Bu özelliklerin değerlerini belirlemek için bkz: [şablon başvurusu](/azure/templates/).

## <a name="resource-names"></a>Kaynak adları

Genel olarak, kaynak adları Kaynak Yöneticisi'nde üç türde çalışır:

* Kaynak adları benzersiz olmalıdır.
* Kaynak adları benzersiz olması gerekmez, ancak kaynak belirlemenize yardımcı olabilecek bir ad vermek seçin.
* Genel kaynak adları.

### <a name="unique-resource-names"></a>Benzersiz kaynak adları

Veri erişim uç noktası olan herhangi bir kaynak türü için benzersiz bir kaynak adı belirtin. Benzersiz bir ad gerektiren bazı yaygın kaynak türleri şunlardır:

* Azure depolama<sup>1</sup> 
* Azure Uygulama Hizmeti’nin Web Apps özelliği
* SQL Server
* Azure Key Vault
* Redis için Azure Önbelleği
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> depolama hesabı adları da küçük olmalıdır, 24 karakterden daha az veya ve herhangi bir kısa çizgi zorunda kalmazsınız.

Ayar adı, el ile benzersiz bir ad oluşturun veya kullanın [uniqueString()](resource-group-template-functions-string.md#uniquestring) bir ad oluşturmak için işlev. Bir ön ek ekleme veya için soneki isteyebilirsiniz **uniqueString** sonucu. Benzersiz bir ad değiştirerek daha fazla kaynak türü adı, kolayca belirlemenize yardımcı olabilir. Örneğin, aşağıdaki değişkeni kullanarak bir depolama hesabı için benzersiz bir ad oluşturabilirsiniz:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Tanımlama için kaynak adları
Adı, ancak adlarını isteyebileceğiniz bazı kaynak türleri, benzersiz olması gerekmez. Bu kaynak türleri için hem kaynak bağlamı hem de kaynak türünü tanımlayan bir ad sağlayabilirsiniz.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Genel kaynak adları
Çoğunlukla farklı bir kaynak erişim kaynak türleri için şablonda sabit kodlanmış bir genel ad kullanabilirsiniz. Örneğin, bir SQL Server'da Güvenlik duvarı kuralları için standart, genel bir ad ayarlayabilirsiniz:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Konum
Şablon dağıtırken, her kaynak için bir konum sağlamalısınız. Farklı kaynak türlerinin farklı konumlarda desteklenir. Aboneliğiniz belirli bir kaynak türü için kullanılabilir konumların bir listesini görmek için Azure PowerShell veya Azure CLI'yı kullanın. 

Aşağıdaki örnek PowerShell konumlarını almak için kullanır. `Microsoft.Web\sites` kaynak türü:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Aşağıdaki örnek, konumlarını almak için Azure CLI'yı kullanır. `Microsoft.Web\sites` kaynak türü:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Kaynaklarınız için desteklenen konumlar belirledikten sonra bu konum, şablonunuzda ayarlayın. Kaynak türlerini destekleyen bir konumda bir kaynak grubu oluşturun ve her konum ayarlamak bu değeri ayarlamak için en kolay yolu olan `[resourceGroup().location]`. Kaynak gruplarına farklı konumlarda şablonu yeniden ve herhangi bir değer şablon veya parametreleri değiştirin değil. 

Aşağıdaki örnekte kaynak grubu ile aynı konumda dağıtılan bir depolama hesabı gösterilmektedir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
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

Şablonunuzda konumu gömülmesi gerekiyorsa, desteklenen bölgelerden birine adını sağlayın. Aşağıdaki örnekte Kuzey Orta ABD için her zaman dağıtılan bir depolama hesabı gösterilmektedir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
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

## <a name="tags"></a>Etiketler
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Etiket şablonunuza ekleme

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Alt kaynakları

İçindeki bazı kaynak türleri, bir dizi alt kaynakları tanımlayabilirsiniz. Alt kaynakları yalnızca başka bir kaynak bağlamı içinde mevcut kaynaklardır. Örneğin, veritabanı sunucusunun bir alt, bu nedenle bir SQL veritabanı bir SQL server var olamaz. Veritabanı sunucusu için tanımı içinde tanımlayabilirsiniz.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

İç içe olduğunda tür kümesine `databases` ancak kendi tam kaynak türü `Microsoft.Sql/servers/databases`. Sağlaması gerekmez `Microsoft.Sql/servers/` üst kaynak türünden varsayıldığından. Alt kaynak adı kümesine `exampledatabase` ancak üst adı tam adını içerir. Sağlaması gerekmez `exampleserver` üst kaynak varsayıldığından.

Alt kaynak türünün biçimi şu şekildedir: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Alt kaynak adı biçimi şöyledir: `{parent-resource-name}/{child-resource-name}`

Ancak veritabanı sunucusu içinde tanımlamanız gerekmez. Alt kaynak en üst düzeyde tanımlayabilirsiniz. Üst kaynak aynı şablonun dağıttıysanız değil veya bu yaklaşımı kullanabilirsiniz kullanmak istediğiniz `copy` birden fazla alt kaynak oluşturmak için. Bu yaklaşımda, tam kaynak türü sağlayın ve üst kaynak adı alt kaynak adında gerekir.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Tam başvuru için bir kaynak oluşturulurken, yalnızca bir birleştirme iki tür ve ad kesimlerinden birleştirilecek sırası değildir. Bunun yerine, sonra ad alanı, bir dizi kullanın *türü/ad* en az belirli bir çiftlerinden en belirgin için:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Örneğin:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` doğru `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` doğru değil



## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu depolama hesaplarını veya birçok kaynak grupları arasında paylaşılan sanal ağlar ile çalışırken yaygın senaryodur. Daha fazla bilgi için [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).
* Kaynak adı kısıtlamaları hakkında daha fazla bilgi için bkz: [Azure kaynakları için önerilen adlandırma kuralları](../guidance/guidance-naming-conventions.md).