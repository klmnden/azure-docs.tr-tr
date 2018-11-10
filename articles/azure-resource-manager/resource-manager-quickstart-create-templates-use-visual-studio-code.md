---
title: Azure Resource Manager şablonu oluşturmak için Visual Studio Code kullanma | Microsoft Docs
description: Resource Manager şablonları üzerinde çalışmak için Visual Studio Code ve Azure Resource Manager araçları eklentisini kullanın.
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
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 092b6f2c3267a2c2cd2cc6304133134825bb7261
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230165"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanılarak Azure Resource Manager şablonları oluşturma

Visual Studio Code ve Azure Resource Manager Araçları uzantısı kullanarak Azure Resource Manager şablonları oluşturmayı ve düzenlemeyi öğrenin. Uzantı olmadan Visual Studio Code'da Resource Manager şablonları oluşturabilirsiniz, ancak uzantı, şablon geliştirmeyi kolaylaştıran otomatik tamamlama seçenekleri sağlar. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](resource-group-overview.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

- [Visual Studio Code](https://code.visualstudio.com/).
- Resource Manager Araçları uzantısı. Yüklemek için şu adımları kullanın:

    1. Visual Studio Code'u açın.
    2. Uzantılar bölmesini açmak için **CTRL+SHIFT+X** tuşlarına basın
    3. **Azure Resource Manager Araçları**’nı arayın ve sonra **Yükle**’yi seçin.
    4. Uzantı yüklemesini tamamlamak için **Yeniden Yükle**’yi seçin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Sıfırdan şablon oluşturmak yerine, [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/)’ndan bir şablon açarsınız. Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır.

Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı yerel bilgisayarınıza **azuredeploy.json** olarak kaydetmek için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Visual Studio Code kullanarak şablon düzenlemeyi öğrenmek için `outputs` bölümüne bir öğe daha eklersiniz.

1. Bir veya birden çok çıkışı dışarı aktarılan şablona ekleyin:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Bitirdiğinizde çıkışlar bölümü şöyle görünür:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Kodu kopyalayıp Visual Studio Code'un içine yapıştırdıysanız, Resource Manager Araçları uzantısının IntelliSense özelliğini denemek için **value** öğesini bir kez daha yazmayı deneyin.

    ![Resource Manager şablonu Visual Studio Code IntelliSense](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Dosyayı kaydetmek için **Dosya**>**Kaydet**’e tıklayın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır.  Bu hızlı başlangıçta, Azure Cloud Shell'i kullanacaksınız. Cloud Shell hem Azure CLI'yi hem de Azure PowerShell'i destekler.

1. [Azure Cloud Shell](https://shell.azure.com)'de oturum açın.

    ![Azure portal Cloud shell CLI](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
2. Cloud Shell'in sol üst köşesinde **PowerShell** veya **Bash** ifadesi gösterilir. CLI kullanmak için bir Bash oturumu açmanız gerekir. PowerShell'i çalıştırmak için bir PowerShell oturumu açmanız gerekir. Bash ve PowerShell arasında geçiş yapmak için aşağı oku seçin. Önceki ekran görüntüsüne bakın. Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.
3. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)
    
    ---

    Kabuktan dağıtmadan önce şablon dosyasını karşıya yüklemeniz gerekir.
5. Önceki bölümde kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy.json** olur.
6. Cloud Shell'den **ls** komutunu çalıştırarak dosyanın başarılı bir şekilde karşıya yüklendiğini doğrulayın. Şablon içeriğini doğrulamak için **cat** komutunu da kullanabilirsiniz. Aşağıdaki resimde komutun Bash'ten çalıştırılması gösterilmektedir.  PowerShell oturumundan da aynı komutlar kullanılır.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)
    
    ---
7. Cloud Shell’den aşağıdaki komutları çalıştırın. PowerShell kodunu veya CLI kodunu gösteren sekmeyi seçin.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the name for this deployment:" &&
    read deploymentName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    az group create --name $resourceGroupName --location $location &&
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file "azuredeploy.json"
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $deploymentName = Read-Host -Prompt "Enter the name for this deployment"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName -TemplateFile "azuredeploy.json"
    ```
    
    ---

    Dosyayı **azuredeploy.json** dışında bir adla kaydederseniz şablon dosyasının adını güncelleştirin.

    Aşağıdaki ekran görüntüsünde bir örnek dağıtım gösterilmektedir:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud Shell şablon dağıtımı](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Azure portal Cloud Shell şablon dağıtımı](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)
    
    ---

    Çıktı bölümündeki depolama hesabı adı ve depolama URL'si ekran görüntüsünde vurgulanmıştır. Bir sonraki adımda depolama hesabına ihtiyacınız olacak.

7. Yeni oluşturulan depolama hesabını listelemek için aşağıdaki CLI komutunu çalıştırın:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```
    
    ---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın ana odak noktası, Visual Studio Code kullanarak Azure Hızlı Başlangıç şablonlarındaki mevcut bir şablon düzenlemektir. Ayrıca,şablonu Azure Cloud Shell'den CLI'yi veya PowerShell'i kullanarak dağıtmayı da öğrendiniz. Azure Hızlı Başlangıç şablonlarındaki şablonlar size ihtiyacınız olan her şeyi sağlamayabilir. Sonraki öğreticide şifrelenmiş bir Azure Depolama hesabı oluşturmak için şablon referansından nasıl bilgi bulacağınız gösterilmektedir.

> [!div class="nextstepaction"]
> [Şifrelenmiş depolama hesabı oluşturma](./resource-manager-tutorial-create-encrypted-storage-accounts.md)
