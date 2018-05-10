---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makale bir Azure Otomasyon karma Runbook Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 08d86e467a51ab786e190c7b834f57e365c8130c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları Kaldır](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)
