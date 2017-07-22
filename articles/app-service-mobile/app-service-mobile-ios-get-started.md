---
title: "Azure Uygulama Hizmeti Mobile Apps’de iOS uygulaması oluşturma | Microsoft Belgeleri"
description: "Objective-C ya da Swift’te iOS geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 36936ae66c458fcbedeec95cfa2f573a40c8af53
ms.contentlocale: tr-tr
ms.lasthandoff: 07/20/2017

---
# <a name="create-an-ios-app"></a>iOS uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir iOS uygulamasına bulut tabanlı arka uç hizmeti olan [Azure Mobile Apps](app-service-mobile-value-prop.md)’i nasıl ekleyeceğiniz gösterilmiştir. Önce yeni bir mobil arka uç oluşturacağız. Ardından, Azure’da verileri depolamak için basit bir *Yapılacaklar listesi* iOS uygulaması kullanacağız.

Bu öğreticiyi tamamlamak için bir [Mac bilgisayar](https://azure.microsoft.com/pricing/free-trial/) ve Azure hesabınızın olması gerekir

## <a name="step-i-create-a-new-azure-mobile-app-backend"></a>Adım I: Yeni bir Azure mobil uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-the-backend-project"></a>Adım II: Arka uç projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-the-ios-app"></a>Adım III: iOS uygulamasını indirme ve çalıştırma
[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203

