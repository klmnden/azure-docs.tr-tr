---
title: Sorun giderme - Azure Otomasyon karma Runbook çalışanları
description: Bu makalede Azure Otomasyon karma Runbook çalışanları sorun giderme bilgileri sağlar
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1cae7253a4bfcb4f83baf003a4d9d3c367d8f014
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064046"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Karma Runbook çalışanları sorun giderme

Bu makalede, karma Runbook çalışanları sorunlarını giderme hakkında bilgi sağlar.

## <a name="general"></a>Genel

Karma Runbook çalışanı çalışan kaydetmek, runbook işlerini almak ve durum raporu için Otomasyon hesabınızın iletişim kurmak için bir aracı bağlıdır. Windows için bu aracı Microsoft Monitoring Agent ' dir. Linux için Linux için OMS aracısının olduğu.

###<a name="runbook-execution-fails"></a>Senaryo: Runbook yürütme başarısız

#### <a name="issue"></a>Sorun

Runbook yürütme başarısız olur ve şu hatayı alıyorsunuz:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook'unuzda üç kez yürütmek kısa bir süre içinde çalışırken askıya alınır. Runbook başarıyla tamamlanmasını kesme koşullar vardır ve ilgili hata iletisi nedenini gösteren ek bilgiler içermez.

#### <a name="cause"></a>Nedeni

Olası nedenleri şunlardır:

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma çalışanı bir proxy veya güvenlik duvarı arkasında olduğunu

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma Runbook çalışanı özelliğini çalıştırmak için belirtilen bilgisayarın en düşük donanım gereksinimlerini karşılıyor.

#### <a name="resolution"></a>Çözüm

Bilgisayarın, bağlantı noktası 443 üzerinde *.azure automation.net giden erişimi olduğunu doğrulayın.

Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi halde, kaynak kullanımı diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden Çekişme bağlı olarak bilgisayar üzerinde kullanılan olur ve bu runbook işi gecikmeler veya zaman aşımları neden.

Karma Runbook çalışanı özelliği yürütmek için atanan bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın. Aşması durumunda, karma Runbook çalışanı işlemlerinin performansını ile Windows arasında herhangi bir bağıntı belirlemek için CPU ve bellek kullanımını izleyin. Bellek veya CPU baskısı ise, bu yükseltme veya ek işlemciler ekleme ya da kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösteriyor olabilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimleri destekleyebilen farklı işlem kaynak seçin.

Denetleme **Microsoft SMA** açıklama ile ilgili bir olayın olay günlüğünü *işlemi Win32 çıkış koduyla [4294967295]*. Bu hatanın nedenini larınızda kimlik doğrulaması yapılandırılmış veya belirtilen karma çalışanı grubu için farklı çalıştır kimlik bilgileri henüz ' dir. Gözden geçirme [Runbook izinleri](../automation-hrw-run-runbooks.md#runbook-permissions) doğru şekilde yapılandırdığınız kimlik doğrulaması runbook'larınızın onaylamak için.

## <a name="linux"></a>Linux

Linux karma Runbook çalışanı OMS çalışan kaydetmek, runbook işlerini almak ve durum raporu için aracı üzerinde Automation hesabınız ile iletişim kurmak Linux için bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:

###<a name="oms-agent-not-running"></a>Senaryo: Linux için OMS Aracısı çalışmıyor

Linux için OMS aracısının çalışmıyorsa bu Linux karma Runbook çalışanı Azure Automation ile iletişim kurmasını engeller. Aracı, aşağıdaki komutu girerek çalıştığını doğrulayın: `ps -ef | grep python`. Python işlemlerle aşağıdakine benzer bir çıktı görmeniz gerekir **nxautomation** kullanıcı hesabı. Güncelleştirme yönetimi veya Azure Otomasyonu çözümleri etkinleştirilmezse, aşağıdaki işlemler hiçbiri çalışıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede, bir Linux karma Runbook çalışanı için başlatılan işlemleri gösterir. Bunlar tüm bulunur `/var/opt/microsoft/omsagent/state/automationworker/` dizin.

