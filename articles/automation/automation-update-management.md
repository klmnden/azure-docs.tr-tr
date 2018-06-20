---
title: Güncelleştirme yönetimi çözümü Azure
description: Bu makale, Azure güncelleştirme yönetimi çözümü, Windows ve Linux bilgisayarlar için güncelleştirmeleri yönetmek için nasıl kullanılacağını anlamanıza yardımcı olmak için tasarlanmıştır.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8013a221f818b2aca8163e5f5195cae17c8a6a95
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232555"
---
# <a name="update-management-solution-in-azure"></a>Güncelleştirme yönetimi çözümü Azure

Azure, şirket içi ortamları veya diğer bulut sağlayıcıları dağıtılan, Windows ve Linux bilgisayarlar için işletim sistemi güncelleştirmeleri yönetmek için Azure Otomasyonu'nda güncelleştirme yönetimi çözümü kullanabilirsiniz. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Azure Otomasyonu hesabınızdan doğrudan sanal makineler için güncelleştirme yönetimi etkinleştirebilirsiniz. Sanal makinelerden Automation hesabınız için güncelleştirme yönetimi etkinleştirme konusunda bilgi edinmek için [birden çok sanal makine için güncelleştirmeleri yönetme](manage-update-multi.md). Güncelleştirme yönetimi, Azure portalında sanal makine bölmesinden tek bir sanal makine için de etkinleştirebilirsiniz. Bu senaryo için kullanılabilir [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) sanal makineler.

## <a name="solution-overview"></a>Çözüme genel bakış

Güncelleştirme yönetimi tarafından yönetilen bilgisayarlara güncelleştirme dağıtımları ve değerlendirmesi gerçekleştirmek için aşağıdaki yapılandırmaları kullanın:

* Microsoft Windows veya Linux Aracısı'nı (MMA) izleme
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Microsoft Update veya Windows bilgisayarları için Windows Server Update Services (WSUS)

Aşağıdaki diyagramda davranışının kavramsal bir görünüm gösterir ve veri akışı nasıl çözüm değerlendirir ve tüm güvenlik güncelleştirmeleri uygular ile Windows Server ve Linux bilgisayarları bir çalışma alanında bağlı:

![Güncelleştirme yönetimi işlem akışı](media/automation-update-management/update-mgmt-updateworkflow.png)

Bir bilgisayarı güncelleştirme uyumluluğu için bir tarama yapar sonra aracıyı Azure günlük Analizi'ne toplu bilgi iletir. Bir Windows bilgisayarda Uyumluluk taraması, varsayılan olarak her 12 saatte gerçekleştirilir. 

Tarama zamanlaması ek olarak, güncelleştirme uyumluluğu için tarama MMA yeniden başlatılırsa 15 dakika içinde güncelleştirmeyi yüklemeden önce ve sonra güncelleştirme yüklemesi başlatılır. 

Bir Linux bilgisayar için Uyumluluk taraması 3 saatte varsayılan olarak gerçekleştirilir. Bir Uyumluluk taraması MMA Aracısı'nı yeniden başlatılırsa, 15 dakika içinde başlatılır.

Bilgisayar, kaynak üzerinde temel nasıl güncel raporları şekilde yapılandırılmış çözümü ile eşitleme. Windows bilgisayarı ne zaman WSUS en son Microsoft Update ile eşitlenen bağlı olarak, WSUS için bildirmek için yapılandırılmışsa, sonuçları ne Microsoft Updates gösterir farklı olabilir. Bu rapor için bir ortak depodaki yerine yerel bir depoya için yapılandırılmış Linux bilgisayarlar için aynıdır.

> [!NOTE]
> Doğru hizmete raporlamak üzere güncelleştirme yönetimi belirli URL'lerin ve bağlantı noktalarının etkin olmasını gerektirir. Bu gereksinimler hakkında daha fazla bilgi için bkz: [ağ karma çalışanları için planlama](automation-hybrid-runbook-worker.md#network-planning).

