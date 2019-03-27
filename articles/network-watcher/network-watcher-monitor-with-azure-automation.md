---
title: Sorun giderme Azure Ağ İzleyicisi ile VPN ağ geçitleri izleyin | Microsoft Docs
description: Bu makalede nasıl Azure Otomasyonu ve Ağ İzleyicisi ile şirket içi bağlantıyı tanılama
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
ms.openlocfilehash: 4995d7ae846652c374a289603f29f88f6f56dfef
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485502"
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Sorun giderme Ağ İzleyicisi ile VPN ağ geçitlerini izleme

Ağ performansına ayrıntılı Öngörüler elde etme, müşterilere güvenilir hizmetleri sağlamak için önemlidir. Bu nedenle, ağ kesintisi hızlı bir şekilde algılayan ve kesinti koşulu gidermek için düzeltici eylemi gerçekleştirme için önemlidir. Azure Otomasyonu ve runbook'ları programlı bir şekilde bir görevi çalıştırmayı sağlar. Azure otomasyonu kullanarak, sürekli ve öngörülü ağ izleme ve uyarı gerçekleştirmek için mükemmel bir tarif oluşturur.

## <a name="scenario"></a>Senaryo

Aşağıdaki görüntüde çok katmanlı bir uygulama ile şirket içi bağlantısı bir VPN ağ geçidi ve tünel kullanılarak senaryodur. VPN ağ geçidinin çalışır durumda olduktan ve çalışan uygulamaları performansı için önemlidir.

Bir runbook ile bağlantı tünel durumunu denetlemek için sorun giderme kaynak API'si kullanarak VPN tüneli bağlantı durumunu denetlemek için bir komut dosyası oluşturulur. Durumu iyi değil, yöneticilere bir e-posta tetikleyicisi gönderilir.

![Senaryo örneği][scenario]

Bu senaryo olur:

- Bir runbook çağırma oluşturma `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet'ini bağlantı durumu sorunlarını giderme
- Runbook'a bir zamanlama Bağla

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoya başlamadan önce aşağıdaki ön koşullar olması gerekir:

- Azure'da bir Azure Otomasyonu hesabı. Otomasyon hesabı son modül yok ve AzureRM.Network modülü de sahip emin olun. AzureRM.Network modülü Otomasyon hesabınıza eklemek gerekirse modülü galerisinde kullanılabilir.
- Azure Otomasyonu'nda yapılandırma kimlik bilgileri kümesi olması gerekir. Daha fazla bilgi [Azure Automation güvenliği](../automation/automation-security-overview.md)
- Geçerli bir SMTP sunucusu (Office 365, şirket içi e-posta veya başka bir) ve Azure Automation'da tanımlanan kimlik bilgileri
- Yapılandırılmış sanal ağ ağ geçidi azure'da.
- Mevcut bir depolama hesabı günlükleri depolamak için var olan bir kapsayıcı ile.

> [!NOTE]
> Önceki resimde gösterilen altyapı gösterim amaçlıdır ve bu makalede yer alan adımlarla oluşturulmaz.

### <a name="create-the-runbook"></a>Runbook oluşturma

Bu örnek yapılandırmanın ilk adımı, runbook oluşturmaktır. Bu örnek, bir farklı çalıştır hesabı kullanır. Farklı çalıştırma hesapları hakkında bilgi edinmek için [Azure farklı çalıştır hesabıyla Runbook kimlik doğrulaması](../automation/automation-create-runas-account.md)

### <a name="step-1"></a>1. Adım

Azure Otomasyonu'nda gidin [Azure portalında](https://portal.azure.com) tıklatıp **runbook'ları**

![Otomasyon hesabına genel bakış][1]

### <a name="step-2"></a>2. Adım

Tıklayın **runbook Ekle** runbook oluşturma işlemini başlatmak için.

![runbook'ları dikey penceresi][2]

### <a name="step-3"></a>3. Adım

Altında **hızlı Oluştur**, tıklayın **yeni bir runbook oluşturmak** runbook oluşturmak için.

![bir runbook dikey penceresi ekleme][3]

### <a name="step-4"></a>4. Adım

Bu adımda, biz runbook bir ad verin, çağrıldığı örnek içinde **Get-VPNGatewayStatus**. Bunu, runbook'a açıklayıcı bir ad vermek önemli ve önerilen standart PowerShell adlandırma standartlarını takip eden bir ad verin. Bu örnek için runbook türü **PowerShell**, grafik, PowerShell iş akışı, diğer seçenekler şunlardır: ve grafik PowerShell iş akışı.

![runbook dikey penceresini][4]

### <a name="step-5"></a>5. Adım

Runbook oluşturulur, bu adımda, örnek için gerekli tüm kodlar aşağıdaki kod örneği sağlar. İçeren kod öğeleri \<değer\> aboneliğinizde değerlerle değiştirilmesi gerekebilir.

Aşağıdaki kodu kullanın tıklatın **Kaydet**

```powershell
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

Runbook kaydedildikten sonra runbook başlangıcını otomatikleştirmek için bir zamanlama bağlanmalıdır. İşlemi başlatmak için tıklatın **zamanlama**.

![6. Adım][6]

## <a name="link-a-schedule-to-the-runbook"></a>Runbook'a bir zamanlama Bağla

Yeni bir zamanlama oluşturulmalıdır. Tıklayın **bir zamanlamayı runbook'a bağlamak**.

![7. Adım][7]

### <a name="step-1"></a>1. Adım

Üzerinde **zamanlama** dikey penceresinde tıklayın **yeni bir zamanlama oluşturun**

![8. Adım][8]

### <a name="step-2"></a>2. Adım

Üzerinde **yeni zamanlama** zamanlama bilgileri dikey penceresini doldurun. Aşağıdaki listede ayarlanabilen değerler şunlardır:

- **Ad** -planı kolay adı.
- **Açıklama** -zamanlama açıklaması.
- **Başlar** -bu değeri tarih ve saat zamanlama Tetikleyicileri zaman olun saat dilimi bir birleşimidir.
- **Yinelenme** -zamanlamaları yineleme bu değeri belirler.  Geçerli değerler **kez** veya **yinelenen**.
- **Yineleme her** -saat, gün, hafta veya ayda zamanlama yinelenme aralığı.
- **Sona erme süresini ayarlamanıza** -zamanlama veya süresi dolarsa, değeri belirler. Ayarlanabilir **Evet** veya **Hayır**. Geçerli tarih ve saat Evet seçilirse sağlanan üzeresiniz.

> [!NOTE]
> (Diğer bir deyişle, 15, 30 saat sonra 45 dakika) farklı aralıklarla saatte daha sık çalıştırmasına runbook ihtiyacınız varsa birden çok zamanlama oluşturulmalıdır

![9. Adım][9]

### <a name="step-3"></a>3. Adım

Zamanlamayı runbook'a kaydetmek için Kaydet'e tıklayın.

![10. adım][10]

## <a name="next-steps"></a>Sonraki adımlar

Ağ İzleyicisi sorun giderme Azure Otomasyonu ile tümleştirme hakkında bir anlama sahip olduğunuza göre VM uyarılar üzerinde paket yakalamaları ederek tetikleme hakkında bilgi edinin [bir uyarı tetiklendi paket yakalamasıAzureAğİzleyicisiileoluşturma](network-watcher-alert-triggered-packet-capture.md).

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
