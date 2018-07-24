---
title: Azure Resource Manager şablonu oluşturmak için Visual Studio Code kullanma | Microsoft Docs
description: Resource Manager şablonları üzerinde çalışmak için Azure Resource Manager Araçları uzantısını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/17/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: a66d845fcf7507613cda475bbc96225f8a7cc7eb
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126896"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanılarak Azure Resource Manager şablonları oluşturma

Visual Studio Code ve Azure Resource Manager Araçları uzantısı kullanarak Azure Resource Manager şablonları oluşturmayı öğrenin. Uzantı olmadan Visual Studio Code'da Resource Manager şablonları oluşturabilirsiniz, ancak uzantı, şablon geliştirmeyi kolaylaştıran otomatik tamamlama seçenekleri sağlar. Azure çözümlerinizi dağıtma ve yönetmeyle ilgili kavramları anlamak için bkz. [Azure Resource Manager’a genel bakış](resource-group-overview.md).

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

Visual Studio Code kullanarak şablon düzenlemeyi öğrenmek için, çıkış bölümüne bir öğe daha eklersiniz.

1. Visual Studio Code’dan bir veya birden çok çıkışı dışarı aktarılan şablona ekleyin:

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

    ![Resource Manager şablonu visual studio code intelliSense](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Dosyayı kaydetmek için **Dosya**>**Kaydet**’e tıklayın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır.  Bu hızlı başlangıçta, Azure portaldan Cloud Shell'i kullanırsınız. Cloud Shell hem Azure CLI’yi hem de Azure PowerShell’i destekler. Burada verilen yönergelerde CLI kullanılır.

1. [Azure portalda](https://portal.azure.com) oturum açma
2. Aşağıdaki resimde gösterildiği gibi, sağ üst köşeden **Cloud Shell**’i seçin:

    ![Azure portal Cloud Shell](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell.png)

3. PowerShell’den CLI’ye geçmek için aşağı oku ve **Bash**’i seçin.

    ![Azure portal Cloud shell CLI](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
4. Kabuğu yeniden başlatmak için **Yeniden Başlat**’ı seçin.
5. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin.

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
4. Hızlı başlangıçta daha önce kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy.json** olur.
5. Cloud Shell'den **ls** komutunu çalıştırarak dosyanın başarılı bir şekilde karşıya yüklendiğini doğrulayın. Şablon içeriğini doğrulamak için **cat** komutunu da kullanabilirsiniz.

    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
6. Cloud Shell’den şu komutları çalıştırın:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```
    İşte örnek bir dağıtımın ekran görüntüsü:

    ![Azure portal Cloud Shell şablon dağıtımı](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    Ekran görüntüsünde şu değerler kullanılır:

    - **&lt;ResourceGroupName>**: myresourcegroup0709. Parametrenin iki görünümü vardır.  Aynı değeri kullandığınızdan emin olun.
    - **&lt;AzureLocation>**: eastus2
    - **&lt;DeployName>**: mydeployment0709
    - **&lt;TemplateFile>**: azuredeploy.json

    Ekran görüntüsü çıkışında, depolama hesabı adı *3tqebj3slyfyestandardsa*’dır. 

7. Yeni oluşturulan depolama hesabını listelemek için şu PowerShell komutunu çalıştırın:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Visual Studio Code kullanarak şablon oluşturmayı ve Azure portal Cloud Shell'i kullanarak şablonu dağıtmayı öğrendiniz. Bu Hızlı Başlangıçta kullanılan şablonda tek bir Azure kaynağı vardır.  Sonraki öğreticide, şablonu birden fazla kaynakla geliştireceksiniz.  Bazı kaynakların bağımlı kaynakları vardır.

> [!div class="nextstepaction"]
> [Visual Studio kullanarak şablon oluşturma](./vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
