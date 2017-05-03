---
title: "İlk Azure Resource Manager şablonunu oluşturma | Microsoft Docs"
description: "İlk Azure Resource Manager şablonunuzu oluşturmaya yönelik adım adım kılavuz. Şablon oluşturmak için bir depolama hesabına ait şablon başvurusunun nasıl kullanılacağını gösterir."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/18/2017
ms.topic: get-started-article
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 3c5520f30b75c0e0a2b1aee890f79d01d325d543
ms.lasthandoff: 04/19/2017

---

# <a name="create-your-first-azure-resource-manager-template"></a>İlk Azure Resource Manager şablonunuzu oluşturma
Bu konu başlığında, ilk Azure Resource Manager şablonunuzu oluşturma adımları gösterilmektedir. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](resource-group-overview.md). Kaynaklarınız varsa ve bu kaynaklara yönelik bir şablon almak istiyorsanız bkz. [Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma](resource-manager-export-template.md).

Şablonları oluşturup düzeltmek için bir JSON düzenleyicisi gerekir. [Visual Studio Code](https://code.visualstudio.com/) basit, açık kaynaklı ve platformlar arası bir kod düzenleyicisidir. Resource Manager şablonlarını bir uzantı ile oluşturup düzenlemeyi destekler. Bu konu başlığı, VS Code kullandığınızı varsayar; ancak başka bir JSON düzenleyiciniz (Visual Studio gibi) varsa kullanabilirsiniz.

## <a name="get-vs-code-and-extension"></a>VS Code ve uzantıyı alma
1. Gerekirse, VS kodu [https://code.visualstudio.com/](https://code.visualstudio.com/) adresinden yükleyin.

2. [Azure Resource Manager Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) uzantısını yüklemek için Hızlı Aç (Ctrl+P) menüsüne erişip şu komutu çalıştırın: 

   ```
   ext install msazurermtools.azurerm-vscode-tools
   ```

3. Uzantının etkinleştirilmesi istendiğinde VS Code’u yeniden başlatın.

## <a name="create-blank-template"></a>Boş şablon oluşturma

İlk olarak, bir şablonun yalnızca temel bölümlerini içeren boş bir şablon oluşturalım.

1. Bir dosya oluşturun. 

2. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [  ],
     "outputs": {  }
   }
   ```

3. Bu dosyayı **azuredeploy.json** olarak kaydedin. 

## <a name="add-storage-account"></a>Depolama hesabı ekleme
1. Dağıtıma yönelik bir depolama hesabı tanımlamak için, ilgili depolama hesabını şablonunuzun **kaynaklar** bölümüne ekleyin. Depolama hesabı için kullanılabilen değerleri bulmak için [depolama hesapları şablon başvurusuna](/azure/templates/microsoft.storage/storageaccounts) bakın. Depolama hesabı için gösterilen JSON dosyasını kopyalayın. 

3. Aşağıdaki örnekte gösterildiği gibi, bu JSON dosyasını şablonunuzun **kaynaklar** bölümüne yapıştırın: 

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [
       {
         "name": "string",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-05-01",
         "sku": {
           "name": "string"
         },
         "kind": "string",
         "location": "string",
         "tags": {},
         "properties": {
           "customDomain": {
             "name": "string",
             "useSubDomain": boolean
           },
           "encryption": {
             "services": {
               "blob": {
                 "enabled": boolean
               }
             },
             "keySource": "Microsoft.Storage"
           },
           "accessTier": "string"
         }
       }
     ],
     "outputs": {  }
   }
   ```

  Önceki örnek çok sayıda yer tutucu değeri ve depolama hesabınızda gerekli olmayabilecek bazı özellikler içermektedir.

## <a name="set-values-for-storage-account"></a>Depolama hesabı değerlerini ayarlama

Artık depolama hesabınızın değerlerini ayarlamaya hazırsınız. 

1. JSON dosyasını kopyaladığınız [depolama hesapları şablon başvurusuna](/azure/templates/microsoft.storage/storageaccounts) tekrar bakın. Özellikleri açıklayan ve kullanılabilir değerleri sağlayan çok sayıda tablo mevcuttur. 

2. **Properties** öğesinde **customDomain**, **encryption** ve **accessTier** seçeneklerinin gerekli değil olarak listelendiğine dikkat edin. Bu değerler senaryolarınız için önemli olabilir, ancak bu örneği basit tutmak için kaldıralım.

   ```json
   "resources": [
     {
       "name": "string",
       "type": "Microsoft.Storage/storageAccounts",
       "apiVersion": "2016-05-01",
       "sku": {
         "name": "string"
       },
       "kind": "string",
       "location": "string",
       "tags": {},
       "properties": {
       }
     }
   ],
   ```

3. Şu anda, **kind** öğesi bir yer tutucu değerine ("dize") ayarlanmıştır. VS Code, şablonunuzda kullanılacak değerleri anlamanıza yardımcı olan çok sayıda özellik içerir. VS Code’un, bu değerin geçerli olmadığını gösterdiğine dikkat edin. Fare ile "dize" üzerine gelirseniz, VS Code **kind** için geçerli değerleri `Storage` veya `BlobStorage` olarak önerir. 

   ![VS Code tarafından önerilen değerleri göster](./media/resource-manager-create-first-template/vs-code-show-values.png)

   Kullanılabilir değerleri görmek için çift tırnak içindeki karakterleri silin ve **Ctrl+Ara Çubuğu** tuşlarını seçin. Kullanılabilir seçeneklerden **Depolama** öğesini seçin.
  
   ![IntelliSense’i göster](./media/resource-manager-create-first-template/intellisense.png)

   VS Code kullanmıyorsanız, depolama hesapları şablon başvurusu sayfasına bakın. Açıklamada aynı geçerli değerlerin listelendiğine dikkat edin. Öğeyi **Depolama**’ya ayarlayın.

   ```json
   "kind": "Storage",
   ```

Şablonunuz şimdi şuna benzer görünmelidir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-template-function"></a>Şablon işlevi ekleme

Şablonun söz dizimini basitleştirmek ve yalnızca şablon dağıtılırken kullanılabilen değerleri almak için, şablonunuzun içindeki işlevleri kullanırsınız. Tüm şablon işlevleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).

