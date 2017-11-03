---
title: "En iyi uygulamalar Resource Manager şablonları oluşturmak için | Microsoft Docs"
description: "Azure Resource Manager şablonlarınızı basitleştirmeye yönelik yönergeler verilmiştir."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Azure Resource Manager şablonları oluşturmak için en iyi uygulamalar
Bu yönergeler, güvenilir ve kullanımı kolay Azure Resource Manager şablonları oluşturmanıza yardımcı olabilir. Yalnızca önerileri yönergelerdir. Bunlar belirtilenler dışında gereksinimleri değildir. Senaryonuz aşağıdaki yaklaşımlar ve örnekler birini çeşitlemesi gerektirebilir.

## <a name="resource-names"></a>Kaynak adları
Genellikle, kaynak adları Kaynak Yöneticisi'nde üç türleri ile çalışır:

* Kaynak adları benzersiz olmalıdır.
* Kaynak adları, benzersiz olması gerekmez, ancak bağlamına dayalı bir kaynak belirlemenize yardımcı olabilecek bir ad vermek seçin.
* Genel kaynak adları.

 Kaynak adı kısıtlamaları hakkında daha fazla bilgi için bkz: [Azure kaynakları için adlandırma kurallarını önerilen](../guidance/guidance-naming-conventions.md).

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

