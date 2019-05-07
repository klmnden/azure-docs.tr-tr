---
title: 'Azure durumu İzleyicisi v2 API Başvurusu: Config ayarlama | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API başvuru Set-ApplicationInsightsMonitoringConfig. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: 953edcb98de6ea705721aef0922562d23e18f0f5
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148255"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Bu belge, bir üyesi olarak sunulan bir cmdlet açıklar [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Açıklama

Yapılandırma dosyası, tam bir yeniden yükleme yinelenen olmadan ayarlayın. Yaptığınız değişiklikleri etkili olması için IIS'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.


## <a name="examples"></a>Örnekler

### <a name="example-with-single-instrumentation-key"></a>Tek bir izleme anahtarı ile örnek
Bu örnekte, geçerli makine üzerindeki tüm uygulamalara tek izleme anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-instrumentation-key-map"></a>İzleme anahtarı eşleme örneği
Bu örnekte, 
- `MachineFilter` geçerli makine kullanarak eşleşir `'.*'` joker karakter.
- `AppFilter='WebAppExclude'` sağlar bir `null` Instrumentationkey. Bu uygulama algılayıcılarla olmaz.
- `AppFilter='WebAppOne'` Bu belirli uygulama benzersiz izleme anahtarını atar.
- `AppFilter='WebAppTwo'` Ayrıca bu belirli uygulama benzersiz izleme anahtarını atar.
- Son olarak, `AppFilter` de kullanır `'.*'` diğer tüm web uygulamalarının eşleştirmek için joker karakter önceki kurallar tarafından eşleştirilen ve varsayılan izleme anahtarını atar.
- Yalnızca okunabilirlik için eklenen boşluk.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parametreler 

### <a name="-instrumentationkey"></a>-Instrumentationkey
**Gerekli.** Hedef makinedeki tüm uygulamalar tarafından kullanım için tek bir iKey sağlamak için bu parametreyi kullanın.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Gerekli.** Birden çok ikey'leri ve hangi uygulamaları hangi ikey kullanmaya ilişkin bir eşleme sağlamak için bu parametreyi kullanın. Çeşitli makineler için bir tek bir yükleme betiği MachineFilter ayarlayarak oluşturabilirsiniz. 

> [!IMPORTANT] 
> Uygulama kuralları sağlanan sırada karşı eşleşir. Bu nedenle en belirgin kurallar ilk belirtmeniz gerekir ve son en genel kurallar.

#### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** bilgisayar veya vm adını bir gerekli c# regex olduğu.
    - '. *' tüm eşleşir
    - Bilgisayarlarda tam adda 'ComputerName' eşleşir.
- **AppFilter** bilgisayar veya vm adını bir gerekli c# regex olduğu.
    - '. *' tüm eşleşir
    - 'ApplicationName' yalnızca IIS uygulamaları, tam adıyla eşleşir.
- **Instrumentationkey** Yukarıdaki iki filtrelerle eşleşen uygulamaları izlemeyi etkinleştirmek için gereklidir.
    - Bu değeri izleme hariç tutmak için kurallar tanımlamak istiyorsanız boş bırakın


### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.


## <a name="output"></a>Çıktı

Varsayılan çıkış yok.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Ayrıntılı örnek yapılandırma dosyası aracılığıyla - Instrumentationkey ayarından çıkış

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Ayrıntılı örnek yapılandırma dosyası aracılığıyla - InstrumentationKeyMap ayarından çıkış

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
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
