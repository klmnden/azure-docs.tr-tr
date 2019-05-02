---
title: En son platform için kod yükseltme | Azure Market
description: Bu konu başlığında, Microsoft Dynamics 365 Operations platform sürümü için en son platform sürümüne yükseltmek açıklanmaktadır
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: aedc2c7474de0fe068a329eb2205e9bb08e62c3a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935276"
---
# <a name="upgrading-code-to-the-latest-platform"></a>En son platform yükseltme kodu

Bu makalede, Microsoft Dynamics 365 Operations platform sürümü için en son platform sürüm yükseltme açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Microsoft Dynamics 365 Operations platform için aşağıdaki bileşenlerden oluşur:

Dynamics 365 uygulama nesnesi sunucusu (AOS), veri yönetimi altyapısını, raporlama ve iş zekası (BI) çerçevesi, geliştirme araçları ve Analiz Hizmetleri gibi işlemleri platform ikililer için. Aşağıdaki uygulama nesne ağacının (AOT) paketleri:

1. Uygulama Platformu
2. Uygulama temeli
3. Sınama Essentials

**Önemli**: İşlem platformuna yönelik en son Dynamics 365 taşımak için işlem uygulaması, Dynamics 365 özelleştirmeler (overlayering) herhangi bir platforma ait AOT paketleri sahip olamaz. Bu kısıtlama, sorunsuz sürekli güncelleştirmeler platforma hale getirilebilir, böylece platform güncelleştirmesi 3, kullanılmaya başlandı. Çalıştırıyorsanız, platformu eski bir platform güncelleştirme 3, güncelleştirme 3, bu makalenin sonunda daha eski bir yapı bölümünden yükseltme platforma bakın.

Kod yükseltme hakkında daha fazla bilgi için lütfen bakın [burada](https://docs.microsoft.com/dynamics365/operations/dev-itpro/migration-upgrade/upgrade-latest-platform-update).