* **OMS.conf** -bu çalışan Yöneticisi işlemi, bu doğrudan DSC'den başlatılır.

* **Worker.conf** -bu işlemi otomatik kayıtlı karma çalışan işlemi, çalışan Yöneticisi tarafından başlatılır. Bu işlem, güncelleştirme yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Güncelleştirme yönetimi çözümü makinede etkin değilse, bu işlem yok.

* **Diy/Worker.conf** -bu işlem Dıy karma çalışanı işlemidir. Dıy karma çalışan işlemi, kullanıcı runbook'ları karma Runbook çalışanını yürütmek için kullanılır. Bunu yalnızca farklıdır otomatik karma çalışan işlemi kullanımdır anahtar ayrıntılı olarak farklı bir yapılandırma kayıtlı. Bu işlem, Azure Otomasyon çözümünü etkin değil ve Dıy Linux karma çalışanı kaydedilmemiş mevcut değil.

Linux çalışmıyor için OMS Aracısı hizmetini başlatmak için aşağıdaki komutu çalıştırırsanız: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

###<a name="class-does-not-exist"></a>Senaryo: Belirtilen sınıfı yok

Hata görürseniz: **belirtilen sınıf yok...** içinde `/var/opt/microsoft/omsconfig/omsconfig.log` Linux için OMS aracısının güncelleştirilmesi gerekir. OMS Aracısı'nı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows karma Runbook çalışanı çalışan kaydetmek, runbook işlerini almak ve durum raporu için Otomasyon hesabınızın iletişim kurmak için Microsoft Monitoring Agent bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:

###<a name="mma-not-running"></a>Senaryo: Microsoft Monitoring Agent çalışmıyor

#### <a name="issue"></a>Sorun

`healthservice` Hizmeti karma Runbook çalışanı makinede çalışmıyor.

#### <a name="cause"></a>Nedeni

Microsoft İzleme Aracısı Windows hizmeti çalışmıyorsa bu karma Runbook çalışanı Azure Automation ile iletişim kurmasını engeller.

#### <a name="resolution"></a>Çözüm

Aracı, PowerShell içinde aşağıdaki komutu girerek çalıştığını doğrulayın: `Get-Service healthservice`. Hizmet durdurulursa, hizmeti başlatmak için PowerShell içinde aşağıdaki komutu girin: `Start-Service healthservice`.

###<a name="event-4502"></a> Operations Manager günlüğüne 4502 olayı

#### <a name="issue"></a>Sorun

İçinde **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü olay 4502 ve EventMessage içeren gördüğünüz **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**aşağıdaki açıklamasıyla: *hizmeti tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından değil yayınlandı. TLS/SSL iletişimi karşılar bir proxy çalıştırıyorsanız görmek için ağ yöneticinize başvurun. KB3126513 makalesine ek sorun giderme bilgileri için bağlantı sorunları var.*

#### <a name="cause"></a>Nedeni

Bu Microsoft Azure iletişimini engelleyen proxy veya ağ güvenlik duvarını neden olabilir. Bilgisayarın, 443 numaralı bağlantı noktalarına *.azure automation.net giden erişimi olduğunu doğrulayın.

#### <a name="resolution"></a>Çözüm

Günlükleri yerel olarak her karma çalışanı C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes konumunda depolanır. Herhangi bir uyarı veya hata olayları için yazılmış olup olmadığını denetleyebilir **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operations** ve **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü bir bağlantı veya ekleme rolünün bir Azure Otomasyonu veya sorun normal işlemleri gerçekleştirirken etkileyen diğer sorunu gösterir.

[Runbook çıkışı ve iletileri](../automation-runbook-output-and-messages.md) Azure Otomasyon karma gönderilen bulutta çalışan runbook işleri gibi çalıştırın. Bu gibi durumlarda, ayrıntılı ve ilerleme akışları ayrıca diğer runbook'lar için olduğu gibi etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmemeniz veya sorunu çözmek oluşturulamıyor, daha fazla destek için aşağıdaki kanallar birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma gereksinim duyarsanız, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
