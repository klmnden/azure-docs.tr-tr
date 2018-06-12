---
title: Sunucusuz senaryo - Azure ile bir müşteri öngörüleri panosu oluşturun | Microsoft Docs
description: Müşteri geri bildirimi, sosyal medya veri ve diğer Azure Logic Apps ve Azure işlevleri içeren bir müşteri Pano oluşturma tarafından nasıl yönetebilmeniz için bilgi edinin
keywords: ''
services: logic-apps
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jehollan; LADocs
ms.openlocfilehash: 3ee3ec3107cf8aad834e8201405c9aa833d838af
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299969"
---
# <a name="create-a-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Azure işlevleri ve Azure Logic Apps ile bir akış müşteri öngörüleri panosu oluşturun

Azure yardımcı sunucusuz araçları hızlı bir şekilde bulutta, yapı ve ana bilgisayar uygulamaları altyapı hakkında düşünmek zorunda kalmadan sunar. Bu öğreticide, müşteri geri bildirimi tetikler, machine learning ile geribildirim analiz eden ve Power BI veya Azure Data Lake gibi bir kaynak Öngörüler yayımlar bir Pano oluşturabilirsiniz.

Bu çözüm için sunucusuz uygulamalar için bu anahtar Azure bileşenleri kullan: [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Azure mantıksal uygulamaları düzenlemelerin sunucusuz bileşenlerinde oluşturabilir ve 200 + Hizmetleri ve API'ları bağlamak bulutta sunucusuz iş akışı altyapısı sağlar. Azure işlevleri sağlayan sunucusuz bulut bilgi işlem. Bu çözüm, önceden tanımlanmış anahtar sözcükleri temel alarak müşteri tweet'leri bayrak eklemek için Azure işlevleri kullanır.

Bu senaryoda, müşterilerden gelen geri bildirim bulunması tetikleyen bir mantıksal uygulama oluşturun. Müşteri geri bildirimine yanıt Yardım içerdiğini Outlook.com, Office 365, anket Monkey, Twitter, bazı bağlayıcılar ve bir [web formu HTTP isteğinden](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/). Oluşturduğunuz iş akışı diyez Twitter'da izler.

Yapabilecekleriniz [çözümün tamamında Visual Studio'da derleme](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) ve [Azure Resource Manager şablonu ile çözümü dağıtmak](../logic-apps/logic-apps-create-deploy-template.md). Bu çözüm oluşturmayı gösteren bir videosu için [bu Channel 9 video izlemeyi](http://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Müşteri verilerine Tetikle

1. Azure portal ya da Visual Studio boş mantıksal uygulama oluşturma. 

   Logic apps yeniyseniz, gözden [Azure portalı için Hızlı Başlangıç](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [Visual Studio için Hızlı Başlangıç](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

2. Mantıksal Uygulama Tasarımcısı'nda bulun ve bu eylem sahip Twitter tetikleyici ekleyin: **yeni tweet zaman nakledilir**

3. Bir anahtar sözcüğü veya diyez göre tweet'leri dinlemek için tetikleyici ayarlayın.

   Twitter tetikleyici gibi yoklama tabanlı tetikleyiciler üzerindeki mantıksal uygulama için yeni öğeler ne sıklıkta denetleyeceğini yineleme özelliği belirler.

   ![Twitter tetikleyici örneği][1]

Bu mantıksal uygulama artık tüm yeni tweet'leri ateşlenir. Ardından alabilir ve böylece ifade düşüncelerin daha iyi anlayabilirsiniz tweet verileri analiz etmek. 

## <a name="analyze-tweet-text"></a>Tweet metin çözümleme

Bazı metinleri arkasında düşünceleri algılamak için kullanabileceğiniz [Azure Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/).

1. Tetikleyici altında mantığı Uygulama Tasarımcısı'nda seçin **yeni adım**.

2. Bul **metin analizi** bağlayıcı.

3. Seçin **algılamak düşünceleri** eylem.

4. İstenirse, metin Analytics hizmeti için geçerli bir Bilişsel hizmetler anahtarı sağlayın.

5. Altında **iste gövde**seçin **Tweet metin** giriş olarak analize tweet metin sağlayan alan.

Tweet hakkında Öngörüler ve tweet verileri aldıktan sonra şimdi bazı ilgili bağlayıcılar ve eylemlerini kullanabilirsiniz:

* **Power BI - satır akış veri kümesine ekleme**: Power BI Panosu üzerinde gelen tweet'leri görüntüleyin.
* **Azure Data Lake - dosya ekleme**: müşteri verilerini analytics işlerini dahil etmek için bir Azure Data Lake veri kümesini ekleyin.
* **SQL - satır ekleme**: deposundaki verileri sonraki alma için bir veritabanı.
* **Boşluk - iletisi gönderin**: eylemi gerektiren olumsuz görüşler hakkında Slack bir kanal bildirin.

Azure işlev özel işleme verilerinizde gerçekleştirebilmeleri için ve ayrıca oluşturabilirsiniz. 

## <a name="process-data-with-azure-functions"></a>Azure işlevleriyle işlem verileri

Bir işlev oluşturmadan önce Azure aboneliğinizde bir işlev uygulaması oluşturun. Ayrıca, mantıksal uygulamanızı doğrudan bir işlevi çağırmak işlevi tetikleyin bağlama, örneğin, kullanmak bir HTTP olmalıdır **HttpTrigger** şablonu. Bilgi [Azure portalında işlevi ve ilk işlev uygulaması oluşturmak nasıl](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Bu senaryo için tweet metin Azure işlevinizi için istek gövdesi olarak kullanın. İşlev kodunuzda tweet metin bir anahtar sözcük veya tümcecik içerip içermediğini belirler mantığı tanımlayın. Basit veya karmaşık senaryo için gerekli olarak işlevini saklayın.
İşlevi sonunda, örneğin bazı verilerle mantıksal uygulama için bir yanıt döndürür, basit bir boolean değeri gibi `containsKeyword` veya karmaşık bir nesne.

> [!TIP]
> Bir mantıksal uygulama bir işlevden karmaşık yanıt erişmek için **ayrıştırma JSON** eylem.

İşiniz bittiğinde işlevi kaydedin ve ardından, oluşturduğunuz mantıksal uygulama içinde eylem olarak işlevi ekleyin.

## <a name="add-azure-function-to-logic-app"></a>Mantıksal uygulama için Azure işlevi ekleme

1. Mantıksal Uygulama Tasarımcısı'nda altında **algılamak düşünceleri** eylemi seçin **yeni adım**.

2. Bul **Azure işlevleri** Bağlayıcısı ve oluşturduğunuz işlevi seçin.

3. Altında **iste gövde**seçin **Tweet metin**.

![Yapılandırılmış Azure işlevi adım][2]

## <a name="run-and-monitor-your-logic-app"></a>Çalıştırın ve mantıksal uygulamanızı izleme

Tüm geçerli veya önceki çalıştırmaları mantıksal uygulamanız için gözden geçirmek için hata ayıklama ve Visual Studio, Azure portalında veya SDK'ları ve Azure REST API'leri aracılığıyla Azure Logic Apps sağlayan özellikleri izleme zengin kullanabilirsiniz.

Kolayca mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı test etmek için tercih **tetikleyici çalıştırmak**. Tetikleyici için tweet'leri ölçütlerinizi karşılayan tweet bulunana kadar belirtilen bir zamanlamaya göre yoklar. Çalışma sürerken Tasarımcısı'nı çalıştıran için dinamik bir görünümü gösterir.

Önceki görünüme geçmişlerini Visual Studio veya Azure Portalı'nı çalıştırın: 

* Visual Studio Cloud Explorer'ı açın. Mantıksal uygulamanızı bulmak, uygulamanın kısayol menüsünü açın. Seçin **açık çalıştırma geçmişi**.

* Azure portalda mantıksal uygulamanızı bulun. Mantığı uygulamanızın menüsünde, **genel bakış**. 

## <a name="create-automated-deployment-templates"></a>Otomatik dağıtım şablonları oluşturma

Bir mantıksal uygulama çözüm oluşturduktan sonra yakalama ve uygulamanızı olarak dağıtma bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md#template-deployment) herhangi bir Azure bölgesine dünyadaki. Hem uygulamanızın farklı sürümlerini oluşturmak ve bir derleme çözümünüzü tümleştirmek için parametreleri değiştirin ve ardışık düzen sürüm için bu özelliği kullanın. Çözümün tamamında tüm bağımlılıkları tek bir şablon olarak yönetebilmeniz için Azure işlevleri dağıtım şablonunuzda de içerebilir. Bilgi [mantığı uygulama dağıtım şablonlarının nasıl oluşturulacağı](../logic-apps/logic-apps-create-deploy-template.md).

Bir Azure işlevi ile bir örnek dağıtım şablonu için denetleyin [Azure Hızlı Başlangıç şablonu deposu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Sonraki adımlar

* [Diğer örnekleri ve senaryoları için Azure mantıksal uygulamaları Bul](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png