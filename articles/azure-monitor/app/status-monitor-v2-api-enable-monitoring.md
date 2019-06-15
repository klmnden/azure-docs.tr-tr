---
title: 'Azure Durum İzleyicisi v2 API Başvurusu: İzlemeyi etkinleştir | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API'si başvurusu. Etkinleştir-ApplicationInsightsMonitoring. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: e87bfad11eee5b86d35e6b4f2846b094c467e0ef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734180"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Etkinleştir-ApplicationInsightsMonitoring (v0.2.1-alpha)

Bu makalede bir üyesi olan bir cmdlet [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>Açıklama

Kodsuz etkinleştirir, hedef bilgisayarda IIS uygulama izleme ekleyin.

Bu cmdlet, IIS applicationHost.config değiştirmek ve bazı kayıt defteri anahtarlarını ayarlayın.
Ayrıca her bir uygulama tarafından kullanılan izleme anahtarını tanımlayan bir applicationinsights.ikey.config dosyası oluşturun.
IIS, uygulamaların başlangıç olarak, Application Insights SDK'sı uygulamalarınızı ekleyecektir başlangıçta RedfieldModule yükler.
Yaptığınız değişiklikleri etkili olması için IIS'yi yeniden başlatın.

İzleme etkinleştirdikten sonra kullanmanızı öneririz [Canlı ölçümleri](live-stream.md) uygulamanızı bize telemetri gönderdiği varsa hemen kontrol etmek için.


> [!NOTE] 
> - Başlamak için bir izleme anahtarı gerekir. Daha fazla bilgi için [kaynak Oluştur](create-new-resource.md#copy-the-instrumentation-key).
> - Bu cmdlet, gözden geçirin ve lisans ve gizlilik bildirimimizi kabul gerektirir.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu yükseltilmiş yürütme İlkesi gerektirir. Daha fazla bilgi için [yükseltilmiş yürütme İlkesi ile yönetici olarak çalıştır PowerShell](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy).

## <a name="examples"></a>Örnekler

### <a name="example-with-a-single-instrumentation-key"></a>Örnek bir tek bir izleme anahtarı ile
Bu örnekte, geçerli bilgisayarda tüm uygulamalar tek izleme anahtarı atanır.

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
    - 'ComputerName' yalnızca bilgisayarlar belirtilen tam adıyla eşleşir.
- **AppFilter** gerekli bir C# regex bilgisayar veya VM adı.
    - '. *' tüm eşleşir
    - 'ApplicationName' yalnızca IIS uygulamaları belirtilen tam adıyla eşleşir.
- **Instrumentationkey** önceki iki filtrelerle eşleşen uygulamaları izlemeyi etkinleştirmek için gereklidir.
    - Bu değeri izleme hariç tutmak için kurallar tanımlamak istiyorsanız boş bırakın.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**İsteğe bağlı.** Olayları ve iletileri yönetilen bir işlem yürütülürken olup bitenler hakkında toplamak izleme altyapısı etkinleştirmek için bu anahtarı kullanın. Bu olayları ve iletileri bağımlılık sonuç kodları, HTTP fiilleri ve SQL komut metni içerir.

İzleme altyapısı, ek yükü ekler ve varsayılan olarak kapalıdır.

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerinde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.

### <a name="-whatif"></a>-WhatIf 
**Ortak parametresi.** Test ve izleme gerçekten etkinleştirmeden giriş parametrelerinizi doğrulamak için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-a-successful-enablement"></a>Başarılı bir etkinleştirme örnek çıktısı

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
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
- [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları.
 
 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
 - [Yapılandırmasını al](status-monitor-v2-api-get-config.md) ayarlarınızı doğru kaydedilmiş olduğunu onaylamak için.
 - [Durumu Al](status-monitor-v2-api-get-status.md) izleme incelemek için.
