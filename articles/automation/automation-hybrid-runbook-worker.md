---
title: Azure Otomasyon karma Runbook çalışanları
description: Bu makalede, yükleme ve yerel veri merkezinde veya Bulut sağlayıcısı makinelerde runbook'lar çalıştırmanıza olanak sağlayan Azure Automation'ın bir özellik olan karma Runbook çalışanı kullanma hakkında bilgiler sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 270990d5dea53c2467bf2d7df4695a14b3dbec8c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Veri merkezi veya karma Runbook çalışanı ile bulut kaynakları otomatikleştirme

Azure Otomasyonu runbook'ları Azure bulutta çalışan bu yana diğer Bulut veya şirket içi ortamınız kaynaklara erişebilir olmayabilir. Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.

Bu işlevsellik aşağıdaki resimde gösterilmiştir:

![Karma Runbook çalışanı genel bakış](media/automation-hybrid-runbook-worker/automation.png)

## <a name="hybrid-runbook-worker-groups"></a>Karma Runbook çalışan grupları

Her karma Runbook çalışanı aracı yüklediğinizde, belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir. Bir gruba bir aracıyı ekleyebilirsiniz, ancak yüksek kullanılabilirlik grubunda birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanını bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin. Gruptaki her çalışan tüm işleri olup olmadığını görmek için Azure Otomasyonu yoklar. Kullanılabilir bir işi ise iş almak için ilk çalışan işlem alır. Belirli bir çalışan belirtemezsiniz.

## <a name="installing-a-hybrid-runbook-worker"></a>Bir karma Runbook çalışanı yükleme

Karma Runbook çalışanı yüklemek için işletim sistemi bağlı olarak farklı işlemidir. Aşağıdaki tabloda bir karma Runbook çalışanı yüklemek için kullanabileceğiniz farklı yöntemler bağlantılar içerir. Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntemi vardır. Önerilen yöntem, tam bir Windows bilgisayarı yapılandırmak için gerekli işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor. Linux makineler için makineye aracı yüklemek için bir Python betiği çalıştırın

|İşletim Sistemi  |Dağıtım türleri  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[El ile](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-linux-hybrid-runbook-worker)        |

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir. Ekleme hakkında daha fazla bilgi için DSC ile yönetimine görebileceği [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).
>
>Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](automation-update-management.md), günlük analizi çalışma alanına bağlı herhangi bir bilgisayarda otomatik olarak bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak yapılandırılır. Ancak, bu Otomasyon hesabında zaten tanımlanmış bir karma çalışanı grubu kayıtlı değil. Bilgisayar grubuna Otomasyon runbook'ları çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanarak sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Gözden geçirme [ağınızı planlama bilgileri](#network-planning) bir karma Runbook çalışanı dağıtmaya başlamadan önce. Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.

## <a name="removing-hybrid-runbook-worker"></a>Karma Runbook çalışanı kaldırma

Bir veya daha fazla karma Runbook çalışanları bir gruptan kaldırdığınızda veya grup gereksinimlerinize bağlı olarak kaldırabilirsiniz. Bir karma Runbook çalışanı bir şirket içi bilgisayardan kaldırmak için aşağıdaki adımları gerçekleştirin:

1. Azure Portal'da, Automation hesabınızı gidin.
2. Gelen **ayarları** dikey penceresinde, select **anahtarları** ve alan değerlerini Not **URL** ve **birincil erişim anahtarını**. Bu bilgiler sonraki adımda gerekir.

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
> Bu Microsoft İzleme Aracısı bilgisayardan, yalnızca işlevsellik ve karma Runbook çalışanı rolü yapılandırmasını kaldırmaz.

## <a name="remove-hybrid-worker-groups"></a>Karma çalışan grupları kaldırın

Bir grubu kaldırmak için önce bir karma Runbook çalışanı daha önce gösterilen yordamı kullanarak grubunun bir üyesi olan her bilgisayarda kaldırmanız gerekir ve ardından grubunu kaldırmak için aşağıdaki adımları gerçekleştirin.

