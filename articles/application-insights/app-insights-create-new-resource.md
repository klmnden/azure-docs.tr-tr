---
title: Yeni bir Azure Application Insights kaynağı oluşturun | Microsoft Docs
description: El ile yeni dinamik bir uygulama için Application Insights izleme işlevini ayarlama.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 59bb8564613e9a0cebda00c2c847283ff218b882
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294726"
---
# <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma
Azure Application Insights, Microsoft Azure'da uygulamanızı hakkındaki verileri görüntüler *kaynak*. Yeni kaynak oluşturma parçasıdır bu nedenle [yeni bir uygulama izlemek için Application Insights'ı ayarlama][start]. Çoğu durumda, kaynak oluştururken otomatik olarak IDE tarafından yapılabilir. Ancak bazı durumlarda, bir kaynak el ile - Örneğin, geliştirme için ayrı kaynaklarınız için oluşturduğunuz ve uygulamanızın veya üretim oluşturur.

Kaynak oluşturduktan sonra kendi izleme anahtarı edinme ve, uygulama SDK'yı yapılandırmak için kullanın. Kaynak anahtarı telemetri kaynağına bağlar.

## <a name="sign-up-to-microsoft-azure"></a>Microsoft Azure'a kaydolun
Henüz geldiyseniz bir [Microsoft hesabı, hemen edinin](http://live.com). (Outlook.com, OneDrive, Windows Phone veya XBox LIVE gibi hizmetleri kullanıyorsanız, zaten bir Microsoft hesabınız vardır.)

Ayrıca bir aboneliğe ihtiyacınız [Microsoft Azure](http://azure.com). Ekibinizin ve kuruluşunuzun bir Azure aboneliğiniz varsa, sahibi, Windows Live ID'nizi kullanarak ekleyebileceğiniz Yalnızca, kullanım için ücret ödersiniz. Belirli bir miktar ücretsiz Deneysel kullanım için varsayılan temel plan sağlar.

Bir abonelik erişimi var olduğunda Application Insights oturum [ http://portal.azure.com ](https://portal.azure.com)ve Live ID'nizi oturum açmak için kullanın.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma
İçinde [portal.azure.com](https://portal.azure.com), Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/app-insights-create-new-resource/01-new.png)

* **Uygulama türü** genel bakış dikey penceresinde ve kullanılabilir özellikler Görünenleri etkiler [ölçüm Gezgini][metrics]. Uygulama, türünü görmüyorsanız, genel seçin.
* **Abonelik** ödeme hesabınız azure'da.
* **Kaynak grubu** özelliklerini yönetmek için kullanışlı olan erişim denetimi ister. Diğer Azure kaynaklarına zaten oluşturduysanız, bu yeni kaynak aynı gruba yerleştirmek seçebilirsiniz.
* **Konum** burada biz verilerinizi tutmak olduğu.
* **Panoya Sabitle** kaynağınız için hızlı erişim kutucuğu, Azure giriş sayfasında koyar. Önerilir.

Uygulamanızı oluştururken yeni bir dikey pencere açılır. Bu dikey pencere, performans ve kullanım verileri uygulamanız hakkında gördüğünüz ' dir. 

İçin bir sonraki Azure'a oturum açışınızda dönmek için uygulamanızın hızlı başlangıç döşeme başlangıç panosunda (giriş ekranı) arayın. Veya dosyayı bulmak için Gözat'ı tıklatın.

## <a name="copy-the-instrumentation-key"></a>İzleme anahtarını kopyalama
İzleme anahtarını oluşturduğunuz kaynak tanımlar. SDK vermeniz gerekir.

![Essentials'ı tıklatın, CTRL + C izleme anahtarı](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Uygulamanıza SDK yükleme
Application Insights SDK, uygulamayı yükleyin. Bu adım, yoğun bir şekilde uygulamanızı türüne göre değişir. 

İzleme anahtarını yapılandırmak için kullanın [uygulamanızda yüklediğiniz SDK][start].

SDK'sı telemetri herhangi bir kod yazmaya gerek kalmadan Gönder Standart modüller içerir. Kullanıcı eylemlerini izlemek veya daha fazla ayrıntı sorunları tanılamak için [API kullanan] [ api] kendi telemetrinizi göndermek için.

## <a name="monitor"></a>Telemetri verileri bakın
Azure portalında uygulama dikey pencerenize geri dönmek için hızlı başlangıç dikey penceresini kapatın.

Görmek için arama kutucuğuna tıklayın [tanılama arama][diagnostic], ilk olayları burada görünür. 

Daha fazla veri bekliyorsunuz tıklatmak **yenileme** birkaç saniye sonra.

## <a name="creating-a-resource-automatically"></a>Bir kaynak otomatik olarak oluşturma
Yazma bir [PowerShell Betiği](app-insights-powershell.md) otomatik olarak bir kaynak oluşturmak için.

## <a name="next-steps"></a>Sonraki adımlar
* [Pano oluşturma](app-insights-dashboards.md)
* [Tanılama Araması](app-insights-diagnostic-search.md)
* [Ölçümleri keşfetme](app-insights-metrics-explorer.md)
* [Analytics sorguları yazma](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

