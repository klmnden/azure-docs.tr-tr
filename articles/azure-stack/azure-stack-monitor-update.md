---
title: Azure stack'teki ayrıcalıklı uç noktayı kullanarak güncelleştirmeleri izleyin | Microsoft Docs
description: Azure Stack tümleşik sistemleri güncelleştirme durumunu izlemek için ayrıcalıklı uç noktası kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.openlocfilehash: 4641dce6fe8518016ee85cd480de6d11354fe170
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037230"
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmelerini izleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Kullanabileceğiniz [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md) Azure Stack ilerlemesini izlemek için güncelleştirme çalıştırın ve başarısız bir güncelleştirme son başarılı adımından çalışmaya devam etmek için Azure Stack portalını kullanılamaz duruma gelir.  Azure Stack portalını kullanarak Azure Stack'te güncelleştirmeleri yönetmek için önerilen yöntemdir.

Güncelleştirme yönetimi için aşağıdaki yeni PowerShell cmdlet'leri, Azure Stack tümleşik sistemleri 1710 güncelleştirmesine dahil edilir.

| Cmdlet  | Açıklama  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Şu anda çalışan, tamamlandı veya başarısız güncelleştirme durumunu döndürür. Güncelleştirme işlemi ve geçerli adımı ve karşılık gelen durumunu hem açıklayan bir XML belgesi üst düzey durumunu sağlar. |
| `Resume-AzureStackUpdate` | Başarısız olduğu bir noktada başarısız bir güncelleştirme devam ettirir. Belirli senaryolarda güncelleştirmeye devam et önce risk azaltma adımlarını tamamlamanız gerekebilir.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Cmdlet'lerin kullanılabilir olduğunu doğrulayın
Cmdlet'ler, Azure Stack için 1710 güncelleştirme paketindeki yeni olduğundan, izleme olanağı kullanılabilir olmadan önce belirli bir noktaya almak 1710 güncelleştirme işlemi gerekiyor. Genellikle, cmdlet'leri Yönetici portalını durum 1710 güncelleştirme sırasında olduğunu gösteriyorsa, kullanılabilir **depolama konakları yeniden** adım. Özellikle, cmdlet güncelleştirme sırasında ortaya çıkan **. adım: adım 2.6 - güncelleştirme PrivilegedEndpoint beyaz liste çalıştıran**.

Ayrıca, cmdlet öğelerini programlı olarak ayrıcalıklı uç noktasından komut listesi sorgulayarak kullanılabilir olup olmadığını belirleyebilirsiniz. Bunu yapmak için donanım yaşam döngüsü konak ya da bir ayrıcalıklı erişim iş istasyonu aşağıdaki komutları çalıştırın. Ayrıca, bir güvenilir ana bilgisayar ayrıcalıklı uç noktası olduğundan emin olun. Daha fazla bilgi için bkz: 1. adımı [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Azure Stack ortamınıza ERCS sanal makinelerin herhangi bir PowerShell oturumu oluşturun (*önek*-ERCS01, *önek*-ERCS02, veya *önek*-ERCS03). Değiştirin *önek* ortamınıza özgü olan sanal makine ön eki dizesi ile.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Kimlik bilgileri istendiğinde kullanın &lt; *Azure Stack etki*&gt;\cloudadmin hesabı veya CloudAdmins grubunun bir üyesi olan bir hesap. CloudAdmin hesabı için AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

2. Ayrıcalıklı uç noktasında kullanılabilir komutların tam listesini alın. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Ayrıcalıklı uç nokta güncelleştirildi belirleyin.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Microsoft.AzureStack.UpdateManagement modülü özgü komutların listesi.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Örneğin:
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>Güncelleştirme yönetimi cmdlet'leri kullanın

> [!NOTE]
> Donanım yaşam döngüsü ana makineden veya bir ayrıcalıklı erişim iş istasyonu aşağıdaki komutları çalıştırın. Ayrıca, bir güvenilir ana bilgisayar ayrıcalıklı uç noktası olduğundan emin olun. Daha fazla bilgi için bkz: 1. adımı [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Ayrıcalıklı uç noktasına bağlanmak ve oturum değişkeni atayın

Azure Stack ortamınıza ERCS sanal makinelerin herhangi bir PowerShell oturumu oluşturmak için aşağıdaki komutları çalıştırın (*önek*-ERCS01, *önek*-ERCS02, veya *önek*-ERCS03) ve bir oturum değişkeni atayın.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Kimlik bilgileri istendiğinde kullanın &lt; *Azure Stack etki*&gt;\cloudadmin hesabı veya CloudAdmins grubunun bir üyesi olan bir hesap. CloudAdmin hesabı için AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Üst düzey geçerli güncelleştirme çalıştırma durumu 

Üst düzey geçerli güncelleştirme çalıştırma durumu almak için aşağıdaki komutları çalıştırın: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Olası değerler şunlardır:

- Çalışıyor
- Tamamlandı
- Başarısız 
- İptal edildi

Art arda en güncel durumunu görmek için şu komutları çalıştırabilirsiniz. Yeniden denetlemek için bir bağlantı yeniden oluşturmak zorunda değilsiniz.

### <a name="get-the-full-update-run-status-with-details"></a>Tam güncelleştirme çalıştırma durumu ayrıntıları alın 

XML dizesi olarak özeti Çalıştır tam güncelleştirme alabilirsiniz. Dize inceleme, bir dosyaya yazma veya bir XML belgesine dönüştürmek ve ayrıştırmak için PowerShell kullanın. Aşağıdaki komut, şu anda çalışan adımlar, hiyerarşik bir listesini almak için XML ayrıştırır.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

Aşağıdaki örnekte, güncelleştirme ve depolama ana bilgisayarları yeniden başlatmak için bir alt planı (bulut güncelleştirmesi) en üst düzey adım vardır. Bu depolama konakları yeniden planı Blob Depolama hizmetini konaklardan birine güncelleştiriyor gösterir.

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

### <a name="resume-a-failed-update-operation"></a>Başarısız güncelleştirme işlemi sürdürme

Güncelleştirme başarısız olursa, güncelleştirme çalışması kaldığı yerden devam edebilir.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Sorun giderme

Ayrıcalıklı uç noktası, Azure Stack ortamında tüm ERCS sanal makinelerde kullanılabilir. Yüksek oranda kullanılabilir bir uç noktasına bağlantı bırakılmaz nedeniyle zaman kesintiler, uyarı veya hata iletilerini karşılaşabilirsiniz. Bu iletiler oturum kesildi veya ECE hizmetiyle iletişim kurulurken bir hata olduğunu gösteriyor olabilir. Bu beklenen bir davranıştır. Birkaç dakika sonra işlemi yeniden deneyin veya diğer ERCS sanal makinelerden birinde yeni bir ayrıcalıklı uç nokta oturumu oluşturur. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) 


