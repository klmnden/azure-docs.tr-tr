---
title: Azure portalı kullanarak Azure Resource Manager şablonu oluşturma ve dağıtma | Microsoft Docs
description: Azure portalı kullanarak ilk Azure Resource Manager şablonunuzu oluşturmayı ve dağıtmayı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 15940d952dad62b3f71bfef6aa1cd8598d044605
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104735"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Hızlı Başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma

Azure portalı kullanarak ilk Azure Resource Manager şablonunuzu oluşturma ve bu şablonu Azure portaldan düzenleyip dağıtmayı öğrenin. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Bu öğreticideki yönergeler bir Azure Depolama hesabı oluşturur. Aynı işlemi kullanarak başka Azure kaynakları da oluşturabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="generate-a-template-using-the-portal"></a>Portalı kullanarak şablon oluşturma

Bu bölümde Azure portalı kullanarak bir depolama hesabı oluşturacaksınız. Depolama hesabını dağıtmadan önce portal tarafından yapılandırmalarınıza göre oluşturulan şablonu keşfedebilirsiniz. Şablonu kaydedebilir ve daha sonra yeniden kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Kaynak oluştur** > **Depolama** > **Depolama hesabı - blob, dosya, tablo, sorgu**'yu seçin.

    ![Azure portalı kullanarak Azure depolama hesabı oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Aşağıdaki bilgileri girin. 

    - **Kaynak grubu**: Yeni bir Azure kaynak grubu oluşturun ve istediğiniz adı verin. Ekran görüntüsünde kaynak grubu adı *mystorage1016rg* olarak belirtilmiştir.
    - **Ad**: Depolama hesabınıza benzersiz bir ad verin. Ekran görüntüsünde bu ad *mystorage1016* olarak belirtilmiştir.

    Diğer özellikler için varsayılan değerleri kullanabilirsiniz.

    ![Azure portalı kullanarak Azure depolama hesabı yapılandırması oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Dışarı aktarılan şablonların bazılarını kullanabilmeniz için yapmanız gereken düzenlemeler vardır.

4. Ekranın alt tarafından **Gözden geçir + oluştur**'u seçin. 
5. Ekranın alt tarafından **Otomasyon için bir şablon indir**'i seçin. Portalda oluşturulan şablon gösterilir:

    ![Portaldan şablon oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Şablon ana bölmede gösterilir. Dört üst düzey öğeye sahip bir JSON dosyasıdır: `schema`, `contentVersion`, `parameters` ve `resources`. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](./resource-group-authoring-templates.md)

    Tanımlı altı parametre bulunur. Bunlardan biri **storageAccountName** olarak adlandırılmıştır. Vurgulanan ikinci bölüm, bu parametreyi şablon olarak kullanma şeklini göstermektedir. Sonraki bölümde şablonu düzenleyerek depolama hesabı için oluşturulan bir adı kullanacaksınız.

    Şablonda bir Azure kaynağı tanımlanmıştır. Türü [Microsoft.Storage/storageAccounts] şeklindedir. Kaynağın nasıl tanımlandığını ve tanım yapısını görebilirsiniz.
6. **Download** (İndir) seçeneğini belirleyin. İndirdiğiniz paketteki **template.json** dosyasını bilgisayarınıza kaydedin. Sonraki bölümde bir şablon dağıtım aracı kullanarak şablonu düzenleyeceksiniz.
7. Parametre için sağladığınız değerleri görmek üzere **Parametre** sekmesini seçin. Bu değerleri not alın. Bir sonraki bölümde şablonu dağıtırken ihtiyacınız olacak.

    ![Portaldan şablon oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Şablon ve parametreler dosyalarını kullanarak bir Azure depolama hesabı kullanabilirsiniz.

## <a name="edit-and-deploy-the-template"></a>Şablonu düzenleme ve dağıtma

Basit şablon düzenleme işlemleri için Azure portalı kullanabilirsiniz. Bu hızlı başlangıçta *Şablon Dağıtımı* adlı bir portal aracını kullanacaksınız. Daha karmaşık bir şablonu düzenlemek için daha gelişmiş düzenleme işlevleri sunan [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md) uygulamasını kullanabilirsiniz.

Azure'daki her Azure hizmetinin adının benzersiz olması gerekir. Var olan bir depolama hesabı adını girerseniz dağıtım başarısız olur. Bu sorunu önlemek için benzersiz depolama hesabı adı oluşturan `uniquestring()` şablon işlevi çağrısını kullanabilirsiniz.

1. Azure portalda **Kaynak oluştur**’u seçin.
2. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.
3. **Şablon dağıtımı**'nı seçin.

    ![Azure Resource Manager şablon kitaplığı](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. **Oluştur**’u seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.
6. **Dosya yükle**'yi seçin ve ardından yönergeleri izleyerek önceki bölümde indirdiğiniz template.json dosyasını yükleyin.
7. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir değişken ekleyin:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Azure Resource Manager şablonları](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Burada iki işlev kullanılmıştır: `concat()` ve `uniqueString()`.

8. Bir önceki ekran görüntüsünde vurgulanan **storageAccountName** parametresini kaldırın.
9. **Microsoft.Storage/storageAccounts** kaynağının name öğesini parametre yerine yeni tanımlanan değişkeni kullanacak şekilde güncelleştirin:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    Şablonun son halinin şu şekilde olması gerekir:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. **Kaydet**’i seçin.
8. Aşağıdaki değerleri girin:

    - **Kaynak grubu**: Kaynak grubunuza benzersiz bir ad verin.
    - **Konum**: Kaynak grubu için bir konum seçin.
    - **Konum**: Depolama hesabı için bir konum seçin.  Kaynak grubu ile aynı konumu kullanabilirsiniz.
    - **Hesap türü**: Girin **Standard_LRS** Bu Hızlı Başlangıç için.
    - **Tür**: Girin **StorageV2** Bu Hızlı Başlangıç için.
    - **Erişim katmanı**: Girin **etkin** Bu Hızlı Başlangıç için.
    - **Yalnızca HTTPS Trafiği Etkin**.  Bu hızlı başlangıç için **true** değerini seçin.
    - **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: (seçin)

    Örnek bir dağıtımın ekran görüntüsü:

    ![Azure Resource Manager şablonlarını dağıtma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. **Satın al**'ı seçin.
11. Dağıtım durumunu görmek için ekranın en üstündeki zil simgesini (bildirimler) seçin. Dağıtım tamamlanana kadar bekleyin.

    ![Azure Resource Manager şablonlarını dağıtma bildirimi](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Bildirim bölmesinden **Kaynak grubuna git**'i seçin. Şuna benzer bir ekran görmeniz gerekir:

    ![Azure Resource Manager şablonlarını dağıtma kaynak grubu](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Dağıtım durumunun başarılı olduğunu ve kaynak grubunda yalnızca bir depolama hesabı olduğunu görebilirsiniz. Depolama hesabı adı, şablon tarafından oluşturulan benzersiz bir dizedir. Azure depolama hesapları kullanma hakkında daha fazla bilgi edinmek için [hızlı başlangıç: Karşıya yükleme, indirme ve Azure portalını kullanarak blobları listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Depolama hesabı kaynak grubunda bulunmalıdır.
4. Üstteki menüden **Kaynak grubunu sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portaldan şablon oluşturmayı ve portalı kullanarak şablonu dağıtmayı öğrendiniz. Bu Hızlı Başlangıçta kullanılan şablon, tek bir Azure kaynağına sahip basit bir şablondur. Şablon karmaşık olduğunda geliştirme için Visual Studio Code veya Visual Studio uygulamasını kullanmak daha kolaydır. Sonraki öğreticide Azure PowerShell ve Azure Komut Satırı Arabirimi (CLI) kullanarak nasıl şablon dağıtacağınız da gösterilmektedir.

> [!div class="nextstepaction"]
> [Visual Studio Code kullanarak şablon oluşturma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
