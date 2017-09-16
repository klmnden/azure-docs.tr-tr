---
title: "İlk Azure Resource Manager şablonunu oluşturma | Microsoft Docs"
description: "İlk Azure Resource Manager şablonunuzu oluşturmaya yönelik adım adım kılavuz. Şablon oluşturmak için bir depolama hesabına ait şablon başvurusunun nasıl kullanılacağını gösterir."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/03/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: d07b2354906994ef7842a64d9f58bcbcc18f96e7
ms.contentlocale: tr-tr
ms.lasthandoff: 09/06/2017

---

# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>İlk Azure Resource Manager şablonunuzu oluşturma ve dağıtma
Bu konu başlığında, ilk Azure Resource Manager şablonunuzu oluşturma adımları gösterilmektedir. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](resource-group-overview.md). Kaynaklarınız varsa ve bu kaynaklara yönelik bir şablon almak istiyorsanız bkz. [Mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma](resource-manager-export-template.md).

Şablonları oluşturup düzeltmek için bir JSON düzenleyicisi gerekir. [Visual Studio Code](https://code.visualstudio.com/) basit, açık kaynaklı ve platformlar arası bir kod düzenleyicisidir. Resource Manager şablonları oluşturmak için Visual Studio Code kullanılması önerilir. Bu makalede, VS Code kullandığınız varsayılmıştır. Başka bir JSON düzenleyiciniz (Visual Studio gibi) varsa kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio Code. Gerekirse, [https://code.visualstudio.com/](https://code.visualstudio.com/) adresinden yükleyin.
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-template"></a>Şablon oluşturma

Aboneliğinize bir depolama hesabı dağıtan basit bir şablonla başlayalım.

1. **Dosya** > **Yeni Dosya**’yı seçin. 

   ![Yeni dosya](./media/resource-manager-create-first-template/new-file.png)

2. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Depolama hesabı adlarını ayarlamayı zorlaştıran birkaç kısıtlama vardır. Ad 3 ila 24 karakter uzunluğunda olmalı, yalnızca sayı ile küçük harf içermeli ve benzersiz olmalıdır. Önceki şablonda bir karma değer oluşturmak için [uniqueString](resource-group-template-functions-string.md#uniquestring) kullanılmıştır. Bu karma değere daha fazla anlam katmak için *storage* ön eki getirilmiştir. 

3. Bu dosyayı yerel bir klasöre **azuredeploy.json** olarak kaydedin.

   ![Şablonu kaydetme](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a>Şablon dağıtma

Bu şablonu dağıtmaya hazırsınız. Bir kaynak grubu oluşturmak için PowerShell veya Azure CLI kullanın. Ardından, bu kaynak grubuna bir depolama hesabı dağıtın.

* PowerShell için, şablonu içeren klasörden aşağıdaki komutları kullanın:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Yerel bir Azure CLI yüklemesi için, şablonu içeren klasörden aşağıdaki komutları kullanın:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

Dağıtım tamamlandığında, depolama hesabınız kaynak grubunda mevcut olur.

## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

[Cloud Shell](../cloud-shell/overview.md)’i kullanarak, şablonunuzu dağıtmak için Azure CLI komutlarını çalıştırabilirsiniz. Ancak, ilk olarak şablonunuzu Cloud Shell dosya paylaşımına yüklemeniz gerekir. Daha önce Cloud Shell kullanmadıysanız, kurulumu hakkında bilgi için bkz. [Azure Cloud Shell’e Genel Bakış](../cloud-shell/overview.md).

1. [Azure Portal](https://portal.azure.com)’da oturum açın.   

2. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-create-first-template/select-storage.png)

4. **Dosyalar**’ı seçin.

   ![Dosya seçme](./media/resource-manager-create-first-template/select-files.png)

5. Cloud Shell için dosya paylaşımı seçin. Ad deseni `cs-<user>-<domain>-com-<uniqueGuid>` şeklindedir.

   ![Dosya paylaşımı seçme](./media/resource-manager-create-first-template/select-file-share.png)

6. **Dizin ekle**’yi seçin.

   ![Dizin ekleme](./media/resource-manager-create-first-template/select-add-directory.png)

7. **Şablonlar** olarak adlandırıp **Tamam**’ı seçin.

   ![Ad dizini](./media/resource-manager-create-first-template/name-templates.png)

8. Yeni dizininizi seçin.

   ![Dizin seçme](./media/resource-manager-create-first-template/select-templates.png)

9. **Karşıya Yükle**’yi seçin.

   ![Karşıya yükleme seçme](./media/resource-manager-create-first-template/select-upload.png)

10. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-create-first-template/upload-files.png)

11. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. Cloud Shell’e aşağıdaki komutları girin:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

Dağıtım tamamlandığında, depolama hesabınız kaynak grubunda mevcut olur.

## <a name="customize-the-template"></a>Şablonu özelleştirme

Şablon düzgün çalışır, ancak esnek değildir. Şablon ABD Orta Güney bölgesine her zaman yerel olarak yedekli depolama dağıtır. Ad her zaman *storage* ve ardından bir karma değer içerir. Şablonu farklı senaryolar için kullanmak üzere şablona parametreler ekleyin.

Aşağıdaki örnekte iki parametre ile birlikte parametreler bölümü gösterilmektedir. İlk `storageSKU` parametresi, yedeklilik türünü belirtmenize olanak sağlar. Bir depolama hesabı için geçerli olan değerlere geçirebileceğiniz değerleri sınırlar. Ayrıca, varsayılan bir değer belirtir. İkinci `storageNamePrefix` parametresi en çok 11 karaktere izin verecek şekilde ayarlanır. Varsayılan bir değer belirtir.

```json
"parameters": {
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
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

Değişkenler bölümünde `storageName` adlı bir değişken ekleyin. Bu değişken, parametrelerden bir ön ek değerini [uniqueString](resource-group-template-functions-string.md#uniquestring) işlevindeki bir karma değer ile birleştirir. Tüm karakterleri küçük harfe dönüştürmek için [toLower](resource-group-template-functions-string.md#tolower) işlevini kullanır.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Depolama hesabınız için bu yeni değerleri kullanmak üzere kaynak tanımını değiştirin:

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

Depolama hesabı adının eklediğiniz değişkene ayarlandığına dikkat edin. SKU adı, parametre değerine ayarlanır. Konum, kaynak grubu ile aynı konuma ayarlanır.

Dosyanızı kaydedin. 

Şablonunuz şimdi şuna benzer görünmelidir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
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
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Şablonu yeniden dağıtma

Şablonu farklı değerlerle yeniden dağıtın.

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Azure CLI için şunu kullanın:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

Cloud Shell için, değiştirdiğiniz şablonu dosya paylaşımına yükleyin. Var olan dosyanın üzerine yazın. Ardından, aşağıdaki komutu kullanın:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="use-autocomplete"></a>Otomatik Tamamlama kullanma

Şablondaki şimdiye kadarki çalışmalarınız bu makaleden JSON kodlarını kopyalamak ve yapıştırmaktan ibaretti. Ancak, kendi şablonlarınızı geliştirirken, kaynak türü için kullanılabilir olan değerleri ve özellikleri bulmanız ve belirtmeniz gerekir. VS Code, şemayı okuyup kaynak türünü bulur ve özellikler ve değerler önerir. Otomatik Tamamlama özelliğini görmek için şablonunuzun özellikler öğesine gidip ve yeni bir satır ekleyin. Tırnak işareti girdiğinizde VS Code hemen özellikler öğesi altında kullanılabilir adları önerir.

![Kullanılabilir özellikler göster](./media/resource-manager-create-first-template/show-properties.png)

**Şifreleme**’yi seçin. İki nokta üst üste (:) girdiğinizde VS Code yeni bir nesne eklemeyi önerir.

![Nesne ekle](./media/resource-manager-create-first-template/add-object.png)

Nesneyi eklemek için sekme veya enter tuşuna basın.

Tekrar bir tırnak işareti girdiğinizde VS Code, şifreleme için kullanılabilir olan özellikleri önerir.

![Şifreleme özelliklerini göster](./media/resource-manager-create-first-template/show-encryption-properties.png)

**Hizmetler**’i seçip VS Code uzantılarını temel alan değerleri eklemeye devam edin:

```json
"properties": {
    "encryption":{
        "services":{
            "blob":{
              "enabled":true
            }
        }
    }
}
```

Depolama hesabı için blob şifrelemeyi etkinleştirdiniz. Ancak, VS Code bir sorun belirledi. Bu şifrelemede bir uyarı olduğuna dikkat edin.

![Şifreleme uyarısı](./media/resource-manager-create-first-template/encryption-warning.png)

Uyarıyı görmek için imlecinizi yeşil çizginin üzerine getirin.

![Eksik özellik](./media/resource-manager-create-first-template/missing-property.png)

Şifreleme öğesinin bir keySource özelliği gerektirdiğini görürsünüz. Hizmetler nesnesinden sonra bir virgül ekleyin ve keySource özelliğini ekleyin. VS Code, geçerli bir değer olarak **"Microsoft.Storage"** önerisinde bulunur. Tamamlandığında, özellikler öğesi şöyle olur:

```json
"properties": {
    "encryption":{
        "services":{
            "blob":{
              "enabled":true
            }
        },
        "keySource":"Microsoft.Storage"
    }
}
```

Son şablon şöyledir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
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
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "encryption":{
          "services":{
            "blob":{
              "enabled":true
            }
          },
          "keySource":"Microsoft.Storage"
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="deploy-encrypted-storage"></a>Şifrelenmiş depolama dağıtma

Bir kez daha şablonu dağıtın ve yeni bir depolama hesabı adı girin.

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix storesecure
```

Azure CLI için şunu kullanın:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageNamePrefix=storesecure
```

Cloud Shell için, değiştirdiğiniz şablonu dosya paylaşımına yükleyin. Var olan dosyanın üzerine yazın. Ardından, aşağıdaki komutu kullanın:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageNamePrefix=storesecure
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

PowerShell için şunu kullanın:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Azure CLI için şunu kullanın:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Sonraki adımlar
* Şablon geliştirme ile ilgili daha fazla yardım için bir VS Code uzantısı yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonu oluşturmak için Visual Studio Code uzantısı kullanma](resource-manager-vscode-extension.md)
* Bir şablonun yapısı hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Bir depolama hesabının özellikleri hakkında bilgi edinmek için bkz. [depolama hesapları şablon başvurusu](/azure/templates/microsoft.storage/storageaccounts).
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).

