---
title: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma | Microsoft Docs
description: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ca07f044a423a4ea1b5dee484c56f457c7d785de
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239740"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Bu öğreticide bir Windows VM'de PowerShell betiği çalıştırmak için Resource Manager şablonundan Özel Betik uzantısını dağıtacaksınız.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı.  Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

Aşağıdaki içeriğe sahip bir PowerShell betiği, [genel erişime açık bir Azure Depolama hesabından](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1) paylaşılmıştır:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemadaki [fileUri] öğesini güncelleştirmeniz gerekir.

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

## <a name="edit-the-template"></a>Şablonu düzenleme

Aşağıdaki içeriği kullanarak var olan şablona bir sanal makine uzantısı kaynağı ekleyin:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Bu kaynak tanımı hakkında daha fazla bilgi edinmek için bkz. [uzantı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions). Önemli öğeler şunlardır:

* **name**: Uzantı kaynağı, sanal makine nesnesinin alt kaynağı olduğundan ad alanında sanal makine adı ön ek olarak kullanılmalıdır. Bkz. [Alt kaynaklar](./resource-manager-templates-resources.md#child-resources).
* **dependsOn**: Uzantı kaynağının sanal makine oluşturulduktan sonra oluşturulması gerekir.
* **fileUris**: Betik dosyalarının bulunduğu konumlardır. Sağlanan dosyayı kullanmazsanız buradaki değerleri güncelleştirmeniz gerekir.
* **commandToExecute**: Betiği çağırmak için kullanılan komuttur.  

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) bölümüne bakın. Sanal makine yönetici hesabı için oluşturulmuş bir parola kullanmanız önerilir. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Portalda VM'yi seçin ve VM özetinde IP adresinin sağ tarafındaki **Kopyalamak için tıklayın**'ı kullanarak adresi kopyalayın ve bir tarayıcı sekmesine yapıştırın. Varsayılan IIS karşılama sayfası açılır ve şu şekilde görünmelidir:

![Azure Resource Manager vm uzantısı dağıtımı müşteri betiği IIS web sunucusu](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makineye IIS web sunucusunu yükledi. SQL Veritabanı uzantısını kullanarak bir BACPAC dosyasını içeri aktarmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [](./resource-manager-tutorial-deploy-vm-extensions.md)
