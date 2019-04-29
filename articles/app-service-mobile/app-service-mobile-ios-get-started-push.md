---
title: İOS uygulaması ile Azure Mobile Apps için anında iletme bildirimleri ekleme
description: İOS uygulamanıza anında iletme bildirimleri göndermek için Azure Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: ''
author: conceptdev
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: crdun
ms.openlocfilehash: 4d56453eb05e88c3558b66250439d6211e8069fd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122583"
---
# <a name="add-push-notifications-to-your-ios-app"></a>İOS uygulamasına anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [iOS hızlı başlangıç] anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Kılavuzu.

[Anında iletme bildirimlerini iOS simülatörü desteklemiyor](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Fiziksel bir iOS cihazına gerekir ve bir [Apple Developer Program üyeliği](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Anında iletme bildirimleri için uygulamayı kaydetme

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'ı yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Anında iletme bildirimleri göndermek için arka uç güncelleştir

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Uygulamaya anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Test anında iletme bildirimleri

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Daha fazla

* Şablonlar, platformlar arası bildirimler ve yerelleştirilmiş bildirimler göndermek için esneklik sağlar. [Kullanım iOS için Azure Mobile Apps istemci kitaplığı nasıl](app-service-mobile-ios-how-to-use-client-library.md#templates) şablonları kaydettirmek gösterilmektedir.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS hızlı başlangıç]: app-service-mobile-ios-get-started.md
