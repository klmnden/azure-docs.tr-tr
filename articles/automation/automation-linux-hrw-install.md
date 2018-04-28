---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makale bir Azure Otomasyon karma Runbook Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: bc6c98784195aaf80cb6ca32ef29f75666099b06
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Bir Linux karma Runbook çalışanı dağıtma

Azure bulutta çalışan beri Azure Otomasyonu'nda Runbook'lar diğer Bulut veya şirket içi ortamınız kaynaklarına erişemez. Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.

Bu işlevsellik aşağıdaki resimde gösterilmiştir:<br>  

![Karma Runbook çalışanı genel bakış](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Karma Runbook çalışanı rolü teknik bir genel bakış için bkz: [Otomasyon mimarisine genel bakış](automation-offering-get-started.md#automation-architecture-overview). Aşağıdaki bilgileri gözden geçirin ilgili [donanım ve yazılım gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker) ve [ağınıza hazırlamaya yönelik bilgi](automation-offering-get-started.md#network-planning) bir karma Runbook çalışanı dağıtmaya başlamadan önce. Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.     

## <a name="hybrid-runbook-worker-groups"></a>Karma Runbook çalışan grupları
Her karma Runbook çalışanı aracı yüklediğinizde, belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir. Bir gruba bir aracıyı ekleyebilirsiniz, ancak yüksek kullanılabilirlik grubunda birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanını bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin. Grubun üyelerini isteği hangi çalışan hizmetleri belirleyin. Belirli bir çalışan belirtemezsiniz.

## <a name="installing-linux-hybrid-runbook-worker"></a>Linux karma Runbook çalışanı yükleme
Yüklemek ve karma Runbook çalışanı Linux bilgisayarınızda yapılandırmak için el ile rolünü yüklemek ve yapılandırmak için bir düz İleri süreci izleyin. Etkinleştirme gerektirir **Otomasyon karma çalışanı** günlük analizi çalışma alanınız ve bilgisayarı çalışan kaydetmek ve yeni veya varolan bir gruba eklemek için komut kümesini çalıştıran çözümde. 

Devam etmeden önce Automation hesabınız bağlandığı günlük analizi çalışma alanı ve ayrıca, Automation hesabınız için birincil anahtarı Not gerekir. Hem portalından Automation hesabınız seçip seçerek bulabilirsiniz **çalışma** çalışma alanı kimliği ve seçerek **anahtarları** birincil anahtar.  

1.  Azure "Otomasyon karma çalışanı" çözümde etkinleştirin. Bu durum ya da yapılabilir:

   1. Ekle **Otomasyon karma çalışanı** adresindeki yordamı kullanarak, aboneliğinizin çözüme [çalışma alanınıza ekleyin günlük analizi yönetim çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions).
   2. Aşağıdaki cmdlet'i çalıştırın:

        ```powershell
         $null = Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

2.  Parametreleri için değerleri değiştirerek aşağıdaki komutu çalıştırın *-w*, *-k*, *-g*, ve *-e*. İçin *-g* parametre değeri yeni Linux karma Runbook çalışanı katılması gereken karma Runbook çalışanı grubu adını değiştirin. Otomasyon hesabınızda adı zaten mevcut değilse yeni bir karma Runbook çalışanı grubu bu ada sahip yapılır.
    
    ```python
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
    ```
3. Komut tamamlandığında, Azure portalında karma çalışan grupları dikey yeni Grup ve üye sayısı gösterilir veya varolan bir grubu, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** dikey penceresinde ve select **karma çalışanları** döşeme. Üzerinde **karma çalışanları** dikey penceresi, her listelenen grubunun üyesi bakın.  


## <a name="turning-off-signature-validation"></a>İmza doğrulama kapatma 
Varsayılan olarak, Linux karma Runbook çalışanları imza doğrulaması gerektirir. İmzasız bir runbook karşı çalışan çalıştırırsanız, "imza doğrulaması başarısız oldu" içeren bir hata görürsünüz. İmza doğrulama devre dışı bırakmak için ikinci parametresi, günlük analizi çalışma alanı Kimliğiniz ile değiştirerek aşağıdaki komutu çalıştırın:

 ```python
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
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları Kaldır](automation-remove-hrw.md)
