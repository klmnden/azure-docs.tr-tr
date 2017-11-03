---
title: "Oluşturma, derleme ve logic apps Visual Studio - Azure mantıksal uygulamaları dağıtma | Microsoft Docs"
description: "Tasarlama, derleme ve Azure mantıksal uygulamaları dağıtmak için Visual Studio projeleri oluşturma."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Tasarım, derleme ve Visual Studio'daki Azure mantıksal uygulamaları dağıtma

Ancak [Azure portal](https://portal.azure.com/) tasarlamayı, derlemeyi ve mantıksal uygulamalarınızı dağıtma için Visual Studio'yu kullanabilirsiniz, oluşturmak ve Azure mantıksal uygulamaları yönetmek harika bir yolunu sunar. Visual Studio, logic apps oluşturmak, dağıtım ve Otomasyon şablonları yapılandırın ve herhangi bir ortama dağıtmak mantığı Uygulama Tasarımcısı gibi zengin araçlar sağlar. 

Azure Logic Apps'i kullanmaya başlamak için bilgi [Azure portalında ilk mantıksal uygulamanızı oluşturmak nasıl](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Yükleme adımları

Yükleyin ve Visual Studio Araçları Azure mantıksal uygulamaları için yapılandırmak için aşağıdaki adımları izleyin.

### <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) veya Visual Studio 2015
* [En son Azure SDK'sı](https://azure.microsoft.com/downloads/) (2.9.1 veya üzeri)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Katıştırılmış Tasarımcısı'nı kullanırken web erişimi

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Azure mantıksal uygulamaları için Visual Studio araçlarını yükleme

Önkoşulları yüklendikten sonra:

1. Visual Studio'yu açın. Üzerinde **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**.
2. Genişletme **çevrimiçi** çevrimiçi arama yapabilmeniz için kategori.
3. Göz atın veya arayın **Logic Apps** bulana kadar **Visual Studio için Azure Logic Apps Araçları**.
4. Karşıdan yükle ve uzantıyı yüklemek için tıklatın **karşıdan**.
5. Yüklendikten sonra Visual Studio'yu yeniden başlatın.

> [!NOTE]
> Ayrıca indirebilirsiniz [Azure Logic Apps araçları Visual Studio 2017 için](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) ve [Visual Studio 2015 için Azure Logic Apps Araçları](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) Visual Studio Marketi'nden doğrudan.

Yüklemeyi tamamladıktan sonra Azure kaynak grubu projesi mantığı uygulama tasarımcı ile birlikte kullanabilirsiniz.

## <a name="create-your-project"></a>Projenizi oluşturma

1. Üzerinde **dosya** menüsünde, Git **yeni**seçip **proje**. Veya varolan bir çözümü projenize eklemek için şu adrese gidin **Ekle**seçip **yeni proje**.

    ![Dosya menüsü](./media/logic-apps-deploy-from-vs/filemenu.png)

2. İçinde **yeni proje** penceresinde Bul **bulut**seçip **Azure kaynak grubu**. Projenizi adlandırın ve tıklatın **Tamam**.

    ![Yeni Proje Ekle](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Seçin **mantıksal uygulama** boş mantığı uygulama dağıtım şablonu, kullanmanız gereken oluşturur şablonu. Şablonunuzu seçtikten sonra tıklayın **Tamam**.

    ![Mantıksal uygulama şablonu seçin](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Şimdi, çözümünüz için mantığı uygulaması projenize ekledik. 
    Çözüm Gezgini'nde, dağıtım dosyanızı görüntülenmesi gerekir.

    ![Dağıtım dosyası](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Mantıksal uygulamanızı mantığı Uygulama Tasarımcısı ile oluşturma

Bir mantıksal uygulama içeren bir Azure kaynak grubu projesi olduğunda, iş akışı oluşturmak için Visual Studio'da mantığı Uygulama Tasarımcısı'nı açabilirsiniz. 

> [!NOTE]
> Tasarımcı kullanılabilir özellikler ve veriler için sorgu bağlayıcılar bir Internet bağlantısı gerektirir. Örneğin, Dynamics CRM Online Bağlayıcısı'nı kullanırsanız, Tasarımcı kullanılabilir özel ve varsayılan özellikleri göstermek için CRM örneğinizi sorgular.

1. Sağ tıklayın, `<template>.json` dosyasını bulun ve seçin **mantığı Uygulama Tasarımcısı ile Aç**. (`Ctrl+L`)

2. Azure abonelik, kaynak grubunu ve konumu, dağıtım şablonu seçin.

    > [!NOTE]
    > Bir mantıksal uygulama tasarlama API bağlantı kaynaklarını özellikleri sorgu tasarımı sırasında oluşturur. Visual Studio tasarım sırasında bu bağlantılar oluşturmak için seçili kaynak grubu kullanır. Görüntülemek veya herhangi bir API bağlantısı değiştirmek için Azure Portalı'na gidin ve göz **API bağlantıları**.

    ![Abonelik Seçici](./media/logic-apps-deploy-from-vs/designer_picker.png)

    Tasarımcı tanımında kullanan `<template>.json` dosya oluşturma işlemi için.

4. Oluşturun ve mantıksal uygulamanızı tasarlayın. Dağıtım şablonu, yaptığınız değişikliklerle güncelleştirilir.

    ![Visual Studio'da mantığı Uygulama Tasarımcısı](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Visual Studio ekler `Microsoft.Web/connections` tüm bağlantılar için kaynak dosyanızı kaynaklara mantıksal uygulamanızı gerekiyor işlev. Bu bağlantı özellikleri ayarlanabilir dağıttığınızda ve içinde dağıttıktan sonra yönetilen **API bağlantıları** Azure portalında.

### <a name="switch-to-json-code-view"></a>JSON kod görünümüne geçin

Mantıksal uygulamanız için JSON gösterimi göstermek için seçin **kod görünümü** Tasarımcısı'nın altındaki sekmesi.

Geri tam kaynak için JSON geçiş yapmak için sağ `<template>.json` dosyasını bulun ve seçin **açık**.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Visual Studio dağıtım şablonları bağımlı kaynaklar için başvurular ekleyin

Bağımlı kaynaklarla başvurmak için mantıksal uygulamanızı istediğinizde kullanabilirsiniz [Azure Resource Manager şablonu işlevleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) mantığı uygulama dağıtım şablonunuzda. Örneğin, mantıksal uygulamanızı mantıksal uygulamanızı dağıtmak istediğiniz bir Azure işlevi veya tümleştirme hesap başvurmak isteyebilirsiniz. Böylece mantığı Uygulama Tasarımcısı'nı doğru şekilde işler, dağıtım şablonu parametrelerini kullanma hakkında aşağıdaki yönergeleri izleyin. 

Bu türden tetikleyiciler ve Eylemler mantığı uygulama parametreleri kullanabilirsiniz:

*   Alt iş akışı
*   İşlev uygulaması
*   APIM çağrısı
*   API bağlantı çalışma zamanı URL'si
*   API bağlantı yolu

Ve şablon işlevleri parametreleri, değişkenleri, ResourceId, concat vb. gibi kullanabilirsiniz. Örneğin, işte Azure işlevi kaynak kimliği nasıl değiştirebilirsiniz:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

Ve burada parametreleri kullanabilirsiniz:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Başka bir örnek olarak Service Bus ileti gönderme işlemini Parametreleştirme:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl isteğe bağlıdır ve varsa, şablondan kaldırılabilir.
> 


> [!NOTE] 
> Mantıksal uygulama parametrelerini kullandığınızda çalışmak Tasarımcı, varsayılan değerleri, örneğin sağlamanız gerekir:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Mantıksal uygulamanızı kaydetme

Mantıksal uygulamanızı zaman kaydetmek için Git **dosya** > **kaydetmek**. (`Ctrl+S`) 

Uygulamanızı kaydederken mantıksal uygulamanızı herhangi bir hata varsa, Visual Studio'da göründükleri **çıkışları** penceresi.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Mantıksal uygulamanızı Visual Studio'dan dağıtma

Uygulamanızı yapılandırdıktan sonra yalnızca birkaç adımda Visual Studio doğrudan dağıtabilirsiniz. 

1. Çözüm Gezgini'nde, projenize sağ tıklayın ve Git **dağıtma** > **yeni dağıtım...**

    ![Yeni dağıtım](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. İstendiğinde, Azure aboneliğinizde oturum açın. 

3. Şimdi mantıksal uygulamanızı dağıtmak istediğiniz kaynak grubu için ayrıntıları seçmeniz gerekir. İşiniz bittiğinde seçin **dağıtma**.

    > [!NOTE]
    > Kaynak grubu için doğru şablonu ve parametre dosyası seçtiğinizden emin olun. Örneğin, bir üretim ortamında dağıtmak istiyorsanız, üretim parametreler dosyası seçin.

    ![Kaynak grubuna Dağıt](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Dağıtım durumu görünür **çıkış** penceresi. 
    Seçilecek olabilir **Azure sağlama** içinde **Göster çıktı** listesi.

    ![Dağıtım durumu çıktı](./media/logic-apps-deploy-from-vs/output.png)

Gelecekte, mantıksal uygulamanızı kaynak denetiminde düzenleyin ve yeni sürümler dağıtmak için Visual Studio'yu kullanın.

> [!NOTE]
> Azure portalında tanımı doğrudan değiştirirseniz, sonraki Visual Studio'dan dağıttığınızda bu değişikliklerin üzerine yazılır. 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Varolan bir kaynak grubu projesine mantıksal uygulamanızı ekleme

Varolan bir kaynak grubu projesi varsa, bu projede JSON ana hattı penceresinin mantıksal uygulamanızı ekleyebilirsiniz. Daha önce oluşturduğunuz uygulama yanında başka bir mantıksal uygulama da ekleyebilirsiniz.

1. `<template>.json` dosyasını açın.

2. JSON ana hattı penceresini açmak için Git **Görünüm** > **diğer pencereler** > **JSON ana hattı**.

3. Kaynak şablon dosyasına eklemek için tıklatın **kaynak ekleme** JSON ana hattı penceresinin üst. Veya JSON ana hattı penceresinde sağ **kaynakları**seçip **yeni kaynak ekleme**.

    ![JSON ana hattı penceresinin](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. İçinde **kaynak ekleme** iletişim kutusu bulun ve seçin, **mantıksal uygulama**. Mantıksal uygulamanızı adlandırın ve seçin **Ekle**.

    ![Kaynak Ekle](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Sonraki Adımlar

* [Logic apps Visual Studio Cloud Explorer ile yönetme](logic-apps-manage-from-vs.md)
* [Sık rastlanan örnekleri ve senaryoları inceleyin](logic-apps-examples-and-scenarios.md)
* [Azure Logic Apps ile iş süreçlerini otomatikleştirmek öğrenin](http://channel9.msdn.com/Events/Build/2016/T694)
* [Sistemlerinizi Azure Logic Apps ile tümleştirme öğrenin](http://channel9.msdn.com/Events/Build/2016/P462)
