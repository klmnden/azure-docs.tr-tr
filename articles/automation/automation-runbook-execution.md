---
title: Azure automation'da Runbook yürütme
description: Azure automation'da bir runbook nasıl işleneceğini ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 38dd4d13aa45b69fc846ef9b6b2e1b56f56de573
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59544764"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure automation'da Runbook yürütme

Azure Automation'da bir runbook başlattığınızda bir iş oluşturulur. Bir iş, runbook’un tek bir yürütme örneğidir. Her bir iş çalıştırmak için bir Azure Otomasyonu çalışanı atanır. Çalışanların birçok Azure hesapları ile paylaşılır, ancak farklı bir Otomasyon hesaplarından işleri birbirinden yalıtılmış. Yoksa sahip denetim işiniz için isteği hangi çalışan hizmetleri. Tek bir runbook'ta aynı anda çalışan çok sayıda iş olabilir. Yürütme Ortamı aynı Otomasyon hesabına ait işler için yeniden kullanılabilir. Daha fazla işi aynı anda çalıştırmak, daha sık, aynı korumalı alan gönderilebilir. Aynı korumalı alan işlemi içinde çalıştırılan işler birbirine etkileyebilir, bir örnek çalışıyor `Disconnect-AzureRMAccount` cmdlet'i. Bu cmdlet'in çalıştırılması paylaşılan korumalı alan işlemi her runbook işi kesin. Runbook'ların listesini Azure portalında görüntülediğinizde, her runbook için başlatılan tüm işlerin durumunu listeler. Her durumunu izlemek her runbook için iş listesini görüntüleyebilirsiniz. İş günlükleri, en fazla 30 gün için saklanır. Farklı iş durumları açıklamasını [iş durumları](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

Bir runbook işi için yaşam döngüsü Aşağıdaki diyagramda gösterilmiştir [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks), [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) ve [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks).

![İş durumları - PowerShell iş akışı](./media/automation-runbook-execution/job-statuses.png)

Azure aboneliğinizde bir bağlantı yaparak Azure kaynaklarınıza erişimi işlerinizi var. Bu kaynaklara genel buluttan erişilemeyen ise yalnızca kaynaklarına erişimi veri Merkezinize sahiptirler.

## <a name="where-to-run-your-runbooks"></a>Runbook'larınızı çalıştırılacak konum:

Azure automation'daki Runbook'lar çalışma zamanı üzerinde bir korumalı alan Azure'da veya bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md). Bir korumalı alan, azure'da birden fazla iş tarafından kullanılan paylaşılan bir ortamdır. Aynı sanal kullanarak işleri tarafından korumalı kaynak sınırlamaları bağlıdır. Karma Runbook çalışanları, runbook'ları doğrudan rolünü barındıran bilgisayarda ve ortamınızda bu yerel kaynakları yönetmek için kaynakların karşı çalıştırabilirsiniz. Runbook'ları tutulur ve yönetilen Azure Otomasyonu'nda ve sonra bir veya daha fazla atanmış bilgisayarlara teslim. Çoğu runbook'ları kolayca Azure sanal içinde çalıştırabilirsiniz. Burada bir karma Runbook, runbook yürütmek için bir Azure sanal seçme önerilen senaryoları da bulunur. Aşağıdaki tabloda bazı örnek senaryolar bir listesi için bkz:

