---
title: Azure Otomasyonu runbook'ları hatalarla ilgili sorunları giderme
description: Azure Otomasyonu runbook'ları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 41eb31ecabb20ec9eec3db13d5eda9f9cfbe6c69
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015475"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook'ları ile hatalarını giderme

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure Otomasyonu runbook'ları ile çalışırken kimlik doğrulaması hataları

### <a name="sign-in-failed"></a>Senaryo: Azure hesap için oturum açın

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Add-AzureAccount` veya `Connect-AzureRmAccount` cmdlet'leri.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Nedeni

Kimlik bilgisi varlığı adı geçerli değil veya kullanıcı adı ve Otomasyonu kimlik bilgisi varlığı ' için kullandığınız parola geçerli değil. Bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Neyin yanlış olduğunu belirlemek için aşağıdaki adımları uygulayın:  

1. Dahil olmak üzere herhangi bir özel karakter içermediğinden emin olun **@** Azure'a bağlanmak için kullandığınız Otomasyon kimlik bilgisi varlığı ad karakteri.  
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

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Hata

Abonelik adı geçerli değil veya abonelik ayrıntıları get yapılmaya çalışılırken Azure Active Directory kullanıcı Abonelik Yöneticisi olarak yapılandırılmamış. Bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Azure'a düzgün bir şekilde kimlik doğrulaması yaptınız ve seçmek için çalıştığınız abonelik erişimi belirlemek için aşağıdaki adımları uygulayın:  

1. Betiğinizi Azure Automation'ın tek başına çalışacağından emin olmak için dışında test edin.
2. Siz çalıştırdığınızdan emin olun **Add-AzureAccount** cmdlet'ini çalıştırmadan önce **Select-AzureSubscription** cmdlet'i.  
3. Ekleyerek bu hatayı görmeye devam ediyorsanız, kodunuzu değiştirin **- AzureRmContext** parametre **Add-AzureAccount** cmdlet'ini ve ardından kod yürütün.

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Senaryo: çok faktörlü kimlik doğrulaması etkin olmadığından Azure kimlik doğrulaması başarısız oldu

#### <a name="issue"></a>Sorun

Azure'da, Azure kullanıcı adı ve parola ile kimlik doğrulaması yapılırken aşağıdaki hatayı alırsınız:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Nedeni

Çok faktörlü kimlik doğrulaması Azure hesabınız varsa, Azure için kimlik doğrulaması için bir Azure Active Directory kullanıcı kullanamazsınız. Bunun yerine, bir sertifika veya bir hizmet sorumlusu, Azure'de kimlik doğrulamasını kullanmanız gerekir.

#### <a name="resolution"></a>Çözüm

Azure Klasik dağıtım modeli cmdlet'leriyle bir sertifikayı kullanmak için başvurmak [oluşturma ve Azure hizmetlerini yönetmek için bir sertifika ekleme.](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Bir hizmet sorumlusu ile Azure Resource Manager cmdlet'lerini kullanmak için başvurmak [hizmet sorumlusu Azure portalını kullanarak oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md) ve [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="common-errors-when-working-with-runbooks"></a>Runbook'larla çalışırken sık karşılaşılan hatalar

### <a name="task-was-cancelled"></a>Senaryo: Runbook hatasıyla başarısız oluyor: bir görev iptal edildi

#### <a name="issue"></a>Sorun

Runbook'unuzda, aşağıdaki örneğe benzer bir hata ile başarısız olur:

```
Exception: A task was canceled.
```

#### <a name="cause"></a>Nedeni

Bu hata eski Azure modüllerini kullanarak neden olabilir.

#### <a name="resolution"></a>Çözüm

Bu hata, Azure modüllerini en son sürüme güncelleştirerek çözülebilir.

Otomasyon hesabınızda tıklayın **modülleri**, tıklatıp **güncelleştirme Azure modüllerini**. Güncelleştirme yaklaşık 15 başarısız runbook yeniden çalıştırın dakika, tam bir kez sürer. Modüllerinizi güncelleştirme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda güncelleştirme Azure modüllerini](../automation-update-azure-modules.md).

### <a name="child-runbook-auth-failure"></a>Senaryo: Birden çok aboneliği ile ilgilenirken alt runbook başarısız

#### <a name="issue"></a>Sorun

Alt runbook'ları yürütürken `Start-AzureRmRunbook`, alt runbook'un Azure kaynaklarını yönetmek başarısız olur.

#### <a name="cause"></a>Nedeni

Alt runbook'un doğru bağlamı çalıştırırken kullanmıyor.

#### <a name="resolution"></a>Çözüm

Birden çok aboneliği ile çalışırken, abonelik bağlamına alt runbook'ları çağrılırken kaybolmuş olabilir. Abonelik bağlamına alt runbook'larına geçirilir emin olmak için ekleme `AzureRmContext` cmdlet'i ve ona geçiş bağlam parametresi.

```azurepowershell-interactive
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

