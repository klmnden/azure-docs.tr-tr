---
title: Azure Resource Manager şablonları için en iyi uygulamalar
description: Azure Resource Manager şablonları yazma için önerilen yaklaşımlara açıklar. Şablonları kullanırken sık karşılaşılan sorunları önlemek için öneriler sunar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: tomfitz
ms.openlocfilehash: bcc529b02505359e6e4e320d4991a082797c5261
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60389585"
---
# <a name="azure-resource-manager-template-best-practices"></a>Azure Resource Manager şablonu en iyi uygulamaları

Bu makalede, Resource Manager şablonunun nasıl oluşturulacağını hakkında öneriler sağlar. Bu önerileri bir çözümü dağıtmak için şablon kullanırken sık karşılaşılan sorunları önlemenize yardımcı olur.

Azure aboneliklerinizi yönetmek nasıl hakkında daha fazla öneri için bkz. [Azure Kurumsal iskelesi: Öngörücü abonelik İdaresi](/azure/architecture/cloud-adoption/appendix/azure-scaffold?toc=%2Fen-us%2Fazure%2Fazure-resource-manager%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json).

Tüm Azure bulut ortamında çalışan şablonları oluşturma hakkında daha fazla öneri için bkz. [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).

## <a name="template-limits"></a>Şablon sınırları

Şablonunuza 1 MB ve her 64 KB parametre dosyası boyutunu sınırlama. Yinelemeli Kaynak tanımları ve değişkenler ve parametreler için değerleri genişletildi sonra şablonunun son duruma 1 MB'lık sınırı uygular. 

İçin sınırlama getirilir:

