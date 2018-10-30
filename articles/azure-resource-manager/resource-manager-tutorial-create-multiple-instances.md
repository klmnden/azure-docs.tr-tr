---
title: Azure Resource Manager kullanarak birden çok kaynak örneği oluşturma | Microsoft Docs
description: Birden çok Azure kaynağı örneği oluşturmak için bir Azure Resource Manager şablonu oluşturmayı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/18/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 1cfccdf644b1748a96f7638e574c66eace8d113a
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456669"
---
# <a name="tutorial-create-multiple-resource-instances-using-resource-manager-templates"></a>Öğretici: Resource Manager şablonları kullanarak birden çok kaynak örneği oluşturma

Bir Azure Kaynağı’nın birden çok örneğini oluşturmak için Azure Resource Manager şablonunuzda yineleme yapmayı öğrenin. Son öğreticide, şifreli bir Azure Depolama hesabı oluşturmak için var olan bir şablonu değiştirdiniz. Bu öğreticide, aynı şablonu üç depolama hesabı örneği oluşturmak için değiştirin.

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı yerel bilgisayarınıza **azuredeploy.json** olarak kaydetmek için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Bu öğreticinin amacı, üç depolama hesabı oluşturmak için kaynak yinelemeyi kullanmaktır.  Örnek şablon, yalnızca temel şifrelenmemiş depolama hesabı oluşturur. 

Visual Studio Code’dan aşağıdaki dört değişikliği yapın:

![Azure Resource Manager birden çok örnek oluşturma](./media/resource-manager-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Depolama hesabı kaynak tanımına bir `copy` öğesi ekleyin. Kopyalama öğesinde, bu döngü için yineleme sayısı ve bir ad belirtin. Sayı değeri pozitif bir tamsayı olmalıdır ve 800’ü aşamaz.
2. `copyIndex()` İşlevi, döngüde geçerli yinelemeyi döndürür. `copyIndex()` sıfır tabanlıdır. Dizin değerini kaydırmak için copyIndex () işlevine bir değer geçirebilirsiniz. Örneğin, *copyIndex(1)*.
3. Artık kullanılmadığı için **değişkenler** öğesini silin.
4. **Çıktılar** öğesini silin.

Tamamlanan şablon aşağıdaki gibi görünür:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Birden çok örnek oluşturma hakkında daha fazla bilgi için, bkz. [Azure Resource Manager Şablonları’nda bir kaynağın veya özelliğin birden çok örneğini dağıtma](./resource-group-create-multiple.md)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için Visual Studio Code hızlı başlangıçta [Şablonu dağıtma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın.

Üç depolama hesabının tümünü listelemek için --ad parametresini atlayın:

# <a name="clitabcli"></a>[CLI](#tab/CLI)
```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az storage account list --resource-group $resourceGroupName
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName
```

---

Depolama hesabı adlarını, şablondaki ad tanımıyla karşılaştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, birden çok depolama hesabı örneği oluşturmayı öğrendiniz. Şu ana kadar bir depolama hesabı veya birden çok depolama hesabı örneği oluşturdunuz. Bir sonraki öğreticide, birden fazla kaynağa ve birden çok kaynak türüne sahip bir şablon geliştireceksiniz. Bazı kaynakların bağımlı kaynakları vardır.

> [!div class="nextstepaction"]
> [Bağımlı kaynaklar oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
