---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makale bir Azure Otomasyon karma Runbook Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: aca68b6e8d0e6b80a1504b16b9b3462f20fdc6c4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı dağıtma

Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Linux karma Runbook çalışanı runbook'ları yükseltme gerekiyor komutlarını çalıştırmak için yükseltilmiş özel bir kullanıcı olarak yürütür. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim. Bu karma Runbook çalışanı Linux makineye yüklemek nasıl decribes makalesi.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Desteklenen Linux dağıtımları listesi aşağıdadır:

* Amazon Linux 2012.09 2015.09 (x86/x64)-->
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5,6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="installing-linux-hybrid-runbook-worker"></a>Linux karma Runbook çalışanı yükleme

Yüklemek ve karma Runbook çalışanı Linux bilgisayarınızda yapılandırmak için el ile rolünü yüklemek ve yapılandırmak için bir düz İleri süreci izleyin. Etkinleştirme gerektirir **Otomasyon karma çalışanı** günlük analizi çalışma alanınız ve bilgisayarı çalışan kaydetmek ve yeni veya varolan bir gruba eklemek için komut kümesini çalıştıran çözümde.

Bir Linux karma Runbook çalışanı için minimum gereksinimleri şunlardır:

* En az iki çekirdek
* En az 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="package-requirements"></a>Paket gereksinimleri

| **Gerekli paket** | **Açıklama** | **En düşük sürüm**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C Kitaplığı| 2.5-12 |
|Openssl| OpenSSL kitaplıkları | 0.9.8E veya 1.0|
|Curl | cURL web istemcisi | 7.15.5|
|Python ctypes | |
|PAM | Eklenebilir kimlik doğrulaması modülleri|

Devam etmeden önce Automation hesabınız bağlandığı günlük analizi çalışma alanı ve ayrıca, Automation hesabınız için birincil anahtarı Not gerekir. Hem portalından Automation hesabınız seçip seçerek bulabilirsiniz **çalışma** çalışma alanı kimliği ve seçerek **anahtarları** birincil anahtar. Bağlantı noktaları ve karma Runbook çalışanı için gerekli olan adresleri hakkında daha fazla bilgi için bkz: [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

1. Azure "Otomasyon karma çalışanı" çözümde etkinleştirin. Bu durum ya da yapılabilir:

   1. Ekle **Otomasyon karma çalışanı** adresindeki yordamı kullanarak, aboneliğinizin çözüme [çalışma alanınıza ekleyin günlük analizi yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md).
   1. Aşağıdaki cmdlet'i çalıştırın:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Aşağıdaki komutu çalıştırarak Linux için OMS aracısı yükleyin değiştirme \<Workspaceıd\> ve \<WorkspaceKey\> alanınızdan uygun değerlerle.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Parametreleri için değerleri değiştirerek aşağıdaki komutu çalıştırın *-w*, *-k*, *-g*, ve *-e*. İçin *-g* parametresi değeri yeni Linux karma Runbook çalışanı katılması gereken karma Runbook çalışanı grup adıyla değiştirin. Otomasyon hesabınızda adı zaten mevcut değilse yeni bir karma Runbook çalışanı grubu bu ada sahip yapılır.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Komut tamamlandıktan sonra yeni Grup ve üye sayısı Azure portalında karma çalışan grupları sayfası gösterilir veya varolan bir grubu, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** döşeme. Üzerinde **karma çalışanları** sayfasında, listelenen grubun her üyesi bakın.

## <a name="turning-off-signature-validation"></a>İmza doğrulama kapatma

Varsayılan olarak, Linux karma Runbook çalışanları imza doğrulaması gerektirir. İmzasız bir runbook karşı çalışan çalıştırırsanız, "imza doğrulaması başarısız oldu" içeren bir hata görürsünüz. İmza doğrulama devre dışı bırakmak için ikinci parametresi, günlük analizi çalışma alanı Kimliğiniz ile değiştirerek aşağıdaki komutu çalıştırın:

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux karma Runbook çalışanları Azure Otomasyonu içinde bulunan runbook türleri, tamamını desteklemez.

Aşağıdaki runbook türleri Linux karma çalışanı üzerinde çalışır:

* Python 2
* PowerShell

Aşağıdaki runbook türleri Linux karma çalışanı üzerinde çalışmıyor:

* PowerShell iş akışı
* Grafik
* Grafik PowerShell iş akışı

## <a name="troubleshooting"></a>Sorun giderme

Linux karma Runbook çalışanı OMS çalışan kaydetmek, runbook işlerini almak ve durum raporu için aracı üzerinde Automation hesabınız ile iletişim kurmak Linux için bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:

### <a name="the-oms-agent-for-linux-is-not-running"></a>Linux için OMS Aracısı çalışmıyor

Linux için OMS aracısının çalışmıyorsa bu Linux karma Runbook çalışanı Azure Automation ile iletişim kurmasını engeller. Aracı, aşağıdaki komutu girerek çalıştığını doğrulayın: `ps -ef | grep python`. Python işlemlerle aşağıdakine benzer bir çıktı görmeniz gerekir **nxautomation** kullanıcı hesabı. Güncelleştirme yönetimi veya Azure Otomasyonu çözümleri etkinleştirilmezse, aşağıdaki işlemler hiçbiri çalıştırırsınız.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede, bir Linux karma Runbook çalışanı için başlatılan işlemleri gösterir. Bunlar tüm bulunur `/var/opt/microsoft/omsagent/state/automationworker/` dizin.

* **OMS.conf** -bu çalışan Yöneticisi işlemi, bu doğrudan DSC'den başlatılır.

* **Worker.conf** -bu işlemi otomatik kayıtlı karma çalışan işlemi, çalışan Yöneticisi tarafından başlatılır. Bu işlem, güncelleştirme yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Bu işlem olduğunu değil güncelleştirme yönetimi çözümü makinede etkin değilse mevcut olmalı.

* **Diy/Worker.conf** -bu işlem Dıy karma çalışanı işlemidir. Dıy karma çalışan işlemi, kullanıcı runbook'ları karma Runbook çalışanını yürütmek için kullanılır. Bunu yalnızca farklıdır otomatik karma çalışan işlemi kullanımdır anahtar ayrıntılı olarak farklı bir yapılandırma kayıtlı. Bu işlem mevcut çözüm etkinleştirilmemiş Azure Automation ve Dıy Linux karma çalışanı değilse, kayıtlı değil.

Linux çalışmıyor için OMS Aracısı hizmetini başlatmak için aşağıdaki komutu çalıştırırsanız: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-does-not-exist"></a>Belirtilen sınıf yok

Hata görürseniz **belirtilen sınıf yok...** içinde `/var/opt/microsoft/omsconfig/omsconfig.log` Linux için OMS aracısının güncelleştirilmesi gerekir. OMS Aracısı'nı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Güncelleştirme yönetimi ile ilgili sorunları giderme konusunda ek adımlar için bkz: [güncelleştirme yönetimi - sorun giderme](automation-update-management.md#troubleshooting)

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları Kaldır](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)
