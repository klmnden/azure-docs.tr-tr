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
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5657ebb2a5b29e4ec5360480c1fef6cb92dad9c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60388543"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Öğretici: Sanal makine uzantıları Azure Resource Manager şablonları ile dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Bu öğreticide, bir Windows VM'de bir PowerShell betiğini çalıştırmak için bir Azure Resource Manager şablonundan bir özel betik uzantısı dağıtın.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı. Bkz: [uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Öğreticisi: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca, her üç ayda bir parolanızı güncelleştirmeniz öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

Bir PowerShell Betiği aşağıdaki içeriğe sahip paylaşılan bir [genel erişimi olan Azure depolama hesabı](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1):

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Kendi konumda dosya yayımlama kullanmayı tercih ederseniz güncelleştirmelisiniz `fileUri` şablondaki öğreticinin ilerleyen bölümlerinde öğe.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları, Resource Manager şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code'da seçin **dosya** > **açık dosya**.
1. İçinde **dosya adı** kutusunda, aşağıdaki URL'yi yapıştırın: https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json

1. Dosyayı açmak için seçmeniz **açın**.  
    Şablon beş kaynakları tanımlar:

   * **Microsoft.Storage/storageAccounts**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * **Microsoft.Network/publicIPAddresses**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * **Microsoft.Network/virtualNetworks**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * **Microsoft.Network/networkInterfaces**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * **Microsoft.Compute/virtualMachines**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Bunu özelleştirmeden önce bazı temel şablon anlamak faydalıdır.

1. Yerel bilgisayarınıza adlı dosyanın bir kopyasını Kaydet *azuredeploy.json* seçerek **dosya** > **Kaydet**.

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

Bu kaynak tanımı hakkında daha fazla bilgi için bkz: [uzantı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions). Önemli öğeler şunlardır:

* **Ad**: Uzantı kaynak sanal makine nesnesini alt kaynak olduğundan, adın sanal makine adı ön eki olmalıdır. Bkz. [Alt kaynaklar](./resource-group-authoring-templates.md#child-resources).
* **dependsOn**: Sanal makineyi oluşturduktan sonra uzantı kaynağını oluşturun.
* **fileUris**: Komut dosyalarının depolandığı konumu. Belirtilen konum kullanmayı tercih ederseniz değerleri güncelleştirmeniz gerekiyor.
* **commandToExecute**: Bu komut, komut dosyasını çağırır.  

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için "şablonu Dağıt" bölümüne bakın. [Öğreticisi: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Sanal makine yönetici hesabı için oluşturulan bir parola kullanmanızı öneririz. Bu makalenin bkz [önkoşulları](#prerequisites) bölümü.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. Azure portalında, sanal Makineyi seçin.
1. Seçerek VM'nin genel IP adresini kopyalayın **kopyalamak için tıklayın**ve ardından bir tarayıcı sekmesinde yapıştırın.  
   ' % S'varsayılan Internet Information Services (IIS) Hoş Geldiniz sayfası açılır.

![Internet Information Services Karşılama sayfası](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıttığınız Azure kaynaklarına artık ihtiyacınız olmadığında, kaynak grubunu silerek temizlenmesi.

1. Azure portalında, sol bölmede seçin **kaynak grubu**.
2. İçinde **ada göre filtrele** kutusunda, kaynak grubu adı girin.
3. Kaynak grubu adını seçin.  
    Altı kaynakların kaynak grubunda görüntülenir.
4. Üst menüde **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makineye IIS web sunucusunu yükledi. Bir BACPAC dosyasını içeri aktarmak için Azure SQL veritabanı uzantısının kullanılması öğrenmek için bkz:

> [!div class="nextstepaction"]
> [SQL uzantıları dağıtma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)