Bir parametre için bir kaynak adı sağlarsanız, kaynak dağıttığınızda benzersiz bir ad sağlamanız gerekir. İsteğe bağlı olarak kullanan bir değişken oluşturabilirsiniz [uniqueString()](resource-group-template-functions-string.md#uniquestring) bir ad oluşturmak için işlev. 

Bir önek ekleyip için soneki isteyebilirsiniz **uniqueString** sonucu. Benzersiz bir ad değiştirerek daha fazla kolayca kaynak türü adı belirlemenize yardımcı olabilir. Örneğin, aşağıdaki değişkeni kullanarak bir depolama hesabı için benzersiz bir ad oluşturabilirsiniz:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Tanımlama için kaynak adları
Ad ancak adlarını isteyebilirsiniz bazı kaynak türleri benzersiz olması gerekmez. Bu kaynak türleri için hem kaynak bağlamı hem de kaynak türünü tanımlayan bir ad sağlayabilirsiniz. Kaynak kaynakların bir listesinde tanımanıza yardımcı olacak açıklayıcı bir ad sağlayın. Farklı bir kaynak adı farklı dağıtımlar için kullanmanız gerekiyorsa, bir parametre adı için kullanabilirsiniz:

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

Dağıtım sırasında bir ad geçirmek gerekmiyorsa bir değişkeni kullanabilirsiniz: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Bir sabit kodlu değer de kullanabilirsiniz:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
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

## <a name="parameters"></a>Parametreler
Parametreler ile çalışırken aşağıdaki bilgiler yararlı olabilir:

* Parametreleri kullanımını en aza indirin. Mümkün olduğunda, bir değişkeni veya hazır değer kullanın. Parametreleri için yalnızca bu senaryoları kullanın:
   
   * Ortam (SKU, boyut, Kapasite) göre varyasyonları kullanmak istediğiniz ayarları.
   * Kolay bir şekilde tanımlanması için belirtmek istediğiniz kaynak adları.
   * (Örneğin, bir yönetici kullanıcı adı) diğer görevleri tamamlamak için sık kullandığınız değerleri.
   * Gizli (parolalar gibi).
   * Sayı veya değerleri dizisi, bir kaynak türü birden çok örneğini oluştururken kullanılacak.
* Ortası büyük parametre adları için kullanın.
* Her parametre meta verilerindeki bir açıklama belirtin:

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

* (Dışında parolalar ve SSH anahtarları) parametrelerinin varsayılan değerleri tanımlayın:
   
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

* Kullanım **SecureString** tüm parolalar ve gizli anahtarları için: 
   
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

* Mümkün olduğunda, bir parametre konumunu belirtmek için kullanmayın. Bunun yerine, kullanın **konumu** kaynak grubunun özelliği. Kullanarak **resourceGroup () .location** ifade tüm kaynaklarınızı şablondaki kaynaklarda için kaynak grubu ile aynı konumda dağıtılır:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Bir kaynak türü konumları, yalnızca sınırlı sayıda destekleniyorsa, doğrudan şablonunda geçerli bir konum belirtmek isteyebilirsiniz. Kullanmanız gerekiyorsa bir **konumu** parametresi mümkün olduğunca Bu parametre değeri aynı konumda büyük olasılıkla kaynakları paylaşabilirsiniz. Bu kullanıcılar Konum bilgileri vermeniz istenir sayısını en aza indirir.
* Bir kaynak türü için API sürümü için bir parametre veya değişken kullanmaktan kaçının. Kaynak özellikleri ve değerlerini sürüm numarasına göre farklılık gösterebilir. IntelliSense kod düzenleyicisinde API sürümü bir parametre veya değişken ayarlandığında, doğru şemayı belirleyemiyor. Bunun yerine, şablonda kod sabit API sürümü.

## <a name="variables"></a>Değişkenler
Değişkenleri ile çalışırken aşağıdaki bilgiler yararlı olabilir:

* Birden çok kez bir şablon kullanmak için gereken değerleri için değişkenlerini kullanın. Bir değer yalnızca bir kez kullandıysanız, sabit kodlanmış bir değer şablonunuzu okumak kolaylaştırır.
* Kullanamazsınız [başvuru](resource-group-template-functions-resource.md#reference) işlevi **değişkenleri** şablon bölümünü. **Başvuru** işlevi kaynağın çalışma zamanı durumunu değerinden türetilir. Ancak, ilk şablonunu Ayrıştırma sırasında çözümlenen değişkenlerdir. Yapı değerleri gereken **başvuru** doğrudan işlev **kaynakları** veya **çıkarır** şablon bölümünü.
* Bölümünde açıklandığı gibi benzersiz kaynak adları için değişkenleri dahil [kaynak adları](#resource-names).
* Değişkenleri karmaşık nesneleri gruplayabilirsiniz. Kullanım **variable.subentry** değeri karmaşık bir nesneden başvurmak için biçimi. Değişkenleri gruplandırma ilgili değişkenleri izlemenize yardımcı olur. Ayrıca, şablonu okunabilirliğini artırır. Örnek aşağıda verilmiştir:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Karmaşık bir nesne değeri karmaşık bir nesneden başvuran bir ifade içeremez. Bu amaç için ayrı bir değişken tanımlayın.
   > 
   > 
   
     Karmaşık nesne değişkenleri olarak kullanarak gelişmiş örnekler için bkz: [paylaşmak Azure Resource Manager şablonları durumda](best-practices-resource-manager-state.md).

## <a name="resources"></a>Kaynaklar
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

* Meta veri kaynakları eklemek için etiketler kullanın. Meta veri kaynaklarınızı hakkında bilgi eklemek için kullanın. Örneğin, bir kaynak için fatura ayrıntılarına kaydetmek için meta verileri ekleyebilirsiniz. Daha fazla bilgi için bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](resource-group-using-tags.md).
* Kullanırsanız, bir *ortak uç nokta* şablonunuzda (örneğin, bir Azure Blob Depolama ortak uç nokta), *olmayan sabit kodlu* ad alanı. Kullanım **başvuru** ad alanı dinamik olarak almak için işlevi. El ile şablon uç değiştirmeden farklı genel ad alanı ortamlar için şablonu dağıtmak için bu yaklaşımı kullanın. Şablonunuzda depolama hesabı için kullandığınız aynı sürüme API sürümü ayarlayın:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Depolama hesabı oluşturmakta olduğunuz şablonda dağıtılırsa, kaynak başvurduğunuzda sağlayıcı ad alanı belirtmek gerekmez. Bu, Basitleştirilmiş sözdizimi şöyledir:
   
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

## <a name="outputs"></a>Çıkışları
Genel IP adresleri oluşturmak için bir şablon kullanıyorsanız içeren bir **çıkarır** IP adresi ve tam etki alanı adı (FQDN) ayrıntılarını döndürür bölümü. Çıkış değerleri kolayca dağıtımdan sonra ortak IP adresleri ve FQDN'ler hakkındaki ayrıntıları almak için kullanabilirsiniz. Kaynağa başvuran oluşturmak için kullanılan API sürümü kullanın: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>İç içe geçmiş şablonları karşılaştırması tek şablonu
Çözümünüzü dağıtmak için birden çok iç içe geçmiş şablonları ile tek bir şablon ya da ana şablon kullanabilirsiniz. İç içe geçmiş şablonları daha Gelişmiş senaryolar için yaygındır. İç içe geçmiş bir şablon kullanarak, aşağıdaki avantajları sunar:

* Bir çözüm hedeflenen bileşenlere aşağı bozulabilir.
* Farklı ana şablonları ile iç içe geçmiş şablonlarını yeniden kullanabilirsiniz.

İç içe geçmiş şablonları kullanmayı seçerseniz, aşağıdaki yönergeleri şablon tasarımı standartlaştırmak yardımcı olabilir. Bu yönergelere dayalı [Azure Resource Manager şablonları tasarlamak için desenleri](best-practices-resource-manager-design-templates.md). Aşağıdaki şablonlardan sahip bir tasarım öneririz:

* **Ana şablon** (azuredeploy.json). Giriş parametreleri için kullanın.
* **Paylaşılan kaynaklar şablon**. Diğer tüm kaynaklar kullanan paylaşılan kaynakları dağıtmak için kullanın (örneğin, sanal ağ ve kullanılabilirlik kümeleri gibi). Kullanım **dependsOn** bu şablonu önce diğer şablonlar dağıtılmasını sağlamak için ifade.
* **İsteğe bağlı kaynakları şablon**. Bir parametre (örneğin, bir jumpbox) temel alarak kaynaklara koşullu dağıtmak için kullanın.
* **Üye kaynakları şablon**. Bir uygulama katmanı içindeki her örnek türü kendi yapılandırmasına sahip. Bir katman içinde farklı bir örnek türleri tanımlayabilirsiniz. (Örneğin, bir küme ilk örneği oluşturur ve ek örnekler varolan bir kümeye eklenir.) Her örneği türü kendi dağıtım şablonu yok.
* **Komut dosyaları**. Yaygın olarak yeniden kullanılabilir komut dosyaları her örneği türü (örneğin, başlatma ve biçimi ek diskleri) için geçerlidir. Belirli özelleştirme amaçla oluşturduğunuz özel komut dosyaları örneği türüne göre farklı olabilir.

![İç içe geçmiş şablonu](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Daha fazla bilgi için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-templates"></a>İç içe geçmiş şablonları koşullu bir Bağla
İç içe geçmiş şablonları koşullu bağlamak için bir parametresini kullanabilirsiniz. Parametre URI şablonu için bir parçası haline gelir:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Şablon biçimi
Şablonunuzu JSON Doğrulayıcı aracılığıyla geçirmek için iyi bir uygulamadır. Bir doğrulayıcı yabancı virgül, parantez ve dağıtım sırasında bir hata neden olabilir parantezler kaldırmanıza yardımcı olabilir. Deneyin [JSONLint](http://jsonlint.com/) veya sık kullanılan için linter paketi düzenleme ortamı (Visual Studio Code, Atom, Sublime metin, Visual Studio).

JSON okunabilirliği biçimlendirmek için de iyi bir fikirdir. Bir JSON biçimlendirici paketi için yerel, Düzenleyicisi kullanabilirsiniz. Belgeyi biçimlendirmek için Visual Studio'da basın **Ctrl + K, Ctrl + D**. Visual Studio Code basın **Alt + üst karakter + F**. Yerel düzenleyicinizi belge biçimlendirmez, kullanabileceğiniz bir [çevrimiçi biçimlendirici](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Sonraki adımlar
* Sanal makineler için çözüm mimarisi oluşturma ile ilgili yönergeler için bkz: [bir Windows VM Azure'da çalışan](../guidance/guidance-compute-single-vm.md) ve [Azure'da bir Linux VM çalışması](../guidance/guidance-compute-single-vm-linux.md).
* Bir depolama hesabı ayarlama hakkında yönergeler için bkz [Azure Storage performans ve ölçeklenebilirlik Yapılacaklar listesi](../storage/common/storage-performance-checklist.md).
* Bir kuruluş Resource Manager etkili bir şekilde Aboneliklerini yönetmek için nasıl kullanabileceğinizi hakkında bilgi edinmek için [Azure enterprise iskele: Düzenleyici abonelik idare](resource-manager-subscription-governance.md).

