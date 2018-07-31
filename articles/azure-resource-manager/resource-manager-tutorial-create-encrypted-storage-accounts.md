---
title: Şifrelenmiş depolama hesabı dağıtmak için bir Azure Resource Manager şablonu oluşturma | Microsoft Docs
description: Visual Studio Code'u kullanarak şifrelenmiş depolama hesabı dağıtmak için bir şablon oluşturun.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/20/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 7c78636a210ae90c5bfe1d0bfd35e4e05633f5cd
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188208"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Öğretici: Şifrelenmiş depolama hesabı dağıtmak için bir Azure Resource Manager şablonu oluşturma

Azure Resource Manager şablonunu tamamlamak için kullanmanız gereken bilgileri nasıl bulacağınızı öğrenin.

Bu öğreticide temel bir Azure Hızlı başlangıç şablonunu kullanarak bir Azure Depolama hesabı oluşturacaksınız.  Şablon başvuru belgelerini kullanarak temel şablonu özelleştirecek ve şifrelenmiş bir depolama hesabı kullanmasını sağlayacaksınız.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablon biçimini anlama
> * Şablonda parametre kullanma
> * Şablonda değişken kullanma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/).
* Resource Manager Araçları uzantısı. Yüklemek için bkz. [Resource Manager Araçları uzantısını yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı yerel bilgisayarınıza **azuredeploy.json** olarak kaydetmek için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="understand-the-format"></a>Biçimi anlama

VS Code'da şablonu kök düzeye daraltın. Aşağıdaki öğelere sahip çok basit bir yapı görürsünüz:

![Resource Manager şablonu basit yapısı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: Şablon dilinin sürümünü tanımlayan JSON şema dosyasının konumunu belirtin.
* **contentVersion**: Şablonunuzdaki önemli değişiklikleri belirtmek için bu öğeye istediğiniz değeri verebilirsiniz.
* **parameters**: Dağıtım, kaynak dağıtımını özelleştirme amacıyla yürütüldüğünde sağlanan değerleri belirtin.
* **variables**: Şablon dili ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerleri belirtin.
* **resources**: Kaynak grubunda dağıtılan veya güncelleştirilen kaynak türlerini belirtin.
* **outputs**: Dağıtım sonrasında döndürülen değerleri belirtin.

## <a name="use-parameters-in-template"></a>Şablonda parametre kullanma

Parametreler, belirli bir ortam için tasarlanmış değerler kullanarak dağıtımı özelleştirmenizi sağlar. Depolama hesabı için gerekli değerleri ayarlarken şablonda tanımlanmış olan parametreleri kullanırsınız.

![Resource Manager şablonu parametreleri](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

Bu şablonda iki parametre tanımlanmıştır. location.defaultValue içinde bir şablon işlevi kullanıldığına dikkat edin:

```json
"defaultValue": "[resourceGroup().location]",
```

resourceGroup() işlevi, geçerli kaynak grubunu temsil eden bir nesne döndürür. Şablon işlevlerinin listesi için bkz. [Azure Resource Manager şablonu işlevleri](./resource-group-template-functions.md).

Şablonda tanımlanan parametreleri kullanmak için:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Şablonda değişken kullanma

Değişkenler, şablonun tamamında kullanılabilecek değerler oluşturmanızı sağlar. Değişkenler, şablonların karmaşıklığının azaltılmasına yardımcı olur.

![Resource Manager şablonu değişkenleri](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Bu şablonda bir değişken tanımlanır: *storageAccountName*. Tanımda iki şablon işlevi kullanılır:

- **concat()**: Dizeleri birleştirir. Daha fazla bilgi için bkz. [concat](./resource-group-template-functions-string.md#concat).
- **uniqueString()**: Parametre olarak sağlanan değerleri temel alarak belirlenimci bir karma dize oluşturur. Her Azure depolama hesabının Azure'da benzersiz bir ada sahip olması gerekir. Bu işlev, benzersiz bir dize sağlar. Diğer dize işlevleri için bkz. [Dize işlevleri](./resource-group-template-functions-string.md).

Şablonda tanımlanan değişkeni kullanmak için:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>Şablonu düzenleme

Depolama hesabı şifrelemesiyle ilgili yapılandırmayı bulmak için Azure Depolama hesabı şablon başvurusunu kullanabilirsiniz.

1. [Azure Şablonları](https://docs.microsoft.com/azure/templates/)'na gidin.
2. Sol taraftaki içindekiler bölümünden **Başvuru**->**Depolama**->**Depolama Hesapları**'nı seçin. Bu sayfada Depolama Hesabı bilgilerini tanımlamayla ilgili yönlendirmeler bulunur.
3. Şifrelemeyle ilgili bilgileri inceleyin.  
4. Depolama hesabı kaynak tanımının properties öğesine aşağıdaki JSON kodunu ekleyin:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Bu bölüm, blob depolama hizmetinin şifreleme işlevini etkinleştirir.

resources öğesinin son hali şu şekilde olur:

![Resource Manager şablonu şifrelenmiş depolama hesabı kaynakları](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır.  Bu öğreticide Azure portaldan Cloud Shell'i kullanacaksınız. Cloud Shell hem Azure CLI’yi hem de Azure PowerShell’i destekler. Burada verilen yönergelerde CLI kullanılır.

1. [Azure portalda](https://portal.azure.com) oturum açma
2. Aşağıdaki resimde gösterildiği gibi, sağ üst köşeden **Cloud Shell**’i seçin:

    ![Azure portal Cloud Shell](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell.png)

3. Aşağı oku seçin ve seçili değilse **Bash**'i seçin. Bu öğreticide Azure CLI'yı kullanacaksınız.

    ![Azure portal Cloud shell CLI](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-choose-cli.png)
4. Kabuğu yeniden başlatmak için **Yeniden Başlat**’ı seçin.
5. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin.

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-upload-file.png)
6. Öğreticide daha önce kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy.json** olur.
7. Cloud Shell'den **ls** komutunu çalıştırarak dosyanın başarılı bir şekilde karşıya yüklendiğini doğrulayın. Şablon içeriğini doğrulamak için **cat** komutunu da kullanabilirsiniz.

    ![Azure portal Cloud Shell dosya listeleme](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-list-file.png)
8. Cloud Shell’den şu komutları çalıştırın:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file azuredeploy.json
    ```
    İşte örnek bir dağıtımın ekran görüntüsü:

    ![Azure portal Cloud Shell şablon dağıtımı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-deploy-template.png)

    Ekran görüntüsünde şu değerler kullanılır:

    * **&lt;ResourceGroupName>**: myresourcegroup0719. Parametrenin iki görünümü vardır.  Aynı değeri kullandığınızdan emin olun.
    * **&lt;AzureLocation>**: eastus2
    * **&lt;DeployName>**: mydeployment0719
    * **&lt;TemplateFile>**: azuredeploy.json

    Ekran görüntüsü çıkışında, depolama hesabı adı *fhqbfslikdqdsstandardsa* şeklindedir. 

9. Yeni oluşturulan depolama hesabını listelemek için şu PowerShell komutunu çalıştırın:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

    Blob depolama için şifrelemenin etkinleştirildiğini gösteren aşağıdaki ekran görüntüsüne benzer bir çıktı görmeniz gerekir.

    ![Azure Resource Manager şifrelenmiş depolama hesabı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şablon başvurusunu kullanarak var olan bir şablonu özelleştirmeyi öğrendiniz. Bu öğreticide kullanılan şablonda tek bir Azure kaynağı vardır.  Sonraki öğreticide, şablonu birden fazla kaynakla geliştireceksiniz.  Bazı kaynakların bağımlı kaynakları vardır.

> [!div class="nextstepaction"]
> [Birden çok kaynak oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
