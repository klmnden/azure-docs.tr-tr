---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makalede, Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ları çalıştırmak için bir Azure Otomasyonu karma Runbook çalışanı yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/28/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cc07aa9c1b2c540c33949a8c591bd98f91b04666
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60738868"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı dağıtma

Runbook'ları doğrudan rolünü barındıran bilgisayarda ve ortamınızda bu yerel kaynakları yönetmek için kaynakların karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Linux karma Runbook çalışanı runbook'ları yükseltme gereken komutları çalıştırmak için yükseltilmiş özel bir kullanıcı olarak yürütür. Runbook'ları tutulur ve yönetilen Azure Otomasyonu'nda ve sonra bir veya daha fazla atanmış bilgisayarlara teslim.

Bu makalede, karma Runbook çalışanı bir Linux makineye yüklemek açıklar.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Karma Runbook çalışanı özelliğini aşağıdaki dağıtımlarını destekler:

* Amazon Linux için 2015.09 2012.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS ve 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı'nı yükleme

Yükleme ve Linux bilgisayarınızda bir karma Runbook Worker'ı yapılandırmak için el ile rolünü yüklemek ve yapılandırmak için basit bir işlem uygulayın. Etkinleştirme gerektirir **Otomasyon karma çalışanı** Azure Log Analytics çalışma alanınızı ve ardından bilgisayarı bir çalışanı olarak kaydedin ve gruba eklemek için komut kümesini çalıştırarak çözümde.

Bir Linux karma Runbook çalışanı için en düşük gereksinimler şunlardır:

* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="package-requirements"></a>Paket gereksinimleri

| **Gerekli paket** | **Açıklama** | **En düşük sürüm**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C Kitaplığı| 2.5-12 |
|openssl| OpenSSL kitaplıkları | 1.0 (TLS 1.1 ve TLS 1.2 desteklenir|
|Curl | cURL web istemcisi | 7.15.5|
|Python ctypes | |
|PAM | Eklenebilir kimlik doğrulaması modülleri|
| **İsteğe bağlı paketi** | **Açıklama** | **En düşük sürüm**|
| PowerShell Core | PowerShell runbook'ları çalıştırmak için PowerShell gereksinimleri yüklenmesi için bkz. [Linux'ta PowerShell Core yükleme](/powershell/scripting/setup/installing-powershell-core-on-linux) yükleneceği hakkında bilgi edinmek için.  | 6.0.0 |

### <a name="installation"></a>Yükleme

Devam etmeden önce bunları Otomasyon hesabınıza bağlı Log Analytics çalışma alanı unutmayın. Ayrıca, Automation hesabınız için birincil anahtarı unutmayın. Hem Azure portalından Otomasyon seçerek hesabı, seçerek bulabilirsiniz **çalışma** çalışma alanı kimliği ve seçerek **anahtarları** birincil anahtar. Bağlantı noktaları ve karma Runbook çalışanı için gereksinim duyduğunuz adresleri hakkında daha fazla bilgi için bkz: [ağınıza yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

1. Etkinleştirme **Otomasyon karma çalışanı** Azure çözümde aşağıdaki yöntemlerden birini kullanarak:

   * Ekle **Otomasyon karma çalışanı** çözüm yordamı kullanarak aboneliğinize [Azure İzleyici'yi eklemek, çalışma alanınıza çözümleri günlükleri](../log-analytics/log-analytics-add-solutions.md).
   * Aşağıdaki cmdlet'i çalıştırın:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Aşağıdaki komutu çalıştırarak Linux için Log Analytics aracısını yükleyin. Değiştirin \<Workspaceıd\> ve \<WorkspaceKey\> çalışma alanınızdan uygun değerlerle.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)] 

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Parametreler için değerleri değiştirerek aşağıdaki komutu çalıştırın *-w*, *-k*, *-g*, ve *-e*. İçin *-g* parametresi değeri yeni Linux karma Runbook çalışanı katılması gereken karma Runbook çalışanı grubunun adıyla değiştirin. Otomasyon hesabınızda adı yoksa, bu ada sahip yeni bir karma Runbook çalışanı grubuna yapılır.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Komut tamamlandıktan sonra **karma çalışan grupları** sayfası Azure portalında yeni bir grup ve üye sayısı gösterir. Bu, varolan bir grubu ise, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** Döşe. Üzerinde **karma çalışanları** sayfasında listelenen bir grubun her üyesi görürsünüz.

> [!NOTE]
> Bir Azure VM için Linux için Azure İzleyici sanal makine uzantısı kullanıyorsanız ayarı öneririz `autoUpgradeMinorVersion` otomatik olarak false sürümlerini yükseltme sorunlarını karma Runbook çalışanı neden olabilir. Uzantıyı el ile yükseltme öğrenmek için bkz. [Azure CLI dağıtım ](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turning-off-signature-validation"></a>İmza doğrulaması kapatma

Varsayılan olarak, Linux karma Runbook çalışanları imza doğrulaması gerektirir. Bir çalışan karşı imzasız bir runbook Çalıştırma "imza doğrulaması başarısız oldu." diyen bir hata görürsünüz İmza doğrulaması devre dışı bırakmak için aşağıdaki komutu çalıştırın. İkinci parametre, log analytics çalışma alanı kimliği ile değiştirin.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux karma Runbook çalışanları, Azure Otomasyonu'nda runbook türleri kümesini desteklemez.

Aşağıdaki runbook türleri, bir Linux karma çalışanı üzerinde çalışır:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell runbook'ları, PowerShell Core, Linux makinesinde yüklü olmasını gerektirir. Bkz: [Linux'ta PowerShell Core yükleme](/powershell/scripting/setup/installing-powershell-core-on-linux) yükleneceği hakkında bilgi edinmek için.

Aşağıdaki runbook türleri, bir Linux karma çalışanı üzerinde çalışmıyor:

* PowerShell iş akışı
* Grafik
* Grafik PowerShell iş akışı

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içi veri merkezinizde veya diğer bulut ortamı işlemlerini otomatikleştirmek için runbook'larınızı yapılandırma konusunda bilgi için bkz: [bir karma Runbook çalışanı üzerinde runbook çalıştırma](automation-hrw-run-runbooks.md).
* Karma Runbook çalışanlarını kaldırma yönergeleri için bkz: [Azure Otomasyon karma Runbook çalışanlarını kaldırma](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Sorun giderme, karma Runbook çalışanları öğrenmek için bkz [Linux karma Runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#linux)