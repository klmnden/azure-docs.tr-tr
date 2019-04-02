---
title: Güncelleştirme yönetimi çözümünü azure'da
description: Bu makale, Azure güncelleştirme yönetimi çözümünü Windows ve Linux bilgisayarlarınızın güncelleştirmelerini yönetmek için nasıl kullanılacağını anlamanıza yardımcı olmak için hazırlanmıştır.
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 42a7ae0e6ca5239aa83d20655817973e8f185d02
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805406"
---
# <a name="update-management-solution-in-azure"></a>Güncelleştirme yönetimi çözümünü azure'da

Azure'da, şirket içi ortamlarda veya diğer bulut sağlayıcıları, Windows ve Linux bilgisayarlar için işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu'nda güncelleştirme yönetimi çözümü kullanabilirsiniz. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Doğrudan Azure Otomasyonu hesabınızdan sanal makineler için güncelleştirme yönetimini etkinleştirebilirsiniz. Otomasyon hesabınızdan sanal makineler için güncelleştirme yönetimini etkinleştirme hakkında bilgi için bkz: [birden çok sanal makine için güncelleştirmeleri yönetme](manage-update-multi.md). Ayrıca, sanal makine sayfasından Azure portalında sanal makine için güncelleştirme yönetimini etkinleştirebilirsiniz. Bu senaryo için kullanılabilir [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) sanal makineler.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Çözüme genel bakış

Güncelleştirme yönetimi tarafından yönetilen bilgisayarlar değerlendirme gerçekleştirmek ve güncelleştirme dağıtımları için aşağıdaki yapılandırmaları kullanın:

* Microsoft Monitoring Agent (MMA) Windows veya Linux için
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Microsoft Update veya Windows bilgisayarlar için Windows Server Update Services (WSUS)

Aşağıdaki diyagramda davranışı kavramsal bir görünümü gösterir ve veri akışının nasıl çözüm değerlendirir ve tüm güvenlik güncelleştirmelerini uygular bağlı Windows Server ve Linux bilgisayarları bir çalışma alanında:

![Güncelleştirme yönetimi işlem akışı](./media/automation-update-management/update-mgmt-updateworkflow.png)

Güncelleştirme yönetimi, yerel makine aynı kiracıda birden çok abonelik içinde kullanılabilir.

Bir CVE kullanıma sunulduğunda, Linux makineleri değerlendirme için gösterilmesi düzeltme eki 2-3 saat sürer.  Windows makineleri için piyasaya sürüldükten sonra değerlendirmesi için gösterilecek yamasının 12-15 saat sürer.

Bilgisayar güncelleştirme uyumluluğu taraması tamamlandıktan sonra aracısı bilgileri toplu Azure İzleyici günlüklerine iletir. Bir Windows bilgisayarda Uyumluluk taraması varsayılan olarak her 12 saatte bir çalıştırılır.

Tarama zamanlamasına ek olarak, güncelleştirme yüklemesi öncesinde ve güncelleştirme yüklemesi sonrasında yeniden başlatılmadan MMA 15 dakika içinde güncelleştirme uyumluluğu için tarama başlatılır.

Bir Linux bilgisayar için Uyumluluk taraması varsayılan olarak her 3 saatte bir gerçekleştirilir. MMA aracısını yeniden başlatılması durumunda 15 dakika içinde Uyumluluk taraması başlatılır.

Raporları dayanarak bilgisayarınızın ne kaynağı üzerinde nasıl güncel şekilde yapılandırılmış çözüm ile eşitleme. Windows bilgisayar ne zaman WSUS en son Microsoft Update ile eşitlenen bağlı olarak, WSUS için bildirmek için yapılandırılmışsa, sonuçları hangi Microsoft Updates gösterir farklı olabilir. Bu davranış genel depo yerine yerel bir depoya rapora şekilde yapılandırılan Linux bilgisayarları aynıdır.

> [!NOTE]
> Düzgün hizmete raporlamak üzere güncelleştirme yönetimi belirli URL'lerini ve bağlantı noktalarını etkinleştirilmesini gerektirir. Bu gereksinimler hakkında daha fazla bilgi için bkz. [ağ karma çalışanları için planlama](automation-hybrid-runbook-worker.md#network-planning).

Zamanlanmış bir dağıtım oluşturarak, yazılım güncelleştirmelerinin gerekli olduğu bilgisayarlara güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. Olarak sınıflandırılmış güncelleştirmeler *isteğe bağlı* Windows bilgisayarlar için dağıtım kapsamına dahil değildir. Yalnızca gerekli güncelleştirmeleri dağıtım kapsamına dahildir. 

