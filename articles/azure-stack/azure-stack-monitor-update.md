---
title: "Güncelleştirmeleri Azure kullanan ayrıcalıklı uç noktasını yığınında izleme | Microsoft Docs"
description: "Azure tümleşik yığını sistemleri güncelleştirme durumunu izlemek için ayrıcalıklı uç noktası kullanmayı öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: mabrigg
ms.openlocfilehash: 55688ad4959d59e41dca9be2d00011e1d41ebd8c
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Azure kullanan ayrıcalıklı uç noktasını yığınında güncelleştirmelerini izleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Ayrıcalıklı endpoint Azure yığın güncelleştirme çalışması ilerlemesini izlemek için ve son başarılı adımından çalıştırmak başarısız bir güncelleştirme devam etmek için kullanabilirsiniz. 

Güncelleştirme yönetimi için aşağıdaki yeni PowerShell cmdlet'leri Azure tümleşik yığını sistemler 1710 güncelleştirmesi dahil edilmiştir.

| Cmdlet  | Açıklama  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Şu anda çalışan, tamamlanan veya başarısız güncelleştirme durumunu döndürür. Güncelleştirme işlemi ve geçerli adımı ve karşılık gelen durum tanımlayan bir XML belgesi üst düzey durumunu sağlar. |
| `Get-AzureStackUpdateVerboseLog` | Güncelleştirme ile oluşturulan ayrıntılı günlükleri döndürür. |
| `Resume-AzureStackUpdate` | Başarısız olduğu bir noktada başarısız bir güncelleştirme devam ettirir. Bazı senaryolarda, güncelleştirmeye devam et önce azaltma adımlarını tamamlamak zorunda kalabilirsiniz.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Cmdlet'leri kullanılabilir doğrulayın
Cmdlet'leri Azure yığınının 1710 güncelleştirme paketindeki yeni olduğundan, izleme yeteneği kullanılabilir olmadan önce belirli bir noktaya almak 1710 güncelleştirme işlemi gerekir. Genellikle, cmdlet'leri Yönetici portalı'nı durum 1710 güncelleştirme sırasında olduğunu gösteriyorsa, kullanılabilir **depolama konaklar yeniden** adım. Özellikle, cmdlet güncelleştirme sırasında ortaya çıkan **. adım: adım 2.6 - güncelleştirme PrivilegedEndpoint beyaz liste çalıştıran**.

Ayrıca, cmdlet'leri program aracılığıyla ayrıcalıklı uç noktasından komut listesi sorgulayarak kullanılabilir olup olmadığını belirleyebilirsiniz. Bunu yapmak için donanım yaşam döngüsü ana bilgisayardan veya bir ayrıcalıklı erişim istasyonundan aşağıdaki komutları çalıştırın. Ayrıca, bir güvenilir ana bilgisayar ayrıcalıklı uç noktası olduğundan emin olun. Daha fazla bilgi için bkz: 1. adımını [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Azure yığın ortamınızdaki ERCS sanal makinelerin herhangi bir PowerShell oturumu oluşturun (*önek*-ERCS01, *önek*-ERCS02, veya *önek*-ERCS03). Değiştir *önek* ortamınıza sanal makine önek dizesi ile.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Kimlik bilgileri istendiğinde kullanmak &lt; *Azure yığın etki alanı*&gt;\cloudadmin hesabı ya da CloudAdmins grubunun bir üyesi olan bir hesap. CloudAdmin hesap için AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

2. Ayrıcalıklı uç kullanılabilir komutların tam listesini alın. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Ayrıcalıklı endpoint güncelleştirildi belirler.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Microsoft.AzureStack.UpdateManagement modülüne özgü komutlar listeleyin.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Örneğin:
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Get-AzureStackUpdateVerboseLog                     0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>Güncelleştirme yönetimi cmdlet'leri kullanın

