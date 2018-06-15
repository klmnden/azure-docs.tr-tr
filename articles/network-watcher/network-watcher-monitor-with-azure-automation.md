---
title: İzleme Azure Ağ İzleyicisi sorun giderme VPN ağ geçitleri | Microsoft Docs
description: Bu makalede Azure Automation ve Ağ İzleyicisi ile şirket içi bağlantı nasıl Tanılama
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: a102916bb0626f5b110fb134a8a25c902cfaefe7
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598141"
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Ağ İzleyicisi sorun giderme VPN ağ geçitleri izleme

Ağ performansı ayrıntılı Öngörüler elde müşterilere güvenilir hizmetler sağlamak için önemlidir. Bu nedenle, hızlı bir şekilde ağ kesintisi koşulları algılamak ve kesinti koşul azaltmak için düzeltme eylemlerini gerçekleştirin çok önemlidir. Azure Otomasyonu, uygulama ve runbook'ları programlı bir şekilde bir görevi çalıştırmayı sağlar. Azure otomasyonu kullanarak, sürekli ve öngörülü ağ izleme ve uyarma gerçekleştirmek için mükemmel bir tarif oluşturur.

## <a name="scenario"></a>Senaryo

Aşağıdaki görüntüde çok katmanlı bir uygulama ile bir VPN ağ geçidi ve tünel kullanarak kurulan bağlantı üzerinde bir senaryodur. VPN ağ geçidi çalışıyor olduktan ve çalışan uygulamaların performansı için önemlidir.

Bir runbook bağlantı tünel durumunu denetlemek için sorun giderme kaynak API'sini kullanarak VPN tüneli bağlantı durumunu denetlemek için bir komut dosyası oluşturulur. Durumu sağlıklı değil, bir e-posta tetikleyicisi yöneticilere gönderilir.

![Senaryo örneği][scenario]

Bu senaryo aşağıdakileri yapar:

- Bir runbook çağırma oluşturmak `Start-AzureRmNetworkWatcherResourceTroubleshooting` bağlantı durumu gidermek için cmdlet
- Runbook'a bir zamanlama Bağla

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo başlamadan önce aşağıdaki ön koşullar sahip olmanız gerekir:

- Bir Azure Otomasyonu hesabı Azure. Otomasyon hesabı son modülleri sahip ve aynı zamanda AzureRM.Network modülü olduğundan emin olun. AzureRM.Network modülü automation hesabınız için eklemeniz gerekiyorsa, modül galerisinde kullanılabilir.
- Azure Otomasyonu'nda yapılandırma kimlik bilgileri kümesi olması gerekir. Hakkında daha fazla bilgi edinin [Azure Automation güvenliği](../automation/automation-security-overview.md)
- Geçerli bir SMTP sunucusu (Office 365, şirket içi e-posta veya başka bir) ve Azure Automation'da tanımlanan kimlik bilgileri
- Yapılandırılmış bir sanal ağ ağ geçidi azure'da.
- Mevcut bir depolama hesabını'ndeki günlükleri depolamak için var olan bir kapsayıcı ile.

> [!NOTE]
> Önceki görüntüde gösterilen altyapı çizim amaçlıdır ve bu makalede yer alan adımları oluşturulmaz.

### <a name="create-the-runbook"></a>Runbook oluşturma

Örnek yapılandırma ilk adımı, runbook oluşturmaktır. Bu örnek, bir farklı çalıştır hesabını kullanır. Çalıştır hesapları hakkında bilgi için [kimlik doğrulaması runbook'larına sahip Azure farklı çalıştır hesabı](../automation/automation-create-runas-account.md)

### <a name="step-1"></a>1. Adım

Azure Otomasyonu'nda gidin [Azure portal](https://portal.azure.com) tıklatıp **runbook'ları**

![Automation hesabına genel bakış][1]

### <a name="step-2"></a>2. Adım

Tıklatın **runbook Ekle** runbook'un oluşturma işlemini başlatmak için.

![runbook'ları dikey penceresi][2]

### <a name="step-3"></a>3. Adım

Altında **hızlı Oluştur**, tıklatın **yeni bir runbook oluşturmak** runbook oluşturmak için.

![bir runbook dikey penceresinde ekleme][3]

### <a name="step-4"></a>4. Adım

Bu adımda, biz runbook bir ad verin, örnekte adlı **Get-VPNGatewayStatus**. Bu runbook açıklayıcı bir ad vermek önemli ve önerilen standart PowerShell adlandırma standartlarını takip eden bir ad verip olur. Bu örnek için runbook türü **PowerShell**, grafik, PowerShell iş akışı, diğer seçenekleri olan ve grafik PowerShell iş akışı.

![runbook dikey penceresi][4]

### <a name="step-5"></a>5. Adım

Runbook oluşturulduğunda bu adımda, aşağıdaki kod örneğinde örnek için gereken tüm kod sağlar. Kod içeren öğelerini \<değeri\> aboneliğinizi değerleri ile değiştirilmeleri gerekir.

Aşağıdaki kodu tıklatın kullanın **Kaydet**

```PowerShell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>6. Adım

Runbook kaydedildikten sonra runbook'u Başlat otomatikleştirmek için bir zamanlama bağlanmalıdır. İşlemi başlatmak için tıklatın **zamanlama**.

![6. Adım][6]

## <a name="link-a-schedule-to-the-runbook"></a>Runbook'a bir zamanlama Bağla

Yeni bir zamanlama oluşturulması gerekir. Tıklatın **runbook'a bir zamanlama Bağla**.

![7. Adım][7]

### <a name="step-1"></a>1. Adım

Üzerinde **zamanlama** dikey penceresinde tıklatın **yeni bir zamanlama oluşturun**

![8. Adım][8]

### <a name="step-2"></a>2. Adım

Üzerinde **yeni zamanlama** zamanlama bilgileri dikey penceresini doldururken. Aşağıdaki listede ayarlanabilir değerleri şunlardır:

- **Ad** -zamanlama kolay adı.
- **Açıklama** -zamanlama açıklaması.
- **Başlatır** -bu değer tarih, saat ve saat dilimi, zaman çizelgesi Tetikleyicileri olun bir birleşimidir.
- **Yineleme** -bu değer, zamanlamaları yineleme belirler.  Geçerli değerler **kez** veya **yinelenen**.
- **Tekrarlamayı her** -saat, gün, hafta veya ay zamanlamada yineleme aralığı.
- **Sona erme süresini ayarlamanıza** -zamanlama veya sona varsa değeri belirler. Ayarlanabilir **Evet** veya **Hayır**. Geçerli tarih ve saat Evet seçilirse sağlanması üzeresiniz.

> [!NOTE]
> (Diğer bir deyişle, 15, 30, 45 dakika saat sonra) farklı aralıklarla saatte daha sık çalışacak bir runbook'tan değiştirilmesi gerekiyorsa, birden çok zamanlama oluşturulmalıdır

![9. Adım][9]

### <a name="step-3"></a>3. Adım

Zamanlamayı runbook'a kaydetmek için Kaydet'i tıklatın.

![10. adım][10]

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi sorun giderme Azure Automation ile tümleştirme hakkında bir anlama sahip olduğunuza göre paket yakalamaları VM uyarılar hakkında ziyaret ederek tetiklemek öğrenin [ile Azure Ağ İzleyicisi bir uyarı tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md).

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
