---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makalede, Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ları çalıştırmak için bir Azure Otomasyon karma Runbook çalışanı yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8507cf99ea22b24aa3026565cb7c4139e4c3742d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268125"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı dağıtma

Runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Linux karma Runbook çalışanı runbook'ları yükseltme gerekiyor komutlarını çalıştırmak için yükseltilmiş özel bir kullanıcı olarak yürütür. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.

Bu makalede, bir Linux makinesinde karma Runbook çalışanı yüklemeyi açıklar.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Karma Runbook çalışanı özelliği aşağıdaki dağıtımları destekler:

* Amazon Linux için 2015.09 2012.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS ve 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı yükleme

Yüklemek ve karma Runbook çalışanı Linux bilgisayarınızda yapılandırmak için el ile yükleyin ve yapılandırın için kolay bir işlem uygulayın. Etkinleştirme gerektirir **Otomasyon karma çalışanı** Azure günlük analizi çalışma alanınız ve bilgisayarı çalışan kaydetmek ve bir gruba eklemek için komut kümesini çalıştıran çözümde.

Bir Linux karma Runbook çalışanı için minimum gereksinimleri şunlardır:

* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="package-requirements"></a>Paket gereksinimleri

| **Gerekli paket** | **Açıklama** | **En düşük sürüm**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C Kitaplığı| 2.5-12 |
|Openssl| OpenSSL kitaplıkları | 0.9.8E veya 1.0|
|Curl | cURL web istemcisi | 7.15.5|
|Python ctypes | |
|PAM | Eklenebilir kimlik doğrulaması modülleri|
| **İsteğe bağlı paket** | **Açıklama** | **En düşük sürüm**|
| PowerShell çekirdek | PowerShell runbook'ları çalıştırmak için PowerShell gereksinimleri yüklenmesi için bkz. [yükleme PowerShell çekirdek Linux'ta](/powershell/scripting/setup/installing-powershell-core-on-linux) nasıl yükleneceği öğrenin.  | 6.0.0 |

### <a name="installation"></a>Yükleme

Devam etmeden önce Automation hesabınızı bağlandığı günlük analizi çalışma alanı unutmayın. Ayrıca, Automation hesabınız için birincil anahtar unutmayın. Hem Azure portalından Automation'ınızı seçerek hesabı, belirlemeyi bulabileceğiniz **çalışma** çalışma alanı kimliği için seçerek **anahtarları** birincil anahtar. Bağlantı noktaları ve karma Runbook çalışanı için gereksinim duyduğunuz adresleri hakkında daha fazla bilgi için bkz: [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

1. Etkinleştirme **Otomasyon karma çalışanı** aşağıdaki yöntemlerden birini kullanarak Azure çözümde:

   * Ekle **Otomasyon karma çalışanı** adresindeki yordamı kullanarak, aboneliğinizin çözüme [çalışma alanınıza ekleyin günlük analizi yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md).
   * Aşağıdaki cmdlet'i çalıştırın:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Aşağıdaki komutu çalıştırarak Linux için OMS Aracısı'nı yükleyin. Değiştir \<Workspaceıd\> ve \<WorkspaceKey\> alanınızdan uygun değerlerle.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Parametre değerlerini değiştirerek aşağıdaki komutu çalıştırın *-w*, *-k*, *-g*, ve *-e*. İçin *-g* parametresi değeri yeni Linux karma Runbook çalışanı katılması gereken karma Runbook çalışanı grup adıyla değiştirin. Otomasyon hesabınızda adı yoksa, yeni bir karma Runbook çalışanı grubu bu ada sahip yapılır.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Komut tamamlandığında, **karma çalışan grupları** Azure portalında sayfası, yeni Grup ve üye sayısını gösterir. Varolan bir grubu varsa üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** döşeme. Üzerinde **karma çalışanları** sayfasında, listelenen grubun her üyesi bakın.

## <a name="turning-off-signature-validation"></a>İmza doğrulama kapatma

Varsayılan olarak, Linux karma Runbook çalışanları imza doğrulaması gerektirir. İmzasız bir runbook worker karşı çalıştırırsanız, "imza doğrulaması başarısız oldu." diyen bir hata bakın İmza doğrulama devre dışı bırakmak için aşağıdaki komutu çalıştırın. Günlük analizi çalışma alanı kimliğinizin ikinci parametre değiştirme

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux karma Runbook çalışanları Azure Automation runbook türleri, tamamını desteklemez.

Aşağıdaki runbook türleri Linux karma çalışanı üzerinde çalışır:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell runbook'ları PowerShell çekirdeği Linux makinesinde yüklü olmasını gerektirir. Bkz [yükleme PowerShell çekirdek Linux'ta](/powershell/scripting/setup/installing-powershell-core-on-linux) nasıl yükleneceği öğrenin.

Aşağıdaki runbook türleri Linux karma çalışanı üzerinde çalışmıyor:

* PowerShell iş akışı
* Grafik
* Grafik PowerShell iş akışı

## <a name="troubleshooting"></a>Sorun giderme

Linux karma Runbook çalışanı OMS çalışan kaydetmek, runbook işlerini almak ve durum raporu için aracı üzerinde Automation hesabınız ile iletişim kurmak Linux için bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır.

### <a name="the-oms-agent-for-linux-isnt-running"></a>Linux için OMS Aracısı çalışmıyor

Linux için OMS Aracısı çalışıyor durumda değilse, Linux karma Runbook çalışanı Azure Automation ile iletişim kuramıyor. Aracı komutunu girerek çalıştığını doğrulayın `ps -ef | grep python`. 

Aşağıdakine benzer bir çıktı görmeniz gerekir (Python işler ile **nxautomation** kullanıcı hesabı). Güncelleştirme yönetimi ya da Azure Otomasyonu çözümü etkin değilse, aşağıdaki işlemler hiçbiri çalıştırırsınız.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki işlemler için Linux karma Runbook çalışanı başlatıldı. Tüm içinde bulundukları `/var/opt/microsoft/omsagent/state/automationworker/` dizin.

* **OMS.conf**: Çalışan Yöneticisi işlem budur. Doğrudan istenen durum yapılandırması (DSC'den) başlatıldı.

* **Worker.conf**: Bu otomatik kaydedilmiş karma çalışanı işlemidir. Çalışan yöneticisi tarafından başlatılır. Bu işlem, güncelleştirme yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Bu işlem, yalnızca güncelleştirme yönetimi çözümü makinede etkinse mevcuttur.

* **Diy/Worker.conf**: Dıy karma çalışan işlemi budur. Dıy karma çalışan işlemi, kullanıcı runbook'ları karma Runbook çalışanını yürütmek için kullanılır. Yalnızca farklı bir yapılandırma kullandığından otomatik kayıtlı karma çalışanı işleminden farklıdır. Bu işlem yalnızca Azure Otomasyon çözümünü etkinleştirilmişse ve Dıy Linux karma çalışanı kayıtlı mevcuttur.

Linux çalışmadığı için OMS Aracısı hizmetini başlatmak için aşağıdaki komutu çalıştırırsanız: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-doesnt-exist"></a>Belirtilen sınıf yok

Hata görürseniz "Belirtilen sınıf yok" `/var/opt/microsoft/omsconfig/omsconfig.log`, Linux için OMS aracısının güncelleştirilmesi gerekiyor. OMS Aracısı'nı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Güncelleştirme yönetimi ile ilgili sorunları giderme konusunda ek adımlar için bkz: [güncelleştirme yönetimi: sorun giderme](automation-update-management.md#troubleshooting).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md).
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları kaldırmak](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
