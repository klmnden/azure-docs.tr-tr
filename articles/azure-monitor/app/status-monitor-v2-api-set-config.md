---
title: 'Azure Durum İzleyicisi v2 API Başvurusu: Config ayarlama | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API'si başvurusu. Set-ApplicationInsightsMonitoringConfig. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: 562ce8a4267370be9b049e3b56f213f82deb89c0
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734998"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Bu belgede bir üyesi olan bir cmdlet açıklanmaktadır [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>Açıklama

Yapılandırma dosyası yeniden tam bir yükleme yapmadan ayarlar.
Yaptığınız değişiklikleri etkili olması için IIS'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.


## <a name="examples"></a>Örnekler

### <a name="example-with-a-single-instrumentation-key"></a>Örnek bir tek bir izleme anahtarı ile
Bu örnekte, geçerli bilgisayarda tüm uygulamalar, bir tek bir izleme anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Bir izleme anahtarı eşleme örneği
Bu örnekte:
- `MachineFilter` Geçerli bilgisayar kullanarak eşleşen `'.*'` joker karakter.
- `AppFilter='WebAppExclude'` sağlar bir `null` izleme anahtarı. Belirtilen uygulama algılayıcılarla olmaz.
- `AppFilter='WebAppOne'` Belirtilen uygulama benzersiz izleme anahtarını atar.
- `AppFilter='WebAppTwo'` Belirtilen uygulama benzersiz izleme anahtarını atar.
- Son olarak, `AppFilter` de kullanır `'.*'` önceki kurallar tarafından eşleşen olmayan ve varsayılan izleme anahtarı atama tüm web uygulamaları eşleştirmek için joker karakter.
- Okunabilirlik için boşluk eklenir.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parametreler

### <a name="-instrumentationkey"></a>-Instrumentationkey
**Gerekli.** Hedef bilgisayarda tüm uygulamalar tarafından kullanım için bir tek bir izleme anahtarı sağlamak için bu parametreyi kullanın.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Gerekli.** Birden çok izleme anahtarı ve her bir uygulama tarafından kullanılan izleme anahtarı ilişkin bir eşleme sağlamak için bu parametreyi kullanın.
Birkaç bilgisayar için bir tek bir yükleme betiği ayarlayarak oluşturabileceğiniz `MachineFilter`.

> [!IMPORTANT]
> Uygulama kuralları kuralları sağlanan sırayla karşı eşleşir. Bu nedenle en belirgin kurallar ilk belirtmeniz gerekir ve son en genel kurallar.

#### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** gerekli bir C# regex bilgisayar veya VM adı.
    - '. *' tüm eşleşir
    - Belirtilen ada sahip bilgisayarlarda 'ComputerName' eşleşir.
- **AppFilter** gerekli bir C# regex bilgisayar veya VM adı.
    - '. *' tüm eşleşir
    - 'ApplicationName' yalnızca IIS uygulama belirtilen ad ile eşleşir.
- **Instrumentationkey** önceki iki filtrelerle eşleşen uygulamaları izlemeyi etkinleştirmek için gereklidir.
    - Bu değeri izleme hariç tutmak için kurallar tanımlamak istiyorsanız boş bırakın.


### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.


## <a name="output"></a>Çıktı

Varsayılan olarak, çıkış yok.

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
 - [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md) performans ve kullanımı izlemek için.
- [Olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) sorunları tanılamak için.
- [Analytics'i](../../azure-monitor/app/analytics.md) daha gelişmiş sorgular için.
- [Panolar oluşturma](../../azure-monitor/app/overview-dashboard.md).
 
 Daha fazla telemetri ekleyin:
 - [Web testleri oluşturun](monitor-web-app-availability.md) sitenizin Canlı kalması için.
- [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) web sayfası koduna ait özel durumları görmek ve izleme çağrıları etkinleştirmek için.
- [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları
 
 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
 - [Yapılandırmasını al](status-monitor-v2-api-get-config.md) ayarlarınızı doğru kaydedilmiş olduğunu onaylamak için.
 - [Durumu Al](status-monitor-v2-api-get-status.md) izleme incelemek için.
