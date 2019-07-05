---
title: Sorun giderme - Azure Otomasyon karma Runbook çalışanları
description: Bu makalede Azure Otomasyon karma Runbook çalışanları sorun giderme bilgileri sağlar.
services: automation
ms.service: automation
ms.subservice: ''
author: bobbytreed
ms.author: robreed
ms.date: 02/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1ab9de1e11fa4f43894a6789fb2ba6fedbd1b77e
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477507"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Karma Runbook çalışanları sorunlarını giderme

Bu makalede, karma Runbook çalışanları sorunlarını giderme hakkında bilgi sağlar.

## <a name="general"></a>Genel

Bir aracıda çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için Otomasyon hesabı ile iletişim kurmak için karma Runbook çalışanı bağlıdır. Windows için bu, Microsoft Monitoring Agent aracısıdır. Linux için bu Linux için OMS Aracısı olur.

### <a name="runbook-execution-fails"></a>Senaryo: Runbook yürütmesi başarısız

#### <a name="issue"></a>Sorun

Runbook yürütmesi başarısız oluyor ve aşağıdaki hatayı alırsınız:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Kısa bir süre içinde üç kez yürütülecek denemeden sonra runbook askıya alındı. Runbook'un tamamlanmasını kesintiye uğratabilecek koşullar vardır. Bu durumda, ilgili hata iletisini neden belirten ek bilgileri içermeyebilir.

#### <a name="cause"></a>Nedeni

Olası nedenleri şunlardır:

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma çalışanı bir proxy veya güvenlik duvarı olduğunu

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma Runbook çalışanı özelliğini çalıştırmak için yapılandırılmış bilgisayar en düşük donanım gereksinimlerini karşılamıyor.

#### <a name="resolution"></a>Çözüm

Bilgisayar bağlantı noktası 443 üzerinde *.azure-automation.net giden erişimi olduğunu doğrulayın.

Bu özellik barındırmak için yapılandırılmadan önce karma Runbook çalışanı çalıştıran bilgisayarların en düşük donanım gereksinimlerini karşılamalıdır. Runbook'ları ve kullandıkları arka plan işlemleri sistemin üzerinde kullanılan ve runbook işi gecikmeler veya zaman aşımları neden neden olabilir.

Karma Runbook çalışanı özelliğini çalıştıran bilgisayarın en düşük donanım gereksinimlerini karşıladığını doğrulayın. Aksi halde İzleyici CPU ve bellek karma Runbook çalışanı işlemlerin performansını ve Windows arasındaki herhangi bir ilişki belirlemek için kullanın. Bellek veya CPU baskısı ise, bu kaynakları yükseltmek gösterebilir. İş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimlerini destekleyen bir farklı işlem kaynağı de seçebilirsiniz.

