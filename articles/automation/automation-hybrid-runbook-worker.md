---
title: Azure Otomasyon karma Runbook çalışanı
description: Bu makalede, yükleme ve yerel veri merkezinde veya Bulut sağlayıcısı makinelerde runbook'ları çalıştırmak için kullanabileceğiniz bir Azure Otomasyonu özelliğidir karma Runbook çalışanı kullanma hakkında bilgiler sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/05/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4c91bf389f5c63b95e5b68784b6657e92b109a46
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59265874"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Veri merkezinde veya bulutta kaynaklarında karma Runbook çalışanı kullanarak otomatik hale getirin.

Bunlar Azure bulut platformunda çalıştığından Azure Otomasyonu runbook'ları, şirket içi ortamınızda veya diğer bulutlarda kaynaklarına erişimi olmayabilir. Runbook'ları doğrudan rolünü barındıran bilgisayarda ve ortamınızda bu yerel kaynakları yönetmek için kaynakların karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Runbook'ları tutulur ve yönetilen Azure Otomasyonu'nda ve sonra bir veya daha fazla atanmış bilgisayarlara teslim.

Aşağıdaki görüntüde bu işlevselliği gösterilmektedir:

![Karma Runbook Çalışanına genel bakış](media/automation-hybrid-runbook-worker/automation.png)

