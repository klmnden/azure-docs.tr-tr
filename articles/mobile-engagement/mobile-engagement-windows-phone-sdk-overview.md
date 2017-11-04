---
title: "Azure Mobile Engagement Windows Phone Silverlight SDK'sına genel bakış | Microsoft Docs"
description: "Azure Mobile Engagement için Windows Phone Silverlight SDK'sına genel bakış"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: c4e8ceee4104c3d3a6c3e6b79322ba1cf8463b22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK'sına genel bakış için Azure Mobile Engagement
Azure Mobile Engagement Windows Phone Silverlight uygulaması içinde tümleştirme hakkında bilgi almak buradan başlayın. Try ilk vermek istiyorsanız, tamamladığınızdan emin olun bizim [15 dakika Öğreticisi](mobile-engagement-windows-phone-get-started.md).

Görmek için tıklatın [SDK içerik](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Tümleştirme yordamları
1. Buradan başlayın: [Mobile Engagement Windows Phone Silverlight uygulamanızda tümleştirme](mobile-engagement-windows-phone-integrate-engagement.md)
2. Bildirimlerinin: [ulaşma (bildirimleri), Windows Phone Silverlight uygulaması tümleştirme](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Etiket planı uygulama: [API, Windows Phone Silverlight uygulaması etiketleme Gelişmiş Mobile Engagement kullanma](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Sürüm notları
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Parçası *MicrosoftAzure.MobileEngagement* Nuget paketi **v3.4.1**

* İstikrara yönelik iyileştirmeler.

Önceki sürümü için lütfen bkz. [tamamlamak sürüm notları](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Yükseltme yordamları
Uygulamanıza bizim SDK daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK çeşitli sürümleri eksik, birçok yordamı uygulamanız gerekebilir. Tam bkz [yükseltme yordamları](mobile-engagement-windows-phone-upgrade-procedure.md). Örneğin, 0.10.1 önce "Kimden 0.9.0'dan 0.10.1 için" yordamını takip etmek için sahip 0.11.0 sonra "Kimden 0.10.1 0.11.0 için" yordamı geçiş ise.

### <a name="from-200-to-330"></a>2.0.0 3.3.0 için
#### <a name="test-logs"></a>Test günlükleri
SDK'sı tarafından üretilen konsol günlükleri artık etkin/devre dışı bırakılmış/filtrelenmiş olabilir. Bu özelleştirmek için özellik güncelleştirme `EngagementAgent.Instance.TestLogEnabled` bulunan değer birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Eski sürümlerinden yükseltme
Bkz: [yükseltme yordamları](mobile-engagement-windows-phone-upgrade-procedure.md)

