---
title: "Azure Application Insights HockeyApp verilerde keşfetme | Microsoft Docs"
description: "Kullanım ve Application Insights ile Azure, uygulamanızın performansını analiz edin."
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: a925241d10b068e377fa9a11fc854db34c808343
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Application Insights HockeyApp verilerde gezinme

> [!NOTE]
> Visual Studio mobil merkezi Microsoft'tan önerilen hizmeti yeni mobil uygulamaları izlemek için sunulmuştur. [Uygulamalarınızı mobil merkezi ve Application Insights ile ayarlama öğrenin](app-insights-mobile-center-quickstart.md).
> 
> 

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) Canlı Masaüstü ve mobil uygulamaları izlemek için bir hizmettir. HockeyApp özel gönderebilir ve kullanımını izlemek ve (ek olarak kilitlenme verilerini alma) tanılama yardımcı olmak için telemetriyi izleme. Bu akış telemetri güçlü aracılığıyla sorgulanabilir [Analytics](app-insights-analytics.md) özelliği [Azure Application Insights](app-insights-overview.md). Buna ek olarak, şunları yapabilirsiniz [özel verme ve izleme telemetri](app-insights-export-telemetry.md). Bu özellikleri etkinleştirmek için Application Insights için HockeyApp özel veri aktaran bir köprü ayarlayın.

## <a name="the-hockeyapp-bridge-app"></a>HockeyApp köprüsü uygulama
HockeyApp köprüsü uygulama izleme telemetri Application Insights ve HockeyApp özel aracılığıyla analizi ve sürekli dışarı aktarma özellikleri erişmenize olanak sağlayan çekirdek özelliğidir. HockeyApp köprüsü uygulama oluşturulduktan sonra HockeyApp tarafından toplanan özel ve izleme olayları bu özelliklerinden erişilebilir olacaktır. Bu köprüsü uygulamalardan birini ayarlama görelim.

Hesap ayarları HockeyApp içinde açmak [API belirteçleri](https://rink.hockeyapp.net/manage/auth_tokens). Yeni bir belirteç oluşturmak veya mevcut bir yeniden kullanabilirsiniz. En düşük hakları "salt okunur" gerekir. API kopyasını belirteci alın.

![HockeyApp API belirteci alma](./media/app-insights-hockeyapp-bridge-app/01.png)

Microsoft Azure Portalı'nı açın ve [bir Application Insights kaynağı oluşturma](app-insights-create-new-resource.md). Uygulama türü "HockeyApp köprüsü uygulamayı" ayarlayın:

![Yeni Application Insights kaynağı](./media/app-insights-hockeyapp-bridge-app/02.png)

Bir ad ayarlamanız gerekmez - bu HockeyApp adından otomatik olarak ayarlanır.

HockeyApp köprüsü alanlar görünür. 

![Köprü alanları girin](./media/app-insights-hockeyapp-bridge-app/03.png)

Daha önce not ettiğiniz HockeyApp belirteci girin. Bu eylem "HockeyApp uygulama" açılır menüsünde tüm HockeyApp uygulamaları ile doldurur. Kullanmak istediğiniz birini seçin ve alanları tamamlayın. 

Yeni kaynak açın. 

Veri akışının başlatmak için biraz zaman alır.

![Uygulama Insights kaynağı için veri bekleniyor](./media/app-insights-hockeyapp-bridge-app/04.png)

Bu kadar! Bu noktadan itibaren HockeyApp izlenmiş uygulamanızda toplanan özel ve izleme verileri şimdi de, Application Insights analizi ve sürekli dışarı aktarma özellikleri kullanılabilir.

Şimdi kısa bir süre için artık kullanılabilir bu özelliklerin her biri gözden geçirin.

## <a name="analytics"></a>Analiz
Analytics geçici tanılamak ve telemetrinizi analiz ve nedenlerini ve desenler hızla bulmak sağlayarak, verileri sorgulamak için güçlü bir araçtır.

![Analiz](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Analytics hakkında daha fazla bilgi edinin](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Sürekli dışarı aktarma
Sürekli verme verileriniz Azure Blob Storage kapsayıcısının içine vermenize olanak sağlar. Verilerinizi şu an Application Insights tarafından sunulan saklama süresinden daha uzun süre tutmak gerekiyorsa bu oldukça yararlıdır. Blob storage'da verileri tutmak, bir SQL veritabanı veya depolama çözümü, tercih edilen verilerinizi işlem.

[Sürekli dışarı aktarma hakkında daha fazla bilgi edinin](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Verilerinizi veri analizi Uygula](app-insights-analytics-tour.md)

