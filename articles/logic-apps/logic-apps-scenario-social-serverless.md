---
title: Sunucusuz senaryo - Azure Hizmetleri ile müşteri öngörüleri panosu oluşturun | Microsoft Docs
description: Müşteri geri bildirimi, sosyal medya verileri ve diğer Azure Logic Apps ve Azure işlevleri ile müşteri Pano oluşturma tarafından yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: estfan, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: 638b29dd2a15d0467c41e20ecfed9f333b34c04d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60508040"
---
# <a name="create-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Azure Logic Apps ve Azure işlevleri ile akış müşteri öngörüleri panosu oluşturma

Azure tekliflerini [sunucusuz](https://azure.microsoft.com/solutions/serverless/) hızlı bir şekilde yardımcı olacak araçlar oluşturmanıza ve barındırmanıza uygulamalarını bulutta, altyapı konusunda düşünmek zorunda kalmadan. Bu öğreticide, müşteri geri bildirimi tetikler, machine learning ile geri bildirim analiz eder ve Power BI veya Azure Data Lake gibi bir kaynaktaki ınsights yayımlar bir Pano oluşturabilirsiniz.

Bu çözüm için sunucusuz uygulamalar için bu anahtarı Azure bileşenlerini kullanın: [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Azure Logic Apps sunucusuz bileşenlerinde düzenlemeleri oluşturun ve 200'den fazla Hizmetleri ve API'ler bağlanmak bulutta bir sunucusuz bir iş akışı altyapısı sağlar. Azure işlevleri, sunucusuz bulut bilgi işlem sağlar. Bu çözüm, önceden tanımlanmış bir anahtar sözcüklere göre müşteri tweetleri bayrak eklemek için Azure işlevleri kullanır.

Bu senaryoda, müşterilerin geri bildirim bulmaya Tetikleyiciler bir mantıksal uygulama oluşturun. Bazı bağlayıcılar, müşteri geri bildirimlerini yanıt Yardım Outlook.com, Office 365, Twitter, anket Monkey, içerir ve bir [web formu HTTP isteğinden](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/). Oluşturduğunuz iş akışı bir diyez etiketi Twitter'da izler.

Yapabilecekleriniz [tüm çözümünü Visual Studio'da derleme](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) ve [Azure Resource Manager şablonu ile bir çözüm dağıtma](../logic-apps/logic-apps-create-deploy-template.md). Bu çözümün nasıl oluşturulacağını gösteren video kılavuz [bu Channel 9 videosunu izleyin](https://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Müşteri verilerinizi tetikleyin

1. Azure portalı ya da Visual Studio, boş bir mantıksal uygulama oluşturun. 

   Logic apps kullanmaya yeni başladıysanız gözden [Hızlı Başlangıç için Azure portalında](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [Hızlı Başlangıç için Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

2. Mantıksal Uygulama Tasarımcısı'nda bulun ve bu eylemi olan bir Twitter tetikleyicisi ekleyin: **Yeni bir tweet gönderildiğinde**

3. Bir anahtar sözcüğü veya diyez etiketi dayalı tweetleri dinlemek için tetikleyici ayarlayın.

   Twitter tetikleyicisi gibi yoklama temel alan Tetikleyicileri üzerinde yineleme özelliği, mantıksal uygulama için yeni öğeleri ne sıklıkla denetleyeceğini belirler.

   ![Twitter tetikleyicisi örneği][1]

Bu mantıksal uygulama, artık tüm tweet'leri tetikler. Ardından, alın ve ifade yaklaşımları daha iyi anlayabilmeniz tweet verileri analiz edin. 

## <a name="analyze-tweet-text"></a>Tweet metni analiz edin

Metin arkasında duyarlılığını algılamak için kullanabileceğiniz [Azure Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/).

1. Logic Apps Tasarımcısı'nda tetikleyicinin altında seçin **yeni adım**.

2. Bulma **metin analizi** bağlayıcı.

3. Seçin **duygu algılama** eylem.

4. İstenirse, metin analizi hizmeti için geçerli bir Bilişsel hizmetler anahtarı sağlayın.

5. Altında **istek gövdesi**seçin **Tweet metni** alanı tweet metni, analiz için giriş olarak sağlar.

Tweet ile ilgili Öngörüler ve tweet verileri aldıktan sonra artık birden fazla uygun bağlayıcılar ve eylemlerinin kullanabilirsiniz:

* **Power BI - akış veri kümesi için satır ekleme**: Gelen tweetleri bir Power BI Panoda görüntüleyin.
* **Azure Data Lake - dosyasına**: Müşteri verilerinin analytics işlerinde dahil etmek için bir Azure Data Lake veri kümesi ekleyin.
* **SQL - Satır Ekle**: Daha sonra almak için bir veritabanında veri Store.
* **Slack - ileti gönderme**: Bir Slack kanalına negatif eylemi gerektiren geri bildirimler hakkında bilgilendirin.

Oluşturabilirsiniz ve böylece özel işleme, verilerinizde gerçekleştirebileceğiniz bir Azure işlevi. 

## <a name="process-data-with-azure-functions"></a>Azure işlevleri ile verilerini işleme

Bir işlev oluşturmadan önce Azure aboneliğinizde bir işlev uygulaması oluşturun. Ayrıca, mantıksal uygulamanızı doğrudan bir işlevi çağırmak bir HTTP tetikleyicisi bağlama, örneğin, kullanın işlevinin olmalıdır **HttpTrigger** şablonu. Bilgi [Azure portalında işlev ve ilk işlev uygulaması oluşturma işlemini](../azure-functions/functions-create-first-azure-function-azure-portal.md).

Bu senaryo için Azure işleviniz için istek gövdesi olarak tweet metni kullanın. İşlev kodunuzu tweet metni, bir anahtar sözcük veya tümcecik içerip içermediğini belirler mantıksal tanımlayın. İşlevi, basit veya karmaşık senaryo için gerekli olarak tutun.
İşlevin sonunda, örneğin bir mantıksal uygulama bazı verilerle yanıt döndürür, basit bir boolean değeri gibi `containsKeyword` veya karmaşık bir nesne.

> [!TIP]
> Mantıksal uygulama bir işlevden karmaşık bir yanıt erişmek için **JSON Ayrıştır** eylem.

İşiniz bittiğinde işlevi kaydedin ve ardından oluşturduğunuz mantıksal uygulama içinde eylem olarak işlevi ekleyin.

## <a name="add-azure-function-to-logic-app"></a>Mantıksal uygulama için Azure işlevi Ekle

1. Mantıksal Uygulama Tasarımcısı'nda altında **duygu algılama** eylemi seçin **yeni adım**.

2. Bulma **Azure işlevleri** bağlayıcı ve oluşturduğunuz işlevi seçin.

3. Altında **istek gövdesi**seçin **Tweet metni**.

![Yapılandırılmış bir Azure işlevi adım][2]

## <a name="run-and-monitor-your-logic-app"></a>Çalıştırma ve mantıksal uygulamanızı izleme

Mantıksal uygulamanız için tüm geçerli veya önceki çalıştırmaları gözden geçirmek için zengin hata ayıklama ve izleme Azure Logic Apps, Visual Studio, Azure portalında veya Azure REST API'leri ve SDK'ları aracılığıyla sağlar özelliklerini kullanabilirsiniz.

Mantıksal uygulamanızı Logic Apps Tasarımcısı'nda kolayca test edin, tercih **tetikleyici çalıştırması**. Tetikleyici için ölçütlerinizi karşılayan bir tweet bulunana kadar belirtilen bir zamanlamaya göre tweetleri yoklar. Çalıştırma sürerken bu çalıştırma için dinamik bir Görünüm Tasarımcısı'nda gösterilir.

Visual Studio veya Azure portalında, önceki görünüme geçmişleri çalıştırın: 

* Visual Studio Cloud Explorer'ı açın. Mantıksal uygulamanızı bulun, uygulamanın kısayol menüsünü açın. Seçin **açık çalıştırma geçmişini**.

* Azure portalında mantıksal uygulamanızı bulun. Mantıksal uygulama menüsünde, **genel bakış**. 

## <a name="create-automated-deployment-templates"></a>Otomatik dağıtım şablonları oluşturma

Bir mantıksal uygulama çözümünü oluşturduktan sonra yakalama ve uygulamanızı olarak dağıtma bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md#template-deployment) dünyanın herhangi bir Azure bölgesine. Uygulamanızın farklı sürümlerini oluşturmak ve çözümünüzü Azure işlem hatlarıyla tümleştirmek için parametreleri değiştirmek için hem de bu özelliği kullanabilirsiniz. Tüm bağımlılıkları olan tüm çözümü tek bir şablon olarak yönetebilmeniz için Azure işlevleri, dağıtım şablonunuza ekleyebilirsiniz. Bilgi [mantıksal uygulama dağıtım şablonları oluşturma](../logic-apps/logic-apps-create-deploy-template.md).

Bir Azure işlevi ile bir örnek dağıtım şablonu için [Azure Hızlı Başlangıç şablonu depo](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Sonraki adımlar

* [Diğer örnekler ve senaryolar için Azure Logic Apps bulun](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
