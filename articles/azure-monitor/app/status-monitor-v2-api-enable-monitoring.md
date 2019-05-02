---
title: 'Azure durumu İzleyicisi v2 API Başvurusu: İzlemeyi etkinleştir | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API başvuru Enable-ApplicationInsightsMonitoring. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: e2d964316e83138711547fb18fc5d04c56b4002f
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870530"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Etkinleştir-ApplicationInsightsMonitoring (v0.2.1-alpha)

Bu belge, bir üyesi olarak sunulan bir cmdlet açıklar [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Açıklama

Etkinleştirme kodu daha az bir hedef makine üzerinde IIS uygulamaları izleme ekleyin.
Bu cmdlet, IIS applicationHost.config değiştirmek ve bazı kayıt defteri anahtarlarını ayarlayın.
Bu cmdlet, ayrıca hangi izleme anahtarını hangi uygulama tarafından kullanılan tanımlayan bir applicationinsights.ikey.config oluşturulur.
IIS, başlangıçta bu uygulama başladığında, uygulamalarına Application Insights SDK'sı ekleyecektir RedfieldModule yükler.
Yaptığınız değişiklikleri etkili olması için IIS'yi yeniden başlatın.

İzleme etkinleştirdikten sonra kullanılması önerilir [Canlı ölçümleri](live-stream.md) hızlı bir şekilde uygulamanızı bize telemetri gönderdiği varsa gözlemleyin.


> [!NOTE] 
> Başlamak için bir izleme anahtarı olması gerekir. Daha fazla bilgi için okuma [yeni kaynak Oluştur](create-new-resource.md#copy-the-instrumentation-key).


> [!IMPORTANT] 
> Bu cmdlet bir PowerShell oturumu yönetici izinleri ile bir yükseltilmiş yürütme İlkesi gerektirir. Okuma [burada](status-monitor-v2-detailed-instructions.md#run-powershell-as-administrator-with-an-elevated-execution-policy) daha fazla bilgi için.

> [!NOTE] 
> Bu cmdlet gözden geçirin ve lisans ve gizlilik bildirimimizi kabul etmenizi gerektirir.


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


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**İsteğe bağlı.** Olayları ve iletileri için yönetilen bir işlem yürütülürken olup bitenleri toplamak izleme altyapısı etkinleştirmek için bu anahtarı kullanın. Ancak bunlarla sınırlı olmamak bağımlılık sonuç kodları, HTTP fiilleri ve SQL komut metnini içeren. İzleme altyapısı, ek yükü ekler ve varsayılan olarak kapalıdır.

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerinde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.

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

