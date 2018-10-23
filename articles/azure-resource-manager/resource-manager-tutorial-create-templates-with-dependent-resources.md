---
title: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma | Microsoft Docs
description: Birden fazla kaynakla bir Azure Resource Manager şablonu oluşturmayı ve Azure portalı kullanarak dağıtmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/09/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 50f1c81f08787181de2fe3a9f6fb97a96a2bd882
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114321"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Öğretici: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma

Birden fazla kaynağı dağıtmak için kullanabileceğiniz bir Azure Resource Manager şablonu oluşturmayı öğrenin.  Şablonu oluşturduktan sonra Azure portaldan Cloud Shell kullanarak dağıtacaksınız.

Bu öğreticide bir depolama hesabı, bir sanal makine, bir sanal ağ ve ek birkaç bağımlı kaynak oluşturacaksınız. Kaynakların bazıları başka bir kaynak var olana kadar dağıtılamaz. Örneğin depolama hesabı ve ağ arabirimi oluşturulmadan sanal makineyi oluşturamazsınız. Bu ilişkiyi, kaynakların birini diğer kaynaklara bağımlı hale getirerek tanımlarsınız. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki kaynakları dağıtma sırasını belirleme](./resource-group-define-dependencies.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Anahtar kasasını hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu keşfetme
> * Parametre dosyasını düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı.  Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="prepare-key-vault"></a>Key Vault'u hazırlama

Parola spreyi saldırılarını önlemek için sanal makine yönetici hesabı için otomatik olarak oluşturulan bir parolayı kullanmanız ve bu parolayı Key Vault'ta depolamanız önerilir. Aşağıdaki yordam, parolayı depolamak için bir anahtar kasası ve gizli dizi oluşturur. Ayrıca şablon dağıtımının Key Vault'ta depolanan gizli diziye erişmesi için gerekli izinleri yapılandırır. Key Vault farklı bir Azure aboneliğindeyse ek erişim ilkelerinin yapılandırılması gerekir. Ayrıntılar için bkz. [Dağıtım sırasında gizli parametre değeri geçirmek için Azure Key Vault'u kullanma](./resource-manager-keyvault-parameter.md).

1. [Azure Cloud Shell](https://shell.azure.com)'de oturum açın.
2. Sol üst köşeden tercih ettiğiniz **PowerShell** veya **Bash** ortamına geçiş yapın.
3. Aşağıdaki Azure PowerShell veya Azure CLI komutunu çalıştırın.  

    ```azurecli-interactive
    keyVaultName='<your-unique-vault-name>'
    resourceGroupName='<your-resource-group-name>'
    location='Central US'
    userPrincipalName='<your-email-address-associated-with-your-subscription>'
    
    # Create a resource group
    az group create --name $resourceGroupName --location $location
    
    # Create a Key Vault
    keyVault=$(az keyvault create \
      --name $keyVaultName \
      --resource-group $resourceGroupName \
      --location $location \
      --enabled-for-template-deployment true)
    keyVaultId=$(echo $keyVault | jq -r '.id')
    az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

    # Create a secret
    password=$(openssl rand -base64 32)
    az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: $keyVaultId."
    ```

    ```azurepowershell-interactive
    $keyVaultName = "<your-unique-vault-name>"
    $resourceGroupName="<your-resource-group-name>"
    $location='Central US'
    $userPrincipalName="<your-email-address-associated-with-your-subscription>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
    # Create a Key Vault
    $keyVault = New-AzureRmKeyVault `
      -VaultName $keyVaultName `
      -resourceGroupName $resourceGroupName `
      -Location $location `
      -EnabledForTemplateDeployment
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
      
    # Create a secret
    $password = openssl rand -base64 32
    
    $secretValue = ConvertTo-SecureString $password -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretValue
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: " $keyVault.ResourceID
    ```
4. Çıkış değerlerini not edin. Öğreticinin sonraki bölümlerinde bunlara ihtiyacınız olacaktır

> [!NOTE]
> Her Azure hizmetinin parola gereksinimleri farklıdır. Örneğin Azure sanal makinelerinin gereksinimleri, "VM oluşturma sırasında parola gereksinimleri nedir?" sayfasında yer alır.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.
5. 1-4 arası adımları tekrarlayarak **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json** adresini açın ve dosyayı **azuredeploy.parameters.json** olarak kaydedin.

## <a name="explore-the-template"></a>Şablonu keşfetme

Bu bölümdeki şablonu inceledikten sonra şu soruları yanıtlamaya çalışın:

- Bu şablonda kaç adet Azure kaynağı tanımlanmıştır?
- Kaynaklardan biri bir Azure depolama hesabıdır.  Tanım, son öğreticide kullanılan tanıma benziyor mu?
- Bu şablonda tanımlanan kaynaklara ilişkin şablon başvurularını bulabilir misiniz?
- Kaynakların bağımlılıklarını bulabilir misiniz?

1. Visual Studio Code'da birinci düzeydeki öğeleri ve **resources** içindeki ikinci düzey öğeleri görene kadar öğeleri daraltın:

    ![Visual Studio Code Azure Resource Manager şablonları](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Şablonun tanımladığı beş kaynak vardır.
2. İlk kaynağı genişletin. Bir depolama hesabıdır. Tanım, son öğreticinin başında kullanılan tanımla aynı olacaktır.

    ![Visual Studio Code Azure Resource Manager şablonları depolama hesabı tanımı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. İkinci kaynağı genişletin. Kaynak türü: **Microsoft.Network/publicIPAddresses**. Şablon başvurusunu bulmak için, [şablon referansı](https://docs.microsoft.com/azure/templates/) bölümüne göz atın, **Başlığa göre filtrele** alanına **genel IP adresi** veya **genel IP adresleri** girin. Kaynak tanımını şablon başvurusu ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları genel IP adresi tanımı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Bu şablonda tanımlanan diğer kaynaklara ilişkin şablon başvurularını bulmak için son adımı yineleyin.  Kaynak tanımlarını başvurular ile karşılaştırın.
5. Dördüncü kaynağı genişletin:

    ![Visual Studio Code Azure Resource Manager şablonları dependson](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    dependsOn öğesi, kaynaklardan birini diğer kaynaklardan birine veya daha fazlasına bağımlı olarak tanımlamanızı sağlar. Bu örnekte bu kaynak networkInterface olmuştur.  İki farklı kaynağa bağımlıdır:

    * publicIPAddress
    * virtualNetwork

6. Beşinci kaynağı genişletin. Bu kaynak bir sanal makinedir. İki farklı kaynağa bağımlıdır:

    * storageAccount
    * networkInterface

Aşağıdaki diyagramda bu şablondaki kaynaklar ve bağımlılık bilgileri gösterilmiştir:

![Visual Studio Code Azure Resource Manager şablonları bağımlılık diyagramı](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Bağımlılıkların belirtilmesi, Resource Manager'ın çözümü verimli bir şekilde dağıtmasını sağlar. Depolama hesabı, genel IP adresi ve sanal ağ herhangi bir bağımlılığa sahip olmadığından paralel olarak dağıtılır. Genel IP adresi ve sanal ağ dağıtıldıktan sonra ağ arabirimi oluşturulur. Resource Manager, diğer tüm kaynaklar dağıtıldıktan sonra sanal makineyi dağıtır.

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Şablon dosyasında değişiklik yapmanıza gerek yoktur. Ancak Key Vault'tan yönetici parolasını almak için parametre dosyasını değiştirmeniz gerekir.

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
    **id** yerine son yordamda oluşturduğunuz anahtar kasası kaynak kimliğini yazın. Çıktılardan biridir. 

    ![Key Vault ve Resource Manager şablonu sanal makine dağıtımını tümleştirme parametre dosyası](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Şu değerleri verin:

    - **adminUsername**: Sanal makine yönetici hesabının adı.
    - **dnsLabelPrefix**: dnsLablePrefix adı.
4. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır.  Bu öğreticide Azure portaldan Cloud Shell'i kullanacaksınız.

1. [Cloud Shell](https://shell.azure.com)'de oturum açın. Dilerseniz [Azure portal](https://portal.azure.com) oturumu açtıktan sonra aşağıdaki resimde gösterildiği gibi, sağ üst köşeden **Cloud Shell**’i seçebilirsiniz:

    ![Azure portal Cloud Shell](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
2. Cloud Shell'in sol üst köşesinden **PowerShell**'i ve ardından **Onayla**'yı seçin.  Bu öğreticide PowerShell'i kullanacaksınız.
3. Cloud Shell'de **Dosya yükle**'yi seçin:

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Öğreticide daha önce kaydettiğiniz dosyaları seçin. Varsayılan ad **azuredeploy.json** ve **azuredeploy.paraemters.json** şeklindedir.  Aynı ada sahip dosyalar varsa bildirim gösterilmeden eski dosyaların üzerine yazılır.
5. Dosyanın başarıyla yüklendiğini doğrulamak için Cloud Shell'de aşağıdaki komutu çalıştırın. 

    ```bash
    ls
    ```

    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    Ekranda gösterilen dosya adı: azuredeploy.json.

6. Cloud Shell'de aşağıdaki komutu çalıştırarak JSON dosyasının içeriğini doğrulayın:

    ```bash
    cat azuredeploy.json
    cat azuredeploy.parameters.json
    ```
7. Cloud Shell'de aşağıdaki PowerShell komutlarını çalıştırın. Örnek betikte Key Vault için oluşturulmuş olan kaynak grubu kullanılır. Aynı kaynak grubunu kullanmak, kaynakları silme işlemini kolaylaştıracaktır.

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $deploymentName = "<Enter a deployment name>"

    New-AzureRmResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json `
        -TemplateparameterFile azuredeploy.parameters.json
    ```
8. Yeni oluşturulan sanal makineyi listelemek için aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    Sanal makine adı şablon içinde **SimpleWinVM** olarak kodlanmıştır ve değiştirilemez.

9. Yönetici kimlik bilgilerini test etmek için sanal makinede oturum açın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makine, sanal ağ ve bağımlı kaynaklar oluşturmak için bir şablon geliştirdiniz ve dağıttınız. Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin:


> [!div class="nextstepaction"]
> [Koşulları kullanma](./resource-manager-tutorial-use-conditions.md)

