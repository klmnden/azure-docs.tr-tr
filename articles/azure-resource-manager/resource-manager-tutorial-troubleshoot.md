---
title: Resource Manager dağıtım sorunlarını giderme | Microsoft Docs
description: İzleme ve sorun giderme Resource Manager dağıtımları hakkında bilgi edinin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/15/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: c889c3123160680d96889227d6964ff197dc41cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60388667"
---
# <a name="tutorial-troubleshoot-resource-manager-template-deployments"></a>Öğretici: Resource Manager şablon dağıtımları sorunlarını giderme

Resource Manager şablonu dağıtım hatalarını giderme hakkında bilgi edinin. Bu öğreticide, bir şablon iki hataları ayarlama ve sorunları gidermek için etkinlik günlükleri ve dağıtım geçmişini kullanmayı öğrenin.

Şablon dağıtımı için ilişkili hatalar iki tür vardır:

- **Doğrulama hataları** dağıtımdan önce belirlenebilir senaryoları durumlardan kaynaklanır. Söz dizimi hataları şablonunuzu veya abonelik kotanızı aşılmasına kaynakları dağıtılmaya çalışılırken içerirler. 
- **Dağıtım hataları** dağıtım işlemi sırasında ortaya koşullarını durumlardan kaynaklanır. Paralel olarak dağıtılan bir kaynağa erişmeye içerirler.

Her iki türde hatalar dağıtım sorunlarını gidermek için kullandığınız hata kodunu döndürür. Hatalar her iki tür etkinlik günlüğü'nde görünür. Ancak, dağıtım hiç başlatılmadı olduğundan doğrulama hatalarını deployment geçmişinizi görünmez.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Sorunlu bir şablon oluşturma
> * Doğrulama hataları sorunlarını giderme
> * Dağıtım hatalarını giderme
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

- [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites) içeren [Visual Studio Code](https://code.visualstudio.com/).

## <a name="create-a-problematic-template"></a>Sorunlu bir şablon oluşturma

Adlı bir şablonu açmak [standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) gelen [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/)ve iki şablon sorunlarını ayarlayın.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Değişiklik **apiVersion** aşağıdaki satırı için satır:

    ```json
    "apiVersion1": "2018-07-02",
    ```
    - **apiVersion1** geçersiz öğe adı. Bir doğrulama hatası var.
    - API sürümü "2018-07-01" olmalıdır.  Dağıtım bir hatadır.

5. Dosyayı yerel bilgisayarınıza **azuredeploy.json** olarak kaydetmek için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="troubleshoot-the-validation-error"></a>Doğrulama hatasını giderme

Başvurmak [şablonu dağıtmak](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) şablonu dağıtmak için bölüm.

Bir hata kabuğundan benzer alacaksınız:

```
New-AzResourceGroupDeployment : 4:29:24 PM - Error: Code=InvalidRequestContent; Message=The request content was invalid and could not be deserialized: 'Could not find member 'apiVersion1' on object of type 'TemplateResource'. Path 'properties.template.resources[0].apiVersion1', line 36, position 24.'.
```

Sorunu ile hata iletisi gösterir **apiVersion1**.

Değiştirerek sorunu düzeltmek için Visual Studio Code'u kullanmak **apiVersion1** için **apiVersion**ve şablonu kaydedin.

## <a name="troubleshoot-the-deployment-error"></a>Dağıtım hatasını giderme

Başvurmak [şablonu dağıtmak](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) şablonu dağıtmak için bölüm.

Bir hata kabuğundan benzer alacaksınız:

```
New-AzResourceGroupDeployment : 4:48:50 PM - Resource Microsoft.Storage/storageAccounts 'storeqii7x2rce77dc' failed with message '{
  "error": {
    "code": "NoRegisteredProviderFound",
    "message": "No registered resource provider found for location 'centralus' and API version '2018-07-02' for type 'storageAccounts'. The supported api-versions are '2018-07-01, 2018-03-01-preview, 2018-02-01, 2017-10-01, 2017-06-01, 2016-12-01, 2016-05-01, 2016-01-01, 2015-06-15, 2015-05-01-preview'. The supported locations are 'eastus, eastus2, westus, westeurope, eastasia, southeastasia, japaneast, japanwest, northcentralus, southcentralus, centralus, northeurope, brazilsouth, australiaeast, australiasoutheast, southindia, centralindia, westindia, canadaeast, canadacentral, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth, francecentral'."
  }
}'
```

Dağıtım hatası aşağıdaki yordamı kullanarak Azure portalından bulunabilir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Kaynak grubu seçerek açın **kaynak grupları** ve sonra kaynak grubu adı. Göreceksiniz **1 başarısız** altında **dağıtım**.

    ![Resource Manager Öğreticisi sorun giderme](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error.png)
3. Seçin **hata ayrıntılarını**.

    ![Resource Manager Öğreticisi sorun giderme](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error-details.png)

    Hata iletisi daha önce gösterilene aynıdır:

    ![Resource Manager Öğreticisi sorun giderme](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-error-summary.png)

Ayrıca, etkinlik günlüklerinden hata bulabilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **İzleyici** > **etkinlik günlüğü**.
3. Günlük bulmak için filtreleri kullanın.

    ![Resource Manager Öğreticisi sorun giderme](./media/resource-manager-tutorial-troubleshoot/resource-manager-template-deployment-activity-log.png)

Sorunu düzeltmek için Visual Studio Code kullanın ve ardından şablonu yeniden dağıtın.

Sık karşılaşılan bir listesi için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](./resource-manager-common-deployment-errors.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Resource Manager şablonu dağıtım hatalarını giderme öğrendiniz.  Daha fazla bilgi için [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](./resource-manager-common-deployment-errors.md).
