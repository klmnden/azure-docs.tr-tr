---
title: Azure Durum İzleyicisi v2 ayrıntılı yönergeleri | Microsoft Docs
description: Durum İzleyicisi v2 ile başlamak için ayrıntılı yönergeler için. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: c8199c960229f9cc53cf57f9da3e1f17ebd9f5c7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074153"
---
# <a name="status-monitor-v2-detailed-instructions"></a>Durum İzleyicisi'ni v2: Ayrıntılı yönergeler

Bu makalede nasıl PowerShell Galerisi ve indirme ApplicationMonitor modül eklemek için.
Bunu kullanmaya başlamak için ihtiyaç duyacağınız en yaygın parametreleri açıklar.
Internet erişiminiz yoksa durumunda el ile yönergeleri de içerir.

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="get-an-instrumentation-key"></a>Bir izleme anahtarı edinme

Başlamak için bir izleme anahtarı gerekir. Daha fazla bilgi için [Application Insights kaynağı oluşturma](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>PowerShell'i yönetici bir yükseltilmiş yürütme İlkesi ile çalıştırın.

**Yönetici Çalıştır**

PowerShell, bilgisayarınızda değişiklik yapmak için yönetici düzeyi izinleri olması gerekir.

**Yürütme İlkesi**
- Açıklama: Varsayılan olarak, PowerShell betikleri çalıştırma devre dışıdır. Yalnızca geçerli kapsam için RemoteSigned betikleri izin vererek öneririz.
- Referans: [Yürütme ilkeleri hakkında daha fazla](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) ve [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
).
- Komut: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- İsteğe bağlı parametre:
    - `-Force`. Onay istemi atlar.

**Örnek hataları**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>PowerShell için Önkoşullar

PowerShell örneğinizin çalıştırarak denetim `$PSVersionTable` komutu.
Bu komut şu çıktıyı üretir:


```
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Bu yönergeler, yazılmış ve Windows 10 ve yukarıda listelenen sürümleri çalıştıran bir bilgisayarda test.

## <a name="prerequisites-for-powershell-gallery"></a>PowerShell Galerisi için Önkoşullar

Bu adımları sunucunuzun modülleri PowerShell Galerisi'nden yüklemek için hazırlık yapacaksınız.

> [!NOTE] 
> PowerShell Galerisi, Windows 10, Windows Server 2016 ve PowerShell 6 desteklenir.
> Önceki sürümleri hakkında daha fazla bilgi için bkz. [PowerShellGet yükleme](https://docs.microsoft.com/powershell/gallery/installing-psget).


1. PowerShell ile bir yükseltilmiş yürütme İlkesi Yöneticisi olarak çalıştırın.
2. NuGet paket sağlayıcıyı yükleyin.
    - Açıklama: NuGet tabanlı depoları gibi PowerShell Galerisi ile etkileşim kurmak için bu sağlayıcıyı ihtiyacınız vardır.
    - Referans: [Yükleme PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6).
    - Komut: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. Bir istek için proxy sunucusunu belirtir.
        - `-Force`. Onay istemi atlar.
    
    NuGet ayarlanmadıysa, bu uyarıyı alırsınız:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. PowerShell Galerisi, güvenilen bir depo yapılandırın.
    - Açıklama: Varsayılan olarak, PowerShell Galerisi güvenilmeyen bir depodur.
    - Referans: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6).
    - Komut: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - İsteğe bağlı parametre:
        - `-Proxy`. Bir istek için proxy sunucusunu belirtir.

    PowerShell Galerisi güvenilir değilse, bu uyarıyı alırsınız:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    Bu değişikliği onaylayın ve çalıştırarak tüm PSRepositories denetim `Get-PSRepository` komutu.

4. PowerShellGet en yeni sürümünü yükleyin.
    - Açıklama: Bu modül, diğer modülleri PowerShell Galerisi'nden almak için kullanılan araçları içerir. Sürüm 1.0.0.1 Windows 10 ve Windows Server ile birlikte gelir. 1\.6.0 sürümü veya üzeri bir sürüm gereklidir. Hangi sürümünün yüklü olduğunu belirlemek için çalıştırın `Get-Command -Module PowerShellGet` komutu.
    - Referans: [PowerShellGet yükleme](https://docs.microsoft.com/powershell/gallery/installing-psget).
    - Komut: `Install-Module -Name PowerShellGet`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. Bir istek için proxy sunucusunu belirtir.
        - `-Force`. "Yüklü" uyarısı atlar ve en son sürümünü yükler.

    PowerShellGet en yeni sürümü kullanmıyorsanız, bu hatayı alırsınız:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. PowerShell yeniden başlatın. Geçerli oturumda yeni sürümü yüklenemiyor. Yeni PowerShell oturumlarında PowerShellGet en son sürümünü yükler.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>PowerShell Galerisi yoluyla modülü yükleyip

Bu adımları Az.ApplicationMonitor modülü PowerShell Galerisi'nden indirilir.

1. PowerShell Galerisi tüm önkoşulların karşılandığından emin olun.
2. PowerShell ile bir yükseltilmiş yürütme İlkesi Yöneticisi olarak çalıştırın.
3. Az.ApplicationMonitor modülünü yükleyin.
    - Referans: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6).
    - Komut: `Install-Module -Name Az.ApplicationMonitor`.
    - İsteğe bağlı parametreler:
        - `-Proxy`. Bir istek için proxy sunucusunu belirtir.
        - `-AllowPrerelease`. Alfa ve beta sürümleri yüklenmesine izin verir.
        - `-AcceptLicense`. "Lisans kabul" istemi atlar
        - `-Force`. "Güvenilmeyen depo" uyarısı atlar.

## <a name="download-and-install-the-module-manually-offline-option"></a>Modül el ile yükleyip (çevrimdışı seçeneği)

Herhangi bir nedenle PowerShell modülüne bağlanamıyorsanız, el ile indirin ve Az.ApplicationMonitor modülünü yükleyin.

### <a name="manually-download-the-latest-nupkg-file"></a>En son nupkg dosyasını el ile indirin

1. https://www.powershellgallery.com/packages/Az.ApplicationMonitor kısmına gidin.
2. Dosyanın en son sürümü seçin **sürüm geçmişi** tablo.
3. Altında **yükleme seçenekleri**seçin **el ile indirin**.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>1\. seçenek: Bir PowerShell modülleri dizine yükleme
PowerShell oturumları tarafından bulunabilir olacaktır, böylece bir PowerShell dizinine el ile yüklenen PowerShell modülünü yükleyin.
Daha fazla bilgi için [PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module).


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Expand-arşiv (v1.0.1.0) kullanarak bir zip dosyası olarak nupkg sıkıştırmasını açın

- Açıklama: Temel sürümü Microsoft.PowerShell.Archive (v1.0.1.0) nupkg dosyaları ayıklayın olamaz. .Zip uzantılı dosyayı yeniden adlandırın.
- Referans: [Genişletin arşiv](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6).
- Komut:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Expand-arşiv (v1.1.0.0) kullanarak nupkg sıkıştırmasını açın

- Açıklama: Uzantı değiştirmeden nupkg dosyaların sıkıştırmasını genişletme arşiv güncel bir sürümünü kullanın.
- Referans: [Genişletin arşiv](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) ve [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Komut:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>2\. seçenek: Sıkıştırmasını açın ve nupkg el ile içeri aktarma
PowerShell oturumları tarafından bulunabilir olacaktır, böylece bir PowerShell dizinine el ile yüklenen PowerShell modülünü yükleyin.
Daha fazla bilgi için [PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module).

Başka bir dizine modülü yüklüyorsanız, el ile modülü kullanarak içeri aktarın [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6).

> [!IMPORTANT] 
> DLL'leri göreli yollar yükler.
> Paket içeriğini hedeflenen çalışma zamanı dizininizde Store ve erişim izinlerini okuma, ancak değil yazma izin onaylayın.

1. Uzantıyı ".zip" olarak değiştirin ve hedeflenen yükleme dizininize paket içeriğini ayıklayın.
2. Az.ApplicationMonitor.psd1 dosya yolunu bulun.
3. PowerShell ile bir yükseltilmiş yürütme İlkesi Yöneticisi olarak çalıştırın.
4. Kullanarak Modülü `Import-Module Az.ApplicationMonitor.psd1` komutu.
    

## <a name="route-traffic-through-a-proxy"></a>Bir ara sunucu aracılığıyla trafiği yönlendirme

Bir bilgisayar, özel intranet üzerinde izlerken, bir proxy aracılığıyla HTTP trafiği yönlendirmek gerekir.

İndirip Az.ApplicationMonitor PowerShell Galerisi'nden yüklemek için PowerShell komutlarını destekleyen bir `-Proxy` parametresi.
Yükleme komut dosyalarınızı yazmak yukarıdaki yönergeleri gözden geçirin.

Application Insights SDK'sı, uygulamanızın telemetri Microsoft'a gerekecektir. Web.config dosyanızda uygulamanız için proxy ayarlarını yapılandırmanızı öneririz. Daha fazla bilgi için [Application Insights hakkında SSS: Proxy geçiş](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough).


## <a name="enable-monitoring"></a>İzlemeyi etkinleştirme

Kullanım `Enable-ApplicationInsightsMonitoring` izlemeyi etkinleştirmek için komutu.

Bkz: [API Başvurusu](status-monitor-v2-api-enable-monitoring.md) bu cmdlet'in nasıl kullanılacağı ayrıntılı bir açıklaması.



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
