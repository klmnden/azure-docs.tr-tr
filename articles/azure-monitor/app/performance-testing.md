---
title: Performans ve yük testi ile Azure Application Insights | Microsoft Docs
description: Performans ve yük testleri Azure Application Insights ile ayarlama
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 55d743e32f6db0828317d3764a97bcb35b104dad
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305020"
---
# <a name="performance-testing"></a>Performansı test etme

> [!NOTE]
> Bulut tabanlı yük testi hizmetinin kullanım dışıdır. Kullanımdan kaldırma, hizmet kullanılabilirliği ve diğer hizmetleri hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops).

Application Insights, yük testleri için Web siteleri oluşturmanıza olanak sağlar. Gibi [kullanılabilirlik testleri](monitor-web-app-availability.md), ya da temel isteği gönderebilir veya [çok adımlı istekler](availability-multistep.md) Azure'dan test aracılarını dünyanın dört bir yanındaki. Performans testleri, 60 dakikaya kadar 20.000 adede kadar eşzamanlı kullanıcının benzetimini olanak tanır.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Performans testi oluşturmak için öncelikle bir Application Insights kaynağı oluşturmanız gerekir. Bir kaynak zaten oluşturduysanız sonraki bölüme geçin.

Azure portalından seçin **kaynak Oluştur** > **Geliştirici Araçları** > **Application Insights** ve bir Application Insights'ı oluşturma Kaynak.

## <a name="configure-performance-testing"></a>Performans testi Yapılandır

Bu performans testi seçin oluşturma ilk kez olup olmadığını **ayarlayın kuruluş** ve performans testleri için bir kaynak olarak Azure DevOps kuruluş seçin.

Altında **yapılandırma**Git **performans testi** tıklatıp **yeni** bir test oluşturmak için.

![En azından web sitenizin URL'sini doldurma](./media/performance-testing/new-performance-test.png)

Bir temel performans testi oluşturmak için bir test türü seçmek **el ile Test** ve test için istenen ayarları doldurun.

|Ayar| En yüksek değer
|----------|------------|
| Kullanıcı Yükü | 20.000 |
| Süre (dakika)  | 60 |  

Testinizi oluşturduktan sonra tıklayın **test çalıştırması**.

Test tamamlandıktan sonra aşağıdaki sonuçları benzer sonuçlar görürsünüz:

![Test Sonuçları](./media/performance-testing/test-results.png)

## <a name="configure-visual-studio-web-test"></a>Visual Studio web testi Yapılandır

Test projeleri application ınsights'ı Gelişmiş performansı test etme özellikleri Visual Studio performans ve yük üzerinde oluşturulur.

![Visual Studio ](./media/performance-testing/visual-studio-test.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testleri](availability-multistep.md)
* [URL ping testlerini](monitor-web-app-availability.md)