Denetleme **Microsoft SMA** açıklamasıyla karşılık gelen bir olay için olay günlüğünü *Win32 işlemden çıkıldı [4294967295] kodla*. Bu hatanın nedenini kimlik doğrulaması, runbook'larında yapılandırılmış veya karma çalışanı grubu için farklı çalıştır kimlik bilgilerinin henüz ' dir. Gözden geçirme [Runbook izinleri](../automation-hrw-run-runbooks.md#runbook-permissions) doğru şekilde yapılandırdığınız kimlik doğrulaması için runbook'larınızı onaylamak için.

### <a name="no-cert-found"></a>Senaryo: Karma Runbook çalışanında sertifika deposunda sertifika bulunamadı

#### <a name="issue"></a>Sorun

Bir karma Runbook çalışanı üzerinde çalışan bir runbook şu hata iletisiyle başarısız olur:

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Nedeni

Kullanmayı denediğinizde, bu hata oluşur. bir [farklı çalıştır hesabı](../manage-runas-account.md) karma Runbook çalışanı nerede farklı çalıştır hesabı sertifikası değil mevcut üzerinde çalışan bir runbook'ta. Karma Runbook çalışanları sertifika varlığı, farklı çalıştır hesabı tarafından düzgün çalışması için gerekli olan varsayılan olarak, yerel olarak yok.

#### <a name="resolution"></a>Çözüm

Bir Azure sanal makinesi, karma Runbook çalışanı ise kullanabileceğiniz [Azure kaynakları için yönetilen kimlikleri](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) yerine. Bu senaryo, farklı çalıştır hesabı yerine Azure VM'nin yönetilen kimlik kullanarak, kimlik doğrulaması basitleştirme Azure kaynaklarında kimlik doğrulamasını sağlar. Karma Runbook çalışanı şirket içi makine, makine üzerinde farklı çalıştır hesabı sertifikası yüklemeniz gerekir. Sertifikayı yüklemek öğrenmek için adımları çalıştırmak için bkz. [dışarı aktarma RunAsCertificateToHybridWorker](../automation-hrw-run-runbooks.md#runas-script) runbook.

## <a name="linux"></a>Linux

Linux karma Runbook çalışanı çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için OMS aracısı üzerinde Otomasyon hesabınız ile iletişim kurmak Linux bağlıdır. Çalışan kaydı başarısız olursa hatanın bazı olası nedenleri şunlardır:

### <a name="oms-agent-not-running"></a>Senaryo: Linux için OMS Aracısı çalışmıyor

#### <a name="issue"></a>Sorun

Linux için OMS Aracısı çalışmıyor.

#### <a name="cause"></a>Nedeni

Linux için OMS Aracısı çalışmıyorsa Linux karma Runbook çalışanı Azure Otomasyonu ile iletişim kurmasını önler. Aracı çeşitli nedenlerden dolayı çalışmıyor olabilir.

#### <a name="resolution"></a>Çözüm

 Aracı, aşağıdaki komutu girerek çalıştığını doğrulamak: `ps -ef | grep python`. Python işlemlerle aşağıdakine benzer bir çıktı görmeniz gerekir **nxautomation** kullanıcı hesabı. Güncelleştirme yönetimi veya Azure Otomasyon çözümleri olmayan etkinleştirilirse, aşağıdaki işlemler hiçbiri çalışıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede, bir Linux karma Runbook çalışanı için başlatılan işlemleri gösterilmektedir. Tüm bulunan `/var/opt/microsoft/omsagent/state/automationworker/` dizin.


* **OMS.conf** -çalışan manager işlemi değerdir. Doğrudan DSC başlatıldı.

* **Worker.conf** -bu işlem otomatik kayıtlı karma çalışan işlemi, çalışan Yöneticisi tarafından başlatılır. Bu işlem, güncelleştirme yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Bu işlem, güncelleştirme yönetimi çözümü makine üzerinde etkin değilse mevcut değil.

* **Diy/Worker.conf** -bu işlem kendin YAP karma çalışanı işlemidir. Kendin YAP karma çalışan işlemi, kullanıcı runbook'ları karma Runbook çalışanı üzerinde yürütmek için kullanılır. Yalnızca farklıdır, otomatik karma çalışan işlemi farklı bir yapılandırma kullandığı anahtar ayrıntılı olarak kayıtlı. Azure Otomasyon çözümünü devre dışıysa ve kendin YAP Linux karma çalışanı kayıtlı değilse, bu işlem mevcut değil.

Linux çalışmadığı için OMS Aracısı hizmeti başlatmak için aşağıdaki komutu çalıştırırsanız: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Senaryo: Belirtilen sınıf yok

Şu hatayı görürseniz: **Belirtilen sınıf yok...** içinde `/var/opt/microsoft/omsconfig/omsconfig.log` sonra Linux için OMS Aracısı güncelleştirilmesi gerekiyor. OMS Aracısı'nı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows karma Runbook çalışanı çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için Otomasyon hesabı ile iletişim kurmak için Microsoft Monitoring Agent bağlıdır. Çalışan kaydı başarısız olursa hatanın bazı olası nedenleri şunlardır:

### <a name="mma-not-running"></a>Senaryo: Microsoft Monitoring Agent çalışmıyor

#### <a name="issue"></a>Sorun

`healthservice` Karma Runbook çalışanı makinede hizmet çalışmadığından.

#### <a name="cause"></a>Nedeni

Microsoft İzleme Aracısı Windows hizmeti çalışmıyorsa bu durum, karma Runbook çalışanı Azure Otomasyonu ile iletişim kurmasını engeller.

#### <a name="resolution"></a>Çözüm

PowerShell'de aşağıdaki komutu girerek aracının çalıştığını doğrulamak: `Get-Service healthservice`. Hizmet durdurulmuşsa hizmeti başlatmak için PowerShell'de aşağıdaki komutu girin: `Start-Service healthservice`.

### <a name="event-4502"></a> Operations Manager günlüğünde 4502 olayı

#### <a name="issue"></a>Sorun

İçinde **uygulama ve hizmet günlükleri\operations Manager** olay günlüğü gördüğünüz olay 4502 ve içeren EventMessage **sorun**şu açıklama ile: *Hizmet tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com değil Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından verilmiş. TLS/SSL iletişimini kesen bir proxy çalıştırıyorsanız görmek için ağ yöneticinize başvurun. KB3126513 makalesinde bağlantı sorunlarına yönelik ek bilgiler vardır.*

#### <a name="cause"></a>Nedeni

Bu sorunun nedeni Microsoft Azure, Ara sunucu veya ağ güvenlik duvarı iletişimi engelliyor olabilir. Bilgisayar bağlantı noktası 443 üzerinde *.azure-automation.net giden erişimi olduğunu doğrulayın.

#### <a name="resolution"></a>Çözüm

Günlükler her karma çalışanında C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, yerel olarak depolanır. Hiçbir uyarı veya hata olayları olup olmadığını kontrol edebilirsiniz **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operations** ve **uygulama ve hizmet günlükleri\operations Manager** olduğu olay günlüğü bir bağlantı veya ekleme, Azure automation'da rol etkileyen diğer sorun veya normal işlemler sırasında altında sorunu gösterir.

[Runbook çıkışı ve iletileri](../automation-runbook-output-and-messages.md) yalnızca bulutta çalışan runbook işleri gibi için Azure Otomasyon karma çalışanı gönderilir. Bu gibi durumlarda, ayrıntılı ve ilerleme akışları ayrıca diğer runbook'lar için yaptığınız şekilde etkinleştirebilirsiniz.

### <a name="corrupt-cache"></a> Karma Runbook çalışanı raporlama

#### <a name="issue"></a>Sorun

Karma Runbook çalışanı makineniz çalışıyor, ancak çalışma alanında makine için sinyal verileri göremiyorum.

Aşağıdaki örnek sorgu makineleri bir çalışma alanı ve kendi son sinyal gösterir:

```loganalytics
// Last heartbeat of each computer
Heartbeat 
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Nedeni

Karma Runbook çalışanı üzerinde bozuk bir önbelleği tarafından bu soruna neden olabilir.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için karma Runbook çalışanı'için oturum açın ve aşağıdaki betiği çalıştırın. Bu betik Microsoft Monitoring Agent durdurur, önbelleğinde kaldırır ve hizmetini yeniden başlatır. Bu eylem, Azure Otomasyonu yapılandırmasını yeniden indirmek için karma Runbook çalışanı zorlar.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Senaryo: Karma Runbook çalışanı eklenemiyor

#### <a name="issue"></a>Sorun

Bir karma Runbook çalışanı kullanarak eklenmeye çalışılırken şu iletiyi alıyorsunuz `Add-HybridRunbookWorker` cmdlet'i.

```error
Machine is already registered
```

#### <a name="cause"></a>Nedeni

Bu makinede zaten farklı bir Otomasyon hesabı ile kayıtlı değilse veya karma Runbook çalışanı bir makineden kaldırdıktan sonra yeniden eklemeyi denerseniz neden olabilir.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için aşağıdaki kayıt defteri anahtarını kaldırın ve yeniden `HealthService` deneyin `Add-HybridRunbookWorker` cmdlet'ini yeniden:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

