---
title: 'Azure Durum İzleyicisi v2 API Başvurusu: Yapılandırmasını al | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API'si başvurusu. Get-ApplicationInsightsMonitoringConfig. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 7eaaaa2dd7b22d138ea2f0a52d0bf0a1b2eab026
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807083"
---
# <a name="status-monitor-v2-api-get-applicationinsightsmonitoringconfig-v040-alpha"></a>Durum İzleyicisi'ni v2 API'si: Get-ApplicationInsightsMonitoringConfig (v0.4.0-alpha)

Bu makalede bir üyesi olan bir cmdlet [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>Açıklama

Yapılandırma dosyasını alır ve değerleri konsola yazdırır.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>Parametreler

Gerekli parametre.

## <a name="output"></a>Output


#### <a name="example-output-from-reading-the-config-file"></a>Yapılandırma dosyası okunurken gelen örnek çıktı

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md) performans ve kullanımı izlemek için.
- [Olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) sorunları tanılamak için.
- Kullanım [analytics](../../azure-monitor/app/analytics.md) daha gelişmiş sorgular için.
- [Panolar oluşturma](../../azure-monitor/app/overview-dashboard.md).
 
 Daha fazla telemetri ekleyin:
 - [Web testleri oluşturun](monitor-web-app-availability.md) sitenizin Canlı kalması için.
- [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) web sayfası koduna ait özel durumları görmek ve izleme çağrıları etkinleştirmek için.
- [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları.
 
 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
 - Kullanarak yapılandırma için değişiklik [kümesi yapılandırma](status-monitor-v2-api-set-config.md) cmdlet'i.
