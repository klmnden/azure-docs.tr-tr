---
title: "Azure Mobile Engagement Windows Evrensel SDK tümleştirmesi | Microsoft Docs"
description: "Azure Mobile Engagement SDK'sı tümleştirmesi Evrensel Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: d616ad58156a19e89b3e106639a38df67cbd0abb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Azure Mobile Engagement için Windows Evrensel SDK tümleştirmesi
Bu belgede, tümleştirme ve yapılandırma için tüm seçenekleri kullanılabilir Azure Mobile Engagement Windows Evrensel SDK açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce ilk tamamlamalısınız bizim [15 dakikalık Öğreticisi](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Gelişmiş özellikler
### <a name="reporting-features"></a>Raporlama özellikleri
Bu özellikler ekleyebilirsiniz:

1. [Gelişmiş raporlama seçenekleri](mobile-engagement-windows-store-advanced-reporting.md)
2. [Gelişmiş yapılandırma seçenekleri](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Bildirimler
[Windows Evrensel uygulamanız ulaşma (bildirimler) tümleştirme](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Etiket planı uygulama:
[Gelişmiş Mobile Engagement Windows Evrensel uygulamanız API etiketleme kullanma](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Sürüm notları
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* İstikrara yönelik iyileştirmeler.

Önceki sürümler için bkz: [tamamlamak sürüm notları](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Yükseltme yordamları
Uygulamanıza katılım daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK çeşitli sürümleri eksik, birkaç yordamları izleyin olabilir. Tam bkz [yükseltme yordamları](mobile-engagement-windows-store-upgrade-procedure.md). Örneğin, 0.10.1 önce "Kimden 0.9.0'dan 0.10.1 için" yordamını takip etmek için sahip 0.11.0 sonra "Kimden 0.10.1 0.11.0 için" yordamı geçiş ise.

### <a name="from-330-to-340"></a>3.3.0 3.4.0 için
#### <a name="test-logs"></a>Test günlükleri
SDK'sı tarafından üretilen konsol günlükleri artık etkin/devre dışı bırakılmış/filtrelenmiş olabilir. Özelleştirmek için özelliğini güncelleştirme `EngagementAgent.Instance.TestLogEnabled` kullanılabilir değerlerden birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Kaynaklar
Reach katmana geliştirilmiştir. SDK'sı NuGet paketi kaynakları parçasıdır.

SDK'ın yeni sürümüne yükseltirken, varolan dosyalarınızı kaynaklarınızı katmana klasöründen veya tutmak isteyip istemediğinizi seçebilirsiniz:

* Önceki katmana sizin yerinize çalıştığından veya tümleştirme `WebView` öğeleri el ile daha sonra karar, çıkma tutmak dosyaları, bunu hala çalışır.
* Yeni katmana güncelleştirmek için tüm Değiştir `overlay` kaynaklarınızı klasöründen SDK paketinden yeni bir (UWP uygulamaları: yükseltmeden sonra yeni katmana klasör % USERPROFILE % alabilirsiniz\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Yeni katmana kullanarak önceki bir sürümünde yapılan tüm özelleştirmeler üzerine yazar.
> 
> 

### <a name="upgrade-from-older-versions"></a>Eski sürümlerinden yükseltme
Bkz: [yükseltme yordamları](mobile-engagement-windows-store-upgrade-procedure.md)