Zamanlanmış bir dağıtım oluşturarak, yazılım güncelleştirmelerinin gerekli olduğu bilgisayarlara güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. Güncelleştirmeleri sınıflandırılmış olarak *isteğe bağlı* Windows bilgisayarlar için dağıtım kapsamdaki dahil edilmez. Yalnızca gerekli güncelleştirmeleri dağıtım kapsamda dahil edilir. 

Zamanlanmış dağıtım hangi hedef bilgisayarlara uygulanabilir, bilgisayar açıkça belirtilmesi veya seçerek güncelleştirmelerle tanımlayan bir [bilgisayar grubu](../log-analytics/log-analytics-computer-groups.md) bilgisayarları belirli bir dizi günlük aramaları dayanır. Ayrıca onaylamak ve güncelleştirmeleri yüklenebilir süreyi belirlemek için bir zamanlama belirtin. 

Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook'lar görüntüleyemez ve runbook'ları herhangi bir yapılandırma gerektirmez. Bir güncelleştirme dağıtım oluşturulduğunda, güncelleştirme dağıtımı bulunan bilgisayarlar için belirtilen zamanda bir ana güncelleştirme runbook başlatır bir zamanlama oluşturur. Ana runbook alt runbook'un gerekli güncelleştirmeleri yüklemesini gerçekleştirmek için her Aracısı'nı başlatır.

Tarih ve saatte güncelleştirme dağıtımda belirtildi, hedef bilgisayarlara dağıtım paralel olarak yürütün. Yüklemeden önce güncelleştirmelerinin hala gerekli olduğunu doğrulamak için bir tarama gerçekleştirilir. WSUS istemci bilgisayarlar için WSUS'de onaylanmış olmayan güncelleştirmeler güncelleştirme dağıtımı başarısız olur.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, desteklenen işletim sistemlerinin bir listesini gösterir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Destekler, yalnızca değerlendirmeleri güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve sonraki sürümler     |.NET framework 4.5 veya üstü gereklidir. ([Karşıdan .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 veya üstü gereklidir. ([Karşıdan WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 daha fazla güvenilirlik için önerilir.  ([Karşıdan WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı düzeltme eki uygulama CentOS kutu dışı yok güvenlik verileri döndürmek için 'yum' gerektirir.         |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14.04 LTS ve 16.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen işletim sistemleri listelenmektedir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.        |
|Windows Server 2016 Nano Server     | Desteklenmiyor.       |

### <a name="client-requirements"></a>İstemci gereksinimleri

#### <a name="windows"></a>Windows