|Görev|En iyi seçimi|Notlar|
|---|---|---|
|Azure kaynakları ile tümleştirme|Azure sanal|Azure'da barındırılan, kimlik doğrulaması basittir. Bir Azure sanal makinesinde bir karma Runbook çalışanı'nı kullanıyorsanız, kullanabileceğiniz [kimliklerini Azure kaynakları için yönetilen](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)|
|Azure kaynaklarını yönetmek için en iyi performans|Azure sanal|Betik daha az gecikme süresi sırayla sahip aynı ortamda çalıştırılır.|
|İşletim maliyetlerinin en aza|Azure sanal|Hiçbir işlem ek yükü, bir VM için gerek yoktur|
|Uzun süre çalışan betik|Karma Runbook Çalışanı|Azure sanal sahip [kaynaklarını bir sınırlama](../azure-subscription-service-limits.md#automation-limits)|
|Yerel Hizmetleri ile etkileşim kurma|Karma Runbook Çalışanı|Doğrudan konak makine erişebilir|
|3. taraf yazılım ve yürütülebilir dosyalar gerektirir|Karma Runbook Çalışanı|İşletim sistemi yönetmek ve yazılım yükleyebilirsiniz|
|Bir dosya veya klasör bir runbook ile izleme|Karma Runbook Çalışanı|Kullanım bir [İzleyici görevi](automation-watchers-tutorial.md) bir karma Runbook çalışanı üzerinde|
|Yoğun kaynak betiği|Karma Runbook Çalışanı| Azure sanal sahip [kaynaklarını bir sınırlama](../azure-subscription-service-limits.md#automation-limits)|
|Belirli gereksinimlerine modüllerini kullanma| Karma Runbook Çalışanı|Bazı örnekler şunlardır:</br> **WinSCP** -winscp.exe bağımlılığı </br> **Iısadministration** -IIS etkinleştirilmesi gerekiyor|
|Yükleyici gerektiren bir modülünü yükleme|Karma Runbook Çalışanı|Modüller için korumalı alan copiable olmalıdır|
|Runbook'ları veya 4.7.2 farklı .NET Framework gerektiren modülleri kullanma|Karma Runbook Çalışanı|Otomasyon korumalı alanları, .NET Framework 4.7.2 sahip ve yükseltme yolu yoktur|
|Yetki yükseltmesi gerektiren betikleri|Karma Runbook Çalışanı|Sanal yükseltme izin vermez. Bunu çözmek için bir karma Runbook çalışanı kullanmak ve UAC ve kullanım Kapat `Invoke-Command` komutu çalıştıran gerektirdiğinde yükseltme|
|WMI erişmesi betikleriniz|Karma Runbook Çalışanı|Bulutta sanal çalışan işleri [WMI'ya erişimi yoktur](#device-and-application-characteristics)|

## <a name="runbook-behavior"></a>Runbook davranışı

Runbook'ları yürütmesine içinde tanımlanan mantık göre. Bir runbook kesintiye uğrarsa, runbook başına yeniden başlatır. Bu davranış, burada geçici bir sorun varsa yeniden başlatılmadan destekledikleri bir şekilde yazılması için runbook'ları gerektirir.

Bir Runbook'tan başlatılan PowerShell işleri çalıştıran bir Azure sanal tam dil modunda çalışmayabilir. PowerShell dil modları hakkında daha fazla bilgi için bkz. [PowerShell dil modları](/powershell/module/microsoft.powershell.core/about/about_language_modes). Azure Otomasyon işleri ile etkileşim kurmak hakkında ek ayrıntılar için bkz. [PowerShell ile iş durumunu alma](#retrieving-job-status-using-powershell)

### <a name="creating-resources"></a>Kaynakları oluşturma

Betiğinizi kaynakları oluşturursa, yeniden oluşturmaya başlamadan önce kaynak zaten olup olmadığını denetlemelisiniz. Aşağıdaki örnekte basit bir örneği gösterilmektedir:

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzureRmResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exists, creating"
    New-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

### <a name="time-dependant-scripts"></a>Zaman bağımlı betikler

Runbook'ları yazarken dikkatli yapılması gerekir. Daha önce bahsedildiği gibi runbook'ları, sağlam bir şekilde yazılması gerekir ve runbook yeniden başlatın veya başarısız olmasına neden olabilecek geçici hatalar işleyebilir. Bir runbook başarısız olursa, yeniden denenir. Bir runbook normalde bir zaman kısıtlaması içinde çalışıyorsa, yürütme zamanı, başlangıç gibi işlemleri sağlamak için runbook'ta uygulanması gereken Kapat bakın veya ölçeği genişletme için mantık yalnızca belirli saatlerde çalıştırılır.

### <a name="tracking-progress"></a>İlerlemeyi izleme

Doğası gereği modüler olmak runbook'ları yazmak için iyi bir uygulamadır. Bu mantık, runbook sağlayacak şekilde yeniden ve kolayca yeniden yapılandırılması anlamına gelir. Bir runbook devam eden izleme ilgili sorunlar varsa bir runbook mantığının doğru şekilde çalıştığından emin olmak için iyi bir yoludur. Runbook ilerlemesini izlemek için bazı olası yolları, depolama hesabı, bir veritabanı veya paylaşılan dosyalar gibi dış kaynak kullanmaktır. Durumu harici olarak izleyerek, mantıksal runbook'unuzda ilk denetlenecek runbook geçen son eylemi durumunu oluşturabilirsiniz. Ardından atlamanız sonuçlarına veya belirli görevleri runbook'ta devam.

### <a name="prevent-concurrent-jobs"></a>Eşzamanlı iş engelle

Aynı anda birden çok iş arasında çalıştırıyorsanız, bazı runbook'ları garip davranabilir. Bu durumda, bir runbook zaten çalışan bir iş olup olmadığını denetlemek için mantık uygulamak önemlidir. Bu davranışı nasıl yapabilirsiniz, basit bir örneği aşağıdaki örnekte gösterilmiştir:

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzureRmAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzureRmContext $AzureContext

# If then check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

### <a name="working-with-multiple-subscriptions"></a>Birden fazla abonelik ile çalışma

Runbook'ları birden çok aboneliği ile uğraşmak yazarken, kullanım runbook'unuzu gerekiyor [Disable-AzureRmContextAutosave](/powershell/module/azurerm.profile/disable-azurermcontextautosave) , kimlik doğrulaması bağlamı olabilecek başka bir runbook'tan alınmamış emin olmak için cmdlet aynı sanal alanda çalışır. Daha sonra kullanmanız gerekir `-AzureRmContext` parametresi, `AzureRM` cmdlet'leri ve uygun Bağlamınızı geçirin.

```powershell
# Ensures you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave –Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal `
-Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzureRmContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzureRmAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

### <a name="handling-exceptions"></a>Özel durum işleme

Betik yazarken, özel durumlar ve olası aralıklı hatalar işleyebilmesi önemlidir. Özel durumlar veya runbook'larınızı aralıklı sorunlar işlemek için farklı bazı yollar şunlardır:

#### <a name="erroractionpreference"></a>$ErrorActionPreference

[$ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) tercih değişkeni, PowerShell, bir sonlandırıcı olmayan hata nasıl yanıt vereceğini belirler. Sonlandıran hata etkilenmez `$ErrorActionPreference`, her zaman sonlandırın. Kullanarak `$ErrorActionPreference`, normalde Sonlandırıcı olmayan bir hata ister `PathNotFound` gelen `Get-ChildItem` cmdlet'i tamamlanmasını runbook'u durdurur. Aşağıdaki örnek, gösterir kullanarak `$ErrorActionPreference`. En son `Write-Output` satırı hiçbir zaman yürütülecek betiği durduracak şekilde.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-Childitem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Son olarak, try catch

[Try catch](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) PowerShell betiklerini Sonlandırıcı hataları işlemek amacıyla kullanılır. Try Catch kullanarak özel durum ya da genel özel durumları yakalayabilir. Catch deyimi hataları izlemek için kullanılan veya hatayı işlemek kullanılan. Aşağıdaki örnek, var olmayan bir dosya indirmeyi dener. Zaman yakalar `System.Net.WebException` son değeri döndürülür başka bir özel durum oluştuysa, özel durum.

```powershell-interactive
try
{
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc")
}
catch [System.Net.WebException]
{
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch
{
    "An error occurred that could not be resolved."
}
```

#### <a name="throw"></a>throw

[Throw](/powershell/module/microsoft.powershell.core/about/about_throw) sonlandırmalı bir hata oluşturmak için kullanılabilir. Bir runbook'ta kendi mantığınızı tanımlarken bu yararlı olabilir. Belirli bir ölçütü karşılanırsa komut durdurma, kullanabileceğiniz `throw` betik durdurmak için. Aşağıdaki örnekte gösterildiği bir işlev parametresi kullanarak gerekli makine `throw`.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Yürütülebilir dosyaları'nı kullanarak veya işlemleri çağırma

Runbook'ları Azure sanal çalıştırın (örneğin, bir .exe veya subprocess.call) arama işlemleri desteklemez. Azure sanal, temel alınan tüm API'lerine erişimi olmayabilir, kapsayıcılarda çalıştırılan paylaşılan işlemlerin olduğundan budur. Burada ihtiyaç duyduğunuz 3. taraf yazılım veya arama alt işlemlerin senaryoları için üzerinde runbook yürüttüğünüz önerilir bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md).

### <a name="device-and-application-characteristics"></a>Cihaz ve uygulama özellikleri

Runbook işleri çalıştırmak Azure sanal bir cihaz veya uygulama özelliklerine erişimi. Windows üzerinde sorgu performans ölçümleri için kullanılan en yaygın WMI API'dir. Bu ortak ölçümleri bazıları şunlardır: bellek ve CPU kullanımı. Ancak, ne bir önemi yoktur API'si kullanılır. Bulutta çalışan işleri, Web tabanlı kuruluş yönetimi (Ortak Bilgi Modeli (CIM) üzerinde yerleşik, WBEM), Microsoft uygulaması için cihaz ve uygulama özelliklerini tanımlamak için endüstri standartları olduğu erişiminiz yok.

## <a name="job-statuses"></a>İş durumları

Aşağıdaki tablo, bir iş için olası farklı durumlarını tanımlar. PowerShell hataları sonlandıran ve sonlandırmayan hatalar iki tür vardır. Hataları sonlandıran kümesine runbook durumu **başarısız** oluşursa. Sonlandırıcı olmayan hatalar ortaya daha sonra devam etmek komut dosyası sağlar. Bir sonlandırıcı olmayan hata örneği kullanarak `Get-ChildItem` cmdlet'i mevcut olmayan bir yola sahip. PowerShell yolu mevcut değil, bir hata oluşturur ve bir sonraki klasöre devam görür. Bu hata runbook durumu kümesine mıydı **başarısız** ve olarak işaretlenmesi **tamamlandı**. Bir runbook Sonlandırıcı olmayan bir hatada dur zorlamak için kullanabileceğiniz `-ErrorAction Stop` cmdlet üzerinde.

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |İçin [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md), runbook derlenemedi. İçin [PowerShell Betiği runbook'ları](automation-runbook-types.md), runbook başlatılamadı ya da bir özel durum iş vardı. |
| Kaynaklar Bekleniyor başarısız oldu |İş başarısız oldu, ulaştığınız için [adil paylaşımı](#fair-share) sınırlamak üç kez ve her zaman aynı kontrol noktasından veya runbook'un başından başlatıldı. |
| Sıraya Alındı |İş, başlatılabilmek için Otomasyon çalışanındaki kaynakları bekliyor. |
| Başlatılıyor |İş bir çalışana atandı ve sistem başlatıyor. |
| Sürdürülüyor |Sistem, işi askıya alındıktan sonra sürdürüyor. |
| Çalışıyor |İş çalışıyor. |
| Çalışan, kaynaklar bekleniyor |Bunu ulaştığınız için iş kaldırıldı [adil paylaşımı](#fair-share) sınırı. Bu kısa bir süre sonra son denetim noktasından sürdürülür. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |Sistem, işi durduruluyor. |
| Askıya Alındı |İş; kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Bir runbook bir denetim noktası yoksa, runbook'un başından başlar. Bir denetim noktası varsa, bunu yeniden başlatın ve en son denetim noktasından sürdürün. Bir özel durum oluştuğunda runbook yalnızca sistem tarafından askıya alındı. Varsayılan olarak, ErrorActionPreference kümesine **devam**, yani iş hata üzerinde çalışmaya devam eder. Bu tercih değişkeni ayarlanmışsa **Durdur**, sonra da bir hatada işini askıya alır. Uygulandığı [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |
| Askıya alınıyor |Sistem, kullanıcının isteği üzerine işi askıya almak çalışıyor. Runbook, askıya alınmadan önce sonraki denetim noktasına erişmelidir. Son denetim noktasını zaten geçmiş, askıya alınmadan önce tamamlar. Uygulandığı [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Azure portalında iş durumunu görüntüleme

Azure portalında bir belirli bir runbook işinin ayrıntılarını incelemek ya da özetlenen tüm runbook işlerinin durumunu görüntüleyin. Runbook iş durumunu ve iş akışları iletmek için Log Analytics çalışma alanınız ile tümleştirme de yapılandırabilirsiniz. Azure İzleyici günlüklerine ile tümleştirme hakkında daha fazla bilgi için bkz. [Otomasyon iş durumunu ve iş akışları için Azure İzleyici günlüklerine iletmek](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Otomasyon runbook işleri özeti

Seçili Otomasyon hesabınızın sağ tarafta, tüm runbook işlerinin bir özeti görebilirsiniz **iş istatistikleri** Döşe.

![İş istatistikleri kutucuğu](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Bu kutucuk, sayı ve grafik temsilini yürütülen tüm işler için iş durumunu görüntüler.

Kutucuğa sunan **işleri** yürütülen tüm işlerin bir Özet listesini içeren sayfa. Bu sayfa, durumunu, başlangıç saati ve tamamlanma sürelerini gösterir.

![Automation hesabı işler sayfası](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

İşlerin listesini seçerek filtreleyebilirsiniz **filtre işleri** ve iş durumu, belirli bir runbook'u veya aşağı açılan liste ve zaman aralığı içinde arama filtresi.

![İş durumu Filtrele](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, belirli bir runbook için iş Özet ayrıntıları bu runbook'tan seçerek görüntüleyebileceğiniz **runbook'ları** sayfasında Otomasyon hesabınızı ve ardından **işleri** Döşe. Bu eylem sunan **işleri** sayfasında ve buradan iş kaydın ayrıntılarını ve çıktısını görüntülemek için tıklayın.

![Automation hesabı işler sayfası](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>İş Özeti

Belirli bir runbook ve en son durumlarını için oluşturulan tüm işlerin bir listesini görüntüleyebilirsiniz. Bu listeyi işe ve işe son değişikliğin tarih aralığını filtreleyebilirsiniz. Çıkış ve ayrıntılı bilgileri görüntülemek için bir proje adını tıklayın. İşin ayrıntılı görünümü, bu iş için sağlanan runbook parametrelerinin değerlerini içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanabilirsiniz.

1. Azure portalında **Otomasyon** ve ardından bir Otomasyon hesabının adını seçin.
2. Hub'ından seçin **runbook'ları** ve ardından **runbook'ları** sayfasında, listeden bir runbook seçin.
3. Seçili runbook sayfasında tıklayın **işleri** Döşe.
4. İşleri listesinde birine tıklayın ve runbook işi Ayrıntıları sayfasında ayrıntılarını ve çıktısını görüntüleyebilirsiniz.

## <a name="retrieving-job-status-using-powershell"></a>PowerShell kullanarak iş durumunu alma

Kullanabileceğiniz [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) işi oluşturulan bir runbook'u ve belirli bir işin ayrıntılarını alabilirsiniz. PowerShell ile bir runbook başlatırsanız [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), sonuçtaki işi verir. Kullanım [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) işin çıktısını bir işi almak için.

Aşağıdaki örnek komutlar örnek bir runbook'un son işini alıp, runbook parametreleri ve iş çıktısını için sağlanan değerler durumunu görüntüleyin.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir işin çıktı alır ve her bir kayıt döndürür. Case kayıtlardan biri için bir özel durum oluştu özel durum değeri yerine yazılır. Bu davranışı, özel durumları çıkış sırasında normal şekilde oturum açmamış ek bilgi sağlamak kullanışlıdır.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Etkinlik günlüğü'nden ayrıntılarını Al

Kişi veya runbook'u başlatan hesabı gibi diğer ayrıntıları Otomasyon hesabı için etkinlik günlüğü alınabilir. Aşağıdaki PowerShell örneği, söz konusu runbook çalıştırmak için son kullanıcı sağlar:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzureRmLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzureRmResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    { 
        # Get runbook
        $Runbook = Get-AzureRmAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hash table
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="fair-share"></a>Orta paylaşımı

Buluttaki tüm runbook'lar arasında kaynaklarını paylaşmak için Azure Otomasyonu geçici olarak kaldırır veya üç saatten uzun süre çalışan tüm işleri durdurur. İşler [PowerShell tabanlı runbook'ları](automation-runbook-types.md#powershell-runbooks) ve [Python runbook'ları](automation-runbook-types.md#python-runbooks) durdurulur ve yeniden değil ve durdurulmuş iş durumunu gösterir.

Uzun çalıştırmak için görevler önerilir kullanmak için bir [karma Runbook çalışanı](automation-hrw-run-runbooks.md#job-behavior). Karma Runbook çalışanları tarafından adil paylaşımı sınırlı değildir ve bir sınırlama yoktur üzerinde ne kadar bir runbook çalıştırabilirsiniz. Başka bir iş [sınırları](../azure-subscription-service-limits.md#automation-limits) hem Azure sanal hem de karma Runbook çalışanları için geçerlidir. Karma Runbook çalışanları 3 saat adil paylaşım sınırı sınırlı değildir, ancak bunlar üzerinde çalışan runbook'ları beklenmeyen yerel altyapı sorunlardan yeniden davranışları desteklemek için geliştirilen.

Runbook alt runbook'ları kullanarak en iyi duruma başka bir seçenektir. Runbook'unuzdaki bir veritabanı işlemi birkaç veritabanlarında gibi çeşitli kaynaklar aynı işlev aracılığıyla döngü varsa, bu işleve taşıyabilirsiniz bir [alt runbook](automation-child-runbooks.md) ve beraber [ Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet'i. Her biri bu alt runbook'ları paralel ayrı işlemlerde yürütür. Bu davranış, üst runbook tamamlanması için toplam süreyi azaltır. Kullanabileceğiniz [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) alt runbook'un tamamlandıktan sonra gerçekleştiren işlemleri varsa her alt için iş durumunu denetlemek için runbook'unuzda cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Automation'da bir runbook başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md)