### <a name="not-recognized-as-cmdlet"></a>Senaryo: Runbook nedeniyle eksik bir cmdlet başarısız olur.

#### <a name="issue"></a>Sorun

Runbook'unuzda, aşağıdaki örneğe benzer bir hata ile başarısız olur:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Cmdlet içeren modülü Otomasyon hesabına aktarılmamış
2. Cmdlet içeren modül alınır, ancak güncel değil

#### <a name="resolution"></a>Çözüm

Bu hata, aşağıdaki görevlerden birini tamamlayarak çözülebilir:

Bir Azure modülü modülü ise bkz [Azure automation'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md) modüllerinizi Otomasyon hesabınızdaki güncelleştirme hakkında bilgi edinmek için.

Modülde Otomasyon hesabınızda içeri ayrı bir modül ise, emin olun.

### <a name="job-attempted-3-times"></a>Senaryo: Runbook işi başlangıç üç kez denendi ancak her zaman başlatılamadı

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Bellek sınırı. Bir korumalı alan için ne kadar bellek tahsis belgelenmiş sınırlamanın olan [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits) şekilde 400 MB'tan fazla bellek kullanıyorsa, bir işi başarısız olabilir.

1. Ağ yuvaları. Anlatıldığı gibi Azure sanal 1000 eşzamanlı ağ yuvaları ile sınırlı [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits).

1. Modül uyumsuz. Modül bağımlılıklarının doğru değilse ve bunlar değilseniz, bu hata oluşabilir, runbook'unuzu genellikle "komut bulunamadı" döndürür veya "parametresi bağlanılamıyor" iletisi.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Bellek sınırı içinde çalışmak için önerilen yöntemler bellekteki gereksiz çıkış, runbook'lardan yazamazlar kadar verileri işlemek veya PowerShell akışınıza yazma kaç kontrol noktaları göz önünde bulundurun değil iş yükünü birden çok runbook arasında bölmek üzeresiniz runbook'ları. Clear yöntemi gibi kullanabileceğiniz `$myVar.clear()` kullanın ve değişken temizlemek için `[GC]::Collect()` çöp toplama çalıştırmak için hemen, bu bellek Ayak izi, runbook'un sırasında çalışma zamanı azaltır.

* Azure modüllerini adımları izleyerek [Azure automation'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md).  

* Runbook'u çalıştırmak için başka bir çözüm olan bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md). Karma çalışanları tarafından Azure sanal bellek ve ağ sınırları sınırlı değildir.

### <a name="fails-deserialized-object"></a>Senaryo: Runbook seri durumdan çıkarılmış nesne nedeniyle başarısız olur.

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
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

### <a name="quota-exceeded"></a>Senaryo: Runbook işi başarısız oldu, ayrılmış kotasını aştığı için

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Nedeni

İş yürütme hesabınız için 500 dakikalık ücretsiz kotayı aşıyor. Bu hata oluşur. Bu kota bir işi test etme, portaldan bir işi başlatılıyor, Web kancalarını kullanma ve Azure portalını kullanarak veya yürütmek için bir iş zamanlama iş yürütme, veri merkezinizdeki iş yürütme görevleri tüm türleri için geçerlidir. Otomasyon için fiyatlandırma hakkında daha fazla bilgi için bkz: [Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Çözüm

Ücretsiz aboneliğinizi değiştirmek gereken işleme başına aylık 500 dakikadan fazla kullanmak istiyorsanız, temel katmana katman. Aşağıdaki adımları izleyerek, temel katmana yükseltebilirsiniz:  

1. Azure aboneliğinizde oturum açın  
2. Yükseltmek istediğiniz Otomasyon hesabını seçin  
3. Tıklayarak **ayarları** > **fiyatlandırma**.
4. Tıklayın **etkinleştirme** hesabınıza yükseltmek için alt sayfa üzerinde **temel** katmanı.

### <a name="cmdlet-not-recognized"></a>Senaryo: bir runbook çalıştırılırken tanınmıyor cmdlet'i

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Nedeni

PowerShell altyapısı runbook'unuzu kullandığınız cmdlet bulamadığında bu hataya neden olur. Bu cmdlet içeren modül hesaptan eksik olduğundan, bir runbook adı ile bir ad çakışması veya cmdlet ayrıca başka bir modülde var ve Otomasyon adı çözümlenemiyor olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:  

* Cmdlet adı doğru girdiğinizden denetleyin.  
* Cmdlet, Otomasyon hesabınızda var olduğundan ve hiçbir çakışma yok emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için düzenleme modu ve arama çalıştırın veya kitaplığı içinde bulmak istediğiniz cmdlet'in bir runbook'u açın `Get-Command <CommandName>`. Hesaba cmdlet kullanılabilir doğruladıktan ve diğer cmdlet'leri veya runbook'ları adı çakışması yok sonra tuvaline ekleyin ve runbook'unuzu ayarlayın, geçerli bir parametre kullandığınızdan emin olun.  
* Bir ad çakışması varsa ve iki farklı modülde cmdlet kullanılabilir, bu cmdlet'i için tam olarak nitelenmiş adını kullanarak çözebilirsiniz. Örneğin, kullanabileceğiniz **ModuleName\CmdletName**.  
* Ardından bir karma çalışanı grubu içinde şirket runbook yürütme, modül ve cmdlet yüklendiğini karma çalışanı barındıran makinede emin olun.

### <a name="long-running-runbook"></a>Senaryo:, Tamamlanması uzun süre çalışan bir runbook başarısız.

#### <a name="issue"></a>Sorun

Runbook'unuzda gösterir bir **durduruldu** 3 saat çalıştıktan sonra durum. Ayrıca hata iletisini alabilirsiniz:

```
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Bu tasarım gereği, Azure sanal "Adil Share" işlemleri üç saatten uzun çalıştırırsa, otomatik olarak bir runbook'u durdurur Azure automation'da izlenmesini nedeniyle, davranıştır. Serginin paylaşımı süre giden bir runbook'un durumunu runbook türüne göre farklılık gösterir. PowerShell ve Python runbook'ları ayarlanmış bir **durduruldu** durumu. PowerShell iş akışı runbook'ları ayarlandığında **başarısız**.

#### <a name="cause"></a>Nedeni

Runbook bir Azure sanal adil paylaşımı tarafından izin verilen 3 saatlik sınırın üstünde çalıştı.

#### <a name="resolution"></a>Çözüm

Bir önerilen çözümdür runbook'u çalıştırmak için bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md).

Karma çalışanları tarafından sınırlı olmayan [adil paylaşımı](../automation-runbook-execution.md#fair-share) Azure sanal olan 3 saat runbook sınırı. Karma Runbook çalışanları 3 saat adil paylaşım sınırı sınırlı değildir, ancak runbook'ları çalıştıran karma Runbook çalışanları hala geliştirilen beklenmeyen yerel altyapı sorunları durumunda yeniden davranışları desteklemek için.

Runbook oluşturarak iyileştirmek için başka bir seçenektir [alt runbook'ları](../automation-child-runbooks.md). Runbook'unuz bir dizi gibi birden fazla veritabanı bir veritabanı işlem kaynakları üzerinde aynı işlevi aracılığıyla döngü bir alt runbook için bu işlevi taşıyabilirsiniz. Bu alt runbook'ların her biri ayrı işlemler halinde yürütülerek üst runbook'un tamamlanması için gereken toplam süreyi kısaltır.

Alt runbook senaryoyu PowerShell cmdlet'leri şunlardır:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) -Bu cmdlet, bir runbook başlatın ve runbook için parametreler sağlar

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) -alt runbook'un tamamlandıktan sonra yapılması gereken işlemler varsa her alt için iş durumunu denetlemek Bu cmdlet'i sağlar.

### <a name="expired webhook"></a>Senaryo: Durumu: 400 Hatalı istek bir Web kancası çağrılırken

#### <a name="issue"></a>Sorun

Bir Azure Otomasyonu runbook için bir Web kancası çağırma denediğinizde aşağıdaki hatayı alırsınız.

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Nedeni

Çağırmaya çalıştığınız Web kancası devre dışı bırakılmış veya süresi doldu.

#### <a name="resolution"></a>Çözüm

Web kancası devre dışı bırakılırsa, Azure portalı üzerinden bir Web kancası yeniden etkinleştirebilirsiniz. Web kancasının süresi dolmuşsa, Web kancası silinmesi ve yeniden oluşturulması gerekir. Yalnızca [bir Web kancasını yenileme](../automation-webhooks.md#renew-webhook) zaten dolmadı durumunda.

### <a name="429"></a>Senaryo: 429: İstek hızı anda de çok büyük. Lütfen yeniden deneyin

#### <a name="issue"></a>Sorun

Uygulamanızı çalıştırdığınızda aşağıdaki hata iletisini alırsınız `Get-AzureRmAutomationJobOutput` cmdlet:

```
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Nedeni

Bu hata, iş çıktısı birçok runbook'tan alınırken oluşabilir [ayrıntılı akışları](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için iki yolu vardır:

* Runbook'u düzenleme ve iş akışları, kendisini çıkaran derlemeninkinden sayısını azaltın.
* Cmdlet çalışırken alınacak akış sayısını azaltın. Belirtebileceğiniz bunu yapmanın `-Stream Output` parametresi `Get-AzureRmAutomationJobOutput` almak için cmdlet'i yalnızca çıkış akışlarına. 

## <a name="common-errors-when-importing-modules"></a>Modüller içeri aktarılırken yaygın hataları

### <a name="module-fails-to-import"></a>Senaryo: Modülü içeri aktarmak başarısız olursa veya cmdlet'leri içeri aktardıktan sonra yürütülemez.

#### <a name="issue"></a>Sorun

Bir modülü içeri aktarmak başarısız olursa veya başarıyla alır, ancak hiçbir cmdlet'leri ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modül başarıyla Azure Otomasyonu'na ekleme alabilir olmayan bazı yaygın nedenleri şunlardır:

* Yapı Otomasyonu olması gereken yapısı eşleşmiyor.
* Modül, Otomasyon hesabınıza dağıtmamış başka bir modül bağımlıdır.
* Modülün bağımlılıklarını klasöründe eksik.
* `New-AzureRmAutomationModule` Cmdlet modülü ve tam depolama yolu verildiğinde have't karşıya yüklemek için kullanılan veya modülü genel olarak erişilebilir bir URL kullanarak yüklenen henüz.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Modül aşağıdaki biçimde uyduğundan emin olun: ModuleName.Zip **->** ModuleName veya sürüm numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .Psd1 dosyasını açın ve modülü herhangi bir bağımlılığın olup olmadığına bakın. Aksi halde bu modüller Otomasyon hesabına yükleyin.
* Başvurulan tüm .dll Modülü klasörde mevcut olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.