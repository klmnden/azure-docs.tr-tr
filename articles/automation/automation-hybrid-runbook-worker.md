---
title: Azure Otomasyon karma Runbook çalışanı
description: Bu makalede, yükleme ve yerel veri merkezinde veya Bulut sağlayıcısı makinelerde runbook'ları çalıştırmak için kullanabileceğiniz bir Azure Otomasyonu özelliğidir karma Runbook çalışanı kullanma hakkında bilgiler sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 54b2cab2ad6b1a22d35fcf0755f257063573e58b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128631"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Karma Runbook çalışanı kullanarak veri merkezi veya Bulut kaynakları otomatikleştirme

Azure Otomasyonu runbook'ları Azure bulut platformu üzerinde çalıştıkları diğer Bulut veya şirket içi ortamınız kaynaklara erişmek kuramamış olabilir. Runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.

Aşağıdaki resimde bu işlevselliği gösterilmektedir:

![Karma Runbook Çalışanına genel bakış](media/automation-hybrid-runbook-worker/automation.png)

Her karma Runbook çalışanı aracı yüklediğinizde, belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir. Bir gruba bir aracıyı ekleyebilirsiniz, ancak yüksek kullanılabilirlik grubunda birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanını bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin. Gruptaki her çalışan tüm işleri olup olmadığını görmek için Azure Otomasyonu yoklar. Bir işi varsa, iş almak için ilk worker sürer. Belirli bir çalışan belirtemezsiniz.

## <a name="install-a-hybrid-runbook-worker"></a>Karma Runbook Worker yükleme

Bir karma Runbook çalışanı yükleme işlemini işletim sisteminde bağlıdır. Aşağıdaki tabloda, yükleme için kullanabileceğiniz yöntemler bağlantılar içerir. 

Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntem kullanabilirsiniz. Önerilen yöntem, tam bir Windows bilgisayarı yapılandırma işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor. Linux makineler için makineye aracı yüklemek için bir Python betiği çalıştırın.

|İşletim Sistemi  |Dağıtım türleri  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[El ile](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolünü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir. Ekleme hakkında daha fazla bilgi için DSC ile yönetimine görebileceği [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).
>
>Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](automation-update-management.md), bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak Azure günlük analizi çalışma alanına bağlı herhangi bir bilgisayarda otomatik olarak yapılandırılır. Ancak, bilgisayar Otomasyon hesabınızda zaten tanımlanmış bir karma çalışanı grubu kayıtlı değil. Bilgisayar grubuna Automation runbook çözümü ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanmakta olduğunuz sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir. Bu işlevsellik, karma Runbook çalışanı 7.2.12024.0 sürümüne eklenmiştir.