Depolama hesabının kaynak grubuyla aynı konuma dağıtıldığını belirtmek için **location** özelliğini şu şekilde ayarlayın:

```json
"location": "[resourceGroup().location]",
```

VS Code yine kullanılabilir işlevler önererek size yardımcı olur. 

![işlevleri göster](./media/resource-manager-create-first-template/show-functions.png)

İşlevin köşeli ayraç içine alındığına dikkat edin. [ResourceGroup](resource-group-template-functions.md#resourcegroup) işlevi, `location` adlı bir özellikle nesne döndürür. Kaynak grubu, çözümünüzle ilgili tüm kaynakları tutar. Location özelliğini "Orta ABD" gibi bir değere kodlayabilirsiniz, ancak farklı bir konuma yeniden dağıtmak için şablonu el ile değiştirmeniz gerekecektir. `resourceGroup` işlevinin kullanılması, bu şablonun farklı bir konumdaki farklı bir kaynak grubuna dağıtılmasını kolaylaştırır.

Şablonunuz şimdi şuna benzer görünmelidir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-parameters-and-variables"></a>Parametre ve değişken ekleme
Şablonunuzda ayarlanması gereken yalnızca iki değer kalmıştır: **name** ve **sku.name**. Bu özellikler için, dağıtım sırasında bu değerleri özelleştirmenize olanak tanıyan parametreleri ekleyin. 

Depolama hesabı adlarını ayarlamayı zorlaştıran birkaç kısıtlama vardır. Ad 3 ila 24 karakter uzunluğunda olmalı, yalnızca sayı ile küçük harf içermeli ve benzersiz olmalıdır. Kısıtlamalara uyan benzersiz bir değer tahmin etmeye çalışmak yerine, [uniqueString](resource-group-template-functions.md#uniquestring) işlevini kullanarak bir karma değer oluşturabilirsiniz. Bu karma değere daha fazla anlam katmak için, dağıtımdan sonra değeri depolama hesabı olarak tanımlamanıza yardımcı olacak bir ön ek ekleyin. 

1. Adlandırma kurallarınıza uyan bir ad ön eki geçirmek için şablonunuzun **parametreler** bölümüne gidin. Şablona, depolama hesabı adı için ön ek kabul eden bir parametre ekleyin:

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     }
   },
   ```

  `uniqueString` 13 karakter döndürdüğünden ve ad 24 karakterden uzun olamayacağından, ön ek en fazla 11 karakter ile sınırlıdır. Dağıtım sırasında parametre için bir değer geçirmezseniz varsayılan değer kullanılır.

2. Şablonun **değişkenler** bölümüne gidin. Ön ek ve benzersiz dizeden adı oluşturmak için aşağıdaki değişkeni ekleyin:

   ```json
   "variables": {
     "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
   },
   ```

3. **Kaynaklar** bölümünde depolama hesabı adını bu değişkene ayarlayın.

   ```json
   "name": "[variables('storageName')]",
   ```

3. Depolama hesabı için farklı SKU'ları geçirmeyi etkinleştirmek için **parametreler** bölümüne gidin. Depolama adı ön ekine ait parametreden sonra, izin verilen SKU değerlerini ve varsayılan değeri belirten bir parametre ekleyin. İzin verilen değerleri şablon başvurusu sayfasından veya VS Code’dan bulabilirsiniz. Aşağıdaki örnekte, SKU için tüm geçerli değerleri eklersiniz. Ancak, izin verilen değerleri yalnızca bu şablon aracılığıyla dağıtmak istediğiniz SKU'lar ile sınırlayabilirsiniz.

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     },
     "storageSKU": {
       "type": "string",
       "allowedValues": [
         "Standard_LRS",
         "Standard_ZRS",
         "Standard_GRS",
         "Standard_RAGRS",
         "Premium_LRS"
       ],
       "defaultValue": "Standard_LRS",
       "metadata": {
         "description": "The type of replication to use for the storage account."
       }
     }
   },
   ```

3. Parametrenin değerini kullanmak için SKU özelliğini değiştirin:

   ```json
   "sku": {
     "name": "[parameters('storageSKU')]"
   },
   ```    

4. Dosyanızı kaydedin.

## <a name="final-template"></a>Son şablon

Bu makaledeki adımları tamamladıktan sonra, artık şablonunuz şöyle görünür:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Şablonunuz tamamlanır ve şablonu aboneliğinize dağıtmaya hazır olursunuz. Dağıtmak için bkz. [Kaynakları Azure’a dağıtma](resource-manager-quickstart-deploy.md).
* Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).