Zamanlanmış dağıtım, bilgisayarları açıkça belirterek veya seçerek uygun güncelleştirmeleri hangi hedef bilgisayarların alma tanımlayan bir [bilgisayar grubu](../azure-monitor/platform/computer-groups.md) belirli bir bilgisayar kümesinin günlük aramaları dayanır. Onayla ve güncelleştirmeleri yüklenebilir süreyi ayarlamak için bir zamanlama de belirtirsiniz.

Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook'ları görüntüleyemezsiniz ve runbook'lar herhangi bir yapılandırma gerekmez. Güncelleştirme dağıtımı oluşturulduğunda, güncelleştirme dağıtımına dahil edilen bilgisayarlar için belirtilen zamanda ana güncelleştirme runbook'unu başlatan bir zamanlama oluşturur. Ana runbook, gerekli güncelleştirmelerin yükleneceği her aracıda bir alt runbook başlatır.

Tarih ve güncelleştirme dağıtımında belirtilen saatte, hedef bilgisayarlar dağıtımı paralel olarak yürütün. Yüklemeden önce güncelleştirmelerin hala gerekli olduğunu doğrulamak için bir tarama çalıştırılır. WSUS istemci bilgisayarları için WSUS'de güncelleştirmeleri onaylı olmayan güncelleştirme dağıtımının başarısız olur.

