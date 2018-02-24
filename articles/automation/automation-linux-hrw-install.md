---
title: "Azure Otomasyonu Linux karma Runbook çalışanı | Microsoft Docs"
description: "Bu makale bir Azure Otomasyon karma Runbook Linux tabanlı bilgisayarlarda yerel veri merkezinde veya Bulut ortamında runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: magoedte
ms.openlocfilehash: 9926ceef9777032d52c7655e4c2d03f63a4a6af7
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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
Yüklemek ve karma Runbook çalışanı Linux bilgisayarınızda yapılandırmak için el ile rolünü yüklemek ve yapılandırmak için bir düz İleri süreci izleyin. Etkinleştirme gerektirir **Otomasyon karma çalışanı** OMS çalışma alanınızı ve bilgisayarı çalışan kaydetmek ve yeni veya varolan bir gruba eklemek için komut kümesini çalıştıran çözümde. 

Devam etmeden önce Automation hesabınız bağlandığı günlük analizi çalışma alanı ve ayrıca, Automation hesabınız için birincil anahtarı Not gerekir. Hem portalından Automation hesabınız seçip seçerek bulabilirsiniz **çalışma** çalışma alanı kimliği ve seçerek **anahtarları** birincil anahtar.  

1.  OMS "Otomasyon karma çalışanı" çözümde etkinleştirin. Bu durum ya da yapılabilir:

   1. İçinde çözümleri galerisinden [OMS portalı](https://mms.microsoft.com), etkinleştirme **Otomasyon karma çalışanı** çözümü
   2. Aşağıdaki cmdlet'i çalıştırın:

        ```powershell
         $null = Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

2.  Parametreleri için değerleri değiştirerek aşağıdaki komutu çalıştırın *-w*, *-k*, *-g*, ve *-e*. İçin *-g* parametre değeri yeni Linux karma Runbook çalışanı katılması gereken karma Runbook çalışanı grubu adını değiştirin. Otomasyon hesabınızda adı zaten mevcut değilse yeni bir karma Runbook çalışanı grubu bu ada sahip yapılır.
    
    ```
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <OMSworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
    ```
3. Komut tamamlandığında, Azure portalında karma çalışan grupları dikey yeni Grup ve üye sayısı gösterilir veya varolan bir grubu, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** dikey penceresinde ve select **karma çalışanları** döşeme. Üzerinde **karma çalışanları** dikey penceresi, her listelenen grubunun üyesi bakın.  


## <a name="turning-off-signature-validation"></a>İmza doğrulama kapatma 
Varsayılan olarak, Linux karma Runbook çalışanları imza doğrulaması gerektirir. İmzasız bir runbook karşı çalışan çalıştırırsanız, "imza doğrulaması başarısız oldu" içeren bir hata görürsünüz. İmza doğrulama devre dışı bırakmak için ikinci parametresi, OMS çalışma alanı Kimliğiniz ile değiştirerek aşağıdaki komutu çalıştırın:

    ```
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <OMSworkspaceId>
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
