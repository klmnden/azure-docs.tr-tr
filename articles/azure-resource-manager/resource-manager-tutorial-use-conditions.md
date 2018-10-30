---
title: Azure Resource Manager şablonlarında koşulları kullanma | Microsoft Docs
description: Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin.
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
ms.openlocfilehash: 552b39c520396942fa81f963c0cfa1c8c7b47db4
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456975"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarında koşulları kullanma

Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin. 

Bu öğreticide kullanılan senaryo, [Öğretici: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md) ile benzerdir. O öğreticide bir sanal makine, bir sanal ağ ve depolama hesabı dahil olmak üzere ek birkaç bağımlı kaynak oluşturmuştunuz. Her defasında yeni depolama hesabı oluşturmak yerine kullanıcıların yeni depolama hesabı oluşturma veya var olan depolama hesabını kullanma arasında seçim yapmasını sağlayacaksınız. Bu hedefe ulaşmak için ek bir parametre tanımlamanız gerekir. Parametrenin değeri "new" olduğunda yeni bir depolama hesabı oluşturulur.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu değiştirme
> * Şablonu dağıtma
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="modify-the-template"></a>Şablonu değiştirme

Var olan şablonda iki değişiklik yapın:

* Depolama hesabı ad parametresi ekleyin. Kullanıcılar ya yeni bir depolama hesabı adını ya da var olan bir depolama hesabı adını belirleyebilir.
* **newOrExisting** adlı yeni bir parametre ekleyin. Dağıtım bu parametreyi kullanarak yeni depolama hesabı oluşturma veya var olan depolama hesabını kullanma kararını verir.

1. **azuredeploy.json** dosyasını Visual Studio Code ile açın.
2. Şablon genelinde **variables('storageAccountName')** girişlerini **parameters('storageAccountName')** olarak değiştirin.  **variables('storageAccountName')** üç yerde geçmektedir.
3. Aşağıdaki değişken tanımını kaldırın:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Şablona aşağıdaki iki parametreyi ekleyin:

    ```json
    "storageAccountName": {
      "type": "string"
    },    
    "newOrExisting": {
      "type": "string", 
      "allowedValues": [
        "new", 
        "existing"
      ]
    },
    ```
    Güncelleştirilmiş parametre tanımı şu şekilde görünür:

    ![Resource Manager kullanım koşulu](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Depolama hesabı tanımının başına aşağıdaki satırı ekleyin.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    ```

    Koşul, **newOrExisting** adlı parametrenin değerini denetler. Parametre değeri **new** ise dağıtım, depolama hesabını oluşturur.

    Güncelleştirilmiş depolama hesabı tanımı şu şekilde görünür:

    ![Resource Manager kullanım koşulu](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)
6. **storageUri**’yi aşağıdaki değerle değiştirin:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Farklı bir kaynak grubu altındaki var olan bir depolama hesabını kullandığınızda bu değişiklik gereklidir.

7. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümündeki yönergeleri izleyerek şablonu dağıtın.

Şablonu Azure PowerShell kullanarak dağıttığınızda belirtmeniz gereken bir parametre daha vardır:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password"
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
$vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
New-AzureRmResourceGroupDeployment -Name mydeployment1018 -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin -adminPassword $vmPW `
    -dnsLabelPrefix $dnsLabelPrefix -storageAccountName $storageAccountName -newOrExisting $newOrExisting `
    -TemplateFile azuredeploy.json
```

> [!NOTE]
> **newOrExisting** değeri **new** olduğunda ancak belirtilen depolama hesabı adına sahip olan bir depolama hesabı mevcut olduğunda dağıtım başarısız olur.

**newOrExisting** için "existing" değerini ileterek ve var olan bir depolama hesabını belirterek başka bir dağıtım yapmayı deneyin. Depolama hesabını önceden oluşturmak için bkz. [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanıcılara yeni depolama hesabı oluşturma ve var olan depolama hesabını kullanma arasında seçim yapma imkanı sunan bir şablon dağıttınız. Bu öğreticide oluşturulan sanal makine için bir yönetici kullanıcı adı ve parola kullanılması gerekmektedir. Parolayı dağıtım sırasında geçirmek yerine Azure Key Vault ile önceden depolayabilir ve dağıtım sırasında oradan alabilirsiniz. Azure Key Vault'tan gizli dizi alma ve gizli dizileri şablon dağıtımı sırasında kullanma hakkında bilgi için bkz.:

> [!div class="nextstepaction"]
> [Key Vault'u şablon dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md)