Birden fazla Log Analytics çalışma alanları (çoklu yönlendirmeyi) ortamında güncelleştirme yönetimi için kayıtlı bir makine olması desteklenmiyor.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, desteklenen işletim sistemlerinin bir listesi gösterilir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Destekler, yalnızca değerlendirme güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve üzeri (dahil olmak üzere Windows Server 2012 ve 2016)    |.NET framework 4.5.1 veya üzeri gereklidir. ([İndirme .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 veya üzeri gereklidir. ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1, daha fazla güvenilirlik için önerilir.  ([İndirme WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı düzeltme eki uygulama, CentOS, kullanıma hazır yoksa güvenlik verileri döndürmek için 'ı yum' gerektirir. CentOS üzerinde sınıflandırmaya düzeltme eki uygulama ile ilgili daha fazla bilgi için bkz: [güncelleştirme sınıflandırmaları Linux'ta](#linux-2)          |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14.04 LTS, 16.04 LTS ve 18.04 (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen bir işletim sistemleri listelenmektedir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.        |
|Windows Server 2016 Nano sunucu     | Desteklenmiyor.       |

### <a name="client-requirements"></a>İstemci gereksinimleri

#### <a name="windows"></a>Windows

Windows aracıları bir WSUS sunucusuyla iletişim kuracak şekilde yapılandırılması gerekir veya Microsoft Update'e erişimi olmalıdır. System Center Configuration Manager ile güncelleştirme yönetimi kullanabilirsiniz. Tümleştirme senaryoları hakkında daha fazla bilgi için bkz: [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md#configuration). [Windows Aracısı](../azure-monitor/platform/agent-windows.md) gereklidir. Onboarding bir Azure sanal makinesi kullanıyorsanız aracı otomatik olarak yüklenir.

#### <a name="linux"></a>Linux

Linux için makine bir güncelleştirme havuzuna erişimi olmalıdır. Güncelleştirme deposu, özel veya genel olabilir. Güncelleştirme yönetimi ile etkileşim kurmak için TLS 1.1 veya TLS 1.2 gerekir. Bir rapor birden fazla Log Analytics çalışma alanları için yapılandırılmış Linux için Log Analytics Aracısı, bu çözüm ile desteklenmez.

Linux için Log Analytics aracısını yükleme ve en son sürümü indirmek için hakkında daha fazla bilgi için bkz. [Linux için Log Analytics aracısını](https://github.com/microsoft/oms-agent-for-linux). Windows için Log Analytics aracısını yükleme hakkında daha fazla bilgi için bkz: [İzleme Aracısı Windows için Microsoft](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>İzinler

Oluşturma ve güncelleştirme dağıtımlarını yönetmek için belirli izinler gerekir. Bu izinleri hakkında bilgi edinmek için [rol tabanlı erişim - güncelleştirme yönetimi](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Çözüm bileşenleri

Çözüm, aşağıdaki kaynaklardan oluşur. Kaynaklar, Otomasyon hesabınıza eklenir. Ya da doğrudan bağlı aracılar oldukları veya bir Operations Manager bağlantılı yönetim grubunda.

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözümü etkinleştirdikten sonra Log Analytics çalışma alanınıza doğrudan bağlı herhangi bir Windows bilgisayarda otomatik olarak bu çözümde yer alan runbook'ların desteklenmesi için bir karma Runbook çalışanı olarak yapılandırılır.

Çözüm tarafından yönetilen her Windows bilgisayarı listelenen **karma çalışan grupları** bölmesi bir **sistem karma çalışan grubu** Otomasyon hesabı için. Çözümler adlandırma kuralını kullanır *Hostname fqdn_guıd*. Hesabınızdaki runbook'larla bu grupları hedefleyemezsiniz. Denerseniz başarısız. Bu gruplar yalnızca yönetim çözümünü desteklemeye yöneliktir.

Windows bilgisayarları, çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanırsanız, Otomasyon gruplarını desteklemek için Otomasyon hesabınızdaki bir karma Runbook çalışanı grubuna ekleyebilirsiniz. Bu işlev karma Runbook çalışanının 7.2.12024.0 sürümüne eklenmiştir.

### <a name="management-packs"></a>Yönetim paketleri

System Center Operations Manager yönetim grubunuzun bir Log Analytics çalışma alanınıza bağlıysa, aşağıdaki yönetim paketlerini Operations Manager'da yüklenir. Çözüm ekledikten sonra bu yönetim paketleri de doğrudan bağlı Windows bilgisayarlarına yüklenir. Bu yönetim paketleri yönetme veya yapılandırma gerekmez.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

> [!NOTE]
> Geçersiz kılmak için bir çalışma alanıyla ilişkilendirilmesi için aracılar yönetim grubu düzeyinde yapılandırılan bir Operations Manager 1807 yönetim grubu varsa, bunları gösterilecek geçerli geçici olan **IsAutoRegistrationEnabled** için **True** içinde **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** kuralı.

Çözüm yönetim paketlerini nasıl güncelleştirileceğini hakkında daha fazla bilgi için bkz. [Azure İzleyici için Operations Manager'ı bağlama günlüklerini](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Operations Manager Aracısı ile sistemler için güncelleştirme yönetimi tarafından tam olarak yönetilmeye Aracısı Microsoft Monitoring Agent'olarak güncelleştirilmesi gerekir. Aracı güncelleştirme hakkında bilgi edinmek için bkz: [bir Operations Manager Aracısı yükseltme](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Operations Manager'ı kullanarak ortamlar için gerekli olan 14 ya da daha yeni System Center Operations Manager 2012 R2 UR çalıştırıyorsanız.

## <a name="onboard"></a>Güncelleştirme yönetimini etkinleştirme

Sistemlerine yama yapma başlamak için güncelleştirme yönetimi çözümünü etkinleştirmeniz gerekir. Güncelleştirme yönetimi için makine birçok yolu vardır. Aşağıdaki önerilen bağlıdır ve çözüm yollarını desteklenir:

* [Bir sanal makineden](automation-onboard-solutions-from-vm.md)
* [Birden çok makine gözatmanızı](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
* [Azure Otomasyonu runbook'u ile](automation-onboard-solutions.md)
  
### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Azure olmayan makineler eklenmedi olduğundan emin olun

Doğrudan bağlı makineleri birkaç dakika sonra Azure İzleyici günlüklerine ile iletişim kurduğunu onaylamak için bir aşağıdaki günlük aramaları çalıştırabilirsiniz.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Bir Windows bilgisayarda, Azure İzleyici günlüklerine ile aracı bağlantısını doğrulamak için aşağıdaki bilgileri gözden geçirebilirsiniz:

1. Denetim Masası'nda açın **Microsoft Monitoring Agent**. Üzerinde **Azure Log Analytics** sekmesinde aracı şu iletiyi görüntüler: **Log Analytics'e Microsoft Monitoring Agent başarıyla bağlantı kurdu**.
2. Windows olay günlüğünü açın. Git **uygulama ve hizmet günlükleri\operations Manager** ve olay kimliği 3000 ve kaynak Olay Kimliği 5002 arama **hizmet Bağlayıcısı**. Bu olaylar bilgisayarın Log Analytics çalışma alanına kaydolduğunu ve yapılandırmayı aldığını gösterir.

Aracı ile iletişim kuramıyorsa Azure İzleyici günlükleri ve aracı yapılandırılmış bir güvenlik duvarı veya Ara sunucu üzerinden internet ile iletişim kurmak için güvenlik duvarı onaylayın veya proxy sunucusunun düzgün yapılandırıldığından. Güvenlik Duvarı veya Ara sunucunun düzgün yapılandırıldığını doğrulama hakkında bilgi edinmek için [Windows aracısı için ağ yapılandırması](../azure-monitor/platform/agent-windows.md) veya [Linux aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Linux sistemleriniz bir ara sunucu ile iletişim kurmak için yapılandırılmış olan veya Log Analytics Gateway ve kullandığınız ekleme, bu çözüm, güncelleştirme *proxy.conf* izinlerini, omi kullanıcı grubuna vermek için okuma izni dosya çubuğunda kullanarak Aşağıdaki komutlar:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracılarında durumunu göster **güncelleştirilmiş** bir değerlendirme yapıldıktan sonra. Bu işlem 6 saat kadar sürebilir.

Bir Operations Manager yönetim grubu ile Azure İzleyici günlüklerine iletişim kurduğunu onaylamak için bkz: [Azure İzleyici günlüklerine doğrulamak Operations Manager entegrasyonunu](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve ardından gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Çözüm, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve ardından desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br/>Azure İzleyici günlüklerine Operations Manager Aracısı'ndan doğrudan bir bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API'si çağrılarak son güncelleştirme zamanı durumu değişip değişmediğini belirlemek için sorgulanamıyor. Durum değiştiyse, bir Uyumluluk taraması başlatılır.

Yönetilen her Linux bilgisayarı için 3 saatte bir tarama gerçekleştirilir.

30 dakika ve Panoda yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin görüntülenmesi için 6 saat arasında sürebilir.

Azure İzleyici günlüklerine veri kullanımı güncelleştirme Yönetimi'ni kullanarak bir makine için yaklaşık 25 MB / ay ortalamadır. Bu değer yalnızca yaklaşık ve ortamınıza bağlı değiştirilebilir. Ortamınızı sahip olduğunuz tam kullanımını görmek için izlemeniz önerilir.

## <a name="viewing-update-assessments"></a>Güncelleştirme değerlendirmelerini görüntüleme

Otomasyon hesabınızı seçin **güncelleştirme yönetimi** makinelerinizin durumunu görüntülemek için.

Bu görünüm, güncelleştirmeleri, güncelleştirme dağıtımları ve zamanlanan güncelleştirme dağıtımları eksik makineleriniz hakkında bilgi sağlar. İçinde **uyumluluk sütun**, makine değerlendirilen en son ne zaman görebilirsiniz. İçinde **güncelleştirme ARACISI hazırlığı** sütun varsa görebilirsiniz Güncelleştirme Aracısı sistem durumu. Bir sorun varsa, sorunu düzeltmek için hangi adımlar öğrenin sorun giderme için yardımcı olabilecek belgeleri gitmek için bağlantıyı seçin.

Makine, güncelleştirme veya dağıtım hakkında bilgi döndüren bir günlük araması çalıştırmak için öğeyi listeden seçin. **Günlük araması** seçilen öğe için bir sorgu ile bölmesini açar:

![Güncelleştirme yönetimi varsayılan görünümü](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Güncelleştirmeleri yükle

Çalışma alanınızdaki tüm Linux ve Windows bilgisayarlar için güncelleştirmeleri değerlendirdikten sonra oluşturarak gerekli güncelleştirmeleri yükleyebilirsiniz bir *güncelleştirme dağıtım*. Güncelleştirme dağıtımı bir veya daha fazla bilgisayar için gerekli güncelleştirmelerin zamanlanmış yüklemesidir. Tarih ve saat için dağıtım ve bilgisayar veya bilgisayar grubundaki bir dağıtım kapsamında içerecek şekilde belirt Bilgisayar grupları hakkında daha fazla bilgi için bkz: [bilgisayar grupları Azure İzleyici günlüklerine](../azure-monitor/platform/computer-groups.md).

 Güncelleştirme dağıtımınıza bilgisayar gruplarını eklediğinizde, grup üyeliği bir zamanlama oluşturma sırasında yalnızca bir kez değerlendirilir. Sonraki değişiklikler bir grup için geçerli değildir. Bu kullanım almak için [dinamik gruplar](#using-dynamic-groups), bu grupları dağıtım sırasında çözümlenir ve bir sorgu tarafından tanımlanır.

> [!NOTE]
> Varsayılan olarak Azure Market'ten dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için ayarlanır. Bu çözümü eklediğinizde veya Windows sanal makineleri çalışma alanınıza eklemek, bu davranış değişmez. Bu çözümü kullanarak güncelleştirmeleri etkin olarak yönetmiyorsanız, varsayılan davranış (güncelleştirmeleri otomatik olarak uygulama için) yöneliktir.

Güncelleştirmelerin ubuntu'daki bakım penceresinin dışında uygulanmasının önlemek için Otomatik Güncelleştirmeler devre dışı bırakmak için Katılımsız Yükseltme paketini yeniden yapılandırın. Paketin yapılandırma hakkında daha fazla bilgi için bkz: [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Azure Market'te kullanıma sunulan isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan sanal makineler kayıtlı erişimi [Red Hat Update Infrastructure'a (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) Azure'da dağıtılır. Diğer tüm Linux dağıtımları, dağıtım'ın desteklenen yöntemleri izleyerek dağıtım'ın çevrimiçi dosya deposundan güncelleştirilmelidir.

Yeni bir güncelleştirme dağıtımı oluşturmak için Seç **güncelleştirme dağıtımı zamanla**. **Yeni güncelleştirme dağıtımı** bölmesi açılır. Aşağıdaki tabloda açıklanan özellikler için değerleri girin ve ardından **Oluştur**:

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| Linux veya Windows|
| Grupları güncelleştir (Önizleme)|Abonelik, kaynak grupları, konumları ve etiketleri, dağıtımınızdaki dahil etmek için Azure vm'leri dinamik bir grup oluşturmak için bir birleşimini temel bir sorgu tanımlarsınız. Daha fazla bilgi için bkz: [dinamik gruplar](automation-update-management.md#using-dynamic-groups)|
| Güncelleştirilecek makineler |İçeri aktarılan grubu, kayıtlı bir aramayı seçin veya makine açılan listeden seçin ve tek bir makine seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure İzleyici günlüklerine bilgisayar grupları oluşturma farklı yöntemleri hakkında bilgi edinmek için bkz: [Azure İzleyici günlüklerine bilgisayar grupları](../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri Ekle/Dışla|Bu açılır **dahil edin/dışlayın** sayfası. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. Ekleme nasıl ele alındığını daha fazla bilgi için bkz: [ekleme davranışı](automation-update-management.md#inclusion-behavior) |
|Zamanlama ayarları|Her iki kez başlatın ve saati seçin veya yineleme için yineleme|
| Ön betiklerini + sonrası betikleri|Önce ve sonra dağıtımınız betiklerin seçin|
| Bakım penceresi |Güncelleştirmeler için dakika sayısı. Değeri 30 dakika ile en fazla 6 saat değerinden küçük olamaz |
| Denetim yeniden başlatma| Yeniden başlatma işlemlerini nasıl işleneceğini belirler. Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Güncelleştirme dağıtımları da bir program aracılığıyla oluşturulabilir. REST API ile bir güncelleştirme dağıtımı oluşturmak nasıl öğrenmek için bkz. [yazılım güncelleştirme yapılandırmaları - oluşturma](/rest/api/automation/softwareupdateconfigurations/create). Haftalık bir güncelleştirme dağıtımı oluşturmak için kullanılan bir örnek runbook yoktur. Bu runbook hakkında daha fazla bilgi için bkz: [bir kaynak grubundaki bir veya daha fazla sanal makineleri için haftalık bir güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

### <a name="multi-tenant"></a>Kiracılar arası güncelleştirme dağıtımları

Güncelleştirme yönetimi için düzeltme yapmanız raporlama başka bir Azure kiracısı makineleriniz varsa, zamanlanmış getirmek için aşağıdaki geçici çözümü kullanmanız gerekir. Kullanabilirsiniz [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) anahtarıyla cmdlet'i `-ForUpdate` bir zamanlama oluşturmak ve kullanmak için [yeni AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet'i ve geçirin diğer kiracıdaki makineler `-NonAzureComputer` parametresi. Aşağıdaki örnek bunu nasıl bir örnek gösterir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüle

Seçin **eksik güncelleştirmeler** makinelerinizden eksik olan güncelleştirmeler listesini görüntülemek için. Her güncelleştirme listelenir ve seçilebilir. Daha fazla bilgi için güncelleştirme, işletim sistemi ve bağlantı gerektiren makineler sayısı hakkında bilgiler gösterilir. **Günlük araması** güncelleştirmeleri hakkında daha fazla bilgi bölmesinde gösterilir.

## <a name="view-update-deployments"></a>Güncelleştirme dağıtımları görüntüle

Seçin **güncelleştirme dağıtımları** var olan güncelleştirme dağıtımlarının listesini görüntülemek için sekmesinde. Tabloda açmak için güncelleştirme dağıtımlarını seçin **güncelleştirme dağıtımı çalıştırın** bölmesinde söz konusu güncelleştirme dağıtımı için.

![Güncelleştirme dağıtımı sonuçlarına genel bakış](./media/automation-update-management/update-deployment-run.png)

Güncelleştirme dağıtımı REST API'sinden görüntülemek için bkz [yazılım güncelleştirme yapılandırması çalıştırmaları](/rest/api/automation/softwareupdateconfigurationruns).

## <a name="update-classifications"></a>Güncelleştirme sınıflandırmaları

Aşağıdaki tablolarda her sınıflandırma için bir tanım güncelleştirme sınıflandırmalarıyla güncelleştirme Yönetimi ' nde listelenmiştir.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Kritik, güvenlikle ilgili olmayan bir hatayı ele belirli bir sorun için bir güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir soruna yönelik bir güncelleştirme.        |
|Güncelleştirme paketleri     | Düzeltmeleri kolay dağıtım için bir arada paketlenmiş toplu bir dizi.        |
|Özellik paketleri     | Bir ürünün piyasaya sürümü dışında dağıtılan yeni ürün özellikleri.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan düzeltmeleri toplu kümesi.        |
|Tanım güncelleştirmeleri     | Virüs veya diğer tanım dosyalarına için güncelleştirme.        |
|Araçlar     | Yardımcı programı veya bir veya daha fazla görevleri tamamlama özelliği.        |
|Güncelleştirmeler     | Bir uygulama veya şu anda yüklü olan bir dosya için bir güncelleştirme.        |

### <a name="linux"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeler.         |
|Diğer güncelleştirmeler     | Doğası gereği kritik olmayan veya güvenlik güncelleştirmeleri olmayan tüm diğer güncelleştirmeleri.        |

Linux için güncelleştirme yönetimi kritik arasında ayrım yapabilme kolaylığı ve veri zenginleştirme nedeniyle değerlendirme verileri bulutta görüntüleme çalışırken bulutta güvenlik güncelleştirmeleri. Düzeltme eki uygulama için veri sınıflandırma makinede kullanılabilir güncelleştirme yönetimi kullanır. Diğer dağıtımları, CentOS yok Bu bilgilere kullanıma hazır. Aşağıdaki komutu için güvenlik verilerini döndürmek için bir yol yapılandırılmış CentOS makineleriniz varsa, güncelleştirme yönetimi üzerinde sınıflandırmaları Tabanlı Yama mümkün olacaktır.

```bash
sudo yum -q --security check-update
```

Şu anda yerel sınıflandırma veri kullanılabilirliğine CentOS etkinleştirmek için desteklenen yöntem yöntemi yok. Şu anda bu, kendi etkinleştirmiş olabilir müşteriler yalnızca en yüksek çaba destek sağlanır.

## <a name="firstparty-predownload"></a>Gelişmiş ayarlar

Güncelleştirme yönetimi, Windows Güncelleştirmeleri indirmek ve yüklemek için Windows Update'te kullanır. Sonuç olarak, Windows Update tarafından kullanılan ayarları birçoğu saygı gösteririz. Windows olmayan güncelleştirmeleri etkinleştirmek için Ayarlar'ı kullanıyorsanız, güncelleştirme yönetimi güncelleştirmeleri de yönetebilir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirme indiriliyor etkinleştirmek istiyorsanız, güncelleştirme dağıtımları hızlı gitmesi ve bakım penceresi aşan daha yüksektir.

### <a name="pre-download-updates"></a>Öncesi güncelleştirmeleri indir

Grup İlkesi'nde otomatik olarak karşıdan yüklenen güncelleştirmeleri yapılandırmak için ayarlayabileceğiniz [Otomatik Güncelleştirmeleri Yapılandır ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#BKMK_comp5) için **3**. Bu, arka planda gerekli güncelleştirmeleri indirir, ancak bunları yüklemez. Bu güncelleştirme yönetimi zamanlamaları denetiminde tutar ancak güncelleştirme yönetimi bakım penceresinin dışında karşıdan yükleyeceğiniz güncelleştirmeleri izin verir. Bu engelleyebilir **bakım penceresi aşıldı** güncelleştirme yönetimi hataları.

Ayrıca bu güncelleştirmeleri otomatik olarak indir istediğiniz sistemde aşağıdaki PowerShell PowerShell'le ayarlayabilirsiniz.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="disable-automatic-installation"></a>Otomatik Yükleme devre dışı bırak

Azure VM'ler, varsayılan olarak etkin olan güncelleştirmeleri otomatik olarak yüklenmesini sahiptir. Bu, güncelleştirme yönetimi tarafından yüklenmesini zamanlama önce yüklenecek güncelleştirmelerin neden olabilir. Ayarlayarak bu davranışı devre dışı bırakabilirsiniz `NoAutoUpdate` kayıt defteri anahtarına `1`. Aşağıdaki PowerShell kod parçacığı, bunu yapmanın bir yolu gösterir.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

### <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştirecek

Varsayılan olarak, Windows Update, güncelleştirmeleri yalnızca Windows için sağlar. Etkinleştirirseniz **sağla güncelleştirmeleri diğer Microsoft ürünleri için Windows güncelleştirebilirim olduğunda**, diğer ürünler için güncelleştirmelerin ile sağlanan güvenlik yamaları SQL Server veya diğer birinci taraf yazılımlar gibi. Bu seçenek, Grup İlkesi tarafından yapılandırılamaz. Aşağıdaki PowerShell, diğer birinci taraf düzeltme ekleri üzerinde etkinleştirmek istediğiniz sistemlerinde çalıştırın ve güncelleştirme yönetimi, bu ayar ne uygun olacaktır.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="third-party"></a> Windows üzerinde üçüncü taraf düzeltme ekleri

WSUS güncelleştirme yönetimi kullanır veya Windows sistemlerini Windows güncelleştirmesi düzeltme eki desteklenmiyor. Gibi araçları [System Center Updates Publisher](/sccm/sum/tools/updates-publisher
) (Updates Publisher) WSUS özel güncelleştirmeleri yayımlamanıza olanak sağlar. Bu senaryo, WSUS, üçüncü taraf yazılım güncelleştirme deposu olarak kullanma düzeltme eki makinelere güncelleştirme yönetimi sağlar. Updates Publisher yapılandırma konusunda bilgi için bkz: [yükleme Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="ports"></a>Ağ planlama

Aşağıdaki adresleri özellikle güncelleştirme yönetimi için gereklidir. Bu adresler için iletişim bağlantı noktası 443 üzerinden gerçekleşir.

|Azure kamu  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Karma Runbook çalışanı gereken bağlantı noktaları hakkında daha fazla bilgi için bkz: [karma çalışanı rolü bağlantı noktaları](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Özel durumlar tanımlarken listelenen adreslerini kullanmak için önerilir. IP adreslerinin indirebilirsiniz [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtır.

## <a name="search-logs"></a>Günlük ara

Azure portalında sağlanan Ayrıntılar ek olarak, günlükler karşı aramalar yapabilirsiniz. Çözüm sayfasında seçtiğiniz **Log Analytics**. **Günlük araması** bölmesi açılır.

Ayrıca sorguları özelleştirebilir veya farklı istemcilerin ve daha devam ederek kullanın öğrenebilirsiniz:  [Log Analytics arama API'si belgeleri](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki bölümlerde, bu çözüm tarafından toplanan güncelleştirme kayıtlarına için örnek günlük sorguları sağlar:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Tek Azure sanal makine değerlendirmesini sorgular (Windows)

VMUUID değeri sorguladığınız sanal makinenin VM GUID ile değiştirin. Azure İzleyici günlüklerine aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID bulabilirsiniz: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmelerin listesi

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Tek Azure VM değerlendirmesi sorgular (Linux)

Bazı Linux dağıtımları için var olan bir [endianness](https://en.wikipedia.org/wiki/Endianness) Azure Resource Manager ve Azure İzleyici günlüklerine depolanan geldiği VMUUID değerle eşleşmiyor. Aşağıdaki sorgu ya da endianness bir eşleşme olup olmadığını denetler. VMUUID değerleri büyük endian ve endian GUID'sinin biçimi doğru sonuçları döndürmek için ile değiştirin. Azure İzleyici günlüklerine aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID bulabilirsiniz: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmelerin listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Çoklu VM değerlendirmesi sorguları

##### <a name="computers-summary"></a>Bilgisayarların özeti

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="computers-list"></a>Bilgisayarlar listesi

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmelerin listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Dinamik gruplar (Önizleme) kullanma

Güncelleştirme yönetimi, Azure Vm'leri dinamik bir grup güncelleştirme dağıtımları için hedef olanağı sağlar. Bu grupları tanımlanan bir sorgu tarafından bir güncelleştirme dağıtımı başladığında, bu grubun üyelerinin değerlendirilir. Dinamik gruplar, Klasik Vm'leri ile çalışmaz. Sorgunuzu tanımlarken, aşağıdaki öğeler birlikte dinamik Grup doldurmak için kullanılabilir

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik bir Grup sonuçlarını önizlemek için tıklayın **Önizleme** düğmesi. Bu grup üyeliğini başlatıldığında, bu örnekte, biz arama için etiket makinelerle önizlemesini **rol** eşittir **BackendServer**. Daha fazla makine eklenen bu etiket varsa, bu gruba göre tüm gelecekteki dağıtımlar için eklenir.

![Önizleme grupları](./media/automation-update-management/preview-groups.png)

## <a name="integrate-with-system-center-configuration-manager"></a>System Center Configuration Manager ile tümleştirme

System Center Configuration Manager'da bilgisayarları, sunucuları ve mobil cihazları yönetmek için yatırım yapmış müşteriler aynı zamanda gücü ve yazılım güncelleştirmelerini yönetmenize yardımcı olmak için Configuration Manager'ın olgunluk yararlanmaktadır. Configuration Manager yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçasıdır.

Yönetim çözümü System Center Configuration Manager ile tümleştirmeyi öğrenin için bkz: [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>Ekleme davranışı

Güncelleştirme ekleme uygulamak için belirli güncelleştirmeleri belirtmenizi sağlar. Düzeltme ekleri veya dahil edilen paketler yüklenir. Düzeltme ekleri veya paketleri dahil edilir ve de sınıflandırması seçildiğinde dahil edilen öğeleri ve Sınıflandırmayı karşılayan öğeleri yüklenir.

Özel durumlar eklenen geçersiz kılma bilmek önemlidir. Örneği için bir çıkarma kuralı tanımlarsanız `*`, tüm hariç tutulan gibi hiçbir düzeltme ekleri veya paketler yüklü. Düzeltme ekleri makineden eksik olarak hala show dışlandı. Linux makineler için bir paket dahildir ancak dışarıda bırakıldı bağımlı paketi varsa paket yüklü değil.

## <a name="patch-linux-machines"></a>Düzeltme eki Linux makineleri

Aşağıdaki bölümler Linux düzeltme eki uygulama ile ilgili olası sorunları açıklar.

### <a name="unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltir.

Red Hat Enterprise Linux gibi bazı Linux çeşitleri üzerinde düzey işletim sistemi yükseltme paketleri ortaya çıkabilir. Bu işletim sistemi sürüm numarası nerede değişiklikleri güncelleştirme yönetimi çalışır neden olabilir. Güncelleştirme yönetimi, yönetici Linux bilgisayarı üzerinde yerel olarak kullanacağınız paketleri güncelleştirmek için aynı yöntemleri kullandığından bu davranış kasıtlıdır.

Güncelleştirme yönetimi çalışmalarını aracılığıyla işletim sistemi sürümünü güncelleştirme önlemek için **dışlama** özelliği.

Red Hat Enterprise Linux, redhat yayın server.x86_64 tutulacak paket adı.

![Linux için tutulacak paketlerin](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritik / güvenlik yamaları olmayan uygulanan

Bir Linux makineye güncelleştirmeleri dağıtırken, güncelleştirme sınıflandırmaları seçebilirsiniz. Bu, belirtilen ölçütleri karşılayan bir makineye uygulanan güncelleştirmeleri filtreler. Güncelleştirme dağıtıldığında bu filtre makine üzerinde yerel olarak uygulanır.

Güncelleştirme yönetimi, güncelleştirme zenginleştirme, bulutta gerçekleştirdiğinden, bu bilgileri yerel makinede yok olsa bile bazı güncelleştirmeler güncelleştirme Yönetimi'nde güvenlik, etkili olarak işaretlenebilir. Sonuç olarak, bir Linux makineye kritik güncelleştirmeler uygularsanız, olabilir güvenlik etkisi olması olarak makine ve güncelleştirmeleri uygulanmaz işaretlenmez güncelleştirmeleri.

Ancak, güncelleştirme yönetimi, ilgili güncelleştirme hakkında ek bilgi içerdiğinden olarak uyumlu söz konusu makine hala bildirebilir.

Güncelleştirme sınıflandırmasına göre güncelleştirmeler dağıtma CentOS üzerinde kullanıma hazır çalışmaz. Güncelleştirmeler için CentOS düzgün bir şekilde dağıtmak için güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmaları seçin. SUSE için seçme *yalnızca* sınıflandırma bazı güvenlik neden olabileceğinden ' diğer ' güvenlik güncelleştirmeleri, ayrıca yüklenen güncelleştirmeler zypper (Paket Yöneticisi) için ilgili veya bağımlılıklarından ilk gereklidir. Bu davranış, zypper kısıtlamasıdır. Bazı durumlarda, güncelleştirme dağıtımının yeniden çalıştırmanız gerekebilir. Doğrulamak için update günlüğünü denetleyin.

## <a name="remove-a-vm-for-update-management"></a>Bir VM için güncelleştirme yönetimini Kaldır

VM güncelleştirme yönetiminden kaldırmak için:

* Log Analytics çalışma alanınızda kayıtlı arama kapsamı yapılandırması için VM kaldırma `MicrosoftDefaultScopeConfig-Updates`. Kayıtlı aramalar, altında bulunabilir **genel** çalışma alanınızdaki.
* Kaldırma [Microsoft Monitoring agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmeleri yönetme konusunda bilgi almak için öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Windows Vm'leriniz için güncelleştirme ve yamaları yönetmenize](automation-tutorial-update-management.md)

* Te günlük aramalarını kullanın [Azure İzleyici günlükleri](../log-analytics/log-analytics-log-searches.md) ayrıntılı güncelleştirme verilerini görüntülemek için.
* [Uyarı oluşturma](automation-tutorial-update-management.md#configure-alerts) güncelleştirme dağıtım durumu için.

* Güncelleştirme yönetimi REST API aracılığıyla etkileşim öğrenmek için bkz. [yazılım güncelleştirme yapılandırmaları](/rest/api/automation/softwareupdateconfigurations)
* Güncelleştirme yönetimi sorunlarını giderme konusunda bilgi almak için bkz: [güncelleştirme yönetiminde sorun giderme](troubleshoot/update-management.md)
