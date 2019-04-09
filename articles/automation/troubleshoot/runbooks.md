---
title: Azure Otomasyonu runbook'ları hatalarla ilgili sorunları giderme
description: Azure Otomasyonu runbook'ları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: f93f6c8891ba9f7407310a8f09387e97f5c1f578
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267353"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook'ları ile hatalarını giderme

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure Otomasyonu runbook'ları ile çalışırken kimlik doğrulaması hataları

### <a name="sign-in-failed"></a>Senaryo: Başarısız Azure hesabınızda oturum açın

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Add-AzureAccount` veya `Connect-AzureRmAccount` cmdlet'leri.
:

```error
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Nedeni

Kimlik bilgisi varlığı adı geçerli değilse bu hata oluşur. Kullanıcı adı ve Otomasyonu kimlik bilgisi varlığı ' için kullandığınız parola geçerli değil. Bu hata ayrıca ortaya çıkabilir.

#### <a name="resolution"></a>Çözüm

Neyin yanlış olduğunu belirlemek için aşağıdaki adımları uygulayın:  

1. Özel karakterler içermediğinden emin olun. Bu karakterler **\@** Azure'a bağlanmak için kullandığınız Otomasyon kimlik bilgisi varlığı ad karakteri.  
2. Azure Otomasyonu kimlik bilgisi, yerel PowerShell ISE Düzenleyici içinde depolanan parola ve kullanıcı adı kullanıp kullanmadığını denetleyin. Bunu yapabilirsiniz kullanıcı adı ve parola doğru PowerShell ISE'de aşağıdaki cmdlet'leri çalıştırarak denetleyin:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Yerel olarak, kimlik doğrulama başarısız olursa, Azure Active Directory kimlik bilgilerini düzgün kurmadığınızı fark anlamına gelir. Başvurmak [Azure Active Directory'yi kullanarak Azure için kimlik doğrulama](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog gönderisi doğru şekilde ayarlanan Azure Active Directory hesabı.  

4. Geçici bir hata gibi görünüyorsa, daha sağlam hale kimlik doğrulaması için kimlik doğrulama yordamına yeniden deneme mantığı eklemeyi deneyin.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Senaryo: Azure aboneliği bulunamadı

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Select-AzureSubscription` veya `Select-AzureRmSubscription` cmdlet'leri:

```error
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Hata

Bu hata oluşabilir:

* Abonelik adı geçerli değil

* Abonelik ayrıntılarının get yapılmaya çalışılırken Azure Active Directory kullanıcı Abonelik Yöneticisi olarak yapılandırılmamış.

#### <a name="resolution"></a>Çözüm

Azure'da kimlik doğrulaması yaptınız ve seçmek için çalıştığınız abonelik erişimi belirlemek için aşağıdaki adımları uygulayın:  

1. Tek başına çalışacağından emin olmak için betiğinizi Azure Otomasyonu dışında test edin.
2. Siz çalıştırdığınızdan emin olun `Add-AzureAccount` cmdlet'ini çalıştırmadan önce `Select-AzureSubscription` cmdlet'i. 
3. Ekleme `Disable-AzureRmContextAutosave –Scope Process` runbook'unuzu başlangıcına. Bu cmdlet herhangi bir kimlik bilgisi yalnızca geçerli runbook yürütme uygulanmasını sağlar.
4. Ekleyerek bu hatayı görmeye devam ediyorsanız, kodunuzu değiştirin **AzureRmContext** parametre `Add-AzureAccount` cmdlet'ini ve ardından kod yürütün.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Senaryo: Çok faktörlü kimlik doğrulaması etkin olmadığından Azure kimlik doğrulaması başarısız oldu.

#### <a name="issue"></a>Sorun

Azure'da, Azure kullanıcı adı ve parola ile kimlik doğrulaması yapılırken aşağıdaki hatayı alırsınız:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Nedeni

Çok faktörlü kimlik doğrulaması Azure hesabınız varsa, Azure için kimlik doğrulaması için bir Azure Active Directory kullanıcı kullanamazsınız. Bunun yerine, bir sertifika veya bir hizmet sorumlusu, Azure'de kimlik doğrulamasını kullanmanız gerekir.

#### <a name="resolution"></a>Çözüm

Azure Klasik dağıtım modeli cmdlet'leriyle bir sertifikayı kullanmak için başvurmak [oluşturma ve Azure hizmetlerini yönetmek için bir sertifika ekleme.](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Bir hizmet sorumlusu ile Azure Resource Manager cmdlet'lerini kullanmak için başvurmak [hizmet sorumlusu Azure portalını kullanarak oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md) ve [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="common-errors-when-working-with-runbooks"></a>Runbook'larla çalışırken sık karşılaşılan hatalar

### <a name="child-runbook-object"></a>Çıkış akışına basit veri türleri yerine nesneleri içerdiğinde alt runbook hata döndürür.

#### <a name="issue"></a>Sorun

Bir alt runbook ile çağrılırken aşağıdaki hata iletisini `-Wait` anahtar ve çıkış akışına içerir ve nesne:

```error
Object reference not set to an instance of an object
```

#### <a name="cause"></a>Nedeni

Bilinen bir sorun olduğu [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) nesneler içeriyorsa, çıkış akışına düzgün işlemez.

#### <a name="resolution"></a>Çözüm

Bunun yerine bir yoklama mantığı uygulamak ve kullanmak önerilir bu sorunu çözmek için [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) çıktısını almak için cmdlet'i. Aşağıdaki örnekte bu mantığı örneği tanımlanır.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

### <a name="get-serializationsettings"></a>Senaryo: İş akışlarınız get_SerializationSettings yöntemi hakkında bir hata bakın

#### <a name="issue"></a>Sorun

Şu iletiyle bir runbook için iş akışlarında, hataya bakın:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type 
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly 
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' 
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Nedeni

Bir runbook'ta hem AzureRM hem de Az cmdlet'leri kullanarak bu hataya neden olur. İçeri aktardığınız oluşur `Az` içeri aktarmadan önce `AzureRM`.

#### <a name="resolution"></a>Çözüm

Az hem de AzureRM cmdlet'leri içeri aktarılamıyor ve Azure automation'da Az desteği hakkında daha fazla bilgi edinmek için aynı runbook'ta kullanılan bkz [Az Modül desteği, Azure automation'da](../az-modules.md).

### <a name="task-was-cancelled"></a>Senaryo: Runbook hatasıyla başarısız oluyor: Bir görev iptal edildi

#### <a name="issue"></a>Sorun

Runbook'unuzda, aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
Exception: A task was canceled.
```

#### <a name="cause"></a>Nedeni

Bu hata eski Azure modüllerini kullanarak neden olabilir.

#### <a name="resolution"></a>Çözüm

Bu hata, Azure modüllerini en son sürüme güncelleştirerek çözülebilir.

Otomasyon hesabınızda tıklayın **modülleri**, tıklatıp **güncelleştirme Azure modüllerini**. Güncelleştirme alır yaklaşık 15 dakika, işlem tamamlandıktan sonra başarısız runbook yeniden çalıştırın. Modüllerinizi güncelleştirme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda güncelleştirme Azure modüllerini](../automation-update-azure-modules.md).

### <a name="runbook-auth-failure"></a>Senaryo: Runbook'ları birden çok aboneliği ile ilgilenirken başarısız

#### <a name="issue"></a>Sorun

Runbook'ları yürütürken `Start-AzureRmAutomationRunbook`, Azure kaynaklarını yönetmek runbook başarısız.

#### <a name="cause"></a>Nedeni

Runbook'un doğru bağlamı çalıştırırken kullanmıyor.

#### <a name="resolution"></a>Çözüm

Birden çok aboneliği ile çalışırken, abonelik bağlamına runbook'ları çağrılırken kaybolmuş olabilir. Abonelik bağlamına için runbook'ları geçirilir emin olmak için ekleme `AzureRmContext` cmdlet'i ve ona geçiş bağlam parametresi. Ayrıca kullanılması önerilir `Disable-AzureRmContextAutosave` cmdlet'iyle **işlem** kimlik bilgileri yalnızca geçerli bir runbook için kullanılmasını sağlamak için kapsamı.

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Senaryo: Eksik bir cmdlet nedeniyle runbook başarısız

#### <a name="issue"></a>Sorun

Runbook'unuzda, aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Nedeni

Bu hata göre bir aşağıdaki nedenlerden kaynaklanabilir:

1. Cmdlet içeren modülü Otomasyon hesabına aktarılmamış
2. Cmdlet içeren modül alınır, ancak güncel değil

#### <a name="resolution"></a>Çözüm

Bu hata, aşağıdaki görevlerden birini tamamlayarak çözülebilir:

Bir Azure modülü modülü ise bkz [Azure automation'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md) modüllerinizi Otomasyon hesabınızdaki güncelleştirme hakkında bilgi edinmek için.

Modülde Otomasyon hesabınızda içeri ayrı bir modül ise, emin olun.

### <a name="job-attempted-3-times"></a>Senaryo: Runbook işi başlangıç üç kez denendi ancak her zaman başlatılamadı

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```error
The job was tried three times but it failed
```

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki sorunlardan biri nedeniyle oluşur:

1. Bellek sınırı. Bir korumalı alan için ne kadar bellek ayrılan üzerinde belgelenmiş sınırlamanın konumunda bulundu [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits). Bir işi 400 MB'tan fazla bellek kullanması durumunda başarısız olabilir.

2. Ağ yuvaları. Anlatıldığı gibi Azure sanal 1000 eşzamanlı ağ yuvaları ile sınırlı [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits).

3. Modül uyumsuz. Modül bağımlılıklarının doğru değilse ve bunlar değilseniz, bu hata oluşabilir, runbook'unuzu genellikle "komut bulunamadı" döndürür veya "parametresi bağlanılamıyor" iletisi.

4. Runbook'unuzdaki bir yürütülebilir dosya çağrı yapma veya bir Azure korumalı alanında çalışan bir runbook'ta subprocess çalışıldı. Bu senaryo Azure sanal desteklenmiyor.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Bellek sınırı içinde çalışmak için önerilen yöntemler bellekteki gereksiz çıkış, runbook'lardan yazamazlar kadar verileri işlemek veya PowerShell akışınıza yazma kaç kontrol noktaları göz önünde bulundurun değil iş yükünü birden çok runbook arasında bölmek üzeresiniz runbook'ları. Clear yöntemi gibi kullanabilir `$myVar.clear()` kullanın ve değişken temizlemek için `[GC]::Collect()` çöp toplamanın hemen çalıştırma. Bu Eylemler, çalışma zamanı sırasında bellek Ayak izi, runbook'un azaltın.

* Azure modüllerini adımları izleyerek [Azure automation'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md).  

* Runbook'u çalıştırmak için başka bir çözüm olan bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md). Karma çalışanları tarafından Azure sanal bellek ve ağ sınırları sınırlı değildir.

* Bir runbook'ta (örneğin .exe veya subprocess.call) bir işlem çağırmak gerekiyorsa, runbook'u çalıştırmak ihtiyacınız bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md).

### <a name="fails-deserialized-object"></a>Senaryo: Runbook nedeniyle seri durumdan çıkarılmış nesne başarısız

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Nedeni

PowerShell iş akışı runbook'unuzu ise karmaşık nesneler iş akışını askıya alınırsa, runbook durumu kalıcı hale getirmek için seri durumdan çıkarılmış biçimde depolar.

#### <a name="resolution"></a>Çözüm

Aşağıdaki üç çözümlerden birini bu sorunu düzeltin:

1. Karmaşık bir cmdlet nesnelerden diğerine öğesine ekleyerek sorgularınızı, bu cmdlet'ler bir Inlinescript kaydır.
2. Tüm nesneyi geçirmek yerine karmaşık nesnesinden adını veya ihtiyaç duyduğunuz değerini geçirin.
3. PowerShell runbook'u bir PowerShell iş akışı runbook'u yerine kullanın.

### <a name="runbook-fails"></a>Senaryo: Runbook Uygulamam başarısız, ancak ne zaman çalıştığını yerel olarak çalıştı

#### <a name="issue"></a>Sorun

Komut başarısız olur bir runbook çalışır, ancak ne zaman çalıştığını yerel olarak çalıştı.

#### <a name="cause"></a>Nedeni

Betiğinizi bir runbook aşağıdaki nedenlerden birinden dolayı çalıştırılırken başarısız olabilir:

1. Kimlik doğrulaması sorunları
2. Gerekli modülleri içeri aktarılan veya güncel değil.
3. Betiğinizi için kullanıcı etkileşimi isteyen.
4. Bazı modüller, Windows bilgisayarlarında mevcut olan kitaplıkları hakkında varsayımlar. Bu kitaplıklar, bir korumalı alan üzerinde mevcut olmayabilir.
5. Bazı modüller, korumalı alan üzerinde kullanılabilir olandan farklı bir .NET sürümüne bağlıdır.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini bu sorunu giderebilir:

1. Doğru olduğundan emin olun [Azure için kimlik doğrulama](../manage-runas-account.md).
2. Olun, [Azure modülleri içeri aktarılan ve güncel](../automation-update-azure-modules.md).
3. Cmdlet'lerinizi hiçbiri bilgi istemi doğrulayın. Bu davranış, runbook'ları desteklenmiyor.
4. Modülün parçası olan her şeyi bir bağımlılık modülde bulunmayan bir şey olup olmadığını denetleyin.
5. Bir modül işe yaramaz daha yüksek bir sürümü kullanıyorsa, .NET Framework 4.7.2, azure sanal kullanın. Bu durumda, kullanmanız gereken bir [karma Runbook çalışanı](../automation-hybrid-runbook-worker.md)

Bu çözümlerin hiçbirinin, problemReview çözmek, [iş günlükleri](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) için neden belirli Ayrıntılar için runbook başarısız olmuş olabilir.

### <a name="quota-exceeded"></a>Senaryo: Ayrılmış kotasını aştığı için Runbook işi başarısız oldu.

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```error
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Nedeni

İş yürütme hesabınız için 500 dakikalık ücretsiz kotayı aşıyor. Bu hata oluşur. Bu kota, iş yürütme görevleri tüm türleri için geçerlidir. Bu görevlerden bazılarını test Web kancalarını kullanma veya Azure portalını kullanarak veya yürütmek için bir iş zamanlaması, veri merkezinizdeki iş yürütme portalı, bir işi başlatılıyor, bir iş. Otomasyon için fiyatlandırma hakkında daha fazla bilgi için bkz: [Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Çözüm

Ücretsiz aboneliğinizi değiştirmek gereken işleme başına aylık 500 dakikadan fazla kullanmak istiyorsanız, temel katmana katman. Aşağıdaki adımları izleyerek, temel katmana yükseltebilirsiniz:  

1. Azure aboneliğinizde oturum açın  
2. Yükseltmek istediğiniz Otomasyon hesabını seçin  
3. Tıklayın **ayarları** > **fiyatlandırma**.
4. Tıklayın **etkinleştirme** hesabınıza yükseltmek için alt sayfa üzerinde **temel** katmanı.

### <a name="cmdlet-not-recognized"></a>Senaryo: Cmdlet'i bir runbook çalıştırılırken tanınmıyor

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Nedeni

PowerShell altyapısı runbook'unuzu kullandığınız cmdlet bulamadığında bu hataya neden olur. Bu hata, bir cmdlet'i içeren modül hesaptan eksik olduğundan, bir runbook adı ile bir ad çakışması veya cmdlet ayrıca başka bir modülde mevcut ve Otomasyon adı çözümlenemiyor olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:  

* Cmdlet adı doğru girdiğinizden denetleyin.  
* Cmdlet, Otomasyon hesabınızda var olduğundan ve hiçbir çakışma yok emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için düzenleme modu ve arama çalıştırın veya kitaplığı içinde bulmak istediğiniz cmdlet'in bir runbook'u açın `Get-Command <CommandName>`. Hesaba cmdlet kullanılabilir doğruladıktan ve diğer cmdlet'leri veya runbook'ları adı çakışması yok sonra tuvaline ekleyin ve runbook'unuzu ayarlayın, geçerli bir parametre kullandığınızdan emin olun.  
* Bir ad çakışması varsa ve iki farklı modülde cmdlet kullanılabilir cmdlet'i için tam olarak nitelenmiş adını kullanarak bu sorunu çözebilir. Örneğin, kullanabileceğiniz **ModuleName\CmdletName**.  
* Ardından bir karma çalışanı grubu içinde şirket runbook yürütme, modül ve cmdlet yüklendiğini karma çalışanı barındıran makinede emin olun.

### <a name="long-running-runbook"></a>Senaryo: Tamamlanması uzun süre çalışan bir runbook başarısız

#### <a name="issue"></a>Sorun

Runbook'unuzda gösterir bir **durduruldu** 3 saat çalıştıktan sonra durum. Ayrıca hata iletisini alabilirsiniz:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Bu tasarım gereği, Azure sanal "Adil Share" işlemlerinin Azure Otomasyonu'nda izleme nedeniyle, davranıştır. Üç saatten uzun yürütülürse, adil Paylaşımı otomatik olarak bir runbook'u durdurur. Serginin paylaşımı süre giden bir runbook'un durumunu runbook türüne göre farklılık gösterir. PowerShell ve Python runbook'ları ayarlanmış bir **durduruldu** durumu. PowerShell iş akışı runbook'ları ayarlandığında **başarısız**.

#### <a name="cause"></a>Nedeni

Runbook bir Azure sanal adil paylaşımı tarafından izin verilen 3 saatlik sınırın üstünde çalıştı.

#### <a name="resolution"></a>Çözüm

Bir önerilen çözümdür runbook'u çalıştırmak için bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md).

Karma çalışanları tarafından sınırlı olmayan [adil paylaşımı](../automation-runbook-execution.md#fair-share) Azure sanal olan 3 saat runbook sınırı. Runbook'ları çalıştırıldığı yer karma Runbook çalışanları geliştirilen beklenmeyen yerel altyapı sorunları varsa, yeniden başlatma davranışları desteklemek için.

Runbook oluşturarak iyileştirmek için başka bir seçenektir [alt runbook'ları](../automation-child-runbooks.md). Runbook'unuzdaki bir veritabanı işlemi birkaç veritabanlarında gibi çeşitli kaynaklar aynı işlev aracılığıyla döngü bir alt runbook için bu işlevi taşıyabilirsiniz. Her biri bu alt runbook'ları paralel ayrı işlemlerde yürütür. Bu davranış, üst runbook tamamlanması için toplam süreyi azaltır.

Alt runbook senaryoyu PowerShell cmdlet'leri şunlardır:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) -Bu cmdlet, bir runbook başlatın ve runbook için parametreler sağlar

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) -alt runbook'un tamamlandıktan sonra yapılması gereken işlemler varsa, bu cmdlet'i için her bir alt iş durumunu denetlemenizi sağlar.

### <a name="expired webhook"></a>Senaryo: Durum: Bir Web kancası çağırırken 400 Hatalı istek

#### <a name="issue"></a>Sorun

Bir Azure Otomasyonu runbook için bir Web kancası çağırma çalıştığınızda şu hatayı alırsınız:

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Nedeni

Çağırmaya çalıştığınız Web kancası devre dışı bırakılmış veya süresi doldu.

#### <a name="resolution"></a>Çözüm

Web kancası devre dışı bırakılırsa, Azure portalı üzerinden bir Web kancası yeniden etkinleştirebilirsiniz. bir Web kancası süresi dolduğunda, Web kancası silinmesi ve yeniden oluşturulması gerekir. Yalnızca [bir Web kancasını yenileme](../automation-webhooks.md#renew-webhook) süresi dolmadıysa.

### <a name="429"></a>Senaryo: 429: Şu anda istek oranı fazla büyük. Lütfen tekrar deneyin

#### <a name="issue"></a>Sorun

Çalıştırdığınızda aşağıdaki hata iletisini alırsınız `Get-AzureRmAutomationJobOutput` cmdlet:

```error
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Nedeni

Bu hata, iş çıktısı birçok runbook'tan alınırken oluşabilir [ayrıntılı akışları](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için iki yolu vardır:

* Runbook'u düzenleme ve iş akışları, kendisini çıkaran derlemeninkinden sayısını azaltın.
* Cmdlet çalışırken alınacak akış sayısını azaltın. Bu davranış takip etmek için belirleyebileceğiniz `-Stream Output` parametresi `Get-AzureRmAutomationJobOutput` almak için cmdlet'i yalnızca çıkış akışlarına. 

### <a name="cannot-invoke-method"></a>Senaryo: PowerShell iş şu hatayla başarısız oluyor: Yöntemi çağrılamaz

#### <a name="issue"></a>Sorun

Azure'da çalışan bir runbook'ta PowerShell iş başlatma sırasında şu hata iletisini alıyorsunuz:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

#### <a name="cause"></a>Nedeni

Bir runbook işi Azure'da çalışan bir PowerShell başlattığınızda bu hata oluşabilir. Runbook'ları bir Azure'da çalıştırdığınız için bu davranış oluşabilir korumalı alan çalışmayabilir [tam dil modu](/powershell/module/microsoft.powershell.core/about/about_language_modes)).

#### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için iki yolu vardır:

* Yerine `Start-Job`, kullanın `Start-AzureRmAutomationRunbook` bir runbook başlatmak için
* Bu hata iletisini runbook'unuz varsa çalıştırın bir karma Runbook çalışanı üzerinde

Bu davranış ve diğer davranışlarını Azure Otomasyonu runbook'ları hakkında daha fazla bilgi için bkz: [Runbook davranışı](../automation-runbook-execution.md#runbook-behavior).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