Her karma Runbook çalışanı, aracı yükleme sırasında belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir. Bir grubu tek bir aracı ekleyebilirsiniz, ancak yüksek kullanılabilirlik için bir grupta birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanı üzerinde bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin. Gruptaki her çalışan tüm işleri olup olmadığını görmek için Azure Otomasyonu yoklar. Bir iş varsa, bu işi almak için ilk worker götürür. Karma çalışanı donanım profili ve yükleme işleri sırasının işleme süresi bağlıdır. Belirli bir alt belirtemezsiniz. Karma Runbook çalışanları, birçok Azure sanal sahip sınırları paylaşmayın. Bunlar aynı sınırlarını disk alanı, bellek veya ağ yuvaları üzerinde yok. Karma Runbook çalışanları, yalnızca karma Runbook çalışanında kendisini kaynaklar tarafından sınırlandırılmıştır. Ayrıca, karma Runbook çalışanları 180 dakika paylaşmayın [adil paylaşımı](automation-runbook-execution.md#fair-share) süre sınırı, Azure sanal yapın. Azure sanal ve karma Runbook çalışanları için hizmet sınırları hakkında daha fazla bilgi için iş bkz [sınırları](../azure-subscription-service-limits.md#automation-limits) sayfası.

## <a name="install-a-hybrid-runbook-worker"></a>Karma Runbook çalışanı'nı yükleme

İşletim sisteminde bir karma Runbook çalışanı yükleme işlemini bağlıdır. Aşağıdaki tabloda, yükleme için kullanabileceğiniz yöntemleri bağlantılar içerir.

Yükleme ve yapılandırma Windows karma Runbook çalışanı için iki yöntem kullanabilirsiniz. Önerilen yöntem, tam bir Windows bilgisayar yapılandırma işlemi otomatik hale getirmek için bir Otomasyon runbook'unu kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam kullanılmasıdır. Linux makineler için aracı makinede yüklemeye yönelik bir Python betiği çalıştırın.

|İşletim Sistemi  |Dağıtım türleri  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Karma Runbook çalışanı rolü Desired State Configuration (DSC) ile destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir. Ekleme hakkında daha fazla bilgi için DSC ile yönetim için görebileceği [makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).
>
>Etkinleştirirseniz [güncelleştirme yönetimi çözümünü](automation-update-management.md), Azure Log Analytics çalışma alanınıza bağlı herhangi bir bilgisayarda otomatik olarak bu çözümde yer alan runbook'ların desteklenmesi için bir karma Runbook çalışanı olarak yapılandırılır. Ancak, bilgisayar Otomasyon hesabınızda önceden tanımlı karma çalışanı gruplarıyla kaydedilmemiştir. Bilgisayar çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon gruplarını desteklemek için Otomasyon hesabınızdaki bir karma Runbook çalışanı grubuna eklenebilir. Bu işlev karma Runbook çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Gözden geçirme [ağınızı planlama bilgileri](#network-planning) bir karma Runbook çalışanı dağıtıma başlamadan önce. Çalışan başarıyla dağıtıldıktan sonra gözden [bir karma Runbook çalışanı üzerinde runbook çalıştırma](automation-hrw-run-runbooks.md) şirket içi veri merkezinizde veya diğer bulut ortamı işlemlerini otomatikleştirmek için runbook'larınızı yapılandırma hakkında bilgi edinmek için.

## <a name="remove-a-hybrid-runbook-worker"></a>Karma Runbook çalışanı Kaldır

Bir veya daha fazla karma Runbook çalışanları gruptan kaldırdığınızda veya grup gereksinimlerinize bağlı olarak kaldırabilirsiniz. Karma Runbook çalışanı şirket içi bir bilgisayardan kaldırmak için aşağıdaki adımları kullanın:

1. Azure portalında, Otomasyon hesabınıza gidin.
2. Altında **hesap ayarları**seçin **anahtarları** ve değerlerini Not **URL** ve **birincil erişim anahtarı**. Bu bilgiler sonraki adımda ihtiyacınız var.

### <a name="windows"></a>Windows

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın. Kullanım **-Verbose** kaldırma işleminin ayrıntılı günlüğü için geçiş.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Eski makineler, karma çalışanı grubundan kaldırmak için isteğe bağlı kullanın `machineName` parametresi.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

Komutunu kullanabilirsiniz `ls /var/opt/microsoft/omsagent` workspaceıd almak için karma Runbook çalışanı üzerinde. Klasörün adını çalışma alanı kimliği olduğu dizinde bir klasör yok

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Bu kod, bilgisayarda, işlevsellik ve karma Runbook çalışanı rolü yapılandırması yalnızca Microsoft Monitoring Agent kaldırmaz.

## <a name="remove-a-hybrid-worker-group"></a>Karma çalışanı grubu Kaldır

Bir grubu kaldırmak için önce daha önce gösterilen bir yordamı kullanarak grubunun bir üyesi olan her bir bilgisayardan karma Runbook çalışanı kaldırmanız gerekir. Ardından grubunu kaldırmak için aşağıdaki adımları kullanın:

1. Azure portalında Otomasyon hesabını açın.
2. Altında **süreç otomasyonu**seçin **karma çalışan grupları**. Silmek istediğiniz grubu seçin. Bu grup için Özellikler sayfasında görünür.

   ![Özellikler sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Seçili grubun Özellikler sayfasında, seçin **Sil**. Bu eylemi onaylamanız için soran bir ileti görüntülenir. Seçin **Evet** devam etmek istediğinizden emin değilseniz.

   ![Onay iletisi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye sürebilir. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="network-planning"></a>Ağınızı yapılandırmak

### <a name="hybrid-worker-role"></a>Karma çalışan rolü

Karma Runbook bağlanmak ve Azure Otomasyonu'na kaydetmek çalışanı için bu bağlantı noktası numarası ve bu bölümünde açıklanan URL'lere erişimi olmalıdır. En üste bu erişime açıktır [bağlantı noktaları ve URL'ler Microsoft izleme aracısının gerektirdiği](../azure-monitor/platform/agent-windows.md) Azure İzleyici günlüklerine bağlanmak için.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure Otomasyonu ile aracı arasındaki iletişim için bir ara sunucu kullanıyorsanız uygun kaynakların erişilebilir olduğundan emin olun. Karma Runbook çalışanı ve Otomasyon hizmetleri gelen istekleri için zaman aşımını 30 saniyedir. 3 denemeden sonra istek başarısız olur. İnternet'e erişimi kısıtlamak için Güvenlik Duvarı'nı kullanıyorsanız erişime izin vermek için güvenlik duvarını yapılandırmanız gerekir. Log Analytics ağ geçidi bir ara sunucu kullanırsanız, karma çalışanları için yapılandırıldığından emin olun. Bunun nasıl yapılacağı hakkında yönergeler için bkz [Otomasyon karma çalışanı için Log Analytics ağ geçidini yapılandırma](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

Aşağıdaki bağlantı noktası ve URL'leri Otomasyonu ile iletişim kurmak karma Runbook çalışanı rolü için gereklidir:

* Bağlantı Noktası: Yalnızca TCP 443 giden internet erişimi için gereklidir.
* Genel URL: *.azure-automation.net
* ABD Devleti Virginia genel URL: *.azure-automation.us
* Aracı hizmeti: https://\<Workspaceıd\>.agentsvc.azure-automation.net

Özel durumlar tanımlarken listelenen adreslerini kullanmak için önerilir. IP adreslerinin indirebilirsiniz [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri vardır.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa bu Bölgesel veri merkezi iletişimin kısıtlayabilirsiniz. Aşağıdaki tabloda, her bölge için DNS kaydı verilmiştir:

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Batı Orta ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Batı ABD 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japonya Doğu |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Birleşik Krallık Güney | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| ABD Devleti Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Bölge bölge adları yerine IP adresleri listesi için indirme [Azure veri merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasından Microsoft Download Center.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyası içerir.
>
>Güncelleştirilen bir dosya haftalık olarak gönderilir. Dosya şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz.
>
> Her hafta yeni XML dosyasını indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı her ayın ilk haftasında Azure alanındaki Border Gateway Protocol (BGP) reklamı güncelleştirmek için kullanıldığını unutmamalısınız.

### <a name="update-management"></a>Güncelleştirme Yönetimi

Standart adresleri ve karma Runbook çalışanı gereken bağlantı noktaları üzerinde aşağıdaki adresleri özellikle güncelleştirme yönetimi için gereklidir. Bu adresler için iletişim bağlantı noktası 443 üzerinden gerçekleştirilir.

|Azure kamu  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içi veri merkezinizde veya diğer bulut ortamı işlemlerini otomatikleştirmek için runbook'larınızı yapılandırma konusunda bilgi için bkz: [bir karma Runbook çalışanı üzerinde runbook çalıştırma](automation-hrw-run-runbooks.md).
* Sorun giderme, karma Runbook çalışanları öğrenmek için bkz [karma Runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#general)

