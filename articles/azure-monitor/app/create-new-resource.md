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
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 9da52e5a9dfa3b55431d66ed3162172226f71a40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073281"
---
# <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure Application Insights, Microsoft Azure'da uygulamanızla ilgili verileri görüntüler *kaynak*. Yeni kaynak oluşturma, bu nedenle parçası [yeni bir uygulama izlemek için Application ınsights'ı ayarlama][start]. Yeni kaynak oluşturduktan sonra izleme anahtarı edinme ve, Application Insights SDK'sını yapılandırmak için kullanın. İzleme anahtarını telemetrinizi kaynağına bağlar.

## <a name="sign-in-to-microsoft-azure"></a>Microsoft Azure'da oturum açın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Oturum [Azure portalında](https://portal.azure.com)ve bir Application Insights kaynağı oluşturun:

![Sol üst köşesinde '+' işaretine tıklayın. Application Insights tarafından izlenen Geliştirici Araçları'nı seçin](./media/create-new-resource/new-app-insights.png)

   | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad. |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verileri barındırmak için yeni veya mevcut bir kaynak grubunun adı. |
   | **Konum** | Doğu ABD | Size yakın bir konum seçin veya uygulamanızın barındırıldığı yakın. |

Gerekli alanlara uygun değerleri girin ve ardından **gözden geçir + Oluştur**.

![Alanlara gerekli değerleri girin ve "gözden geçir + oluştur" seçin.](./media/create-new-resource/review-create.png)

Uygulamanızı oluştururken, yeni bir bölme açılır. Bu bölme, performans ve kullanım verilerini, izlenen uygulama hakkında gördüğünüz ' dir. 

## <a name="copy-the-instrumentation-key"></a>İzleme anahtarını kopyalama

Telemetri verileriniz ile ilişkilendirmek istediğiniz kaynağın izleme anahtarını tanımlar. İzleme anahtarı için uygulamanızın kod eklemek için kopyalama gerekir.

![' A tıklayın ve izleme anahtarını kopyalama](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Uygulamanıza SDK yükleme

Uygulamanıza Application Insights SDK'sını yükleyin. Bu adım, yoğun bir şekilde uygulama türüne bağlıdır.

İzleme anahtarını yapılandırmak için kullanın [uygulamanıza yükleme SDK'sı][start].

SDK'sı telemetri ek kod yazmaya gerek kalmadan gönderme Standart modüller içerir. Kullanıcı eylemlerini izlemek veya sorunları daha ayrıntılı tanılama için [API'sini] [ api] kendi telemetrinizi göndermek için.

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