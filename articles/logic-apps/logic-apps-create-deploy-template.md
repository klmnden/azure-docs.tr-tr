---
title: Dağıtım şablonları oluşturmak için Azure Logic Apps | Microsoft Docs
description: Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.date: 10/18/2016
ms.openlocfilehash: ffa619351ca4a4bfd3a812775ee7ff6cd71ddea4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53089710"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma

Mantıksal uygulama oluşturulduktan sonra bir Azure Resource Manager şablonu oluşturmak isteyebilirsiniz.
Böylece, herhangi bir ortam veya kaynak grubu burada gerekebilir mantıksal uygulamayı kolayca dağıtabilirsiniz.
Resource Manager şablonları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md) ve [Azure Resource Manager şablonları kullanarak kaynakları dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Mantıksal uygulama dağıtım şablonu

Bir mantıksal uygulama, üç temel bileşeni vardır:

* **Mantıksal uygulama kaynağı**: fiyatlandırma planı, konum ve iş akışı tanımı gibi birçok şey hakkında bilgiler içerir.
* **İş akışı tanımı**: mantıksal uygulamanızın iş akışı adımları ve Logic Apps altyapısı iş akışının nasıl yürütüleceğini açıklar.
Bu tanım, mantıksal uygulamanızın içinde görüntüleyebilirsiniz **kod görünümü** penceresi.
Mantıksal uygulama kaynağı içinde bu tanımında bulabilirsiniz `definition` özelliği.
* **Bağlantıları**: ayrı bir bağlantı dizesi ve bir erişim belirteci gibi herhangi bir bağlayıcı bağlantısı hakkındaki meta verileri güvenli bir şekilde saklayın kaynaklarını ifade eder.
Mantıksal uygulama kaynağı içinde mantıksal uygulamanız bu kaynaklara başvuran `parameters` bölümü.

Var olan mantıksal uygulamalar'ın bu parçaların tamamı gibi bir araç kullanarak görüntüleyebileceğiniz [Azure kaynak Gezgini](http://resources.azure.com).

Kaynak grubu dağıtımı ile kullanılacak bir mantıksal uygulama için bir şablon yapmak, kaynakları tanımlayan ve gerektikçe parametreleştirin.
Geliştirme, test ve üretim ortamı dağıtıyorsanız, örneğin, büyük olasılıkla bir SQL veritabanı'na farklı bağlantı dizeleri her ortamda kullanmak istediğiniz.
Ya da farklı bir abonelik veya kaynak grupları dağıtmak isteyebilirsiniz.  

## <a name="create-a-logic-app-deployment-template"></a>Mantıksal uygulama dağıtım şablonu oluşturma

Geçerli mantıksal uygulama dağıtım şablonu için en kolay yolu kullanmaktır [Logic Apps için Visual Studio Araçları](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites).
Visual Studio Araçları, herhangi bir abonelik veya konum kullanılabilecek geçerli bir dağıtım şablonu oluşturun.

Mantıksal uygulama dağıtım şablonu oluşturma gibi diğer bazı araçları size yardımcı olabilir.
El ile zaten gerektiğinde parametreleri oluşturmak için burada tartışılan kaynakları kullanarak diğer bir deyişle, yazabilirsiniz.
Başka bir seçenek kullanmaktır bir [mantıksal uygulama şablonu Oluşturucu](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell modülü. Bu açık kaynaklı modül ilk mantıksal uygulama ve bunu kullanıyor ve sonra dağıtım için gerekli parametrelerle şablon kaynakları oluşturur herhangi bir bağlantı değerlendirir.
Örneğin, bir Azure Service Bus kuyruğuna bir ileti alır ve bir Azure SQL veritabanına veri ekleyen bir mantıksal uygulama varsa, araç tüm düzenleme mantığı korur ve böylece sırasında ayarlanabilir dizeleri dağıtma SQL ve Service Bus bağlantı parametreleştiren menüsü.

> [!NOTE]
> Bağlantılar, mantıksal uygulama ile aynı kaynak grubunda olmalıdır.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Mantıksal uygulama şablonu PowerShell modülünü yükleme
Aracılığıyla modülü yüklemek için en kolay yolu olan [PowerShell Galerisi](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), komutunu kullanarak `Install-Module -Name LogicAppTemplate`.  

PowerShell modülünü el ile de yükleyebilirsiniz:

1. En son sürümünü indirin [mantıksal uygulama şablonu Oluşturucu](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. PowerShell modülü klasörünüzde klasörünü ayıklayın (genellikle `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Herhangi bir kiracı ve abonelik erişimi ile çalışmak modülü için belirteç ile kullanmanızı öneririz [ARMClient](https://github.com/projectkudu/ARMClient) komut satırı aracı.  Bu [blog gönderisi](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient daha ayrıntılı olarak ele alınmaktadır.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>PowerShell kullanarak bir mantıksal uygulama şablonunu oluşturma
PowerShell'i yükledikten sonra aşağıdaki komutu kullanarak bir şablon oluşturabilirsiniz:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` Azure abonelik kimliğidir. Bu satır, ilk bir erişim belirteci ARMClient alır sonra PowerShell betiğine kanallar aracılığıyla ve ardından şablonu bir JSON dosyası oluşturur.

## <a name="add-parameters-to-a-logic-app-template"></a>Parametre için bir mantıksal uygulama şablonunu ekleyin
Mantıksal uygulama şablonunu oluşturduktan sonra eklemek veya gerek duyabileceğiniz parametreleri değiştirmek devam edebilirsiniz. Örneğin, bir kaynak kimliği için bir Azure işlevine veya tek bir dağıtımda dağıtmayı planladığınız iç içe geçmiş iş akışı tanımınızı içeriyorsa, daha fazla kaynak şablonunuza eklemek ve kimlikleri gerektikçe parametreleştirin. Aynı özel API'ler veya Swagger yönelik tüm başvuruları her bir kaynak grubuyla dağıtmayı beklediğiniz uç noktalar için geçerlidir.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Bağımlı kaynaklar için başvuruları için Visual Studio dağıtım şablonları ekleme

Mantıksal uygulamanızın bağımlı kaynaklar başvurmak istediğinizde kullanabilirsiniz [Azure Resource Manager şablonu işlevleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) mantıksal uygulama dağıtım şablonunuzdaki. Örneğin, mantıksal uygulamanızın mantıksal uygulamanızla dağıtmak istediğiniz bir Azure işlevi veya tümleştirme hesabı başvurusu isteyebilirsiniz. Logic Apps Tasarımcısı'nı doğru şekilde işlediğinden emin parametreleri, dağıtım şablonunuzda kullanma hakkında aşağıdaki yönergeleri izleyin. 

Mantıksal uygulama Tetikleyicileri ve eylemleri bu tür parametrelerinde kullanabilirsiniz:

*   Alt iş akışı
*   İşlev uygulaması
*   APIM çağrısı
*   API bağlantısı çalışma zamanı URL'si
*   API bağlantı yolu

Ve parametreler, değişkenleri, ResourceId, concat vb. gibi şablon işlevleri kullanabilirsiniz. Örneğin, işte Azure işlevi kaynak Kimliğini nasıl değiştirebilir:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

Ve burada parametrelerini kullanın:

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
Başka bir örnek olarak Service Bus ileti gönderme işlemini parametreleştirebilirsiniz:

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
> host.runtimeUrl isteğe bağlıdır ve varsa şablonunuzdan kaldırılabilir.
> 


> [!NOTE] 
> Mantıksal uygulama parametrelerini kullanırken çalışmaya Tasarımcısı için örneğin varsayılan değerler sağlamanız gerekir:
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

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Mantıksal uygulamanız için mevcut bir kaynak grubu projesi ekleme

Mevcut bir kaynak grubu projesi varsa, mantıksal uygulamanızın JSON ana hattı penceresinin projede ekleyebilirsiniz. Daha önce oluşturduğunuz uygulama yanı sıra başka bir mantıksal uygulama da ekleyebilirsiniz.

1. `<template>.json` dosyasını açın.

2. JSON ana hattı penceresinin açmak için Git **görünümü** > **diğer Windows** > **JSON ana hattı**.

3. Şablon dosyasına bir kaynak eklemek için tıklatın **kaynak Ekle** JSON ana hattı penceresinin üst kısmındaki. JSON ana hattı penceresinin içinde sağ tıklatın **kaynakları**seçip **yeni kaynak Ekle**.

    ![JSON ana hattı penceresinin](./media/logic-apps-create-deploy-template/jsonoutline.png)
    
4. İçinde **kaynak Ekle** bulun ve seçin iletişim kutusu **mantıksal uygulama**. Mantıksal uygulamanızı adlandırın ve seçin **Ekle**.

    ![Kaynak ekle](./media/logic-apps-create-deploy-template/addresource.png)


## <a name="deploy-a-logic-app-template"></a>Bir mantıksal uygulama şablonunu dağıtma

PowerShell, REST API gibi herhangi bir aracı kullanarak, şablonunuzu dağıtmak [Azure DevOps Azure işlem hatları](#team-services)ve Azure portalı üzerinden şablon dağıtımı.
Ayrıca, parametreleri için değerleri depolamak için oluşturduğunuz öneririz bir [parametre dosyası](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Bilgi edinmek için nasıl [kaynakları Azure Resource Manager şablonları ve PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md) veya [kaynakları Azure Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>OAuth bağlantıları yetkilendirin

Dağıtımdan sonra mantıksal uygulama için uçtan uca geçerli parametreler ile çalışır.
Ancak, yine de geçerli erişim belirteci oluşturmak için OAuth bağlantıları yetkilendirmeniz gerekir.
OAuth bağlantılarını yetkilendirmek için Logic Apps Tasarımcısı'nda mantıksal uygulama açın ve bu bağlantıları yetkilendirin. Veya otomatik dağıtım için her OAuth bağlantısı için onay için bir komut dosyası kullanabilirsiniz.
Altında github'daki bir örnek betiği yoktur [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) proje.

<a name="team-services"></a>
## <a name="azure-devops-azure-pipelines"></a>Azure DevOps Azure işlem hatları

Dağıtma ve bir ortamı yönetmek için yaygın bir senaryo, bir mantıksal uygulama dağıtım şablonu ile Azure DevOps, Azure işlem hatları gibi bir araçla kullanmaktır. Azure DevOps içeren bir [Azure kaynak grubu dağıtma](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) eklemek için herhangi bir derleme veya yayın işlem hattı görev. İhtiyacınız bir [hizmet sorumlusu](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) için dağıtmak için yetkilendirme ve ardından, yayın ardışık düzeni oluşturabilir.

1. Azure işlem hatlarında, seçin **boş** böylece boş bir işlem hattı oluşturun.

    ![Boş bir işlem hattı oluşturma][1]

2. Bunun için büyük olasılıkla el ile veya yapı işleminin bir parçası olarak oluşturulan mantıksal uygulama şablonunu dahil olmak üzere ihtiyacınız olan tüm kaynakları seçin.
3. Ekleme bir **Azure kaynak grubu dağıtımı** görev.
4. Yapılandırma ile bir [hizmet sorumlusu](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)ve şablon ve şablon parametreleri dosya başvurusu.
5. Diğer bir ortam, otomatikleştirilmiş test veya gerektiğinde onaylayanlar için yayın işlemindeki adımları oluşturmak için devam edin.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
