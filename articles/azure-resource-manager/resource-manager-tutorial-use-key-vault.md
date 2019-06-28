---
title: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme | Microsoft Docs
description: Resource Manager şablonu dağıtımı sırasında parametre değerlerini güvenli bir şekilde geçirme amacıyla Azure Key Vault'u kullanmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: b91a3488750795e8ee9df6602bb2b3df8a9b08ec
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436585"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-resource-manager-template-deployment"></a>Öğretici: Resource Manager şablonu dağıtımınızda Azure anahtar kasası tümleştirme

Bir Azure anahtar kasasından gizli dizilerini alma ve Azure Resource Manager dağıtımı yaptığınızda, gizli dizileri parametreler olarak geçebilir hakkında bilgi edinin. Yalnızca kendi anahtar kasası kimliği başvurduğu için parametre değeri hiçbir zaman, sunulur Daha fazla bilgi için [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](./resource-manager-keyvault-parameter.md).

İçinde [kaynak dağıtım sırasını Ayarla](./resource-manager-tutorial-create-templates-with-dependent-resources.md) öğretici, bir sanal makine (VM) oluşturun. Sanal makine yönetici kullanıcı adı ve parola sağlamanız gerekir. Parolayı sağlamak yerine, önceden parola bir Azure key vault'ta depolamak ve ardından parola, dağıtım sırasında anahtar kasasından almak için bir şablonu özelleştirme.

