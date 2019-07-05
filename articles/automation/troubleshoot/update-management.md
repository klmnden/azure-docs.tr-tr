---
title: Güncelleştirme yönetimi sorunlarını giderme
description: Güncelleştirme yönetimi ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 23139755af812f99bce8c2c255805eaf9e30b2da
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477068"
---
# <a name="troubleshooting-issues-with-update-management"></a>Güncelleştirme yönetimi ile ilgili sorunları giderme

Bu makalede, güncelleştirme yönetimi kullanırken çalışabilir genelinde sorunları çözmek için çözümleri açıklanmaktadır.

Bir aracı sorun giderici arka plandaki sorunu belirlemek karma çalışan aracısı yok. Sorun giderici hakkında daha fazla bilgi için bkz: [güncelleştirme aracı sorunlarını giderme](update-agent-issues.md). Diğer sorunlarda, olası sorunlar hakkında ayrıntılı bilgileri altına bakın.

## <a name="general"></a>Genel

### <a name="components-enabled-not-working"></a>Senaryo: 'Güncelleştirme Yönetim' çözümünün bileşenleri etkinleştirildi ve bu sanal makine artık yapılandırılıyor

#### <a name="issue"></a>Sorun

15 dakika sonra ekleme bir sanal makinede aşağıdaki iletiyi görmeye devam:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Otomasyon hesabını geri iletişimi engelleniyor.
2. Eklenen VM'de Sysprep kullanılarak hazırlanmış Microsoft izleme aracısının yüklü olmadığı bir kopyalanan makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

1. Ziyaret [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) hakkında adresler ve bağlantı noktaları çalışacak şekilde güncelleştirme yönetimi için izin verilmesi gereken öğrenin.
2. Kopyalanmış görüntüsünü kullanıyorsanız:
   1. Log Analytics çalışma alanınızda kayıtlı arama kapsamı yapılandırması için VM kaldırma `MicrosoftDefaultScopeConfig-Updates` gösterilen durumunda. Kayıtlı aramalar, altında bulunabilir **genel** çalışma alanınızdaki.
   2. `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force` öğesini çalıştırın
   3. Çalıştırma `Restart-Service HealthService` yeniden `HealthService`. Bu anahtarı yeniden oluşturun ve yeni UUID oluşturur.
   4. Bu işe yaramazsa, görüntü sysprep ilk ve olaydan sonra MMA aracısını yükleyin.

### <a name="multi-tenant"></a>Senaryo: Makineler için Güncelleştirme dağıtımı başka bir Azure kiracısı oluştururken, bağlı abonelik hata alırsınız.

#### <a name="issue"></a>Sorun

Makineler için Güncelleştirme dağıtımı başka bir Azure kiracısı oluşturmaya çalışırken aşağıdaki hatayı alırsınız:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Nedeni

Bir güncelleştirme dağıtımına dahil başka bir kiracıdaki Azure sanal makineleri olan bir güncelleştirme dağıtımı oluştururken bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Zamanlanmış getirmek için aşağıdaki geçici çözümü kullanmanız gerekir. Kullanabilirsiniz [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) anahtarıyla cmdlet'i `-ForUpdate` bir zamanlama oluşturmak ve kullanmak için [yeni AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet'i ve geçirin diğer kiracıdaki makineler `-NonAzureComputer` parametresi. Aşağıdaki örnek bunu nasıl bir örnek gösterir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="nologs"></a>Senaryo: Makineleri portalında güncelleştirme yönetimi altında görünmüyor

#### <a name="issue"></a>Sorun

Aşağıdaki senaryolarda çalışabilir:

* Makine programlarınızı **yapılandırılmadı** güncelleştirme yönetimi görünümünden bir VM

* Otomasyon hesabınızı güncelleştirme yönetimi görünümünden makinelerinizi eksik

* Gösterme makineler sahip **değerlendirilmeyen** altında **Uyumluluk**, ancak bir karma Runbook çalışanı ancak güncelleştirme yönetimi için sinyal verilerini Azure İzleyici günlüklerine bakın.

#### <a name="cause"></a>Nedeni

