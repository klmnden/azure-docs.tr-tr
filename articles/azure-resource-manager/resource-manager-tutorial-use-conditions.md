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
ms.date: 05/21/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6a03707246f27bcba9cc46168ec04893b7bbc4c3
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990776"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarını koşul kullanın

Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin.

[Kaynak dağıtım sırasını ayarlama](./resource-manager-tutorial-create-templates-with-dependent-resources.md) öğreticisinde bir sanal makine, bir sanal ağ ve bir depolama hesabı dahil olmak üzere ek birkaç bağımlı kaynak oluşturmuştunuz. Her defasında yeni depolama hesabı oluşturmak yerine kullanıcıların yeni depolama hesabı oluşturma veya var olan depolama hesabını kullanma arasında seçim yapmasını sağlayacaksınız. Bu hedefe ulaşmak için ek bir parametre tanımlamanız gerekir. Parametrenin değeri "new" olduğunda yeni bir depolama hesabı oluşturulur. Aksi takdirde, sağlanan ada sahip mevcut bir depolama hesabı kullanılır.

![Resource Manager şablonu kullanım durumu diyagramı](./media/resource-manager-tutorial-use-conditions/resource-manager-template-use-condition-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu değiştirme
> * Şablonu dağıtma
> * Kaynakları temizleme

Bu öğretici yalnızca koşullarını kullanarak temel bir senaryoyu kapsar. Daha fazla bilgi için bkz.

* [Şablon dosya yapısı: Koşul](./resource-group-authoring-templates.md#condition).
* [Koşullu olarak kaynak içinde bir Azure Resource Manager şablonu dağıtma](/azure/architecture/building-blocks/extending-templates/conditional-deploy.md).
* [Şablon işlevi: Varsa](./resource-group-template-functions-logical.md#if).
* [Karşılaştırma işlevleri için Azure Resource Manager şablonları](./resource-group-template-functions-comparison.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites) içeren [Visual Studio Code](https://code.visualstudio.com/).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Öğreticisi: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Şablonun tanımladığı beş kaynak vardır:

   * `Microsoft.Storage/storageAccounts`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
5. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="modify-the-template"></a>Şablonu değiştirme

Var olan şablonda iki değişiklik yapın:

* Depolama hesabı ad parametresi ekleyin. Kullanıcılar ya yeni bir depolama hesabı adını ya da var olan bir depolama hesabı adını belirleyebilir.
* **newOrExisting** adlı yeni bir parametre ekleyin. Dağıtım bu parametreyi kullanarak yeni depolama hesabı oluşturma veya var olan depolama hesabını kullanma kararını verir.

Değişiklik yapmak için aşağıdaki adımları izleyin:

1. **azuredeploy.json** dosyasını Visual Studio Code ile açın.
2. Üç değiştirin **variables('storageAccountName')** ile **parameters('storageAccountName')** tüm şablondaki.
3. Aşağıdaki değişken tanımını kaldırın:

    ![Resource Manager şablonu kullanım durumu diyagramı](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-remove-storageaccountname.png)

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
6. Güncelleştirme **storageUri** özelliğinin şu değerle birlikte sanal makine kaynak tanımı:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Farklı bir kaynak grubu altındaki var olan bir depolama hesabını kullandığınızda bu değişiklik gereklidir.

7. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bölümündeki yönergeleri [şablonu dağıtmak](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) Cloud Shell'i açmak ve düzeltilmiş şablonunu karşıya yükleyin ve ardından izleme şablonu dağıtmak için PowerShell betiğini çalıştırın.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin `
    -adminPassword $vmPassword `
    -dnsLabelPrefix $dnsLabelPrefix `
    -storageAccountName $storageAccountName `
    -newOrExisting $newOrExisting `
    -TemplateFile "$HOME/azuredeploy.json"
```

> [!NOTE]
> **newOrExisting** değeri **new** olduğunda ancak belirtilen depolama hesabı adına sahip olan bir depolama hesabı mevcut olduğunda dağıtım başarısız olur.

**newOrExisting** için "existing" değerini ileterek ve var olan bir depolama hesabını belirterek başka bir dağıtım yapmayı deneyin. Depolama hesabını önceden oluşturmak için bkz. [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Kaynak grubunu silmek için seçin **deneyin** Cloud Shell'i açmak için. PowerShell betiğini yapıştırmak için kabuk bölmesinde sağ tıklayın ve ardından **yapıştırın**.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the same resource group name you used in the last procedure"
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanıcılara yeni depolama hesabı oluşturma ve var olan depolama hesabını kullanma arasında seçim yapma imkanı sunan bir şablon geliştirdiniz. Azure Key Vault'tan gizli dizi alma ve gizli dizileri şablon dağıtımı sırasında parola olarak kullanma hakkında bilgi için bkz.:

> [!div class="nextstepaction"]
> [Key Vault'u şablon dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md)
