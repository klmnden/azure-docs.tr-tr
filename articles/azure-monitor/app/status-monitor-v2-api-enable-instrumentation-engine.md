---
title: 'Azure Durum İzleyicisi v2 API Başvurusu: İzleme altyapısını etkinleştirirsiniz | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API'si başvurusu. Etkinleştir-InstrumentationEngine. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: 79446e6676a35a1b51e5e0839eb539d730b499da
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807123"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v040-alpha"></a>Durum İzleyicisi'ni v2 API'si: Etkinleştir-InstrumentationEngine (v0.4.0-alpha)

Bu makalede bir üyesi olan bir cmdlet [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>Açıklama

Bazı kayıt defteri anahtarlarını ayarlayarak izleme altyapısı sağlar.
Değişikliklerin etkili olması için IIS yeniden başlatın.

İzleme altyapısının .NET SDK'ları tarafından toplanan verileri destekleyebilirsiniz.
Bu, olayları ve yönetilen bir işlemin yürütülmesi açıklayan ileti toplar. Bu olayları ve iletileri bağımlılık sonuç kodları, HTTP fiilleri ve SQL komut metni içerir.

İzleme altyapısının, etkinleştirin:
- Zaten etkin cmdlet'iyle izleme etkinleştirildi ancak izleme altyapısını etkinleştirirsiniz kaydetmedi.
- Uygulamanızı .NET SDK'ları ile el ile işaretlediyseniz ve ek telemetri toplamak istiyorsanız.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

> [!NOTE] 
> - Bu cmdlet, gözden geçirin ve lisans ve gizlilik bildirimimizi kabul gerektirir.
> - İzleme altyapısı, ek yükü ekler ve varsayılan olarak kapalıdır.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerinde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>İzleme altyapısı başarıyla etkinleştirme gelen örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
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
 - [Yapılandırmasını al](status-monitor-v2-api-get-config.md) ayarlarınızı doğru kaydedilmiş olduğunu onaylamak için.
 - [Durumu Al](status-monitor-v2-api-get-status.md) izleme incelemek için.
