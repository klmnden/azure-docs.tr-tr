---
title: 'Azure durumu İzleyicisi v2 API Başvurusu: İzleme altyapısını etkinleştirirsiniz | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API başvuru Enable-InstrumentationEngine. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: d886aa364ca928d32100c570689f13beb0c682c9
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143427"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Etkinleştir-InstrumentationEngine (v0.2.1-alpha)

Bu belge, bir üyesi olarak sunulan bir cmdlet açıklar [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Açıklama

Bu cmdlet, bazı kayıt defteri anahtarlarını ayarlayarak izleme altyapısı olanak tanır.
Bu değişikliklerin etkili olması için IIS yeniden başlatın.

İzleme altyapısının .NET SDK'ları tarafından toplanan verileri destekleyebilirsiniz.
Yönetilen bir işlemin yürütülmesi açıklayan olayları ve iletileri toplanmayacak. Ancak bunlarla sınırlı olmamak bağımlılık sonuç kodları, HTTP fiilleri ve SQL komut metnini içeren. 

İzleme altyapısının, etkinleştirin:
- Zaten etkin cmdlet'ini kullanarak izleme etkinleştirildi ancak InstrumentationEngine etkinleştirme kaydetmedi.
- Uygulamanızı .NET SDK'ları ile el ile işaretlediyseniz ve ek telemetri toplamak istiyorsanız.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

> [!NOTE] 
> Bu cmdlet gözden geçirin ve lisans ve gizlilik bildirimimizi kabul etmenizi gerektirir.

> [!NOTE] 
> İzleme altyapısı, ek yükü ekler ve varsayılan olarak kapalıdır.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler 

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerinde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>İzleme altyapısı başarıyla etkinleştirme gelen örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - Performans ve kullanımı izlemek için [ölçümleri keşfedin](../../azure-monitor/app/metrics-explorer.md)
- [Olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) sorunları tanılamak için
- Daha gelişmiş sorgular için [analiz](../../azure-monitor/app/analytics.md)
- [Panolar oluşturun](../../azure-monitor/app/app-insights-dashboards.md)
 
 Daha fazla telemetri ekleyin:
 - [Web testleri oluşturun](monitor-web-app-availability.md) sitenizin Canlı kalması için.
- [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için.
- [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları
 
 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
 - [Yapılandırmasını al](status-monitor-v2-api-get-config.md) ayarlarınızı doğru kaydedilmiş olduğunu onaylamak için.
 - [Durumu Al](status-monitor-v2-api-get-status.md) izleme incelemek için.
