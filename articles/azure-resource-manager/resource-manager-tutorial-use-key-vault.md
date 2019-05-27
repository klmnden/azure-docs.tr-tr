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
ms.openlocfilehash: 0d78e6eaca708073c3a216507b320fe8783a25b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239251"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Öğretici: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme

Azure Key Vault'tan gizli dizileri almak ve gizli dizileri, Resource Manager dağıtım sırasında parametre olarak geçirmeniz hakkında bilgi edinin. Anahtar kasası kimliğini yalnızca başvuru değeri hiçbir zaman sunulur Daha fazla bilgi için bkz. [Dağıtım sırasında gizli parametre değeri geçirmek için Azure Key Vault'u kullanma](./resource-manager-keyvault-parameter.md)

İçinde [kaynak dağıtım sırasını Ayarla](./resource-manager-tutorial-create-templates-with-dependent-resources.md) öğretici, bir sanal makine oluşturun. Sanal makine yönetici kullanıcı adı ve parola sağlamanız gerekir. Parolayı sağlamak yerine, önceden bir Azure anahtar Kasası'nda parola depolamak ve ardından parola, dağıtım sırasında anahtar kasasından almak için şablonu özelleştirmek.

![Resource Manager şablon anahtar kasası tümleştirme diyagramı](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

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

* [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites) içeren [Visual Studio Code](https://code.visualstudio.com/).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Oluşturulan parolanın sanal makine parola gereksinimlerine uygun olduğundan emin olun. Her Azure hizmetinin parola gereksinimleri farklıdır. VM parola gereksinimleri için bkz. [VM oluşturma sırasında parola gereksinimleri nedir?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Bir anahtar Kasası'nı hazırlama

Bu bölümde, anahtar kasası oluşturma ve gizli dizi alabilmeleri, şablonu dağıttığınızda bir gizli dizi anahtar Kasası'na ekleyin. Bir anahtar kasası oluşturmak için birçok yolu vardır. Bu öğreticide, Azure PowerShell dağıtmak için kullandığınız bir [Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Bu şablonu şu işlemleri gerçekleştirir:

* Key vault ile oluşturma `enabledForTemplateDeployment` özelliği sağlar. Bu anahtar Kasası'nda tanımlanan gizli dizileri şablon dağıtım işlemi erişebilmeniz için önce bu özelliği true olmalıdır.
* Gizli anahtar Kasası'na ekleyin.  Gizli dizi, sanal makine yönetici parolasını depolar.

> [!NOTE]
> (Gibi sanal makine şablonu dağıtmak için kullanıcının) sahibi veya katkıda bulunan anahtar kasasının emin değilseniz, sahibi veya katkıda bulunan anahtar kasasının anahtar kasası için erişim Microsoft.KeyVault/vaults/deploy/action izin vermeniz gerekir. Daha fazla bilgi için bkz. [Dağıtım sırasında gizli parametre değeri geçirmek için Azure Key Vault'u kullanma](./resource-manager-keyvault-parameter.md)

Aşağıdaki PowerShell betiğini çalıştırmak için seçin **deneyin** Cloud Shell'i açmak için. Betik yapıştırmak için kabuk bölmesinde sağ tıklayın ve ardından **yapıştırın**.

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

Önemli bilgiler birkaç parçalarını:

* Kaynak grubu adı ile proje adınızdır **rg** eklenir. Daha kolay hale getirmek için [Bu öğreticide oluşturulan kaynakları temizlemeyin](#clean-up-resources), aynı proje adı ve kaynak grubu adı kullanın, [sonraki şablonu dağıtma](#deploy-the-template).
* Gizli dizi adı varsayılan addır **vmAdminPassword**. Bu şablondaki olur.
* Gizli diziyi almak şablon için kullanabilmek için adlandırılan bir erişim ilkesini etkinleştirirseniz **şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir** anahtar kasası için. Bu ilke, şablonda etkinleştirilir. Bu erişim ilkesi hakkında daha fazla bilgi için bkz. [anahtar kasalarını ve gizli anahtarları dağıtma](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

Adlı bir çıktı değeri şablonda **keyVaultId**. Değeri yazın. Sanal makineyi dağıtırken bu kimliğe ihtiyacınız olacak. Kaynak Kimliği biçimi şu şekildedir:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

KODU kopyalayıp, kimliği çoklu satırlara ayrılmış. Satırları birleştirme ve fazladan boşlukları trim gerekir.

Dağıtımı doğrulama için düz metin parolayı almak için aynı Kabuk bölmesinde aşağıdaki PowerShell komutunu çalıştırın. Bir değişken önceki PowerShell komut dosyasında tanımlanan $keyVaultName kullandığından komutu yalnızca aynı Kabuk oturumda çalışır.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Artık bir anahtar kasası ve gizli dizi, aşağıdaki bölümlerde şunları nasıl yapacağınız dağıtım sırasında gizli diziyi almak için mevcut bir şablonu özelleştirmek için hazırladığınız.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin. [Öğretici: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md) bölümünde kullanılan senaryoyla aynıdır.
4. Şablonun tanımladığı beş kaynak vardır:

   * `Microsoft.Storage/storageAccounts`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
5. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.
6. 1-4 arası adımları tekrarlayarak aşağıdaki URL'yi açın ve dosyayı **azuredeploy.parameters.json** olarak kaydedin.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Şablon dosyasında değişiklik yapmanıza gerek yoktur.

1. Açık değilse, Visual Studio Code’da **azuredeploy.parameters.json** dosyasını açın.
2. **adminPassword** parametresini şu şekilde güncelleştirin:

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
    > Değiştirin **kimliği** anahtar kasanız son yordamda oluşturduğunuz kaynak kimliği.

    ![Key Vault ve Resource Manager şablonu sanal makine dağıtımını tümleştirme parametre dosyası](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Şu değerleri verin:

    * **adminUsername**: Sanal makine yönetici hesabının adı.
    * **dnsLabelPrefix**: dnsLablePrefix’i adlandırın.

    Önceki ekran görüntüsünde bir örneğe bakın.

4. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümündeki yönergeleri izleyerek şablonu dağıtın. Her ikisi de yüklemeniz gerekir **azuredeploy.json** ve **azuredeploy.parameters.json** Cloud Shell'i ve şablonu dağıtmak için aşağıdaki PowerShell betiğini kullanın:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Şablonu dağıtırken, anahtar kasası olarak aynı kaynak grubunu kullanın. Bu işlem kaynakları temizlemeyi kolaylaştırır. İki yerine tek bir kaynak grubu silmeniz gerekir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Başarılı bir şekilde sanal makineyi dağıttıktan sonra anahtar kasasında depolanan parola kullanarak oturum açmayı test edin.

1. [Azure portalı](https://portal.azure.com) açın.
2. **Kaynak grupları**/**KaynakGrubunuzunAdı>** /**simpleWinVM** yolunu izleyin.
3. En üstte **Bağlan**'ı seçin.
4. Seçin **RDP dosyasını indir** ve ardından anahtar Kasası'nda depolanan parola kullanarak sanal makinede oturum açmak için yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Key Vault'tan bir gizli dizi aldınız ve bunu şablon dağıtımında kullandınız.  Bağlı şablon oluşturmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Bağlı şablonlar oluşturma](./resource-manager-tutorial-create-linked-templates.md)
