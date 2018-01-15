---
title: "Azure Uygulama Hizmeti Mobile Apps’de iOS uygulaması oluşturma | Microsoft Belgeleri"
description: "Objective-C ya da Swift’te iOS geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın."
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: 
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 970ad90aa14d5778e8b232e32aa1dacf32789988
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
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
