---
title: Azure Resource Manager şablonu kaynaklarını | Microsoft Docs
description: Bildirim temelli JSON sözdizimini kullanarak Azure Resource Manager şablonları kaynakları bölümünde açıklanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2017
ms.author: tomfitz
ms.openlocfilehash: 74830a5220a75408398af2224204f8195ab27cc6
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kaynakları bölümü

Kaynaklar bölümünde dağıtılan veya güncelleştirilen kaynakları tanımlayın. Sağ değerlerini sağlamak için dağıtıyorsanız türlerini anlamanız gerekir çünkü bu bölümde karmaşık elde edebilirsiniz.

## <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki Yapı kaynaklarını tanımlayın:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
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
| koşul | Hayır | Kaynak dağıtılabilir olup olmadığını gösteren Boole değeri. |
| apiVersion |Evet |Kaynak oluşturmak için kullanmak için REST API sürümü. |
| type |Evet |Kaynak türü. Bu değer, kaynak sağlayıcısı ve kaynak türü ad birleşimidir (gibi **Microsoft.Storage/storageAccounts**). |
| ad |Evet |Kaynağın adı. Ad RFC3986 içinde tanımlanan URI bileşeni kısıtlamaları izlemelidir. Ayrıca, emin olmak için adı dışında tarafların doğrulamak için kaynak adı kullanıma Azure Hizmetleri değil başka bir kimlik aldatma girişimi. |
| location |Değişir |Sağlanan kaynak coğrafi konumda desteklenmiyor. Kullanılabilir konumlardan herhangi birinden seçebilirsiniz, ancak genellikle kullanıcılarınızın yakın olan bir seçmek için mantıklıdır. Genellikle, bu da aynı bölgede birbiriyle etkileşimde kaynakları yerleştirin mantıklıdır. Çoğu kaynak türleri bir konum gerektirir, ancak bazı türleri (örneğin, bir rol ataması) bir konum gerektirmez. |
| etiketler |Hayır |Kaynakla ilişkilendirilmiş etiketler. Aboneliğinizi arasında kaynakları mantıksal olarak düzenlemek için etiketleri uygulayın. |
| Açıklamaları |Hayır |Şablonunuzda kaynaklar belgeleme için notları |
| kopyala |Hayır |Birden fazla örneği gerekirse oluşturmak için kaynak sayısı. Paralel varsayılan moddur. Tüm kullanmak istemiyorsanız, seri modu veya aynı anda dağıtmak amacıyla kaynaklarınızı belirtin. Daha fazla bilgi için bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md). |
| dependsOn |Hayır |Bu kaynak dağıtılmadan önce dağıtılmalıdır kaynaklar. Resource Manager kaynakları arasındaki bağımlılıkları değerlendirir ve doğru sırada dağıtır. Kaynakları birbirlerine bağımlı olmadıkları zaman bunların paralel olarak dağıtılır. Değer bir kaynağın virgülle ayrılmış bir liste olabilir adları veya kaynak benzersiz tanımlayıcıları. Yalnızca bu şablonda dağıtılan kaynakları listeler. Bu şablonda tanımlı değil kaynakları önceden var olmalıdır. Dağıtımınızı yavaş ve döngüsel bağımlılıklar oluşturma gibi gereksiz bağımlılıkları eklemekten kaçının. Bağımlılıklarını ayarlama hakkında yönergeler için bkz [Azure Resource Manager'da bağımlılıkları tanımlama](resource-group-define-dependencies.md). |
| properties |Hayır |Kaynak özgü yapılandırma ayarları. Özelliklerine ilişkin değerleri kaynak oluşturmak REST API işlemi için (PUT yöntemini) istek gövdesinde sağladığınız değerleri ile aynıdır. Ayrıca bir özelliği birden çok örneğini oluşturmak için bir kopya dizisi belirtebilirsiniz. |
| SKU | Hayır | Bazı kaynaklar dağıtmak için SKU tanımlayan değerleri izin verin. Örneğin, bir depolama hesabı için artıklık türünü belirtebilirsiniz. |
| türü | Hayır | Bazı kaynaklar dağıttığınız kaynak türünü tanımlayan bir değeri izin verir. Örneğin, Cosmos oluşturmak için DB türünü belirtebilirsiniz. |
| plan | Hayır | Bazı kaynaklar dağıtmayı planlıyorsunuz tanımlayan değerleri izin verin. Örneğin, bir sanal makine için Market görüntüsü belirtebilirsiniz. | 
| kaynak |Hayır |Tanımlanan kaynağına bağımlı alt kaynakları. Yalnızca üst kaynak şema tarafından izin verilen kaynak türleri sağlar. Tam olarak nitelenmiş tür alt kaynağının üst kaynak türü gibi içerir **Microsoft.Web/sites/extensions**. Üst Kaynak bağımlılığı kullanılmaz. Bu bağımlılık açıkça tanımlamanız gerekir. |

## <a name="resource-specific-values"></a>Kaynak özgü değerleri

**ApiVersion**, **türü**, ve **özellikleri** öğeleri her kaynak türü için farklıdır. **Sku**, **türü**, ve **planı** bazı kaynak türleri için kullanılabilir, ancak tüm öğeler. Bu özelliklerin değerlerini belirlemek için bkz: [şablon başvurusu](/azure/templates/).

## <a name="resource-names"></a>Kaynak adları
Genellikle, kaynak adları Kaynak Yöneticisi'nde üç türleri ile çalışır:

* Kaynak adları benzersiz olmalıdır.
* Kaynak adları, benzersiz olması gerekmez, ancak kaynak belirlemenize yardımcı olabilecek bir ad vermek seçin.
* Genel kaynak adları.

### <a name="unique-resource-names"></a>Benzersiz kaynak adları
Veri erişim uç noktası olan herhangi bir kaynak türü için bir benzersiz kaynak adı sağlamanız gerekir. Benzersiz bir ad gerektiren bazı yaygın kaynak türleri şunlardır:

* Azure depolama<sup>1</sup> 
* Azure Uygulama Hizmeti’nin Web Apps özelliği
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure Hdınsight

<sup>1</sup> depolama hesabı adları de küçük olmalıdır, 24 karakter veya daha az ve tire sahip değil.

Ayar adı, el ile benzersiz bir ad oluşturun veya kullanmak [uniqueString()](resource-group-template-functions-string.md#uniquestring) bir ad oluşturmak için işlev. Bir önek ekleyip için soneki isteyebilirsiniz **uniqueString** sonucu. Benzersiz bir ad değiştirerek daha fazla kolayca kaynak türü adı belirlemenize yardımcı olabilir. Örneğin, aşağıdaki değişkeni kullanarak bir depolama hesabı için benzersiz bir ad oluşturabilirsiniz:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Tanımlama için kaynak adları
Ad ancak adlarını isteyebilirsiniz bazı kaynak türleri benzersiz olması gerekmez. Bu kaynak türleri için hem kaynak bağlamı hem de kaynak türünü tanımlayan bir ad sağlayabilirsiniz.

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
Farklı bir kaynak çoğunlukla erişim kaynak türleri için şablonda sabit kodlanmış olan bir ad kullanabilirsiniz. Örneğin, bir SQL Server'da Güvenlik duvarı kuralları için standart, genel bir ad ayarlayabilirsiniz:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Konum
Bir şablon dağıtılırken, her kaynak için bir konum sağlamanız gerekir. Farklı kaynak türleri, farklı konumlarda desteklenir. Aboneliğiniz belirli bir kaynak türü için kullanılabilir konumların bir listesini görmek için Azure PowerShell veya Azure CLI kullanın. 

Aşağıdaki örnek PowerShell konumlarını almak için kullanır. `Microsoft.Web\sites` kaynak türü:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Aşağıdaki örnek konumlarını almak için Azure CLI 2.0 kullanan `Microsoft.Web\sites` kaynak türü:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Kaynaklarınız için desteklenen konumlardan belirlendikten sonra bu konuma şablonunuzda ayarlayın. Kaynak türlerini destekleyen bir konumda bir kaynak grubu oluşturun ve her konum ayarlamak bu değeri ayarlamak için en kolay yolu olan `[resourceGroup().location]`. Farklı konumlarda kaynak gruplarına şablonu yeniden ve şablon ya da parametre değerleri değiştirin değil. 

Aşağıdaki örnek, kaynak grubu olarak aynı konuma dağıtılan bir depolama hesabı gösterir:

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

Şablonunuzda konumu stillerinizin gerekiyorsa, desteklenen bölgelerinden adını sağlayın. Aşağıdaki örnek, Kuzey Orta ABD için her zaman dağıtılan bir depolama hesabı gösterir:

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

### <a name="add-tags-to-your-template"></a>Şablonunuz için etiketler ekleme

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Alt kaynakları

Bazı kaynak türleri içinde bir dizi alt kaynakları da tanımlayabilirsiniz. Alt kaynakları yalnızca başka bir kaynak bağlamı içinde mevcut kaynaklardır. Örneğin, veritabanı sunucusunun bir alt olacak şekilde bir SQL veritabanı bir SQL server var olamaz. Veritabanı sunucusu için tanımı içinde tanımlayabilirsiniz.

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

İç içe geçmiş zaman türü kümesine `databases` ancak kendi tam kaynak türü `Microsoft.Sql/servers/databases`. Sunmaz `Microsoft.Sql/servers/` üst kaynak türünden varsayıldığından. Alt kaynak adı ayarlamak `exampledatabase` ancak üst adı tam adını içerir. Sunmaz `exampleserver` üst kaynak varsayıldığından.

Alt öğe kaynak türünü biçimdedir: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Alt kaynak adının biçimi şöyledir: `{parent-resource-name}/{child-resource-name}`

Ancak, sunucu veritabanında tanımlamak zorunda değilsiniz. En üst düzeyinde alt kaynak tanımlayabilirsiniz. Üst kaynak şablonun aynı dağıtılmamışsa, ya da varsa bu yaklaşımı kullanabilir kullanmak istediğiniz `copy` birden çok alt kaynaklarını oluşturun. Bu yaklaşımda, tam kaynak türü sağlayın ve üst kaynak adı alt kaynak adında gerekir.

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

Bir kaynağa tam bir başvuru oluşturulurken, tür ve ad bölümlerinin birleştirmek için sipariş iki yalnızca bir birleşimini değil.  Bunun yerine, sonra ad alanı, bir dizi kullanın *türü/adı* en az belirli bir çiftlerinden en belirli:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Örneğin:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` doğru `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` doğru değil

## <a name="recommendations"></a>Öneriler
Kaynakları ile çalışırken aşağıdaki bilgiler yararlı olabilir:

* Diğer katkıda bulunanlar kaynak amacını anlamalarına yardımcı olmak için belirtin **açıklamaları** şablondaki her bir kaynak için:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Kullanırsanız, bir *ortak uç nokta* şablonunuzda (örneğin, bir Azure Blob Depolama ortak uç nokta), *olmayan sabit kodlu* ad alanı. Kullanım **başvuru** ad alanı dinamik olarak almak için işlevi. El ile şablon uç değiştirmeden farklı genel ad alanı ortamlar için şablonu dağıtmak için bu yaklaşımı kullanın. Şablonunuzda depolama hesabı için kullandığınız aynı sürüme API sürümü ayarlayın:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Depolama hesabı oluşturmakta olduğunuz şablonda dağıtılırsa, kaynak başvurduğunuzda sağlayıcı ad alanı belirtmek gerekmez. Aşağıdaki örnekte, Basitleştirilmiş sözdizimi gösterilmektedir:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Ortak ad alanını kullanmak için yapılandırılmış olan diğer değerleri şablonunuzda varsa, aynı yansıtmak üzere bu değerleri değiştirmek **başvuru** işlevi. Örneğin, ayarlayabileceğiniz **storageUri** sanal makine tanılama profili özelliği:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Ayrıca, farklı kaynak grubunda bulunan mevcut bir depolama hesabını başvurabilir:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Yalnızca bir uygulama gerektirdiğinde, bir sanal makineye genel IP adresleri atayın. Bir sanal makine (VM) hata ayıklama için ya da Yönetim veya yönetim amacıyla bağlanmak için gelen NAT kuralları, bir sanal ağ geçidi ya da bir jumpbox kullanın.
   
     Sanal makinelere bağlanma hakkında daha fazla bilgi için bkz:
   
   * [N katmanlı mimari Azure VM'ler çalıştırın](../guidance/guidance-compute-n-tier-vm.md)
   * [Azure Kaynak Yöneticisi'nde yer alan VM'ler için WinRM erişimini Ayarla](../virtual-machines/windows/winrm.md)
   * [Azure Portalı'nı kullanarak, VM'nin dış erişmesine izin vermek](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [PowerShell kullanarak, VM'nin dış erişmesine izin vermek](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Azure CLI kullanarak Linux VM dış erişmesine izin vermek](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* **DomainNameLabel** özelliği genel IP adresleri için benzersiz olmalıdır. **DomainNameLabel** değeri gerekir 3 ile 63 karakter uzunluğunda olmalıdır ve bu normal bir ifadeyle belirtilen kurallara uyar: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Çünkü **uniqueString** işlevi 13 karakter uzunluğunda bir dize oluşturur **dnsPrefixString** parametresi 50 karakterle sınırlıdır:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Bir özel betik uzantısı için bir parola eklediğinizde kullanmak **commandToExecute** özelliğinde **protectedSettings** özelliği:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > VM'ler ve uzantıları için parametre olarak geçirilen zaman gizli şifrelendiğinden emin olmak için kullanın **protectedSettings** özelliği ilgili uzantılar.
   > 
   > 


## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen bir şablonda işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Birden fazla şablon dağıtımı sırasında birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu senaryo, depolama hesapları veya birden çok kaynak grupları arasında paylaşılan sanal ağlar ile çalışırken yaygındır. Daha fazla bilgi için bkz: [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).
* Kaynak adı kısıtlamaları hakkında daha fazla bilgi için bkz: [Azure kaynakları için adlandırma kurallarını önerilen](../guidance/guidance-naming-conventions.md).