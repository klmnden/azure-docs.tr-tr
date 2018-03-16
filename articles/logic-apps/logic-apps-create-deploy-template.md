---
title: "Azure mantıksal uygulamaları için dağıtım şablonları oluşturma | Microsoft Docs"
description: "Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma"
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: SyntaxC4
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; estfan
ms.openlocfilehash: 91d93a02bb9bf48c5bda0304c9d3d52c22e30209
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma

Bir mantıksal uygulama oluşturulduktan sonra bir Azure Resource Manager şablonu oluşturmak isteyebilirsiniz.
Bu şekilde, herhangi bir ortam veya kaynak grubu nerede gereksinim duyabileceğiniz mantıksal uygulama kolayca dağıtabilirsiniz.
Resource Manager şablonları hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md) ve [dağıtma kaynakları Azure Resource Manager şablonları kullanarak](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Mantıksal uygulama dağıtım şablonu

Bir mantıksal uygulama üç temel bileşeni vardır:

* **Mantıksal uygulama kaynağı**: fiyatlandırma planı, konum ve iş akışı tanımı gibi şeyler hakkında bilgiler içerir.
* **İş akışı tanımı**: mantığı uygulamanızın iş akışı adımları ve Logic Apps altyapısı iş akışının nasıl yürütülecek açıklar.
Bu tanım mantığı uygulamanızın içinde görüntüleyebilirsiniz **kod görünümü** penceresi.
Mantıksal uygulama kaynağı bu tanımı'nda bulabilirsiniz `definition` özelliği.
* **Bağlantıları**: güvenli bir bağlantı dizesi ve bir erişim belirteci gibi herhangi bir bağlayıcı bağlantısı ilgili meta verileri depolama kaynaklarını ayırmak için başvuruyor.
Mantıksal uygulama kaynağı mantıksal uygulamanızı bu kaynaklara başvuran `parameters` bölümü.

Mevcut mantıksal uygulamaları bu tüm parçaları gibi bir araç kullanarak görüntüleyebileceğiniz [Azure kaynak Gezgini](http://resources.azure.com).

Kaynak grubu dağıtımı ile kullanmak bir mantıksal uygulama için bir şablon yapmak için kaynakları tanımlamak ve gerektiğinde Parametreleştirme gerekir.
Geliştirme, test ve üretim ortamı dağıtıyorsanız, örneğin, büyük olasılıkla bir SQL veritabanı için farklı bağlantı dizeleri her ortamda kullanmak istediğiniz.
Ya da farklı Aboneliklerde veya kaynak grupları dağıtmak isteyebilirsiniz.  

## <a name="create-a-logic-app-deployment-template"></a>Bir mantıksal uygulama dağıtım şablonu oluşturma

Geçerli mantığı uygulama dağıtım şablonu için en kolay yolu kullanmaktır [mantıksal uygulamalar için Visual Studio Araçları](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites).
Visual Studio Araçları herhangi bir abonelik veya konum arasında kullanılan bir geçerli dağıtım şablonu oluşturun.

Bir mantıksal uygulama dağıtım şablonu oluşturma gibi diğer bazı araçları size yardımcı olabilir.
El ile zaten gerektiğinde parametreleri oluşturmak için aşağıda ele alınan kaynaklar kullanarak diğer bir deyişle, yazabilirsiniz.
Başka bir seçenek kullanmaktır bir [mantıksal uygulama şablonu oluşturan](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell modülü. Bu açık kaynaklı modül önce mantıksal uygulama ve onu kullanıyor ve dağıtımı için gerekli parametreleri şablon kaynaklarla oluşturur herhangi bir bağlantısı değerlendirir.
Örneğin, bir Azure hizmet veri yolu kuyruktan bir ileti alır ve bir Azure SQL veritabanına veri ekleyen bir mantıksal uygulama varsa, araç tüm orchestration mantığı korur ve dağıtımın ayarlamak için SQL ve hizmet veri yolu bağlantı dizeleri parameterizes.

> [!NOTE]
> Bağlantıları mantıksal uygulama aynı kaynak grubunda olması gerekir.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Mantıksal uygulama şablonu PowerShell modülünü yükleyin
Modülünü yüklemek için en kolay yolu durumda [PowerShell Galerisi](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), komutunu kullanarak `Install-Module -Name LogicAppTemplate`.  

PowerShell modülünü el ile de yükleyebilirsiniz:

1. En son sürümünü indirme [mantıksal uygulama şablonu oluşturan](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. PowerShell modül klasörünüze klasöre ayıklayın (genellikle `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Modülün herhangi bir kiracı ve abonelik erişim ile çalışmak belirteç, kendisiyle kullanmanızı öneririz [ARMClient](https://github.com/projectkudu/ARMClient) komut satırı aracı.  Bu [blog gönderisi](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient daha ayrıntılı olarak anlatılmaktadır.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>PowerShell kullanarak bir mantıksal uygulama şablonu oluştur
PowerShell yüklendikten sonra aşağıdaki komutu kullanarak bir şablon oluşturabilirsiniz:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` Azure abonelik kimliği Bu satırı önce bir erişim ARMClient belirtecini alır sonra üzerinden PowerShell Betiği Kanallar ve ardından şablon bir JSON dosyası oluşturur.

## <a name="add-parameters-to-a-logic-app-template"></a>Bir mantıksal uygulama şablonu parametreleri ekleme
Mantıksal uygulama şablonu oluşturduktan sonra eklemek veya gereksinim duyabileceğiniz parametreleri değiştirmek devam edebilirsiniz. Örneğin, bir kaynak kimliği bir Azure işlevi veya tek bir dağıtımda dağıtmayı planladığınız iç içe geçmiş iş akışı tanımınızı içeriyorsa, daha fazla kaynak şablonunuza eklemek ve gerektiğinde kimlikleri Parametreleştirme. Aynı özel API'leri veya Swagger yönelik tüm başvuruları dağıtmak için her kaynak grubu ile beklediğiniz uç noktalar uygular.

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

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Varolan bir kaynak grubu projesine mantıksal uygulamanızı ekleme

Varolan bir kaynak grubu projesi varsa, bu projede JSON ana hattı penceresinin mantıksal uygulamanızı ekleyebilirsiniz. Daha önce oluşturduğunuz uygulama yanında başka bir mantıksal uygulama da ekleyebilirsiniz.

1. `<template>.json` dosyasını açın.

2. JSON ana hattı penceresini açmak için Git **Görünüm** > **diğer pencereler** > **JSON ana hattı**.

3. Kaynak şablon dosyasına eklemek için tıklatın **kaynak ekleme** JSON ana hattı penceresinin üst. Veya JSON ana hattı penceresinde sağ **kaynakları**seçip **yeni kaynak ekleme**.

    ![JSON ana hattı penceresinin](./media/logic-apps-create-deploy-template/jsonoutline.png)
    
4. İçinde **kaynak ekleme** iletişim kutusu bulun ve seçin, **mantıksal uygulama**. Mantıksal uygulamanızı adlandırın ve seçin **Ekle**.

    ![Kaynak ekle](./media/logic-apps-create-deploy-template/addresource.png)


## <a name="deploy-a-logic-app-template"></a>Bir mantıksal uygulama şablonu dağıtma

PowerShell, REST API gibi herhangi bir aracı kullanarak şablonunuzu dağıtabilirsiniz [Visual Studio Team Services yayın Yönetimi](#team-services)ve Azure Portalı aracılığıyla şablon dağıtımı.
Ayrıca, parametrelerin değerlerini depolamak için oluşturduğunuz öneririz bir [parametre dosyası](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Bilgi nasıl [PowerShell ve Azure Resource Manager şablonları kaynaklarla dağıtmak](../azure-resource-manager/resource-group-template-deploy.md) veya [kaynakları Azure Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>OAuth bağlantılarını yetkilendirmek

Dağıtımdan sonra mantıksal uygulama baştan sona geçerli parametrelerle birlikte çalışır.
Ancak, yine geçerli erişim belirtecini oluşturmak için OAuth bağlantıları yetkilendirmeniz gerekir.
OAuth bağlantılarını yetkilendirmek için mantıksal uygulama Logic Apps Tasarımcısı'nda açın ve bu bağlantıları yetkilendirin. Veya otomatik dağıtım için bir komut dosyası her OAuth bağlantı onayı için kullanabilirsiniz.
Github'da altında bir örnek komut dosyası yok [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projesi.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services yayın Yönetimi

Dağıtma ve yönetme bir ortam için yaygın bir senaryo, bir mantıksal uygulama dağıtım şablonu ile Visual Studio Team Services içinde yayın yönetimi gibi bir araç kullanmaktır. Visual Studio Team Services içeren bir [Azure kaynak grubu dağıtma](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) herhangi bir yapı ekleyin veya yayın ardışık düzen görev. Olması gereken bir [hizmet sorumlusu](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) dağıtmak için yetkilendirme sonra yayın tanımı oluşturabilir ve için.

1. Yayın yönetimini seçin **boş** böylece boş bir tanımı oluşturun.

    ![Boş tanımı oluşturun][1]

2. Bunun için büyük olasılıkla el ile veya yapılandırma işleminin bir parçası olarak oluşturulan mantıksal uygulama şablonu dahil olmak üzere gereksinim duyduğunuz tüm kaynakları seçin.
3. Ekleme bir **Azure kaynak grubu dağıtımı** görev.
4. İle yapılandırma bir [hizmet sorumlusu](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)ve şablonu ve şablon parametreleri dosyalarını başvuru.
5. Diğer bir ortam, otomatikleştirilmiş test veya gerektiğinde onaylayanlar için yayın işlemindeki adımlar oluşturmak devam edin.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
