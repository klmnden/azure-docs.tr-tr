---
title: Azure Uygulama Hizmeti Mobile Apps’de Cordova uygulaması oluşturma | Microsoft Belgeleri
description: Apache Cordova geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın.
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: ''
tags: ''
keywords: cordova,javascript,mobil,istemci
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: crdun
ms.openlocfilehash: 7014d09bbb62e78c37a9496628e3509b6eaaa4ac
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123340"
---
# <a name="create-an-apache-cordova-app"></a>Apache Cordova uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Apache Cordova mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Yeni bir mobil uygulama arka ucu ve uygulama verilerini Azure'da depolayan basit bir *Yapılacaklar listesi* Apache Cordova uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Apache Cordova öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio Community 2017] ya da daha yeni sürümünü içeren bir bilgisayar.
* [Apache Cordova için Visual Studio Araçları].
* [Etkin bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/).

Ayrıca Visual Studio’yu atlayabilir ve doğrudan Apache Cordova komut satırını kullanabilirsiniz.  Komut satırını kullanmak, öğreticiyi bir Mac bilgisayarda tamamladığınızda kullanışlıdır.  Komut satırını kullanarak Apache Cordova istemci uygulamalarını derleme bu öğretici kapsamında değildir.

## <a name="create-an-azure-mobile-app-backend"></a>Azure mobil uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Apache Cordova uygulamasını indirme ve çalıştırma
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Bu hızlı başlangıç öğreticisini tamamladığınıza göre, şu eğitimlerden birine geçin:

* Apache Cordova uygulamanıza [Çevrimdışı Veri Ekleme](app-service-mobile-cordova-get-started-offline-data.md).
* Apache Cordova uygulamanıza [Kimlik Doğrulaması Ekleme](app-service-mobile-cordova-get-started-users.md).
* Apache Cordova uygulamanıza [Anında İletme Bildirimleri Ekleme](app-service-mobile-cordova-get-started-push.md).

Azure App Service temel kavramları hakkında daha fazla bilgi edinin.

* [Çevrimdışı Veri]
* [Kimlik doğrulaması]
* [Anında İletme Bildirimleri]

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: https://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Çevrimdışı Veri]: app-service-mobile-offline-data-sync.md
[Kimlik doğrulaması]: app-service-mobile-auth.md
[Anında İletme Bildirimleri]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
