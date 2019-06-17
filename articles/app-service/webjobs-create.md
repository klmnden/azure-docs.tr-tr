---
title: -Azure App Service WebJobs ile arka plan görevleri çalıştırma
description: Azure App Service web apps, API uygulamaları veya mobile apps arka plan görevleri çalıştırmak için WebJobs'ı kullanmayı öğrenin.
services: app-service
documentationcenter: ''
author: ggailey777
manager: jeconnoc
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: glenga;msangapu;david.ebbo;suwatch;pbatum;naren.soni;
ms.custom: seodec18
ms.openlocfilehash: 0f2053e978b7c890f4e175515ed54f69694950c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60833579"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Azure uygulama Hizmeti'nde WebJobs ile arka plan görevleri çalıştırma

## <a name="overview"></a>Genel Bakış
WebJobs'ın bir özelliğidir [Azure App Service](https://docs.microsoft.com/azure/app-service/) programları veya betikleri bir web uygulaması, API uygulaması veya mobil uygulama olarak aynı bağlamda çalıştırmanızı sağlayan. WebJobs'ı kullanmak için hiçbir ek ücret yoktur.

> [!IMPORTANT]
> Web işleri, Linux'ta App Service için henüz desteklenmiyor.

Bu makalede kullanarak Web işleri dağıtma işlemi gösterilmektedir [Azure portalında](https://portal.azure.com) bir yürütülebilir veya betik yüklenecek. Geliştirin ve Visual Studio kullanarak Web işleri dağıtma hakkında daha fazla bilgi için bkz. [Visual Studio kullanarak Web işleri dağıtma](webjobs-dotnet-deploy-vs.md).

Azure WebJobs SDK ile WebJobs, birçok programlama görevlerini basitleştirmek için kullanılabilir. Daha fazla bilgi için [WebJobs SDK nedir](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure işlevleri, programları ve betikleri çalıştırmak için başka bir yol sağlar. WebJobs ve işlevler arasında bir karşılaştırma için bkz. [Flow, Logic Apps, İşlevler ve Web işleri arasında seçim yapma](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>WebJob türü

Aşağıdaki tablo arasındaki farkları açıklar *sürekli* ve *tetiklenen* WebJobs.


|Sürekli  |Tetiklenen  |
|---------|---------|
| Hemen bir Web işi oluşturulduğunda çalışmaya başlar. İş öğesinden bitiş tutmak için program veya komut dosyası genellikle sonsuz bir döngü içinde yapar. İşin bitiş, yeniden başlatabilirsiniz. | Yalnızca el ile veya bir zamanlamaya göre tetiklenen başlatılır. |
| Web uygulamasının üzerinde çalıştığı tüm örneklerinde çalıştırılır. İsteğe bağlı olarak, tek örnekli bir WebJob kısıtlayabilirsiniz. |Yük Dengeleme için Azure'ı seçer, tek bir örneği üzerinde çalışır.|
| Uzaktan hata ayıklamayı destekler. | Uzaktan hata ayıklamayı desteklemiyor.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="acceptablefiles"></a>Betikleri veya programları için desteklenen dosya türleri

Aşağıdaki dosya türlerinde desteklenir:

* .cmd, .bat, .exe (Windows cmd kullanarak)
* .ps1 (PowerShell kullanarak)
* .sh (Bash kullanarak)
* .php (PHP kullanarak)
* .py (Python kullanarak)
* .js (Node.js kullanarak)
* .jar (Java kullanarak)

## <a name="CreateContinuous"></a> Sürekli bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portalında](https://portal.azure.com)Git **App Service** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![WebJobs'ı seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında **Ekle**.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtilen ayarları.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myContinuousWebJob | Bir App Service uygulaması içinde benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_". |
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* programları veya betikleri çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir veya betik dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Türü** | Sürekli | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Ölçeklendirme** | Çoklu örnek | Yalnızca sürekli WebJobs için kullanılabilir. Bir programı veya betiği tüm çalışıp çalışmayacağını belirler örnek veya yalnızca bir örnek. Birden çok örnek üzerinde çalıştırma seçeneği ücretsiz veya paylaşılan uygulanmaz [fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). | 

4. **Tamam**'ı tıklatın.

   Yeni bir WebJob görünür **WebJobs** sayfası.

   ![WebJobs listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Sürekli bir WebJob'ı yeniden başlatmak veya durdurmak için listeyi listeden Webjob'a sağ tıklayın ve **Durdur** veya **Başlat**.

    ![Sürekli bir WebJob Durdur](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="CreateOnDemand"></a> El ile tetiklenen bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portalında](https://portal.azure.com)Git **App Service** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![WebJobs'ı seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında **Ekle**.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtilen ayarları.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myTriggeredWebJob | Bir App Service uygulaması içinde benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_".|
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* programları veya betikleri çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir veya betik dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Türü** | Tetiklenen | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Tetikleyiciler** | Manual | |

4. **Tamam**'ı tıklatın.

   Yeni bir WebJob görünür **WebJobs** sayfası.

   ![WebJobs listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. Webjob'ı çalıştırmak için listenin adını sağ tıklatıp **çalıştırma**.
   
    ![WebJob çalıştırma](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateScheduledCRON"></a> Zamanlanmış bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portalında](https://portal.azure.com)Git **App Service** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![WebJobs'ı seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında **Ekle**.

   ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtilen ayarları.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myScheduledWebJob | Bir App Service uygulaması içinde benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_". |
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* programları veya betikleri çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir veya betik dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Türü** | Tetiklenen | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Tetikleyiciler** | Zamanlanmış | Güvenilir bir şekilde çalışmak üzere zamanlamak için her zaman açık özelliği etkinleştirin. Always On yalnızca temel, standart ve Premium ücretlendirme katmanları için kullanılabilir.|
   | **CRON ifadesi** | 0 0/20 * * * * | [Sıralanmış iş ifadeleri](#cron-expressions) aşağıdaki bölümde açıklanmıştır. |

4. **Tamam** düğmesine tıklayın.

   Yeni bir WebJob görünür **WebJobs** sayfası.

   ![WebJobs listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="cron-expressions"></a>Sıralanmış iş ifadeleri

Girebileceğiniz bir [CRON ifadesi](../azure-functions/functions-bindings-timer.md#cron-expressions) portalında veya dahil bir `settings.job` WebJob'ınıza köküne dosya *.zip* dosya, aşağıdaki örnekte olduğu gibi:

```json
{
    "schedule": "0 */15 * * * *"
}
```

Daha fazla bilgi için bkz. [Tetiklenmiş bir Web işi zamanlaması](webjobs-dotnet-deploy-vs.md#scheduling-a-triggered-webjob).

## <a name="ViewJobHistory"></a> İş geçmişini görüntüleme

1. Webjob'ı için geçmişini görebilir ve ardından istediğiniz seçin **günlükleri** düğmesi.
   
   ![Günlükleri düğmesi](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. İçinde **WebJob ayrıntıları** sayfasında, bir çalıştırma ayrıntılarını görmek için bir saat seçin.
   
   ![WebJob ayrıntıları](./media/web-sites-create-web-jobs/webjobdetails.png)

3. İçinde **WebJob çalıştırma ayrıntıları** sayfasında **çıkışı Aç/Kapat** metnini günlük içeriklerini görmek için.
   
    ![Webjob çalıştırma ayrıntıları](./media/web-sites-create-web-jobs/webjobrundetails.png)

   Çıkış metnini ayrı bir tarayıcı penceresinde görmek için seçin **indirme**. Metni indirmek için sağ **indirme** ve tarayıcı seçeneklerinizi kullanarak dosya içeriklerini kaydedin.
   
5. Seçin **WebJobs** WebJobs listesine gitmek için sayfanın üstündeki içerik haritası bağlantısı.

    ![WebJob içerik haritası](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![WebJobs listesinde geçmişi Panosu](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="NextSteps"></a> Sonraki adımlar

Azure WebJobs SDK ile WebJobs, birçok programlama görevlerini basitleştirmek için kullanılabilir. Daha fazla bilgi için [WebJobs SDK nedir](https://github.com/Azure/azure-webjobs-sdk/wiki).
