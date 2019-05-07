---
title: Azure Durum İzleyicisi v2 kullanmaya başlama | Microsoft Docs
description: Durum İzleyicisi v2 için Hızlı Başlangıç Kılavuzu. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: c20cbf50129247fa01f10f12a99c558bc86492b8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145172"
---
# <a name="getting-started-with-status-monitor-v2"></a>Durum İzleyicisi v2 ile çalışmaya başlama

Bu belge, çoğu ortam için iş beklenen hızlı başlangıç komutlarını içerir. Bu yönergeler, güncelleştirmeleri dağıtmak için PowerShell galerisinde bağlıdır. Bu komutları PowerShell desteği `-Proxy` parametresi.

Gözden geçirme bizim [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) nasıl özelleştirileceği ve sorun giderme yönergeler, bu komutları bir açıklaması için sayfa.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="download--install-via-powershell-gallery"></a>İndirme ve PowerShell Galerisi aracılığıyla yükleme

### <a name="install-prerequisites"></a>Ön koşulları yükleme
PowerShell'i yönetici olarak çalıştırın
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
PowerShell betiğinden çıkın

### <a name="install-status-monitor-v2"></a>Yükleme durumu izleme v2
PowerShell'i yönetici olarak çalıştırın
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>İzlemeyi etkinleştir
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download--install-manually-offline-option"></a>İndirme ve yükleme el ile (çevrimdışı seçeneği)
### <a name="manual-download"></a>El ile yükleme
Modülün en son sürümünü el ile yükleyin: https://www.powershellgallery.com/packages/Az.ApplicationMonitor

### <a name="unzip-and-install-status-monitor-v2"></a>Sıkıştırmasını açın ve Durum İzleyicisi v2 yükleyin
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.2.1-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>İzlemeyi etkinleştir
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
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

- Gözden geçirme [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) bu kılavuzda bulunan komutların açıklaması.
- Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
