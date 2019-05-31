---
title: Yeni bir Azure Application Insights kaynağı oluşturun | Microsoft Docs
description: El ile yeni bir canlı uygulaması için Application Insights izleme işlevini ayarlama.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 712004a1ae8a2a72854b7b2332449a019c0820c3
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256254"
---
# <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma
Azure Application Insights, Microsoft Azure'da uygulamanızla ilgili verileri görüntüler *kaynak*. Yeni kaynak oluşturma, bu nedenle parçası [yeni bir uygulama izlemek için Application ınsights'ı ayarlama][start]. Çoğu durumda, bir kaynak oluşturma otomatik olarak IDE tarafından yapılabilir. Ancak bazı durumlarda, bir kaynak el ile - Örneğin, geliştirme için ayrı kaynaklar için oluşturmanız ve uygulamanızı üretim oluşturur.

Kaynak oluşturduktan sonra izleme anahtarı edinme ve, uygulama SDK'sını yapılandırmak için kullanın. Kaynak anahtarı telemetri kaynağına bağlar.

## <a name="sign-up-to-microsoft-azure"></a>Microsoft Azure'a kaydolun
Gücüne sahip değilseniz, bir [Microsoft hesabı, hemen edinin](https://live.com). (Outlook.com, OneDrive, Windows Phone veya XBox Live gibi hizmetleri kullanıyorsanız zaten bir Microsoft hesabınız varsa.)

Ayrıca bir aboneliğe ihtiyacınız [Microsoft Azure](https://azure.com). Ekibinizin ve kuruluşunuzun bir Azure aboneliği varsa, sahibi, Windows Live ID'nizi kullanarak ekleyebileceğiniz Yalnızca kullandığınız kadarı için ücret ödersiniz. Varsayılan temel plan, Deneysel kullanım ücretsiz olarak belirli bir miktarda için sağlar.

Bir aboneliğe erişim süreyi bulduğunuzda, Application Insights oturum [ https://portal.azure.com ](https://portal.azure.com)ve Live ID'nizi oturum açmak için kullanabilirsiniz.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma
İçinde [portal.azure.com](https://portal.azure.com), Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/create-new-resource/01-new.png)

* **Uygulama türü** genel bakış dikey ve bulunan özelliklerin gördüğünüz etkiler [ölçüm Gezgini][metrics]. Uygulama türünü görmüyorsanız, genel seçin.
* **Abonelik** ödeme hesabınız azure'da.
* **Kaynak grubu** özelliklerini yönetmek için bir kolaylık ister erişim denetimi. Diğer Azure kaynakları zaten oluşturduysanız, bu yeni kaynak aynı gruba yerleştirmek seçebilirsiniz.
* **Konum** olan verilerinizi burada saklarız.
* **Panoya Sabitle** kaynağınızın hızlı erişim kutucuk Azure giriş sayfanızda koyar. Önerilir.

Uygulamanızı oluştururken, yeni bir dikey pencere açılır. Bu dikey pencereyi, performans ve kullanım verilerini uygulamanızla ilgili gördüğünüz ' dir. 

Azure'da oturum açtığınızda dönmek için uygulamanızın (giriş ekranı) başlangıç panosunda hızlı başlangıç kutucuk arayın. Veya dosyayı bulmak için Gözat'a tıklayın.

## <a name="copy-the-instrumentation-key"></a>İzleme anahtarını kopyalama
Oluşturduğunuz kaynağın izleme anahtarını tanımlar. SDK vermek için ihtiyacınız.

![Essentials'ı tıklatın, izleme anahtarı, CTRL + C](./media/create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Uygulamanıza SDK yükleme
Uygulamanıza Application Insights SDK'sını yükleyin. Bu adım, yoğun bir şekilde uygulama türüne bağlıdır. 

İzleme anahtarını yapılandırmak için kullanın [uygulamanıza yükleme SDK'sı][start].

SDK'sı telemetri kod yazmaya gerek kalmadan gönderme Standart modüller içerir. Kullanıcı eylemlerini izlemek veya sorunları daha ayrıntılı tanılama için [API'sini] [ api] kendi telemetrinizi göndermek için.

## <a name="monitor"></a>Telemetri verileri görüntüle
Azure portalındaki uygulama dikey pencerenize geri dönmek için hızlı başlangıç dikey penceresini kapatın.

Görmek için arama kutucuğuna tıklayın [tanılama araması][diagnostic]ilk olayların burada görüntülenir. 

Daha fazla veri bekliyorsanız, tıklayın **Yenile** birkaç saniye sonra.

## <a name="creating-a-resource-automatically"></a>Bir kaynağı otomatik olarak oluşturma
Yazabileceğiniz bir [PowerShell Betiği](../../azure-monitor/app/powershell.md) otomatik olarak bir kaynak oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
* [Tanılama Araması](../../azure-monitor/app/diagnostic-search.md)
* [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md)
* [Analytics sorguları yazma](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md

