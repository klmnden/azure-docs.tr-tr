---
title: "Senaryo - bir müşteri öngörüleri Panosu Azure sunucusuz ile oluşturma | Microsoft Docs"
description: "Müşteri geri bildirimi, sosyal veriler ve Azure Logic Apps ve Azure işlevleri ile daha fazlasını yönetmek için bir Pano nasıl oluşturulacağına ilişkin bir örnek."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: d3e07b8d7194d83e3ba3986177170edff21e1d7a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Azure işlevleri ve Azure Logic Apps ile gerçek zamanlı müşteri öngörüleri panosu oluşturun

Azure sunucusuz araçları hızlı bir şekilde oluşturmak ve bulut uygulamalarında altyapı hakkında düşünmek zorunda kalmadan barındırmak için güçlü özellikler sağlar.  Bu senaryoda, Power BI veya Azure Data Lake gibi bir kaynak müşteri geri bildirimi tetiklemek, machine learning ile geribildirim çözümlemek ve Öngörüler yayımlamak için bir Pano oluşturacağız.

## <a name="overview-of-the-scenario-and-tools-used"></a>Senaryo ve kullanılan Araçları'na genel bakış

Bu çözümü uygulamak için Azure sunucusuz uygulamalarında iki anahtar bileşenlerinin nden: [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Logic Apps, bulutta bir sunucusuz iş akışı altyapısıdır.  Orchestration sunucusuz bileşenlerinde sağlar ve ayrıca 100'den Hizmetleri ve API bağlar.  Bu senaryoda, müşterilerin görüşleri tetiklemek için bir mantıksal uygulama oluşturacağız.  Müşteri geri bildirimi tepki yardımcı olabilecek bağlayıcılar Outlook.com, Office 365, anket Monkey, Twitter ve bir HTTP isteği bazıları [bir web formundan](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  İş akışı için aşağıdaki biz diyez Twitter'da izlenir.

Bulutta sunucusuz işlem işlevler sağlar.  Bu senaryoda, bir dizi önceden tanımlanmış anahtar sözcükleri dayalı müşterilerden tweet'leri bayrak için Azure işlevleri kullanacağız.

Çözümün tamamında olabilir [Visual Studio'da derleme](logic-apps-deploy-from-vs.md) ve [kaynak şablon bir parçası olarak dağıtılan](logic-apps-create-deploy-template.md).  Ayrıca videosu senaryonun olan [Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a>Müşteri verilerine tetiklemek için mantıksal uygulama oluşturma

Sonra [bir mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md) Visual Studio veya Azure Portalı'nda:

1. İçin bir tetikleyici eklemek **üzerinde yeni Tweet'leri** Twitter gelen
2. Bir anahtar sözcüğü veya diyez tweet'leri dinlemek için tetikleyici yapılandırın.

   > [!NOTE]
   > Tetikleyici yinelenme özellikte yoklama tabanlı tetikleyiciler üzerinde yeni öğeler için mantıksal uygulama hangi sıklıkta denetleyeceğini belirler

   ![Twitter tetikleyici örneği][1]

Bu uygulama artık tüm yeni tweet'leri ateşlenir.  Biz sonra tweet verileri alabilir ve daha fazla ifade düşünceleri anlama.  Bunun için kullandığımız [Azure Bilişsel hizmet](https://azure.microsoft.com/services/cognitive-services/) metnin düşünceleri algılamak için.

1. Tıklatın **yeni adım**
1. Seçin veya arama **metin analizi** Bağlayıcısı
1. Seçin **algılamak düşünceleri** işlemi
1. İstenirse, metin Analytics hizmeti için geçerli bir Bilişsel hizmetler anahtarı sağlayın
1. Ekleme **Tweet metin** çözümlemek için metin olarak.

Biz tweet veri ve Öngörüler üzerinde tweet sahip olduğunuza göre diğer bağlayıcıları çeşitli uygun olabilir:
* Power BI - akış veri kümesine satırları ekleyin: görünümü tweet'leri gerçek zamanlı bir Power BI Panoda.
* Azure Data Lake - dosya ekleme: müşteri verilerini analytics işlerini dahil etmek için bir Azure Data Lake veri kümesini ekleyin.
* SQL - satırları ekleyin: deposundaki verileri sonraki alma için bir veritabanı.
* Boşluk - iletisi gönderin: Eylemler gerektirir olumsuz görüşler slack kanalda uyar.

Bir Azure işlevi, verileri üzerinde daha fazla özel işlem yapmak için de kullanılabilir.

## <a name="enriching-the-data-with-an-azure-function"></a>Bir Azure işlevi verilerle zenginleştirmek

Bir işlev oluşturabilmeniz için önce uygulamamız Azure aboneliğimiz bir işlev uygulamanız gerekir.  Portalda bir Azure işlevi oluşturma ile ilgili ayrıntılar için [bulunabilir burada](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Bir HTTP sağlamak gereken doğrudan mantığı uygulamadan çağrılacak işlev için bağlama tetikler.  Kullanmanızı öneririz **HttpTrigger** şablonu.

Bu senaryoda, istek gövdesini Azure işlevinin tweet metin olacaktır.  Bir anahtar sözcük veya tümcecik tweet metin içeriyorsa, işlev kodu mantığı üzerinde yalnızca tanımlayın.  İşlev kadar basit veya karmaşık senaryo için gerektiği şekilde tutulması.

İşlev sonunda, yalnızca bazı verilerle mantıksal uygulama için bir yanıt döndürür.  Bu basit bir Boole değeri olabilir (örneğin `containsKeyword`), veya karmaşık bir nesne.

![Yapılandırılmış Azure işlevi adım][2]

> [!TIP]
> Karmaşık bir yanıt bir mantıksal uygulama bir işlevden erişirken ayrıştırma JSON eylemini kullanın.

İşlev kaydedildikten sonra yukarıda oluşturduğunuz mantıksal uygulama içine eklenebilir.  Mantıksal uygulama:

1. Eklemek için tıklatın bir **yeni adım**
1. Seçin **Azure işlevleri** Bağlayıcısı
1. Var olan işlevi seçin ve oluşturulan işlevi gözatmak için seçin
1. Gönder **Tweet metin** için **istek gövdesinde**

## <a name="running-and-monitoring-the-solution"></a>Çalıştıran ve izleme çözümü

Logic Apps içinde sunucusuz düzenlemelerin yazma avantajlarını zengin hata ayıklama ve izleme olanakları biridir.  Tüm çalışma (geçerli veya geçmiş) Visual Studio'dan Azure portal ya da SDK'ları ve REST API aracılığıyla görüntülenebilir.

Bir mantıksal uygulama test etmek için en kolay yollarından biri kullanarak **çalıştırmak** Tasarımcısı'nda düğmesi.  Tıklatarak **çalıştırmak** tetikleyici bir olay algılandığında kadar her 5 saniyede yoklamak ve çalışma ilerledikçe dinamik bir görünüm vermek devam eder.

Genel Bakış dikey penceresinde Azure portalında veya Visual Studio Cloud Explorer'ı kullanarak önceki çalıştırma geçmişlerini görüntülenebilir.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Otomatik dağıtımları için dağıtım şablonu oluşturma

Bir çözüm geliştirilen sonra yakalanan ve dünya Azure herhangi bir bölgede bir Azure dağıtım şablonu aracılığıyla dağıtılır.  Bu parametre için de değiştirme bu iş akışı farklı sürümleri için aynı zamanda bu çözümü derleme ve sürüm ardışık düzeninde tümleştirmek için kullanışlıdır.  Bir dağıtım şablonu oluşturma hakkında ayrıntılar bulunabilir [bu makalede](logic-apps-create-deploy-template.md).

Çözümün tamamında tüm bağımlılıkları tek bir şablon olarak yönetilecek şekilde azure işlevleri de dağıtım şablonu - birleştirilebilir.  Bir işlev dağıtım şablonu örneği bulunabilir [Azure Hızlı Başlangıç şablonu deposu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Sonraki adımlar

* [Diğer örnekleri ve senaryoları Azure Logic Apps için bkz.](logic-apps-examples-and-scenarios.md)
* [Bu çözüm uçtan uca Oluşturma videosu izleme](http://aka.ms/logicappsdemo)
* [İşlemek ve bir mantıksal uygulama içinde özel durumlarını yakalama hakkında bilgi edinin](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png