Gözden geçirme [ağınızı planlama bilgileri](#network-planning) bir karma Runbook çalışanı dağıtmaya başlamadan önce. Çalışan başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.

## <a name="remove-a-hybrid-runbook-worker"></a>Bir karma Runbook çalışanı kaldırma

Bir veya daha fazla karma Runbook çalışanları bir gruptan kaldırdığınızda veya grup gereksinimlerinize bağlı olarak kaldırabilirsiniz. Bir karma Runbook çalışanı bir şirket içi bilgisayardan kaldırmak için aşağıdaki adımları gerçekleştirin:

1. Azure Portal'da, Automation hesabınızı gidin.
2. Altında **ayarları**seçin **anahtarları** ve değerlerini Not **URL** ve **birincil erişim anahtarını**. Bu bilgiler sonraki adımda gerekir.

### <a name="windows"></a>Windows

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın. Kullanım **-Verbose** geçiş kaldırma işleminin ayrıntılı günlüğü için.

```powershell
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Eski makineler karma çalışanı gruptan kaldırmak için isteğe bağlı kullanın `machineName` parametresi.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Bu kod Microsoft İzleme Aracısı bilgisayardan, yalnızca işlevsellik ve karma Runbook çalışanı rolü yapılandırmasını kaldırmaz.

## <a name="remove-a-hybrid-worker-group"></a>Karma çalışanı grubu Kaldır

Bir grubu kaldırmak için ilk karma Runbook çalışanı grubunun bir üyesi daha önce gösterilen yordamı kullanarak her bilgisayardan kaldırmanız gerekir. Ardından, grubu kaldırmak için aşağıdaki adımları gerçekleştirin:

1. Azure Portal'da Automation hesabını açın.
1. Altında **işlem Otomasyonu**seçin **karma çalışan grupları**. Silmek istediğiniz grubu seçin. Bu grup için özellikleri sayfası görüntülenir.

   ![Özellikler sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Seçilen grup için Özellikler sayfasında, seçin **silmek**. Bu eylemi onaylamanızı soran bir ileti görüntülenir. Seçin **Evet** devam etmek istediğinizden eminseniz.

   ![Onay iletisi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye sürebilir. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="network-planning"></a>Ağınızı yapılandırmak

### <a name="hybrid-worker-role"></a>Karma çalışanı rolü

Karma Runbook bağlanmak ve günlük analizi ile kaydetmek çalışan için bağlantı noktası numarasını ve bu bölümde açıklanan URL'lere erişimi olmalıdır. Ek olarak bu erişimi olan [bağlantı noktaları ve Microsoft Monitoring Agent için gereken URL'leri](../log-analytics/log-analytics-agent-windows.md) günlük Analizi'ne bağlanmak için. 

Günlük analizi hizmeti ile aracı arasındaki iletişim için bir proxy sunucu kullanıyorsanız, uygun kaynaklara erişilebilir olduğundan emin olun. İnternet'e erişimi kısıtlamak için bir güvenlik duvarı kullanıyorsanız, erişimine izin vermek için güvenlik duvarını yapılandırmanız gerekir.

Aşağıdaki bağlantı noktası ve URL'leri otomasyon ile iletişim kurmak karma Runbook çalışanı rolü için gereklidir:

* Bağlantı noktası: Yalnızca TCP 443 giden internet erişimi için gereklidir.
* Genel URL'si: *.azure-automation.net
* ABD kamu Virginia genel URL'sini: *.azure automation.us
* Aracı hizmeti: https://\<Workspaceıd\>.agentsvc.azure-automation.net

Özel durumlar tanımlarken listelenen adresler kullanılması önerilir. IP adreslerinin indirebilirsiniz [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bu dosya, haftalık olarak güncelleştirilen ve şu anda dağıtılmış aralıklarını ve IP aralıklarını yaklaşan değişiklikleri yansıtır.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa, o bölgesel veri merkezine iletişimi kısıtlayabilirsiniz. Aşağıdaki tabloda, her bölge için DNS kaydı sağlar:

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Batı Orta ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-üretim-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-üretim-1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-üretim-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japonya Doğu |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-üretim-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>Ana-agentservice-üretim-1.azure-automation.net |
| Birleşik Krallık Güney | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-üretim-1.azure-automation.net |
| ABD Devleti Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-üretim-1.azure-automation.us |

Bölge adları yerine bölge IP adresleri listesi için indirme [Azure veri merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) Microsoft Download Center XML dosyasından.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyası içerir.
>
>Güncelleştirilen bir dosya haftalık nakledilir. Dosya şu anda dağıtılmış aralıklarını ve IP aralıklarını yaklaşan değişiklikleri yansıtır. Dosyasında yeni aralıkları, en az bir hafta boyunca veri merkezlerinde kullanılmaz.
>
> Her hafta yeni bir XML dosyası indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru bir şekilde tanımlamak için sitenizi güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı Azure alanı sınır ağ geçidi Protokolü (BGP) tanıtım her ayın ilk haftası içinde güncelleştirmek için kullanılır unutmamalısınız.

### <a name="update-management"></a>Güncelleştirme Yönetimi

Standart adresleri ve karma Runbook çalışanı gereken bağlantı noktaları ek olarak, aşağıdaki adresleri özellikle güncelleştirme yönetimi için gereklidir. Bu adresler için iletişim bağlantı noktası 443 üzerinden yapılır.

|Azure genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

## <a name="troubleshoot"></a>Sorun giderme

Karma Runbook çalışanları sorunlarını giderme konusunda bilgi almak için bkz: [karma Runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#general)

## <a name="next-steps"></a>Sonraki adımlar

Runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md).