* 256 parametreleri
* 256 değişkenleri
* 800 kaynakları (kopya sayısı da dahil olmak üzere)
* 64 çıkış değerleri
* bir şablon ifadesi 24.576 karakter

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırlar aşabilir. Daha fazla bilgi için [Azure kaynakları dağıtılırken bağlı şablonları kullanma](resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkış sayısını azaltmak için değerlerden bir nesnesi olarak birleştirebilirsiniz. Daha fazla bilgi için [parametre olarak nesnelerin](resource-manager-objects-as-parameters.md).

## <a name="resource-group"></a>Kaynak grubu

Kaynakları bir kaynak grubuna dağıttığınızda, kaynak grubu, kaynaklarla ilgili meta verileri depolar. Meta veriler, kaynak grubunun konumda depolanır.

Kaynak grubunun bölge geçici olarak kullanılamıyorsa, kaynak grubundaki kaynaklar meta veriler kullanılamaz durumda olduğundan güncelleştirilemiyor. Diğer bölgelerdeki kaynaklara beklendiği gibi çalışmaya devam eder ancak bunları güncelleştirilemiyor. Riski en aza indirmek için aynı bölgede kaynak grubu ve kaynakları bulun.

## <a name="parameters"></a>Parametreler
Bu bölümdeki bilgiler ile çalışırken yararlı olabilir [parametreleri](resource-group-authoring-templates.md#parameters).

### <a name="general-recommendations-for-parameters"></a>Parametreler için genel öneriler

* Parametreleri kullanımını en aza indirin. Bunun yerine, dağıtım sırasında belirtilmesi gerekmez özellikleri için değişkenleri veya değişmez değerler kullanın.

* Parametre adları için ortası büyük harf kullanın.

* SKU, boyut veya kapasitesi gibi ortam göre farklılık gösteren ayarları için parametreleri kullanın.

* Kolay bir şekilde tanımlanması için belirtmek istediğiniz kaynak adları için parametreleri kullanın.

* Her parametre meta verilerinde açıklamasını girin:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Hassas olmayan parametrelerinin varsayılan değerleri tanımlayın. Varsayılan bir değer belirterek, şablonu dağıtmak daha kolay ve uygun bir değer örneği şablonunuzu kullananların bakın. Herhangi bir parametre için varsayılan değer, varsayılan dağıtım yapılandırması içindeki tüm kullanıcılar için geçerli olmalıdır. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* İsteğe bağlı bir parametreyi belirtmek için boş bir dize, varsayılan değer olarak kullanmayın. Bunun yerine, değişmez değer veya bir dil ifadesi bir değer oluşturmak için kullanın.

   ```json
   "storageAccountName": {
     "type": "string",
     "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
     "metadata": {
       "description": "Name of the storage account"
     }
   },
   ```

* Parametre bir kaynak türü için API sürümü için kullanmayın. Kaynak özelliklerini ve değerlerini sürüm numarasına göre farklılık gösterebilir. API sürümü için bir parametre olarak ayarlandığında kod düzenleyicisindeki IntelliSense doğru şemayı belirlenemiyor. Bunun yerine, şablonda kod sabit API sürümü.

* Kullanım `allowedValues` gerektiğinde. Yalnızca bazı değerler izin verilen seçenekler yer almayan emin olmalısınız, bunu kullanın. Kullanırsanız `allowedValues` listenizin güncel tutarak değil geçerli dağıtımları çok geniş çapta engelleyebilir.

* Şablonunuzda bir parametre adı bir PowerShell dağıtım komut parametresi eşleştiğinde, Resource Manager bu adlandırma çakışması sonek ekleyerek çözümler **FromTemplate** şablon parametresi için. Örneğin, adında bir parametre eklerseniz **ResourceGroupName** ile çakışıyor, şablonunuzda **ResourceGroupName** parametresinde [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet'i. Dağıtım sırasında için bir değer sağlamanız istenir **ResourceGroupNameFromTemplate**.

### <a name="security-recommendations-for-parameters"></a>Parametreler için güvenlik önerileri

* Her zaman kullanıcı adları ve parolaları (veya gizli Diziler) parametrelerini kullanın.

* Kullanım `securestring` tüm parolalar ve gizli dizileri. Bir JSON nesnesi, hassas verileri geçirmeniz kullanırsanız `secureObject` türü. Şablon parametreleri güvenli dize veya güvenli nesne türleri ile kaynak dağıtımdan sonra okunamıyor. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Kullanıcı adları, parolalar veya gerektiren herhangi bir değer için varsayılan değerleri sağlamayan bir `secureString` türü.

* Varsayılan değerleri uygulamanın saldırı yüzey alanını artırmak için özellikleri sağlaması gerekmez.

### <a name="location-recommendations-for-parameters"></a>Parametreler için konum önerileri

* Kaynaklar için konumu belirtmek için bir parametre kullanın ve varsayılan değer ayarlamak `resourceGroup().location`. Bir konum parametresi sağlayarak, şablonu dağıtmak için izne sahip oldukları bir konum belirtmek kullanıcıları sağlar.

   ```json
   "parameters": {
     "location": {
       "type": "string",
       "defaultValue": "[resourceGroup().location]",
       "metadata": {
         "description": "The location in which the resources should be deployed."
       }
     }
   },
   ```

* Belirtmeyin `allowedValues` konum parametresi için. Belirttiğiniz konumlara tüm bulutlara kullanılamayabilir.

* Aynı konumda olma olasılığı olan kaynaklar için konum parametresi değeri kullanın. Bu yaklaşım kullanıcılar Konum bilgileri vermeniz istenir sayısını en aza indirir.

* Tüm konumlarda kullanılabilir olmayan kaynaklar için ayrı bir parametre kullanın veya bir sabit değer konum değeri belirtin.

## <a name="variables"></a>Değişkenler

İle çalışırken aşağıdaki bilgiler yararlı olabilir [değişkenleri](resource-group-authoring-templates.md#variables):

* Bir şablonda birden çok kez kullanmanız gereken değerler için değişkenleri kullanın. Değer yalnızca bir kez kullanılırsa, bir sabit kodlu değer şablonunuzu okuma kolaylaştırır.

* Şablon işlevlerinin karmaşık bir düzenlemeyi oluşturmak değerler için değişkenleri kullanın. Şablonunuzu karmaşık ifade değişkenleri yalnızca görüntülendiğinde okumak kolaydır.

* Değişkenleri kullanmayın `apiVersion` kaynak. API sürümü kaynak şemasını belirler. Genellikle, kaynağın özelliklerini değiştirme olmadan sürüm değiştiremezsiniz.

* Kullanamazsınız [başvuru](resource-group-template-functions-resource.md#reference) işlevi **değişkenleri** şablon bölümü. **Başvuru** işlevi kaynağın çalışma zamanı durumu değerinden türetilir. Ancak, değişkenleri şablon ilk Ayrıştırma sırasında çözümlenir. Yapı gereken değerleri **başvuru** doğrudan işlev **kaynakları** veya **çıkarır** şablon bölümü.

* Benzersiz kaynak adları için değişkenleri içerir.

* Kullanım bir [kopyalama döngüsü değişkenlerine](resource-group-create-multiple.md#variable-iteration) yinelenen desen JSON nesneleri oluşturmak için.

* Kullanılmayan değişkenler kaldırın.

## <a name="resource-dependencies"></a>Kaynak bağımlılıkları

Ne verirken [bağımlılıkları](resource-group-define-dependencies.md) ayarlamak için aşağıdaki yönergeleri kullanın:

* Kullanım **başvuru** işlevi ve bir özellik paylaşmak için gereken kaynaklar arasında örtük bir bağımlılık ayarlamak için kaynak adı geçirin. Açık bir ekleme `dependsOn` dolaylı bir bağımlılığı zaten tanımlamış olduğunuz zaman öğesi. Bu yaklaşım, gereksiz bağımlılıkları yaşama riskini azaltır.

* Alt kaynak kendi üst kaynağına bağımlı olarak ayarlayın.

* Kaynaklarla [koşul öğesi](resource-group-authoring-templates.md#condition) false olarak ayarlanırsa bağımlılık siparişi otomatik olarak kaldırılır. Bağımlılıkları kaynağı her zaman dağıtılırsa olarak ayarlayın.

* Açıkça ayarlamadan basamaklı bağımlılıkları sağlar. Örneğin, sanal makinenize bir sanal ağ arabiriminde bağlıdır ve sanal ağ arabirimi bir sanal ağ ve genel IP adresleri bağlıdır. Bu nedenle, dağıtılan tüm üç kaynak sanal makine olduğu halde tüm üç kaynaklara bağlı olarak sanal makine açıkça ayarlamanız gerekmez. Bu yaklaşım, bağımlılık sırası açıklar ve daha sonra şablonu değiştirmek kolaylaştırır.

* Dağıtımdan önce bir değer belirlenemezse olmayan kaynak dağıtma deneyin. Örneğin, bir yapılandırma değeri başka bir kaynak adı gerekiyorsa, bir bağımlılık gerekmeyebilir. Bu kılavuz, bazı kaynaklar diğer kaynak varlığını doğrulamak için her zaman çalışmaz. Bir hata alırsanız, bağımlılık ekleme.

## <a name="resources"></a>Kaynaklar

İle çalışırken aşağıdaki bilgiler yararlı olabilir [kaynakları](resource-group-authoring-templates.md#resources):

* Diğer Katkı Sağlayanlar kaynak amacını anlamalarına yardımcı olmak için belirtin **açıklamaları** şablondaki her bir kaynak için:
   
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

* Kullanıyorsanız bir *genel uç nokta* (örneğin, bir Azure Blob Depolama genel uç nokta), şablonunuzda *sabit kodlu olmayan* ad alanı. Kullanım **başvuru** ad alanını dinamik olarak almak için işlevi. Şablonu farklı bir genel ad alanı ortamlar için el ile uç nokta şablondaki değiştirmeden dağıtmak için bu yaklaşımı kullanabilirsiniz. API sürümü, şablonunuzda depolama hesabı için kullandığınız aynı sürüme ayarlayın:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Depolama hesabı aynı şablonda oluşturmakta olduğunuz dağıtılırsa, kaynağa başvuran sağlayıcı ad alanı belirtmeniz gerekmez. Aşağıdaki örnek, Basitleştirilmiş sözdizimi gösterilmektedir:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Genel bir ad alanını kullanacak şekilde yapılandırılmış olan diğer değerleri şablonunuzdaki varsa, aynı yansıtacak şekilde bu değerleri değiştirmek **başvuru** işlevi. Örneğin, ayarlayabilirsiniz **storageUri** sanal makine tanılama profili özelliği:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Ayrıca, farklı bir kaynak grubunda olan mevcut bir depolama hesabını başvurabilirsiniz:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Yalnızca bir uygulama gerektirdiğinde, sanal makinenin genel IP adresleri atayın. Bir sanal makine (VM) hata ayıklama veya yönetim veya yönetim amaçları için bağlanmak için gelen NAT kuralları, bir sanal ağ geçidi veya bir Sıçrama kutusu kullanın.
   
     Sanal makinelere bağlanma hakkında daha fazla bilgi için bkz:
   
   * [Azure'da N katmanlı mimari için Vm'leri çalıştırma](../guidance/guidance-compute-n-tier-vm.md)
   * [Azure Resource Manager'daki VM'ler için WinRM erişimi ayarlama](../virtual-machines/windows/winrm.md)
   * [Azure portalını kullanarak, bir VM'ye dış erişim izni ver](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [PowerShell kullanarak, bir VM'ye dış erişim izni ver](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Azure CLI kullanarak Linux vm'nize dış erişim verme](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)

* **Etkialanıadetiketi** özelliği genel IP adresleri için benzersiz olmalıdır. **Etkialanıadetiketi** değeri gerekir 3 ile 63 karakter uzunluğunda olmalıdır ve bu normal bir ifadeyle belirtilen kurallara uyar: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Çünkü **uniqueString** işlevi 13 karakter uzunluğunda bir dize oluşturur **dnsPrefixString** parametresi 50 karakterle sınırlıdır:

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

* Bir özel betik uzantısı için bir parola eklediğinizde, kullanın **commandToExecute** özelliğinde **protectedSettings** özelliği:
   
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
   > Vm'leri ve uzantıları için parametre olarak geçirilir, gizli dizileri şifrelendiğinden emin olmak için kullanın **protectedSettings** ilgili uzantı özelliği.
   > 
   > 

## <a name="outputs"></a>Çıkışlar

Genel IP adresleri oluşturmak için bir şablon kullanırsanız, içeren bir [çıkarır bölüm](resource-group-authoring-templates.md#outputs) IP adresi ve tam etki alanı adı (FQDN) ayrıntılarını döndürür. Çıkış değerleri, bir kolayca dağıtımdan sonra genel IP adresleri ve FQDN'ler hakkında ayrıntıları almak için kullanabilirsiniz.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager şablonu dosya yapısı hakkında daha fazla bilgi için bkz: [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
* Tüm Azure bulut ortamında çalışan şablonları oluşturma hakkında daha fazla öneri için bkz. [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).
