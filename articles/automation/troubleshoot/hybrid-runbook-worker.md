---
title: Sorun giderme - Azure Otomasyon karma Runbook çalışanları
description: Bu makalede Azure Otomasyon karma Runbook çalışanları sorun giderme bilgileri sağlar.
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c8dc240172451118fd75b042ba267740999882d
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321776"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Karma Runbook çalışanları sorunlarını giderme

Bu makalede, karma Runbook çalışanları sorunlarını giderme hakkında bilgi sağlar.

## <a name="general"></a>Genel

Bir aracıda çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için Otomasyon hesabı ile iletişim kurmak için karma Runbook çalışanı bağlıdır. Windows için bu, Microsoft Monitoring Agent aracısıdır. Linux için bu Linux için OMS Aracısı olur.

### <a name="runbook-execution-fails"></a>Senaryo: Runbook yürütme başarısız olur.

#### <a name="issue"></a>Sorun

Runbook yürütmesi başarısız oluyor ve aşağıdaki hatayı alırsınız:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook'unuzda, kısa süre içinde üç kez yürütmek denemeden sonra askıya alınır. Runbook başarıyla tamamlanmasını kesintiye uğratabilecek koşullar vardır ve ilgili hata iletisini nedenini gösteren ek bilgiler içermez.

#### <a name="cause"></a>Nedeni

Olası nedenleri şunlardır:

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma çalışanı bir proxy veya güvenlik duvarı olduğunu

* Runbook'lar yerel kaynakları ile kimlik doğrulaması yapamaz

* Karma Runbook çalışanı özelliğini çalıştırmak için belirtilen bilgisayarın en düşük donanım gereksinimlerini karşılıyor.

#### <a name="resolution"></a>Çözüm

Bilgisayar bağlantı noktası 443 üzerinde *.azure-automation.net giden erişimi olduğunu doğrulayın.

Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi takdirde, diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden olduğu çekişmeyi kaynak kullanımını, bağlı olarak bilgisayar üzerinde kullanılan olur ve bu runbook işi gecikmeler veya zaman aşımları neden.

Karma Runbook çalışanı özelliğini çalıştırmak için belirtilen bilgisayarın en düşük donanım gereksinimlerini karşıladığını doğrulayın. Aksi halde, karma Runbook çalışanı işlemlerinin performansını ve Windows arasındaki herhangi bir ilişki belirlemek için CPU ve bellek kullanımı izleyin. Bellek veya CPU baskısı ise, bu yükseltme ek işlemci ekleyin veya kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösterebilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimlerini destekleyen farklı bilgi işlem kaynağı seçin.