1. Azure Portal'da Automation hesabını açın.
1. Altında **işlem Otomasyonu**seçin **karma çalışan grupları**. Silmek istediğiniz grubu seçin. Belirli grup seçtikten sonra **karma çalışanı grubu** özellikleri sayfası görüntülenir.

   ![Karma Runbook çalışan grubu sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Seçilen grup için Özellikler sayfasında, tıklatın **silmek**. Bu eylemi onaylamanızı isteyen bir ileti görüntülenir seçin **Evet** devam etmek istediğinizden eminseniz.

   ![Grubu Sil onay iletişim kutusu](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye alabilir ve ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="network-planning"></a>Ağınızı yapılandırmak

### <a name="hybrid-worker-role"></a>Karma çalışanı rolü

Karma Runbook bağlanmak ve günlük analizi ile kaydetmek çalışan için bağlantı noktası numarasını ve bu bölümde açıklanan URL'lere erişimi olmalıdır. Ek olarak budur [bağlantı noktaları ve Microsoft Monitoring Agent için gereken URL'leri](../log-analytics/log-analytics-agent-windows.md) günlük Analizi'ne bağlanmak için.

Günlük analizi hizmeti ile aracı arasındaki iletişim için bir proxy sunucu kullanıyorsanız, uygun kaynaklara erişilebilir olduğundan emin olun. İnternet'e erişimi kısıtlamak için bir güvenlik duvarı kullanıyorsanız, erişimine izin vermek için güvenlik duvarını yapılandırmanız gerekir.

Aşağıdaki bağlantı noktası ve URL'leri otomasyon ile iletişim kurmak karma Runbook çalışanı rolü için gereklidir:

* Bağlantı noktası: Yalnızca TCP 443 giden internet erişimi için gereklidir.
* Genel URL'si: *.azure-automation.net
* ABD kamu Virginia genel URL'sini: *.azure automation.us
* Aracı hizmeti: https://\<Workspaceıd\>.agentsvc.azure-automation.net

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa, o bölgesel veri merkezine iletişimi kısıtlayabilirsiniz. Aşağıdaki tabloda, her bölge için DNS kaydını sağlar.

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Batı Orta ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-üretim-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-üretim-1.azure-automation.net |
| Kanada Orta |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-üretim-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japonya Doğu |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-üretim-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>Ana-agentservice-üretim-1.azure-automation.net |
| BK Güney | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-üretim-1.azure-automation.net |
| ABD Devleti Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-üretim-1.azure-automation.us |

Bölge adları yerine bölge IP adresleri listesi için indirme [Azure veri merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) Microsoft Download Center XML dosyasından.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyasına dahil edilir.
>
>Güncelleştirilen bir dosya haftalık nakledilir. Dosya şu anda dağıtılmış aralıklarını ve IP aralıklarını yaklaşan değişiklikleri yansıtır. Dosyasında yeni aralıkları, en az bir hafta boyunca veri merkezlerinde kullanılmaz.
>
> Her hafta yeni bir XML dosyası indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru bir şekilde tanımlamak için sitenizi güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı her ayın ilk haftası sınır ağ geçidi Protokolü (BGP) tanıtım Azure alanı güncelleştirmek için kullanılan unutmamalısınız.

### <a name="update-management"></a>Güncelleştirme Yönetimi

Standart adresleri ve karma Runbook çalışanı gereken bağlantı noktaları ek olarak, aşağıdaki adresleri özellikle güncelleştirme yönetimi için gereklidir. Bu adresler için iletişim bağlantı noktası 443 üzerinden yapılır.

|Azure genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

## <a name="troubleshooting"></a>Sorun giderme

Karma Runbook çalışanı çalışan kaydetmek, runbook işlerini almak ve durum raporu için Otomasyon hesabınızın iletişim kurmak için bir aracı bağlıdır. Windows için bu aracı Microsoft Monitoring Agent ' dir. Linux için Linux için OMS aracısının olduğu. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:

### <a name="the-hybrid-worker-is-behind-a-proxy-or-firewall"></a>Karma çalışanı bir proxy veya güvenlik duvarı arkasında olduğunu

Bilgisayarın, bağlantı noktası 443 üzerinde *.azure automation.net giden erişimi olduğunu doğrulayın.

### <a name="the-computer-the-hybrid-worker-is-running-on-has-less-than-the-minimum-hardware-requirements"></a>Karma çalışanı çalışmakta olduğu bilgisayarın en düşük donanım gereksinimlerini daha azı

Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi halde, kaynak kullanımı diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden Çekişme bağlı olarak bilgisayar üzerinde kullanılan olur ve bu runbook işi gecikmeler veya zaman aşımları neden.

Karma Runbook çalışanı özelliği yürütmek için atanan bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın. Aşması durumunda, karma Runbook çalışanı işlemlerinin performansını ile Windows arasında herhangi bir bağıntı belirlemek için CPU ve bellek kullanımını izleyin. Bellek veya CPU baskısı ise, bu yükseltme veya ek işlemciler ekleme ya da kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösteriyor olabilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimleri destekleyebilen farklı işlem kaynak seçin.

Belirli bir işletim sistemi için sorun giderme hakkında ek bilgi için bkz: [Linux karma Runbook çalışanı](automation-linux-hrw-install.md#troubleshooting) veya [Windows karma Runbook çalışanı](automation-windows-hrw-install.md#troubleshooting)

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
