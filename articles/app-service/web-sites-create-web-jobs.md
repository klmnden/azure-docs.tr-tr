---
title: Azure App Service'te Web işleri ile arka plan görevleri Çalıştır
description: Web işleri Azure App Service web apps, API uygulamaları veya mobile apps arka plan görevleri çalıştırmak için nasıl kullanılacağını öğrenin.
services: app-service
documentationcenter: ''
author: tdykstra
manager: erikre
editor: jimbe
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: f41cc83bfb18146e46e7d8501318acd68ce9c421
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30231111"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Azure App Service'te Web işleri ile arka plan görevleri Çalıştır

## <a name="overview"></a>Genel Bakış
Web işleri özelliğidir [Azure App Service](https://docs.microsoft.com/azure/app-service/) bir web uygulaması, API uygulaması veya mobil uygulama olarak aynı bağlamda bir program veya komut dosyasını çalıştırmak etkinleştirir. Web işleri kullanmak için ek bir maliyet yoktur.

Bu makalede, Web işleri kullanarak dağıtmak gösterilmiştir [Azure portal](https://portal.azure.com) yürütülebilir dosya veya komut dosyasını karşıya yüklemek için. Geliştirme ve Visual Studio kullanarak Web işleri dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio'yu kullanarak Web işleri dağıtmak](websites-dotnet-deploy-webjobs.md).

Azure WebJobs SDK ile Web işleri pek çok programlama görevlerini basitleştirmek için kullanılabilir. Daha fazla bilgi için bkz: [WebJobs SDK nedir](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure işlevleri, programları ve betikleri çalıştırmak için başka bir yol sağlar. Web işleri ve işlevleri arasında bir karşılaştırma için bkz: [akış, Logic Apps, İşlevler ve Web işleri arasında seçim yapma](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>Web işi türleri

Aşağıdaki tabloda arasındaki farklar açıklanmaktadır *sürekli* ve *tetiklenen* WebJobs.


|Sürekli  |Tetiklenmiş  |
|---------|---------|
| Web işi oluşturulduktan hemen başlar. İş öğesinden bitiş tutmak için program veya komut dosyası genellikle kendi sonsuz bir döngüde içinde çalışır. İş sonlandırırsanız yeniden başlatabilirsiniz. | Yalnızca el ile veya bir zamanlamaya göre tetiklendiğinde başlatır. |
| Web uygulaması üzerinde çalışan tüm örneklerinde çalışır. İsteğe bağlı olarak, tek örnekli bir Web işi kısıtlayabilirsiniz. |Yük Dengeleme için Azure seçer tek bir örneğinde çalışır.|
| Uzaktan hata ayıklama destekler. | Uzaktan hata ayıklama desteklemiyor.|

> [!NOTE]
> Bir web uygulaması 20 dakika işlem yapılmadığında zaman aşımı olabilir. Yalnızca istekleri scm (dağıtım) sitesine veya portal web uygulamanızın sayfalarında Zamanlayıcı sıfırlayın. Gerçek site isteklerine Zamanlayıcı sıfırlamayı yok. Uygulamanızı sürekli çalışan ya da zamanlanmış Web işleri etkinleştirirseniz **her zaman açık** WebJobs güvenilir bir şekilde çalıştığından emin olmak için. Bu özellik yalnızca temel, standart ve Premium kullanılabilir [fiyatlandırma katmanlarına](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="acceptablefiles"></a>Program veya komut dosyaları için desteklenen dosya türleri

Aşağıdaki dosya türleri desteklenir:

* .cmd, .bat, .exe (using Windows cmd)
* .ps1 (using PowerShell)
* .sh (Bash kullanarak)
* .php (PHP kullanarak)
* .py (Python kullanarak)
* .js (Node.js kullanarak)
* .jar (using Java)

## <a name="CreateContinuous"></a> Sürekli bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portal](https://portal.azure.com)gidin **uygulama hizmeti** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![Web işleri seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında, **Ekle**.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtildiği gibi ayarlar.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myContinuousWebJob | İçinde bir uygulama hizmeti uygulamayı benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_". |
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* program veya komut dosyasını çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir dosya veya komut dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Tür** | Sürekli | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Ölçeklendirme** | Çok örnekli | Yalnızca sürekli Webjob'lar için kullanılabilir. Program veya komut dosyası tüm çalışıp çalışmayacağını belirler örneği veya sadece bir örnek. Birden çok örneği üzerinde çalıştırmaya yönelik seçeneği ücretsiz veya paylaşılan uygulanmaz [fiyatlandırma katmanlarına](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). | 

4. **Tamam**’a tıklayın.

   Yeni Web işi kasasındaki **WebJobs** sayfası.

   ![Web işleri listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Sürekli Web işi yeniden başlatmak veya durdurmak için Webjob'a sağ tıklatıp **durdurmak** veya **Başlat**.

    ![Bir sürekli Webjob'un Durdur](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="CreateOnDemand"></a> El ile Tetiklenmiş WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portal](https://portal.azure.com)gidin **uygulama hizmeti** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![Web işleri seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında, **Ekle**.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtildiği gibi ayarlar.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myTriggeredWebJob | İçinde bir uygulama hizmeti uygulamayı benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_".|
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* program veya komut dosyasını çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir dosya veya komut dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Tür** | Tetiklenmiş | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Tetikleyiciler** | El ile | |

4. **Tamam**’a tıklayın.

   Yeni Web işi kasasındaki **WebJobs** sayfası.

   ![Web işleri listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. Web işi çalıştırmak için listenin adını sağ tıklatıp **çalıştırmak**.
   
    ![WebJob'ı çalıştır](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateScheduledCRON"></a> Zamanlanmış WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. İçinde [Azure portal](https://portal.azure.com)gidin **uygulama hizmeti** App Service web uygulaması, API uygulaması veya mobil uygulama sayfası.

2. Seçin **WebJobs**.

   ![Web işleri seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. İçinde **WebJobs** sayfasında, **Ekle**.

   ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Kullanım **WebJob Ekle** tabloda belirtildiği gibi ayarlar.

   ![WebJob Sayfası Ekle](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Ad** | myScheduledWebJob | İçinde bir uygulama hizmeti uygulamayı benzersiz bir ad. Bir harf veya sayı ile başlamalı ve özel karakterler içeremez "-" ve "_". |
   | **Karşıya dosya yükleme** | ConsoleApp.zip | A *.zip* program veya komut dosyasını çalıştırmak için gerekli tüm destekleyici dosyaları yanı sıra, yürütülebilir dosya veya komut dosyanızı içeren dosya. Desteklenen yürütülebilir dosya veya komut dosyası türlerini de listelenen [desteklenen dosya türleri](#acceptablefiles) bölümü. |
   | **Tür** | Tetiklenmiş | [WebJob türleri](#webjob-types) bu makalenin önceki bölümlerinde açıklanmıştır. |
   | **Tetikleyiciler** | Zamanlanmış | Güvenilir bir şekilde çalışması için zamanlama için her zaman açık özelliğini etkinleştirin. Her zaman yalnızca temel, standart ve Premium fiyatlandırma katmanlarına edinilebilir.|
   | **CRON ifade** | 0 0/20 * * * * | [CRON ifadeleri](#cron-expressions) aşağıdaki bölümde açıklanmıştır. |

4. **Tamam**’a tıklayın.

   Yeni Web işi kasasındaki **WebJobs** sayfası.

   ![Web işleri listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="cron-expressions"></a>CRON ifadeleri

Girdiğiniz bir [CRON ifade](../azure-functions/functions-bindings-timer.md#cron-expressions) Portalı'nda ya da dahil bir `settings.job` , WebJob kökündeki dosya *.zip* aşağıdaki örnekteki gibi dosya:

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

> [!NOTE]
> Visual Studio'dan bir Web işi dağıttığınızda, işaretlemek, `settings.job` dosya olarak özellikleri **yeniyse Kopyala**.

## <a name="ViewJobHistory"></a> İş geçmişini görüntüleme

1. İçin Geçmişi'ne bakın ve ardından istediğiniz Web işi seçin **günlükleri** düğmesi.
   
   ![Günlükleri düğmesi](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. İçinde **WebJob ayrıntıları** sayfasında, bir çalışma ayrıntılarını görmek için bir saat seçin.
   
   ![Web işi ayrıntıları](./media/web-sites-create-web-jobs/webjobdetails.png)

3. İçinde **WebJob çalıştırma ayrıntıları** sayfasında, **geçiş çıktı** metni günlük içeriklerini görmek için.
   
    ![Web işi ayrıntı çalıştırın](./media/web-sites-create-web-jobs/webjobrundetails.png)

   Çıkış metnini ayrı bir tarayıcı penceresinde görmek için seçin **karşıdan**. Metni indirmek için sağ **karşıdan** ve tarayıcı seçeneklerinizi kullanarak dosya içeriklerini kaydedin.
   
5. Seçin **WebJobs** içerik haritası bağlantı sayfanın üst kısmındaki Web işleri listesine gidin.

    ![Web işi içerik haritası](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![Geçmiş Pano Web işleri listesinde](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="NextSteps"></a> Sonraki adımlar

Azure WebJobs SDK ile Web işleri pek çok programlama görevlerini basitleştirmek için kullanılabilir. Daha fazla bilgi için bkz: [WebJobs SDK nedir](https://github.com/Azure/azure-webjobs-sdk/wiki).