Denetleme **Microsoft SMA** açıklamasıyla karşılık gelen bir olay için olay günlüğünü *Win32 işlemden çıkıldı [4294967295] kodla*. Bu hatanın nedenini kimlik doğrulaması, runbook'larında yapılandırılmış veya karma çalışanı grubu için farklı çalıştır kimlik bilgilerinin henüz ' dir. Gözden geçirme [Runbook izinleri](../automation-hrw-run-runbooks.md#runbook-permissions) doğru şekilde yapılandırdığınız kimlik doğrulaması için runbook'larınızı onaylamak için.

## <a name="linux"></a>Linux

Linux karma Runbook çalışanı çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için OMS aracısı üzerinde Otomasyon hesabınız ile iletişim kurmak Linux bağlıdır. Çalışan kaydı başarısız olursa hatanın bazı olası nedenleri şunlardır:

### <a name="oms-agent-not-running"></a>Senaryo: Linux için OMS Aracısı çalışmıyor.

Linux için OMS Aracısı çalışmıyorsa bu Linux karma Runbook çalışanı Azure Otomasyonu ile iletişim kurmasını engeller. Aracı, aşağıdaki komutu girerek çalıştığını doğrulamak: `ps -ef | grep python`. Python işlemlerle aşağıdakine benzer bir çıktı görmeniz gerekir **nxautomation** kullanıcı hesabı. Güncelleştirme yönetimi veya Azure Otomasyon çözümleri etkin değilse, aşağıdaki işlemler hiçbiri çalışıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede, bir Linux karma Runbook çalışanı için başlatılan işlemleri gösterilmektedir. Bunlar tüm bulunur `/var/opt/microsoft/omsagent/state/automationworker/` dizin.

* **OMS.conf** -bu çalışan manager işlemi, bunu doğrudan DSC başlatılır.

* **Worker.conf** -bu işlem otomatik kayıtlı karma çalışan işlemi, çalışan Yöneticisi tarafından başlatılır. Bu işlem, güncelleştirme yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Güncelleştirme yönetimi çözümü makine üzerinde etkin değilse, bu işlem yok.

* **Diy/Worker.conf** -bu işlem kendin YAP karma çalışanı işlemidir. Kendin YAP karma çalışan işlemi, kullanıcı runbook'ları karma Runbook çalışanı üzerinde yürütmek için kullanılır. Yalnızca farklıdır, otomatik karma çalışan işlemi farklı bir yapılandırma kullandığı anahtar ayrıntılı olarak kayıtlı. Azure Otomasyonu çözümün etkin olmadığını ve kendin YAP Linux karma çalışanı kayıtlı değil, bu işlem mevcut değil.

Linux çalışmıyor için OMS Aracısı hizmeti başlatmak için aşağıdaki komutu çalıştırırsanız: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Senaryo: Belirtilen sınıf yok

Hata görürseniz: **belirtilen sınıf yok...** içinde `/var/opt/microsoft/omsconfig/omsconfig.log` sonra Linux için OMS Aracısı güncelleştirilmesi gerekiyor. OMS Aracısı'nı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows karma Runbook çalışanı çalışan kaydetme, runbook işlerini alabilir ve durumunu raporlamak için Otomasyon hesabı ile iletişim kurmak için Microsoft Monitoring Agent bağlıdır. Çalışan kaydı başarısız olursa hatanın bazı olası nedenleri şunlardır:

### <a name="mma-not-running"></a>Senaryo: Microsoft İzleme Aracısı çalışmıyor.

#### <a name="issue"></a>Sorun

`healthservice` Hizmeti karma Runbook çalışanı makine üzerinde çalışmıyor.

#### <a name="cause"></a>Nedeni

Microsoft İzleme Aracısı Windows hizmeti çalışmıyorsa bu karma Runbook çalışanı Azure Otomasyonu ile iletişim kurmasını engeller.

#### <a name="resolution"></a>Çözüm

PowerShell'de aşağıdaki komutu girerek aracının çalıştığını doğrulamak: `Get-Service healthservice`. Hizmet durdurulmuşsa hizmeti başlatmak için PowerShell'de aşağıdaki komutu girin: `Start-Service healthservice`.

### <a name="event-4502"></a> Operations Manager günlüğünde 4502 olayı

#### <a name="issue"></a>Sorun

İçinde **uygulama ve hizmet günlükleri\operations Manager** olay günlüğü olayı 4502 ve EventMessage içeren gördüğünüz **sorun**şu açıklama ile: *hizmet tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com değil Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından verilmiş. TLS/SSL iletişimini kesen bir proxy çalıştırıyorsanız görmek için ağ yöneticinize başvurun. KB3126513 makalesinde bağlantı sorunlarına yönelik ek bilgiler vardır.*

#### <a name="cause"></a>Nedeni

Bunun nedeni Microsoft Azure, Ara sunucu veya ağ güvenlik duvarı iletişimi engelliyor olabilir. Bilgisayar bağlantı noktası 443 üzerinde *.azure-automation.net giden erişimi olduğunu doğrulayın.

#### <a name="resolution"></a>Çözüm

Günlükler her karma çalışanında C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, yerel olarak depolanır. Herhangi bir uyarı veya hata olayları için yazılmış olup olmadığını kontrol edebilirsiniz **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operations** ve **uygulama ve hizmet günlükleri\operations Manager** olay günlüğü bir bağlantı veya ekleme Azure Otomasyonu ya da sorun rolünün normal işlemleri gerçekleştirilirken etkileyen başka bir sorun gösterir.

[Runbook çıkışı ve iletileri](../automation-runbook-output-and-messages.md) karma gönderilen Azure Otomasyonu runbook işleri gibi çalışanları bulutta çalıştırın. Bu gibi durumlarda, ayrıntılı ve ilerleme akışları ayrıca diğer runbook'lar için yaptığınız şekilde etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görülmez veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