Bu, yerel yapılandırma ile ilgili olası sorunları veya yanlış yapılandırılmış kapsam yapılandırması tarafından kaynaklanabilir.

Karma Runbook çalışanı yeniden kayıtlı ve yeniden gerekebilir.

Kota saklanmasını ulaştı ve durdurma veriler çalışma alanınızdaki tanımladığınız.

#### <a name="resolution"></a>Çözüm

* Makinenizde doğru çalışma alanına raporlama emin olun. Hangi çalışma makineniz için raporlama doğrulayın. Bunu doğrulamak yönergeler için bkz: [Log Analytics aracı bağlantısını doğrulamak](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Ardından, bu çalışma alanını Azure Otomasyon hesabınıza bağlı olduğundan emin olun. Bunu doğrulamak için Otomasyon hesabınıza gidin ve **bağlantılı çalışma** altında **ilgili kaynakları**.

* Log Analytics çalışma alanınızda makineler görünmesini emin olun. Otomasyon hesabınıza bağlı Log Analytics çalışma alanınızda aşağıdaki sorguyu çalıştırın. Makinenizde makinenizde sorgu sonuçlarını görmüyorsanız, büyük olasılıkla bir yerel yapılandırma sorunu olduğu anlamına gelir sinyal göndermiyor değil. Sorun Gidericisi çalıştırabileceğiniz [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline) işletim sistemi veya, bağlı olarak olabilir [aracıyı yeniden yükleme](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Sorgu sonuçlarını makinenize gösterilir sonra çok gereken aşağıdaki madde işareti içinde belirtilen kapsam yapılandırması.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Kapsam yapılandırma sorunlarını denetleyin. [Kapsam yapılandırması](../automation-onboard-solutions-from-automation-account.md#scope-configuration) hangi makineleri çözüm için yapılandırılmaları belirler. Makinenizin çalışma alanınızda gösteren ancak değilse, gösteren makineleri hedeflemek için kapsam yapılandırması yapılandırmanız gerekecektir. Bunu yapma hakkında bilgi için bkz: [çalışma alanındaki yerleşik makine](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Yukarıdaki adımlar sorunu çözmezse bölümündeki adımları [Windows karma Runbook çalışanı dağıtma](../automation-windows-hrw-install.md) için karma çalışan Windows yeniden veya [Linux karma Runbook çalışanı dağıtma](../automation-linux-hrw-install.md) Linux için.

* Çalışma alanınızda aşağıdaki sorguyu çalıştırın. Sonuç görürseniz `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` ulaşıldı ve kaydedilmesini veri durdurdu çalışma alanınızda tanımlanan bir kotası vardır. Çalışma alanınızda gidin **kullanım ve Tahmini maliyetler** > **veri hacmi Yönetimi** kotanızı denetleyin ve sahip olduğunuz kotayı Kaldır.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

## <a name="windows"></a>Windows

Ekleme çözüm bir sanal makine üzerinde çalışırken sorunlarla karşılaşırsanız, kontrol **Operations Manager** altında olay günlüğüne **uygulama ve hizmet günlükleri** olan olaylar için yerel makinede Olay Kimliği **4502** ve içeren olay iletisi **sorun**.

Aşağıdaki bölümde her biri için belirli hata iletileri ve olası çözümü vurgulanmıştır. Diğer ekleme için sorunlara bakın, [çözüm ekleme sorunlarını giderme](onboarding.md).

### <a name="machine-already-registered"></a>Senaryo: Makine için farklı bir hesap zaten kayıtlı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alıyorsunuz:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Nedeni

Güncelleştirme yönetimi için başka bir çalışma alanına eklenen makine zaten var.

#### <a name="resolution"></a>Çözüm

Eski yapıtları temizleyin, makine tarafından gerçekleştir [karma runbook grubunu silerek](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) ve yeniden deneyin.

### <a name="machine-unable-to-communicate"></a>Senaryo: Makine hizmetiyle iletişim kuramıyor

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alabilirsiniz:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Nedeni

Bir ara sunucu, ağ geçidi veya güvenlik duvarı ağ iletişimi engelliyor olabilir.

#### <a name="resolution"></a>Çözüm

Ağınızın gözden geçirin ve uygun bağlantı noktalarını ve adresi izin verildiğinden emin olun. Bkz: [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning), bağlantı noktaları ve güncelleştirme yönetimi ve karma Runbook çalışanları tarafından gerekli adresleri listesi.

### <a name="unable-to-create-selfsigned-cert"></a>Senaryo: Otomatik olarak imzalanan sertifika oluşturulamadı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alabilirsiniz:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Nedeni

Karma Runbook çalışanı otomatik olarak imzalanan bir sertifika oluşturmak mümkün değildi

#### <a name="resolution"></a>Çözüm

Sistem hesabı, klasöre okuma erişimi olduğunu doğrulayın **C:\ProgramData\Microsoft\Crypto\RSA** ve yeniden deneyin.

### <a name="failed-to-start"></a>Senaryo: Başarısız bir güncelleştirme dağıtımına başlamak için bir makine gösterir

#### <a name="issue"></a>Sorun

Bir makine durumunda **başlatılamadı** bir makine. Makine için belirli ayrıntılarını görüntülediğinizde, aşağıdaki hatayı görürsünüz:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdakilerden biri nedeniyle oluşabilir:

* Makine artık yok.
* Makine kapalı ve ulaşılamaz çevrilir.
* Makine bir ağ bağlantısı sorunu vardır ve karma çalışanı makineye ulaşılamıyor.
* Değiştirilen SourceComputerId Microsoft İzleme Aracısı için bir güncelleştirme oluştu
* Bir Otomasyon hesabında 2.000 eş zamanlı iş sınırını ulaşırsanız, güncelleştirme çalışması kısıtlandı. Her dağıtım bir iş ve bir güncelleştirme dağıtım sayısı her makinede bir iş olarak kabul edilir. Otomasyon hesabı sayınız eş zamanlı iş sınırını doğrultusunda şu anda çalışan tüm diğer Otomasyon işi veya güncelleştirme dağıtımı.

#### <a name="resolution"></a>Çözüm

Zaman geçerli kullanım [dinamik gruplar](../automation-update-management.md#using-dynamic-groups) güncelleştirme dağıtımlarınız için.

* Makine hala var ve erişilebilir olduğunu doğrulayın. Yoksa, dağıtımınıza düzenleyin ve makineyi kaldırın.
* Bölümüne [ağ planlaması](../automation-update-management.md#ports) bağlantı noktaları ve güncelleştirme yönetimi için gereklidir ve makinenizin bu gereksinimleri karşıladığını doğrulayın adresleri listesi.
* Log Analytics'te aşağıdaki sorguyu çalıştırın bulmak için ortamınızda ayarlanmış makineleri `SourceComputerId` değiştirildi. Aynı sahip bilgisayarlar için konum `Computer` değeri, ancak farklı `SourceComputerId` değeri. Etkilenen makineler bulduktan sonra bu makineler hedef ve kaldırın ve yeniden makineleri eklemek güncelleştirme dağıtımlarını düzenlemeniz gerekir böylece `SourceComputerId` doğru değeri yansıtır.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

### <a name="hresult"></a>Senaryo: Makine değerlendirilmeyen ve bir HResult özel durum gösterir gösterir.

#### <a name="issue"></a>Sorun

Gösterme makineler sahip **değerlendirilmeyen** altında **Uyumluluk**, ve bunun altındaki bir özel durum iletisi görürsünüz.

#### <a name="cause"></a>Nedeni

Windows Update veya WSUS makinede doğru şekilde yapılandırılmamış. Güncelleştirme yönetimi, Windows Update veya gerekli olan güncelleştirmeleri sağlamak üzere düzeltme eki ve düzeltme eklerini dağıtılan sonuçlarını durumunu WSUS kullanır. Güncelleştirme yönetimi, bu bilgiler olmadan gerekli veya yüklü düzeltme ekleri doğru rapor.

#### <a name="resolution"></a>Çözüm

Tüm özel durum iletisi görmek için kırmızı renkte gösterilen özel durumu çift tıklayın. Aşağıdaki tabloda olası çözümleri veya gerçekleştirilecek eylemleri gözden geçirin:

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | İlgili hata kodunu arama [Windows güncelleştirme hatası kodu listesi](https://support.microsoft.com/help/938205/windows-update-error-code-list) özel durumun nedeni hakkında ek ayrıntıları bulmak için.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Bu hatalar, ağ bağlantısı sorunları var. Makinenizde güncelleştirme yönetimi için uygun ağ bağlantısı olduğundan emin olun. Bölümüne [ağ planlaması](../automation-update-management.md#ports) bağlantı noktaları ve gerekli olan adresleri listesi.        |
|`0x8024001E`| Hizmet veya sistem kapatılmakta olduğundan güncelleştirme işlemi tamamlanamadı.|
|`0x8024002E`| Windows Update hizmeti devre dışı bırakıldı.|
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, kayıt defteri değerlerini emin `WUServer` ve `WUStatusServer` kayıt defteri anahtarı altında `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` doğru WSUS sunucusu vardır.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Güncelleştirme Hizmeti (wuauserv) çalıştığından ve devre dışı emin olun.        |
|Herhangi bir genel durum     | Bir arama olası çözümler için internet ve yerel BT desteği ile çalışır.         |

Gözden geçirme `windowsupdate.log` olası nedeni de belirlemeye yardımcı olabilir. Günlük okuma hakkında daha fazla bilgi için bkz. [Windowsupdate.log dosyasında okuma](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Ayrıca, indirebilir ve çalıştırabilirsiniz [Windows Update Sorun Giderici'yi](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) makinede Windows güncelleştirme herhangi bir sorun olup olmadığını denetlemek için.

> [!NOTE]
> [Windows Update Sorun Giderici'yi](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) Windows Server üzerinde de çalışır ancak Windows istemcileri için olduğunu belirtir.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Senaryo: Başlamak güncelleştirme çalışması başarısız

#### <a name="issue"></a>Sorun

Bir Linux makinesinde başlatmak için bir güncelleştirme çalıştırmaları başarısız.

#### <a name="cause"></a>Nedeni

Linux karma çalışanı sağlam değil.

#### <a name="resolution"></a>Çözüm

Aşağıdaki günlük dosyası bir kopyasını alın ve sorun giderme amacıyla Koru:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Senaryo: Güncelleştirme çalışması başlatır, ancak hatalarla karşılaştığında

#### <a name="issue"></a>Sorun

Bir güncelleştirme çalışması başlatmaz, bunun çalıştırma sırasında hatalarla karşılaştığında.

#### <a name="cause"></a>Nedeni

Olası nedenler aşağıdakilerden biri olabilir:

* Paket Yöneticisi sağlam değil
* Bulut tabanlı düzeltme eki uygulama ile belirli paketleri etkileyebilir
* Diğer nedenler

#### <a name="resolution"></a>Çözüm

Linux üzerinde sorunsuz başlatıldıktan sonra Çalıştır güncelleştirilmesi sırasında hatalar olması durumunda, etkilenen makinenin çalıştırmada çıktı iş kontrol edin. Araştırma ve üzerinde işlem gerçekleştir makinenizin Paket Yöneticisi'nden belirli hata iletileri bulabilirsiniz. Güncelleştirme yönetimi, Paket Yöneticisi başarılı güncelleştirme dağıtımları için iyi durumda olmasını gerektirir.

Bazı durumlarda, güncelleştirme bir güncelleştirme dağıtımı tamamlanmasını engelliyor yönetimi ile paket güncelleştirmelerini engelleyebilir. Gördüğünüz, gelecek güncelleştirmelerden çalıştırmalardan bu paketleri Dışla veya bunları el ile yüklemeniz gerekir kendiniz.

Bir düzeltme eki uygulama sorunu çözemezseniz, aşağıdaki günlük dosyası bir kopyasını oluşturun ve bunu korumak **önce** sorun giderme amacıyla sonraki güncelleştirme dağıtımına başlatır:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