> [!NOTE]
> Donanım yaşam döngüsü ana bilgisayardan veya bir ayrıcalıklı erişim istasyonundan aşağıdaki komutları çalıştırın. Ayrıca, bir güvenilir ana bilgisayar ayrıcalıklı uç noktası olduğundan emin olun. Daha fazla bilgi için bkz: 1. adımını [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Ayrıcalıklı bitiş noktasına bağlanmak ve oturum değişkeni atayın

Azure yığın ortamınızdaki ERCS sanal makinelerin herhangi bir PowerShell oturumu oluşturmak için aşağıdaki komutları çalıştırın (*önek*-ERCS01, *önek*-ERCS02, veya *önek*-ERCS03) ve bir oturum değişkeni atamak için.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Kimlik bilgileri istendiğinde kullanmak &lt; *Azure yığın etki alanı*&gt;\cloudadmin hesabı ya da CloudAdmins grubunun bir üyesi olan bir hesap. CloudAdmin hesap için AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Geçerli güncelleştirme Çalıştır üst düzey durumunu Al 

Geçerli güncelleştirme Çalıştır üst düzey durumunu almak için aşağıdaki komutları çalıştırın: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Olası değerler şunlardır:

- Çalışıyor
- Tamamlandı
- Başarısız 
- İptal edildi

Art arda en güncel durumunu görmek için bu komutları çalıştırabilirsiniz. Yeniden denetlemek için bir bağlantı yeniden oluşturmak zorunda değilsiniz.

### <a name="get-the-full-update-run-status-with-details"></a>Çalışma durumu ayrıntılarla tam güncelleştirmeyi alın 

XML dizesi olarak özeti çalıştırın tam güncelleştirme elde edebilirsiniz. Dize incelemek, bir dosyaya yazmak veya bir XML belgesi dönüştürün ve onu ayrıştırmak için PowerShell kullanın. Aşağıdaki komutu şu anda çalışan adımların hiyerarşik bir listesini almak için XML ayrıştırır.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

Aşağıdaki örnekte, güncelleştirme ve depolama ana bilgisayarları yeniden başlatmak için bir alt plan (bulut güncelleştirmesi) en üst düzey adım vardır. Bu depolama konaklar yeniden planı bir ana bilgisayar üzerindeki Blob Depolama Birimi hizmeti güncelleştiriyor gösterir.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']") 

    FullStepIndex : 2
    Index         : 2
    Name          : Cloud Update
    Description   : Perform cloud update.
    StartTimeUtc  : 2017-10-13T12:50:39.9020351Z
    Status        : InProgress
    Task          : Task
    
    FullStepIndex  : 2.9
    Index          : 9
    Name           : Restart Storage Hosts
    Description    : Restart Storage Hosts.
    EceErrorAction : Stop
    StartTimeUtc   : 2017-10-13T15:44:06.7431447Z
    Status         : InProgress
    Task           : Task
    
    FullStepIndex : 2.9.2
    Index         : 2
    Name          : PreUpdate ACS Blob Service
    Description   : Check function level, update deployment artifacts, configure Blob service settings
    StartTimeUtc  : 2017-10-13T15:44:26.0708525Z
    Status        : InProgress
    Task          : Task
```

### <a name="get-the-verbose-progress-log"></a>Ayrıntılı ilerleme durumu günlük Al

Günlük dosyasını incelemek için yazabilirsiniz. Bu güncelleştirme hatası tanılamanıza yardımcı olabilir.

```powershell
$log = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateVerboseLog }

$log > ".\UpdateVerboseLog.txt" 
```

### <a name="actively-view-the-verbose-logging"></a>Ayrıntılı günlük kaydı etkin olarak görüntüleme

İçin etkin olarak ayrıntılı günlüğü'nün güncelleştirilmesi sırasında çalıştırın ve en son girişlere atlama görünüm aşağıdaki komutları çalıştırın oturum etkileşimli modda girin ve günlük göstermek için:

```powershell
Enter-PSSession -Session $pepSession 

Get-AzureStackUpdateVerboseLog -Wait 
```
60 saniyede günlük güncelleştirir ve yeni içerik (varsa) konsoluna yazılır. 

Uzun süre çalışan arka plan işlemleri sırasında konsol çıktısı konsola süre için yazılabilir değil. Etkileşimli çıktıyı iptal etmek için Ctrl + C tuşlarına basın. 

### <a name="resume-a-failed-update-operation"></a>Bir başarısız güncelleştirme işlemi sürdürme

Güncelleştirmesi başarısız olursa, güncelleştirme çalışmasındaki kaldığı yerden devam edebilirsiniz.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Sorun giderme

Ayrıcalıklı uç noktası, tüm ERCS sanal makinelerinde Azure yığın ortamında kullanılabilir. Bağlantı için yüksek oranda kullanılabilir bir uç noktası oluşturulmayan nedeniyle, zaman zaman kesintiler, uyarı veya hata iletileri karşılaşabilirsiniz. Bu iletiler oturum kesildi veya ECE hizmetiyle iletişim kurulurken bir hata olduğunu gösteriyor olabilir. Bu davranış beklenir. İşlemi birkaç dakika içinde yeniden deneyin veya diğer ERCS sanal makinelerden birinde yeni bir ayrıcalıklı endpoint oturumu oluşturun. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md) 