Windows aracılarının bir WSUS sunucusu ile iletişim kurmak için yapılandırılması gerekir veya Microsoft Update erişimi olması gerekir. System Center Configuration Manager ile güncelleştirme yönetimi kullanabilirsiniz. Tümleştirme senaryoları hakkında daha fazla bilgi için bkz: [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md#configuration). [Windows Aracısı](../log-analytics/log-analytics-agent-windows.md) gereklidir. Onboarding bir Azure sanal makinesi iseniz Aracısı otomatik olarak yüklenir.

#### <a name="linux"></a>Linux

Linux için makine için bir güncelleştirme deposu erişimi olması gerekir. Güncelleştirme deposu özel veya genel olabilir. Operations Management Suite (OMS) aracı birden çok günlük analizi çalışma raporu için yapılandırılmış Linux için bu çözüm ile desteklenmiyor.

Linux için OMS Aracısı'nı yüklemek ve en son sürümü karşıdan yüklemek için hakkında daha fazla bilgi için bkz: [Linux için Operations Management Suite Aracısı](https://github.com/microsoft/oms-agent-for-linux). Windows için OMS Aracısı yükleme hakkında daha fazla bilgi için bkz: [Operations Management Suite aracısının for Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>İzinler

Oluşturma ve güncelleştirme dağıtımları yönetmek için belirli izinler gerekir. Bu izinler hakkında bilgi edinmek için [rol tabanlı erişim - güncelleştirme yönetimi](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Çözüm bileşenleri

Çözüm, aşağıdaki kaynakları oluşur. Kaynaklar Automation hesabınız eklenir. Ya da doğrudan bağlı aracıları oldukları veya bir Operations Manager bağlı yönetim grubuna.

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözüm etkinleştirdikten sonra bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak günlük analizi çalışma alanına doğrudan bağlı herhangi bir Windows bilgisayar otomatik olarak yapılandırılır.

Çözümü tarafından yönetilen her Windows bilgisayarı listelenen **karma çalışan grupları** bölmesi olarak bir **sistem karma çalışanı grubu** Otomasyon hesabının. Çözümleri adlandırma kuralı kullanılmasını *ana bilgisayar adı FQDN_GUID*. Bu gruplar runbook'larla hesabınızda hedefleyemez. Denerseniz başarısız. Bu gruplar, yalnızca yönetim çözümü desteklemek üzere tasarlanmıştır.

Otomasyon hesabınızda çözümü ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanırsanız, Otomasyon runbook'ları desteklemek için Windows bilgisayarları bir karma Runbook çalışanı grubuna ekleyebilirsiniz. Bu işlevsellik sürüm karma Runbook çalışanı 7.2.12024.0 eklendi.

### <a name="management-packs"></a>Yönetim paketleri

System Center Operations Manager yönetim grubunuzu günlük analizi çalışma alanına bağlıysa, aşağıdaki yönetim paketleri Operations Manager'da yüklenir. Çözüm ekledikten sonra bu yönetim paketleri de doğrudan bağlı Windows bilgisayarlara yüklenir. Yapılandırın veya bu yönetim paketlerini yönetmenize gerek yoktur.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

Çözüm yönetim paketleri güncelleştirilme biçimini hakkında daha fazla bilgi için bkz: [Operations Manager'a günlük analizi](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Azure olmayan makinelere edildi olduğundan emin olun

Doğrudan bağlı makineler birkaç dakika sonra günlük analizi ile iletişim kuran onaylamak için bir aşağıdaki günlük aramaları çalıştırabilirsiniz.

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Bir Windows bilgisayarda günlük analizi ile Aracısı bağlanabilirliği doğrulamak için aşağıdaki bilgileri gözden geçirebilirsiniz:

1. Denetim Masası'nda açmak **Microsoft İzleme Aracısı**. Üzerinde **Azure günlük analizi** sekmesinde, aracıyı aşağıdaki ileti görüntülenir: **Microsoft Monitoring Agent için günlük analizi başarıyla bağlandı**.
2. Windows Olay Günlüğü'nü açın. Git **uygulama ve Hizmetleri Logs\Operations Yöneticisi** ve olay kimliği 3000 ve olay kimliği 5002 kaynağından arama **hizmet Bağlayıcısı**. Bu olaylar, bilgisayar günlük analizi çalışma alanı ile kayıtlı ve yapılandırmayı almayı gösterir.

Aracı günlük analizi ile iletişim kuramıyor ve aracı bir güvenlik duvarı veya proxy sunucu üzerinden internet ile iletişim kurmak için yapılandırılmışsa güvenlik duvarı veya proxy sunucusunun düzgün şekilde yapılandırıldığını onaylayın. Güvenlik Duvarı veya proxy sunucunun düzgün şekilde yapılandırıldığını doğrulamak öğrenmek için bkz: [Windows aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-windows.md) veya [Linux aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Linux sistemlerinizi Ara sunucusu ile iletişim kurmak için yapılandırılmış olan veya OMS ağ geçidi ve onboarding Bu çözüm, güncelleştirme *proxy.conf* omiuser grup izinleri okuma izni dosyasında aşağıdaki kullanarak komutlar:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracılarını durumunu göster **güncelleştirilmiş** bir değerlendirme gerçekleştirildikten sonra. Bu işlem 6 saat kadar sürebilir.

Bir Operations Manager yönetim grubu günlük analizi ile iletişim kurduğunu doğrulamak için bkz: [günlük analizi ile doğrula Operations Manager tümleştirmesini](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics).

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynakları açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm Windows aracılardan sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmeleri yüklemesini başlatır. |
| Linux aracıları |Evet |Çözüm Linux aracılarını sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımları gerekli güncelleştirmeleri yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br/>Günlük analizi Operations Manager aracısı arasında doğrudan bağlantı gerekli değildir. Veri yönetim grubu için günlük analizi çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Bir tarama her yönetilen Windows bilgisayarı için günde iki kez gerçekleştirilir. Windows API 15 dakikada adlı sorgulamak için durum değişip değişmediğini belirlemek son güncelleştirme zamanı. Bir uyumluluk tarama durumu değiştiyse, başlatılır. 

Her yönetilen Linux bilgisayar için bir tarama 3 saatte gerçekleştirilir.

Yönetilen bilgisayarların güncelleştirilmiş verileri görüntülemek Pano 6 saat arasındaki 30 dakika sürebilir.

## <a name="viewing-update-assessments"></a>Görünüm güncelleştirme değerlendirmeleri

Otomasyon hesabınızı seçin **güncelleştirme yönetimi** makinelerinizi durumunu görüntülemek için.

Bu görünüm makinelerinizi eksik güncelleştirmeleri, güncelleştirme dağıtımları ve zamanlanan güncelleştirme dağıtımları hakkında bilgi sağlar. İçinde **uyumluluk sütun**, makine uygunluk en son ne zaman görebilirsiniz. İçinde **güncelleştirme ARACISI hazırlık** sütun olmadığını görebilir Güncelleştirme Aracısı'nın sistem durumu. Bir sorun varsa, sorunu düzeltmek için uygulamanız için hangi adımları öğrenin sorun giderme yardımcı olabilecek belgeleri gitmek üzere bağlantıyı seçin.

Makine, güncelleştirme ya da dağıtım hakkındaki bilgileri döndürür günlük arama çalıştırmak için listede öğeyi seçin. **Günlük arama** bölmesi açılır seçilen öğe için bir sorgu ile:

![Güncelleştirme yönetimi varsayılan görüntüle](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Güncelleştirmeleri Yükle

Çalışma alanınızdaki tüm Linux ve Windows bilgisayarlar için güncelleştirmeleri uygunluk sonra oluşturarak gerekli güncelleştirmeleri yükleyebilirsiniz bir *güncelleştirme dağıtımı*. Bir veya daha fazla bilgisayar için gerekli güncelleştirmelerin zamanlanmış bir yükleme bir güncelleştirme dağıtımıdır. Tarih ve saat dağıtım ve bilgisayar veya bilgisayar grubu için bir dağıtım kapsamında eklemek için belirtin. Bilgisayar grupları hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te bilgisayar grupları](../log-analytics/log-analytics-computer-groups.md).

 Güncelleştirme dağıtımınızda bilgisayar grupları eklediğinizde, grup üyeliği zamanlama oluşturma aynı anda yalnızca bir kez değerlendirilir. Bir gruba sonraki değişiklikler yansıtılmıyor. Bu sorunu çözmek için zamanlanmış güncelleştirme dağıtımı silin ve yeniden oluşturun.

> [!NOTE]
> Azure Marketi'nde varsayılan olarak dağıtılan Windows sanal makineler, Windows Update hizmetinden otomatik güncelleştirmeleri almak için ayarlanır. Bu çözüme ekleyin veya Windows sanal makineler çalışma alanına eklediğinizde bu davranış değişmez. Bu çözümü kullanarak güncelleştirmeleri etkin olarak yönetmiyorsanız, (otomatik olarak güncelleştirmeleri uygulamak için) varsayılan davranış uygulanır.

Ubuntu üzerinde bir bakım penceresi dışında uygulanan güncelleştirmeleri önlemek için otomatik güncelleştirmeleri devre dışı bırakmak için Katılımsız Yükseltme paketi yeniden yapılandırın. Paket yapılandırma hakkında daha fazla bilgi için bkz: [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Azure Marketi'nde kullanılabilir İsteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerden oluşturulan sanal makineler kayıtlı erişimi [Red Hat güncelleştirme altyapısı (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) Azure'da dağıtılabilir. Dağıtım'ın desteklenen yöntemleri izleyerek, başka bir Linux dağıtım dağıtım'ın çevrimiçi dosya depodan güncelleştirilmesi gerekir.

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüle

Seçin **eksik güncelleştirmeler** , makinelerden eksik güncelleştirmeler listesini görüntülemek için. Her güncelleştirme listelenir ve seçilebilir. Daha fazla bilgi için güncelleştirme, işletim sistemi ve bağlantı gerektiren makineler sayısı hakkında bilgiler gösterilir. **Günlük arama** güncelleştirmeleri hakkında daha fazla bilgi bölmesinde gösterilir.

## <a name="view-update-deployments"></a>Güncelleştirme dağıtımları görüntüle

Seçin **güncelleştirme dağıtımları** var olan güncelleştirme dağıtımları listesini görüntülemek için. Güncelleştirme dağıtımlarının açmak için tabloda seçin **güncelleştirme dağıtımı çalıştırın** bölmesinde o güncelleştirme dağıtımı için.

![Güncelleştirme dağıtım sonuçları genel bakış](./media/automation-update-management/update-deployment-run.png)

## <a name="create-or-edit-an-update-deployment"></a>Oluşturma veya güncelleştirme dağıtımı düzenleme

Yeni bir güncelleştirme dağıtımı oluşturmak için seçin **zamanlama güncelleştirme dağıtımı**. **Yeni güncelleştirme dağıtımı** bölmesini açar. Aşağıdaki tabloda açıklanan özellikler için değerleri girin:

| Özellik | Açıklama |
| --- | --- |
|Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| Seçin **Linux** veya **Windows**.|
|Makineleri güncelleştirmek için |Kaydedilmiş bir aramayı seçin veya seçin **makine** aşağı açılan liste ve seçip tek tek makinelerden. |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin. CentOS desteklemiyor bu kutu dışı.|
|Hariç tutulacak güncelleştirmeler|Dışlanacak güncelleştirmeleri girin. Windows için KB makalesi olmadan girin **KB** öneki. Linux için paket adını girin veya bir joker karakter kullanın.  |
|Zamanlama ayarları|Başlangıç saati seçin ve ardından ya da **kez** veya **yinelenen** yineleme için.|| Bakım penceresi |Güncelleştirmeler için belirlediğiniz dakika sayısı. Değeri 30 dakika ya da 6 saatten daha az olamaz. |

## <a name="update-classifications"></a>Güncelleştirme sınıflandırmaları

Aşağıdaki tablolarda her sınıflandırması için bir tanım güncelleştirmesi Yönetimi ' nde güncelleştirme sınıflandırmalarıyla listeleyin.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Kritik, güvenlikle ilgili olmayan bir hatayı gidermeye belirli bir sorun için bir güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir sorun için bir güncelleştirme.        |
|Güncelleştirme paketleri     | Bir toplu setini kolay dağıtım için bir arada paketlenmiş.        |
|Özellik paketleri     | Bir ürünün sürümü dışında dağıtılan yeni ürün özelliklerini.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan düzeltmeleri toplu kümesi.        |
|Tanım güncelleştirmeleri     | Bir güncelleştirme virüs veya diğer tanım dosyalarına.        |
|Araçlar     | Bir yardımcı programı veya tam bir veya daha fazla görev özelliği.        |
|Güncelleştirmeler     | Bir uygulama veya dosya şu anda bir güncelleştirme yüklendi.        |

### <a name="linux"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeleri.         |
|Diğer güncelleştirmeler     | Doğası gereği kritik olmayan veya güvenlik güncelleştirmeleri olmayan tüm diğer güncelleştirmeleri.        |

Linux için güncelleştirme yönetimi arasındaki kritik ayırt edebilir ve güvenlik güncelleştirmelerini veri iyileştirmesini nedeniyle değerlendirme verileri bulutta görüntüleme sırasında bulutta. Düzeltme eki uygulama için veri sınıflandırma kullanılabilir güncelleştirme yönetimi kullanır. Diğer dağıtımlar, CentOS yok Bu bilgilere kullanıma hazır. Aşağıdaki komutu için güvenlik verileri döndürmek için bir yol yapılandırılmış CentOS makineler varsa, güncelleştirme yönetimi sınıflandırmaları üzerinde temel düzeltme eki yapabilirsiniz.

```bash
sudo yum -q --security check-update
```

Şu anda CentOS yerel sınıflandırma veri kullanılabilirliği etkinleştirmek için desteklenen yöntem yöntemi yoktur. Şu anda bu, kendi etkinleştirmiş olabilir müşteriler için yalnızca en yüksek çaba destek sağlanır.

## <a name="ports"></a>Bağlantı Noktaları

Aşağıdaki adresleri özellikle güncelleştirme yönetimi için gereklidir. Bu adresler için iletişim bağlantı noktası 443 üzerinden gerçekleşir.

|Azure genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

Karma Runbook çalışanı gereken bağlantı noktaları hakkında daha fazla bilgi için bkz: [karma çalışanı rolü bağlantı noktalarını](automation-hybrid-runbook-worker.md#hybrid-worker-role).

## <a name="search-logs"></a>Arama günlükleri

Azure Portalı'nda sağlanan Ayrıntılar yanı sıra, günlüklere karşı arama yapabilirsiniz. İçinde **değişiklik izleme** bölmesinde, **günlük analizi**. **Günlük arama** bölmesini açar.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tabloda bu çözüm tarafından toplanan güncelleştirme kayıtları için örnek günlük aramaları sağlar:

| Sorgu | Açıklama |
| --- | --- |
|Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;Bilgisayar, başlık, KBID, Sınıflandırma, PublishedDate proje |Eksik güncelleştirmeleri olan tüm bilgisayarlar</br>İşletim sistemi sınırlamak için aşağıdakilerden birini ekleyin:</br>OSType! "Linux" =</br>OSType "Linux" == |
| Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;Burada bilgisayar "ContosoVM1.contoso.com" ==</br>&#124;Bilgisayar, başlık, KBID, ürün, PublishedDate proje |Belirli bir bilgisayarda eksik güncelleştirmeler (değeri kendi bilgisayarınızın adıyla değiştirin)|
| Olay</br>&#124;Burada EventLevelName "error" ve bilgisayar == ((güncelleştirme &#124; where (sınıflandırma "Güvenlik güncelleştirmeleri" veya sınıflandırma == "Kritik güncelleştirmeler" ==)</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false </br>&#124;farklı bilgisayar)) |Kritik güncelleştirmeleri veya gerekli güvenlik güncelleştirmeleri eksik olan makineler için hata olayları |
| Güncelleştirme</br>&#124;Burada UpdateState "Gerekli" ve isteğe bağlı == == false</br>&#124;ayrı başlığı |Tüm bilgisayarlardaki ayrı eksik güncelleştirmeler |
| UpdateRunProgress</br>&#124;Burada InstallationStatus "başarısız" == </br>&#124;AggregatedValue özetlemek bilgisayar, başlık, UpdateRunName tarafından count() = |Bir güncelleştirme çalışması başarısız güncelleştirmelere sahip bilgisayarlar</br>İşletim sistemi sınırlamak için aşağıdakilerden birini ekleyin:</br>OSType = "Windows"</br>OSType "Linux" == |
| Güncelleştirme</br>&#124;Burada OSType "Linux" ==</br>&#124;Burada UpdateState! "Gerekli" = ve (sınıflandırma "Kritik güncelleştirmeler" veya sınıflandırma == "Güvenlik güncelleştirmeleri" ==)</br>&#124;AggregatedValue özetlemek bilgisayar tarafından count() = |Paket güncelleştirme kullanılabilir olan tüm Linux makineler, kritik güncelleştirmeler veya güvenlik açığına listesi |
| UpdateRunProgress</br>&#124;Burada UpdateRunName "DeploymentName" ==</br>&#124;AggregatedValue özetlemek bilgisayar tarafından count() =|Bu güncelleştirme çalıştırmasında güncelleştirilmiş olan bilgisayarlar (değeri kendi Güncelleştirme Dağıtımı adınızla değiştirin) |

## <a name="integrate-with-system-center-configuration-manager"></a>System Center Configuration Manager ile tümleştirme

Bilgisayarların, sunucuların ve mobil cihazları yönetmek için System Center Configuration Manager'da yatırım yaptıysanız müşterileri de gücü ve olgunluk yazılım güncelleştirmelerini yönetmenize yardımcı olmak için Configuration Manager'ın kullanır. Configuration Manager, yazılım güncelleştirme yönetimi (SUM) döngüsü bir parçasıdır.

Yönetim çözümü System Center Configuration Manager ile tümleştirmek öğrenmek için bkz: [güncelleştirme yönetimi ile System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patch-linux-machines"></a>Düzeltme eki Linux makineler

Aşağıdaki bölümlerde Linux düzeltme eki uygulama ile ilgili olası sorunları açıklanmaktadır.

### <a name="unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltir

Red Hat Enterprise Linux gibi bazı Linux çeşitleri üzerinde düzey işletim sistemi yükseltme paketleri ortaya çıkabilir. Bu güncelleştirme yönetimi çalıştırmaları için burada işletim sistemi sürüm numarasını değiştirir neden olabilir. Güncelleştirme yönetimi, yönetici Linux bilgisayarda yerel olarak kullanacağınız paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu kasıtlı bir davranıştır.

Güncelleştirme yönetimi çalışmalarını aracılığıyla işletim sistemi sürümü güncelleştirme önlemek için **dışlama** özelliği.

Red Hat Enterprise Linux dışlamak için paket redhat sürüm server.x86_64 adıdır.

![Linux için dışlamak için paketler](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritik / olmayan güvenlik yamaları uygulanan

Bir Linux makineye güncelleştirmeleri dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Belirtilen ölçütlere uyan o uygulanan güncelleştirmeleri filtreler. Güncelleştirme dağıtıldığında bu filtre makinede yerel olarak uygulanır.

Güncelleştirme yönetimi bulutta güncelleştirme iyileştirmesini gerçekleştirdiğinden, bu bilgileri yerel makinede yok olsa bile bazı güncelleştirmeler güncelleştirme Yönetimi'nde güvenlik etkisine sahip olarak işaretlenmesini. Sonuç olarak, kritik güncelleştirmeler için Linux makine uygularsanız, olabilir güvenlik etkisine sahip olarak makine ve güncelleştirmeleri uygulanmamış işaretlenmez güncelleştirmeler.

Ancak, güncelleştirme yönetimi hala ilgili güncelleştirme hakkında ek bilgi sahip olduğundan uyumlu olacak şekilde bu makine bildirebilir.

Güncelleştirme sınıflandırması güncelleştirmelerini dağıtma üzerinde CentOS kutu dışı çalışmaz. SUSE için seçme *yalnızca* sınıflandırma güvenliğe neden olabileceğinden ' diğer ' güvenlik güncelleştirmeleri, ayrıca yüklenen güncelleştirmeler zypper (Paket Yöneticisi) ilgili veya bağımlılıklarını ilk gereklidir. Bu, zypper kısıtlamasıdır. Bazı durumlarda, doğrulamak için Güncelleştirme dağıtımı yeniden çalıştırmak için gerekli olabilecek güncelleştirme günlüğü'nü denetleyin.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, güncelleştirme yönetimi çözümle ilgili sorunları gidermenize yardımcı olacak bilgiler sağlar.

### <a name="windows"></a>Windows

Sizin için yerleşik çözümü veya bir sanal makine çalıştığınızda sorunlarla karşılaşırsanız, denetleme **uygulama ve Hizmetleri Logs\Operations Yöneticisi** yerel makineye olay kimliği 4502 ve olay olayları için olay günlüğü iletileri içeren **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**. Aşağıdaki tabloda özel hata iletileri ve olası bir çözüm her biri için önemli noktalar:

| İleti | Neden | Çözüm |
|----------|----------|----------|
| Yama Yönetimi için Makine Kaydedilemiyor,<br/>Kayıt Özel Durumla Başarısız Oldu<br/>System.InvalidOperationException: {"Message":"Makine zaten<br/>farklı bir hesaba kaydedildi. "} | Güncelleştirme yönetimi için başka bir çalışma sayede makine zaten var. | Tarafından eski yapılarının temizlenmesini [karma Runbook grubu silme](automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group).|
| Düzeltme Eki Yönetimi için makine kaydedilemedi, kayıt özel durumu ile başarısız<br/>System.Net.Http.HttpRequestException: İstek gönderilirken bir hata oluştu. ---><br/>System.Net.WebException: Temel alınan bağlantı<br/>kapatıldı: Alma işlemi sırasında<br/>beklenmeyen bir hata oluştu. ---> System.ComponentModel.Win32Exception:<br/>İstemci ve sunucu iletişim kuramıyor,<br/>çünkü ortak bir algoritmaya sahip değiller | Proxy/ağ geçidi/güvenlik duvarı iletişimi engelliyor. | [Ağ gereksinimlerini gözden geçirme](automation-hybrid-runbook-worker.md#network-planning).|
| Yama Yönetimi için Makine Kaydedilemiyor,<br/>Kayıt Özel Durumla Başarısız Oldu<br/>Newtonsoft.Json.JsonReaderException: Pozitif sonsuz değer ayrıştırılırken hata oluştu. | Proxy/ağ geçidi/güvenlik duvarı iletişimi engelliyor. | [Ağ gereksinimlerini gözden geçirme](automation-hybrid-runbook-worker.md#network-planning).|
| Hizmet tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com<br/>Microsoft hizmetleri için kullanılan bir sertifika yetkilisi<br/>tarafından verilmemiş. İletişim<br/>ağ yöneticinize başvurarak<br/>TLS/SSL iletişimini engelleyen bir proxy çalıştırıp çalıştırmadıklarına bakın. |Proxy/ağ geçidi/güvenlik duvarı iletişimi engelliyor. | [Ağ gereksinimlerini gözden geçirme](automation-hybrid-runbook-worker.md#network-planning).|
| Yama Yönetimi için Makine Kaydedilemiyor,<br/>Kayıt Özel Durumla Başarısız Oldu<br/>AgentService.HybridRegistration.<br/>PowerShell.Certificates.CertificateCreationException:<br/>Otomatik olarak imzalanan sertifika oluşturulamadı. ---><br/>System.UnauthorizedAccessException: Erişim reddedildi. | Otomatik olarak imzalanan sertifika oluşturma hatası. | Sistem hesabının<br/>klasöre okuma erişiminin olduğunu doğrulayın:<br/>**C:\ProgramData\Microsoft\**<br/>** Crypto\RSA **|

### <a name="linux"></a>Linux

Linux makinesinde başlatmak güncelleştirme çalıştırması başarısız olursa, aşağıdaki günlük dosyası bir kopyasını alın ve sorun giderme amacıyla Koru:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

Linux üzerinde başarıyla başlatıldıktan sonra Çalıştır'ün güncelleştirilmesi sırasında hatalar meydana gelirse, etkilenen makinenin çalıştırmada çıktısı işini denetleyin. Araştırma ve eylem gerçekleştiren makinenizin Paket Yöneticisi'nden özel hata iletileri bulabilirsiniz. Güncelleştirme yönetimi, Paket Yöneticisi'ni başarılı güncelleştirme dağıtımları için sağlıklı olmasını gerektirir.

Bazı durumlarda, güncelleştirme bir güncelleştirme dağıtımı tamamlanmasını engelleyen yönetimiyle paket güncelleştirmesi etkileyebilir. Görürseniz, bu paketleri gelecekteki güncelleştirme dosyadan hariç veya bunları el ile yüklemeniz gerekir kendiniz.

Bir düzeltme eki uygulama sorunu çözemezseniz, aşağıdaki günlük dosyasının bir kopyası ve onu korumak **önce** sorun giderme amacıyla sonraki güncelleştirme dağıtımı başlatır:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Sonraki adımlar

Öğretici, Windows sanal makineleriniz için güncelleştirmeleri yönetme konusunda bilgi almak için devam edin.

> [!div class="nextstepaction"]
> [Güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetme](automation-tutorial-update-management.md)

* Günlük aramalarda kullanması [günlük analizi](../log-analytics/log-analytics-log-searches.md) ayrıntılı güncelleştirme verilerini görüntülemek için.
* [Uyarı oluşturma](../log-analytics/log-analytics-alerts.md) kritik güncelleştirmeler bilgisayarlardan eksik olarak algılandığında veya bir bilgisayarda otomatik güncelleştirmeleri devre dışı olduğunda.
