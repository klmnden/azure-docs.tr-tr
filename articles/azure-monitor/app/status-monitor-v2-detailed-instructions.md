---
title: Azure Durum İzleyicisi v2 ayrıntılı yönergeleri | Microsoft Docs
description: Durum İzleyicisi v2 ile başlamak için ayrıntılı yönergeler için. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: 3aca64c7b0f1ad04967782cb3349da302db557a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145081"
---
# <a name="status-monitor-v2-detailed-instructions"></a>Durum İzleyicisi'ni v2 ayrıntılı yönergeler

Bu belge ayrıntıları nasıl PowerShell Galerisi ve indirme ApplicationMonitor modül eklemek için. Başlamak için gerekli olan en yaygın parametreleri belgeledik.
İnternet erişimi kullanılamıyor durumunda, el ile yönergeleri de ekledik.

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="instrumentation-key"></a>İzleme anahtarı

Başlamak için bir izleme anahtarı olması gerekir. Daha fazla bilgi için okuma [Application Insights kaynağı oluşturma](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-administrator-with-an-elevated-execution-policy"></a>PowerShell ile bir yükseltilmiş yürütme İlkesi yönetici olarak çalıştırın

**Yönetici olarak çalıştır**: 
- Açıklama: PowerShell, bilgisayarınızda değişiklik yapmak için yönetici düzeyi izinler gerekir.

**Yürütme ilkesini**:
- Açıklama: Varsayılan olarak, PowerShell betikleri çalıştırma devre dışı bırakılacak. Yalnızca geçerli kapsam için RemoteSigned betikleri izin öneririz.
- Referans: [Yürütme ilkeleri hakkında daha fazla](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) ve [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
)
- Cmd: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`
- İsteğe bağlı parametreler:
    - `-Force` Bu onay istemi atlar.

**Örnek hataları:**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>PowerShell için Önkoşullar

Geçerli sürümünüzü PowerShell komutunu çalıştırarak denetim: `$PSVersionTable`.
Komutu aşağıdaki çıktıyı üretir:


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

Bu yönergeler, yazılmış ve yukarıda listelenen sürümler ile Windows 10 makinede test.

## <a name="prerequisites-for-powershell-gallery"></a>PowerShell Galerisi için Önkoşullar

Bu adımları sunucunuzun modülleri PowerShell Galerisi'nden yüklemek için hazırlık yapacaksınız.

> [!NOTE] 
> PowerShell Galerisi için destek, Windows 10, Windows Server 2016 ve PowerShell 6 bulunmaktadır. Eski sürümler için bu belgeyi gözden geçirin: [PowerShellGet yükleme](https://docs.microsoft.com/powershell/gallery/installing-psget)


1. PowerShell'i yönetici olarak bir yükseltilmiş yürütme İlkesi ile çalıştırın.
2. NuGet paketi sağlayıcısı 
    - Açıklama: NuGet tabanlı depoları gibi PowerShellGallery ile etkileşim kurmak için bu sağlayıcıyı gereklidir
    - Referans: [Yükleme PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6)
    - Cmd: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`
    - İsteğe bağlı parametreler:
        - `-Proxy` Bir istek için proxy sunucusunu belirtir.
        - `-Force` Bu onay istemi atlar. 
    
    NuGet ayarlanmadıysa, bu uyarıyı alırsınız:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. Güvenilen depoları
    - Açıklama: Varsayılan olarak, PowerShellGallery güvenilmeyen bir depodur.
    - Referans: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6)
    - Cmd: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`
    - İsteğe bağlı parametreler:
        - `-Proxy` Bir istek için proxy sunucusunu belirtir.

    PowerShell Galerisi güvenilir değilse, bu uyarıyı alırsınız:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    - Bu değişikliği onaylayın ve tüm PSRepositories cmd çalıştırarak denetim kullanabilirsiniz: `Get-PSRepository`

4. PowerShellGet sürümü 
    - Açıklama: Bu modül, diğer modülleri PowerShell Galerisi'nden almak için kullanılan araçları içerir. V1.0.0.1 Windows 10 ve Windows Server ile birlikte gelir. Gerekli en düşük sürümü v1.6.0 ' dir. Komutunu çalıştırın hangi sürümünün yüklü olduğunu denetlemek için: `Get-Command -Module PowerShellGet`
    - Referans: [PowerShellGet yükleme](https://docs.microsoft.com/powershell/gallery/installing-psget)
    - Cmd: `Install-Module -Name PowerShellGet`
    - İsteğe bağlı parametreler:
        - `-Proxy` Bir istek için proxy sunucusunu belirtir.
        - `-Force` Bu, "yüklü" uyarısını yoksayın ve en son sürümünü yükleyin.

    PowerShellGet en yeni sürümü kullanmıyorsanız, bu hatayı alırsınız:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. PowerShell yeniden başlatın. Geçerli oturumda yeni sürümü yüklemek mümkün değildir. Her yeni Powershell oturumlarında en son PowerShellGet yüklü olacaktır.

## <a name="download--install-via-powershell-gallery"></a>İndirme ve PowerShell Galerisi aracılığıyla yükleme

Bu adımları Az.ApplicationMonitor modülü PowerShell Galerisi'nden indirilir.

1. PowerShell Galerisi için Önkoşullar gereklidir.
2. PowerShell'i yönetici olarak bir yükseltilmiş yürütme İlkesi ile çalıştırın.
3. Az.ApplicationMonitor modülünü yükleme
    - Referans: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6)
    - Cmd: `Install-Module -Name Az.ApplicationMonitor`
    - İsteğe bağlı parametreler:
        - `-Proxy` Bir istek için proxy sunucusunu belirtir.
        - `-AllowPrerelease` Bu, alfa ve beta sürümleri yükleme olanak tanır.
        - `-AcceptLicense` Bunu "Lisans kabul" istemi atla
        - `-Force` Bu durum, "Güvenilmeyen depo" uyarıyı göz ardı eder

## <a name="download--install-manually-offline-option"></a>İndirme ve yükleme el ile (çevrimdışı seçeneği)

Herhangi bir nedenle PowerShell modülüne bağlanamıyorsanız, el ile indirin ve Az.ApplicationMonitor modülünü yüklemek.

### <a name="manually-download-the-latest-nupkg"></a>En son nupkg el ile yükleme

1. Şuraya gidin: https://www.powershellgallery.com/packages/Az.ApplicationMonitor
2. En son sürümü sürüm geçmişini seçin.
3. "Yükleme Seçenekleri" bulun ve "El ile indir" seçeneğini belirtin.

### <a name="option-1-install-into-powershell-modules-directory"></a>1. seçenek: PowerShell modülleri dizinine yükler.
Bu PowerShell oturumları tarafından keşfedilebilir olduğun için el ile yüklenen PowerShell modülünü bir PowerShell dizinine yükleyin.
Daha fazla bilgi için bkz. [Bir PowerShell modülü yükleniyor](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)


#### <a name="unzip-nupkg-as-zip-using-expand-archive-v1010"></a>Expand-arşiv (v1.0.1.0) kullanarak zip olarak nupkg sıkıştırmasını açın

- Açıklama: Temel sürümü Microsoft.PowerShell.Archive (v1.0.1.0) nupkg dosyaları ayıklayın olamaz. ".Zip" uzantılı dosyayı yeniden adlandırın.
- Referans: [Arşiv genişletin](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6)
- Cmd: 

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-using-expand-archive-v1100"></a>Expand-arşiv (v1.1.0.0) kullanarak nupkg sıkıştırmasını açın.

- Açıklama: Uzantı adlandırmadan nupkgs sıkıştırmasını genişletme arşiv güncel bir sürümünü kullanın. 
- Referans: [Genişletin arşiv](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) ve [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0)
- Cmd:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-manually"></a>2. seçenek: sıkıştırmasını açın ve el ile içeri aktarma
Bu PowerShell oturumları tarafından keşfedilebilir olduğun için el ile yüklenen PowerShell modülünü bir PowerShell dizinine yükleyin.
Daha fazla bilgi için bkz. [Bir PowerShell modülü yükleniyor](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)

Başka bir dizine yüklüyorsanız, elle modülünü kullanarak aktarmanız gerekir [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6)

> [!IMPORTANT] 
> Yükleme DLL'leri göreli yollar yükler. Bu paketin içeriğini hedeflenen çalışma dizininize Store ve erişim izinlerini okuma, ancak değil yazma izin onaylayın.

1. Uzantıyı ".zip" olarak değiştirin ve hedeflenen yükleme dizininize paket içeriğini ayıklayın.
2. "Az.ApplicationMonitor.psd1" dosya yolunu bulun.
3. PowerShell'i yönetici olarak bir yükseltilmiş yürütme İlkesi ile çalıştırın. 
4. Komutu ile Modülü: `Import-Module Az.ApplicationMonitor.psd1`.
    

## <a name="proxy"></a>Ara sunucu

Özel bir intranet ağınızdaki bir makine izlerken bir proxy aracılığıyla http trafiği yönlendirmek gerekli olacaktır.

İndirip Az.ApplicationMonitor PowerShell Galerisi'nden yüklemek için PowerShell komutlarını destekleyen bir `-Proxy` parametresi.
Yukarıdaki yönergeleri inceleyin, yükleme komut dosyası yazma.

Application Insights SDK'sı, uygulamanızın telemetri Microsoft'a gerekecektir. Uygulamanızın web.config dosyasında, uygulamanız için proxy ayarlarını yapılandırma öneririz. Bkz: [Application Insights sık sorulan sorular: Proxy geçiş](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough) daha fazla bilgi için.


## <a name="enable-monitoring"></a>İzlemeyi etkinleştir 

Cmd: `Enable-ApplicationInsightsMonitoring`

Gözden geçirme bizim [API Başvurusu](status-monitor-v2-api-enable-monitoring.md) bu cmdlet'in nasıl kullanılacağı ayrıntılı bir açıklaması. 



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