![Bir Resource Manager şablonu ile bir anahtar kasası tümleştirmesini gösteren diyagram](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir anahtar Kasası'nı hazırlama
> * Hızlı başlangıç şablonunu açma
> * Parametre dosyasını düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ile [Resource Manager araçları uzantısını](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak için VM yönetici hesabı için oluşturulan bir parola kullanın. Bir parola oluşturmak için örneği aşağıdadır:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Oluşturulan parola VM parola gereksinimlerini karşıladığını doğrulayın. Her Azure hizmetinin parola gereksinimleri farklıdır. VM parola gereksinimleri için bkz [bir VM oluşturduğunuzda, parola gereksinimleri nelerdir?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Bir anahtar Kasası'nı hazırlama

Bu bölümde, anahtar kasası oluşturma ve bir gizli dizi, şablonu dağıttığınızda bir gizli dizi alabilmeleri ekleyin. Bir anahtar kasası oluşturmak için birçok yolu vardır. Bu öğreticide, Azure PowerShell dağıtmak için kullandığınız bir [Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Bu şablon şunları yapar:

* Bir key vault ile oluşturur `enabledForTemplateDeployment` özelliği etkin. Bu özellik *true* önce şablon dağıtımı, anahtar Kasası'nda tanımlanan gizli dizileri işlem erişebilirsiniz.
* Gizli anahtar Kasası'na ekler. Gizli dizi VM yönetici parolasını depolar.

> [!NOTE]
> Anahtar Kasası'na dağıtıyor sahibi değilseniz, sanal makine şablonu veya katkıda bulunan kullanıcı olarak sahibi veya katkıda bulunan, erişim izni vermesi gerekir *Microsoft.KeyVault/vaults/deploy/action* izni anahtar kasası. Daha fazla bilgi için [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](./resource-manager-keyvault-parameter.md).

Aşağıdaki Azure PowerShell betiğini çalıştırmak için seçin **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk bölmesinde sağ tıklayın ve ardından **yapıştırın**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

> [!IMPORTANT]
> * Proje adı, kaynak grubu adıdır ancak **rg** eklenmiş. Daha kolay hale getirmek için [Bu öğreticide oluşturulan kaynakları temizlemeyin](#clean-up-resources), aynı proje adı ve kaynak grubu adı kullanın, [sonraki şablonu dağıtma](#deploy-the-template).
> * Gizli dizi için varsayılan ad **vmAdminPassword**. Onun şablondaki.
> * Gizli dizi alınacak şablonunun etkinleştirmek için "Şablon dağıtımı için Azure Resource Manager'a erişim için anahtar kasasını etkinleştir" adlı bir erişim ilkesi etkinleştirmeniz gerekir. Bu ilke, şablonda etkinleştirilir. Erişim İlkesi hakkında daha fazla bilgi için bkz. [anahtar kasalarını ve gizli anahtarları dağıtma](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

Adlı bir çıktı değeri şablonda *keyVaultId*. Sanal makine dağıttığınız zaman daha sonra kullanmak üzere kimlik değerini yazın. Kaynak Kimliği biçimi şu şekildedir:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

KODU kopyalayıp, çoklu satırlara ayrılmış. Satırları birleştirme veya fazladan boşluk kesim.

Dağıtımı doğrulama için düz metin parolayı almak için aynı Kabuk bölmesinde aşağıdaki PowerShell komutunu çalıştırın. Değişkeni kullandığından komutu yalnızca aynı Kabuk oturumda çalışır *$keyVaultName*, önceki PowerShell komut dosyasında tanımlanır.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Artık bir anahtar kasası ve gizli dizi hazırladığınıza. Aşağıdaki bölümlerde dağıtım sırasında gizli diziyi almak için mevcut bir şablonun nasıl özelleştirileceğini gösterir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları, Resource Manager şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adlı [basit Windows VM dağıtma](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. Visual Studio Code'da seçin **dosya** > **açık dosya**.

1. İçinde **dosya adı** kutusunda, aşağıdaki URL'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin. Senaryoda kullanılan bir aynıdır [Öğreticisi: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
   Şablon beş kaynakları tanımlar:

   * `Microsoft.Storage/storageAccounts`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

   Bunu özelleştirmeden önce şablonu temel bazı bilgilere sahipsiniz yararlıdır.

1. Seçin **dosya** > **Kaydet**ve ardından dosyanın bir kopyasını ada sahip yerel bilgisayarınıza kaydedin *azuredeploy.json*.

1. Aşağıdaki URL'yi açmaya 1-3. adımları tekrarlayın ve ardından dosyayı farklı Kaydet *azuredeploy.parameters.json*.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Şablon dosyasında değişiklik yapmanıza gerek yoktur.

1. Visual Studio Code'da açmak *azuredeploy.parameters.json* zaten açık değilse.
1. Güncelleştirme `adminPassword` parametresi:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > Değeri Değiştir **kimliği** önceki yordamda oluşturduğunuz anahtar kasasının kaynak kimliği.

    ![anahtar kasası ve Resource Manager şablonu sanal makine dağıtım parametre dosyasını tümleştirin](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. Aşağıdaki değerleri güncelleştirin:

    * **AdminUsername**: Sanal makine yönetici hesabının adıdır.
    * **dnsLabelPrefix**: DnsLabelPrefix değerin adı.

    Bir önceki resimde adları örnekleri için bkz.

1. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bölümündeki yönergeleri [şablonu dağıtmak](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Hem karşıya *azuredeploy.json* ve *azuredeploy.parameters.json* şablonu dağıtmak için Cloud Shell ve sonra aşağıdaki PowerShell betiğini kullanın:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Şablon dağıtımı yaptığınızda, anahtar Kasası'nda kullandığınız aynı kaynak grubunu kullanın. İki yerine yalnızca bir kaynak grubunu silmek gerektiğinden bu yaklaşım, kaynakları temizlemek için kolaylaştırır.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Sanal makine başarıyla dağıttıktan sonra anahtar kasasında depolanan parola kullanarak oturum açma kimlik bilgilerini test edin.

1. [Azure portalı](https://portal.azure.com) açın.

1. Seçin **kaynak grupları** >  **\<*YourResourceGroupName*>**  > **simpleWinVM** .
1. Seçin **bağlanma** en üstünde.
1. Seçin **RDP dosyasını indir**ve ardından anahtar Kasası'nda depolanan parola kullanarak sanal makineye oturum açmak için yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarınızı artık ihtiyacınız olmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure anahtar kasasından gizli dizi alındı. Gizli dizi sonra şablon dağıtımınıza kullanılır. Bağlı şablon oluşturmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Bağlı şablonlar oluşturma](./resource-manager-tutorial-create-linked-templates.md)
