---
title: "OMS’de Güncelleştirme Yönetimi çözümü | Microsoft Belgeleri"
description: "Bu makale, Windows ve Linux bilgisayarlarınızın güncelleştirmelerini yönetmek için bu çözümün nasıl kullanılacağına yönelik bilgiler verir."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: 668065933745168c88a1f4bf755f1adc0cc31d7f
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="update-management-solution-in-oms"></a>OMS’de Güncelleştirme Yönetimi çözümü

![Güncelleştirme Yönetimi sembolü](./media/oms-solution-update-management/update-management-symbol.png)

OMS’deki Güncelleştirme Yönetimi çözümü, Azure’da, şirket içi ortamlarda veya diğer bulut sağlayıcılarında dağıtılmış Windows ve Linux bilgisayarlarınızın işletim sistemi güvenlik güncelleştirmelerini yönetmenize olanak sağlar.  Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

## <a name="update-management-in-azure-automation"></a>Azure Otomasyonu'nda güncelleştirme yönetimi

[Azure Otomasyonu](../automation/automation-offering-get-started.md) hesabınızdan sanal makineleriniz için Güncelleştirme yönetimini etkinleştirebilirsiniz.
Otomasyon hesabınızdan sanal makineler için güncelleştirme yönetimini nasıl etkinleştireceğinizi öğrenmek için bkz. [Birden fazla sanal makine için güncelleştirmeleri yönetme](../automation/manage-update-multi.md).


## <a name="solution-overview"></a>Çözüme genel bakış
OMS tarafından yönetilen bilgisayarlar değerlendirme ve güncelleştirme dağıtımları yapmak için aşağıdakileri kullanır:

* Windows veya Linux için OMS aracısı
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Windows bilgisayarları için Microsoft Update veya Windows Server Update Hizmetleri

Aşağıdaki diyagramlarda, çözümün çalışma alanındaki tüm bağlı Windows Server ve Linux bilgisayarlarını nasıl değerlendirdiğini ve bu bilgisayarlara güvenlik güncelleştirmeleri nasıl uyguladığını ortaya koyan, davranış ve veri akışının kavramsal bir görünümü gösterilir.    

#### <a name="windows-server"></a>Windows Server
![Windows Server güncelleştirme yönetimi işlem akışı](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Linux güncelleştirme yönetimi işlem akışı](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Bilgisayar güncelleştirme uyumluluğu için bir tarama yaptıktan sonra, OMS aracısı bilgileri toplu olarak OMS’ye iletir. Window bilgisayarında, uyumluluk taraması varsayılan olarak her 12 saatte bir gerçekleştirilir.  Tarama zamanlamasına ek olarak, Microsoft Monitoring Agent’ın (MMA) yeniden başlatılması durumunda, güncelleştirme yüklemesi öncesinde ve güncelleştirme yüklemesi sonrasında 15 dakika içinde güncelleştirme uyumluluğu için tarama başlatılır.  Linux bilgisayarıyla, uyumluluk taraması varsayılan olarak her 3 saatte bir gerçekleştirilir ve MMA aracısının yeniden başlatılması durumunda 15 dakika içinde uyumluluk taraması başlatılır.  

Ardından uyumluluk bilgileri çözüme eklenmiş olan panolarda işlenir ve özetlenir ya da kullanıcı tanımlı veya önceden tanımlanmış sorgular kullanılarak aranabilir.  Çözüm, eşitlenmek üzere yapılandırdığınız kaynağa dayanarak bilgisayarınızın ne düzeyde güncel olduğunu raporlar.  Windows bilgisayarı WSUS’a raporlayacak şekilde yapılandırıldıysa, WSUS’un Microsoft Update ile en son ne zaman eşitlendiğine bağlı olarak, sonuçlar Microsoft Update tarafından gösterilenlerden farklı olabilir.  Genel depo yerine bir yerel depoya raporlayacak şekilde yapılandırılan Linux bilgisayarları için de aynı durum geçerlidir.   

Zamanlanmış bir dağıtım oluşturarak, yazılım güncelleştirmelerinin gerekli olduğu bilgisayarlara güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz.  *İsteğe Bağlı* olarak sınıflandırılmış güncelleştirmeler Windows bilgisayarları için dağıtım kapsamına alınmaz; yalnızca gerekli güncelleştirmeler alınır.  Zamanlanmış dağıtım, bilgisayarları açıkça belirterek veya belirli bir bilgisayar kümesinin günlük aramaları temelinde bir [bilgisayar grubu](../log-analytics/log-analytics-computer-groups.md) seçerek, uygun güncelleştirmeleri hangi hedef bilgisayarların alacağını tanımlar.  Ayrıca, güncelleştirmelerin yüklenmesine izin verilen zaman aralığını onaylamak ve belirlemek için bir zamanlama da belirtebilirsiniz.  Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir.  Bu runbook’ları görüntüleyemezsiniz ve bunlar için herhangi bir yapılandırma gerekmez.  Bir Güncelleştirme Dağıtımı oluşturulduğunda, dahil edilen bilgisayarlar için belirtilen zamanda ana güncelleştirme runbook’unu başlatan bir zamanlama oluşturur.  Bu ana runbook, gerekli güncelleştirmelerin yüklemesini gerçekleştiren her aracıda bir alt runbook başlatır.       

Güncelleştirme dağıtımında belirtilen tarih ve saatte, hedef bilgisayarlar dağıtımı paralel olarak yürütür.  İlk olarak güncelleştirmelerin hala gerekli olduğunu doğrulamak için bir tarama yapılır ve bunlar yüklenir.  WSUS istemci bilgisayarları için, güncelleştirmelerin WSUS’ta onaylanmaması durumunda güncelleştirme dağıtımının başarısız olacağı unutulmamalıdır.  Uygulanan güncelleştirmelerin sonuçları, panolarda veya olayları arama yoluyla işlenmek ve özetlenmek üzere OMS’ye iletilir.     

## <a name="prerequisites"></a>Ön koşullar
* Çözüm, Windows Server 2008 ve sonraki sürümlerine yönelik güncelleştirme değerlendirmelerinin yanı sıra Windows Server 2008 R2 SP1 ve sonraki sürümlerine yönelik güncelleştirme dağıtımları gerçekleştirilmesini destekler.  Nano Sunucu desteklenmiyor.

    > [!NOTE]
    > Windows Server 2008 R2 SP1'e yönelik güncelleştirme dağıtımı desteği için .NET Framework 4.5 ve WMF 5.0 veya sonraki bir sürümü gerekir.
    >  
* Windows istemci işletim sistemleri desteklenmez.  
* Windows aracıları Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.  

    > [!NOTE]
    > Windows aracısı System Center Configuration Manager tarafından eşzamanlı olarak yönetilemez.  
    >
* CentOS 6 (x86/x64) ve 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) ve 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)  
* Ubuntu 12.04 LTS ve daha yeni x86/x64   
    > [!NOTE]  
    > Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Bahsedilen yapılandırma işlemiyle ilgili bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.  

    > [!NOTE]
    > Birden çok OMS çalışma alanına raporlayacak şekilde yapılandırılmış bir Linux için OMS Aracısı, bu çözümde desteklenmez.  
    >

Linux için OMS Aracısı’nı yükleme ve en son sürümü indirme hakkında ek bilgi için, [Linux için Operations Management Suite Aracısı](https://github.com/microsoft/oms-agent-for-linux) konusuna bakın.  Windows için OMS Aracısı’nı yükleme hakkında bilgi için, [Windows için Operations Management Suite Aracısı](../log-analytics/log-analytics-windows-agents.md) konusunu gözden geçirin.  

### <a name="permissions"></a>İzinler
Güncelleştirme dağıtımları oluşturmak için hem Otomasyon hesabınızda hem de Log Analytics çalışma alanınızda size katkıda bulunan rolü verilmiş olmalıdır.  

## <a name="solution-components"></a>Çözüm bileşenleri
Bu çözüm, Otomasyon hesabınıza eklenen aşağıdaki kaynaklardan ve doğrudan bağlanılan aracılardan veya Operations Manager bağlantılı yönetim grubundan oluşur.

### <a name="management-packs"></a>Yönetim paketleri
System Center Operations Manager yönetim grubunuz OMS çalışma alanına bağlıysa, Operation Manager’a aşağıdaki yönetim paketleri yüklenir.  Bu çözüm eklendikten sonra bu yönetim paketleri doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerinde yapılandırılacak veya yönetilecek hiçbir şey yoktur.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları
Bu çözümü etkinleştirdikten sonra çözümde yer alan runbook'ların desteklenmesi için OMS çalışma alanınıza doğrudan bağlı tüm Windows bilgisayarları otomatik olarak bir Karma Runbook Çalışanı olarak yapılandırılır.  Çözüm tarafından yönetilen her Windows bilgisayarı için, Otomasyon hesabının Karma Runbook Çalışan Grupları dikey penceresinde, *Hostname FQDN_GUID* adlandırma kuralının ardından listelenir.  Hesabınızdaki runbook’larla bu grupları hedefleyemezsiniz, aksi takdirde başarısız olur. Bu gruplar yalnızca yönetim çözümünü desteklemeye yöneliktir.   

Bununla birlikte, Çözüm ve Karma Runbook Çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon gruplarını desteklemek için Windows bilgisayarlarını Otomasyon hesabınızdaki bir Karma Runbook Çalışanı grubuna ekleyebilirsiniz.  Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.  

## <a name="configuration"></a>Yapılandırma
Güncelleştirme Yönetimi çözümünü OMS çalışma alanınıza eklemek ve aracıların raporladığını doğrulamak için aşağıdaki adımları uygulayın. Çalışma alanınıza zaten bağlı olan Windows aracıları ek bir yapılandırma olmadan otomatik olarak eklenir.

Aşağıdaki yöntemleri kullanarak çözümü dağıtabilirsiniz:

* Azure portalında Azure Market’ten Otomasyon ve Denetim teklifini veya Güncelleştirme Yönetimi çözümünü seçerek
* OMS çalışma alanınızdaki OMS Çözüm Galerisi’nden

Zaten aynı kaynak grubunda ve bölgede birbiriyle bağlantılı bir Otomasyon hesabınız ve OMS çalışma alanınız varsa, Otomasyon ve Denetim’in seçilmesi yapılandırmanızı doğrular ve yalnızca çözümü yükleyip her iki hizmette de yapılandırır.  Azure Market’ten Güncelleştirme Yönetimi çözümünün seçilmesi de aynı davranışa yol açar.  Aboneliğinizde iki hizmet de dağıtılmamışsa, **Yeni Çözüm Oluştur** dikey penceresindeki adımları izleyin ve önceden seçilmiş diğer önerilen çözümleri yüklemek istediğinizi onaylayın.  İsteğe bağlı olarak, Çözüm Galerisi’ndeki [OMS çözümü ekleme](../log-analytics/log-analytics-add-solutions.md) bölümünde açıklanan adımlarla Güncelleştirme Yönetimi çözümünü OMS çalışma alanına ekleyin.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>OMS aracılarını ve OMS’ye bağlı Operations Manager yönetim grubunu doğrulama

Doğrudan bağlı Linux ve Windows için OMS Aracı’nın OMS ile iletişim kurduğunu doğrulamak için, birkaç dakika sonra aşağıdaki günlük aramasını çalıştırabilirsiniz:

* Linux - `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows - `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Windows bilgisayarında, OMS ile aracı bağlantısını doğrulamak için aşağıdakileri gözden geçirebilirsiniz:

1.  Denetim Masası’nda Microsoft Monitoring Agent’i açın; **Azure Log Analytics (OMS)** sekmesinde aracı şöyle bir ileti görüntüler: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı**.   
2.  Windows Olay Günlüğü’nü açın, **Uygulama ve Hizmet Günlükleri\Operations Manager** bölümüne gidin ve kaynak Hizmet Bağlayıcısı’nda Olay Kimliği 3000 ve 5002’yi arayın.  Bu olaylar bilgisayarın OMS çalışma alanına kaydolduğunu ve yapılandırmayı aldığını gösterir.  

Aracı, OMS hizmetiyle iletişim kuramıyorsa ve İnternet ile güvenlik duvarı veya ara sunucu üzerinden iletişim kuracak şekilde yapılandırıldıysa [Windows aracısı için ağ yapılandırması](../log-analytics/log-analytics-windows-agents.md#network) ya da [Linux aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md#network) konu başlığını gözden geçirerek güvenlik duvarının ve ara sunucunun düzgün yapılandırıldığını doğrulayın.

> [!NOTE]
> Linux sistemleriniz bir ara sunucu veya OMS Ağ Geçidi ile iletişim kuracak şekilde yapılandırıldıysa ve bu çözümü ekliyorsanız lütfen şu komutları kullanarak *proxy.conf* izinlerini, omi kullanıcı grubuna dosyada okuma izni verilecek şekilde güncelleştirin:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`


Yeni eklenen Linux aracılarında, değerlendirme yapıldıktan sonra **Güncelleştirildi** durumu gösterilir.  Bu işlem 6 saat kadar sürebilir.

Operations Manager yönetim grubunun OMS’yle iletişim kurduğunu onaylamak için bkz. [OMS ile Operations Manager Tümleştirmesini Doğrulama](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Çözüm, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br>Operations Manager aracısının doğrudan Log Analytics’e bağlanması gerekmez. Veriler yönetim grubundan OMS deposuna iletilir. |
| Azure depolama hesabı |Hayır |Azure Storage, sistem güncelleştirmeleri hakkında bilgi içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı
Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API’si çağrılarak son güncelleştirme zamanı sorgulanır; böylelikle durumun değişip değişmediği saptanır ve değişmişse bir uyumluluk taraması başlatılır.  Yönetilen her Linux bilgisayarı için 3 saatte bir tarama gerçekleştirilir.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında bir zaman alabilir.   

## <a name="using-the-solution"></a>Çözümü kullanma
Güncelleştirme Yönetimi çözümünü OMS çalışma alanınıza eklediğinizde OMS panonuza **Güncelleştirme Yönetimi** kutucuğu eklenir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Güncelleştirme Yönetimi Özet Kutucuğu](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Güncelleştirme değerlendirmelerini görüntüleme
**Güncelleştirme Yönetimi** kutucuğuna tıklayarak **Güncelleştirme Yönetimi** panosunu açın.<br><br> ![Güncelleştirme Yönetimi Özet Panosu](./media/oms-solution-update-management/update-management-dashboard.png)<br>

Bu pano, güncelleştirme durumunun işletim sistemi türüne ve güncelleştirme sınıflandırmasına (kritik, güvenlik ve diğer, örneğin tanım güncelleştirmesi) göre kategorilere ayrılmış, ayrıntılı bir dökümünü sağlar. Bu panonun her bir kutucuğundaki sonuçlar yalnızca bilgisayarların eşitleme kaynağını temel alan dağıtım için onaylanmış güncelleştirmeleri yansıtır.   **Güncelleştirme Dağıtımları** kutucu seçildiğinde, sizi zamanlamaları, çalışmakta olan dağıtımları ve tamamlanmış dağıtımları görüntüleyebileceğiniz veya yeni dağıtım zamanlayabileceğiniz Güncelleştirme Dağıtımları sayfasını yönlendirir.  

Belirli bir kutucuğa tıklayarak tüm kayıtları döndüren bir günlük araması çalıştırabilirsiniz. Öte yandan belirli bir kategorinin veya önceden tanımlanmış ölçütün sorgusunu çalıştırmak için, **Sık Kullanılan Güncelleştirme Sorguları** sütununun altında sağlanan listeden bir seçim yapın.    

## <a name="installing-updates"></a>Güncelleştirmeleri yükleme
Güncelleştirmeler çalışma alanınızdaki tüm Linux ve Windows bilgisayarlar için değerlendirildikten sonra bir *Güncelleştirme Dağıtımı* oluşturarak gerekli güncelleştirmeleri yükleyebilirsiniz.  Güncelleştirme Dağıtımı, bir veya daha fazla bilgisayar için gerekli güncelleştirmelerin zamanlanmış yüklemesidir.  Dağıtımın tarih ve saatine ek olarak, dağıtım kapsamına alınacak bilgisayarı veya bilgisayar grubunu belirtmeniz gerekir.  Bilgisayar grupları hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te bilgisayar grupları](../log-analytics/log-analytics-computer-groups.md).  Güncelleştirme dağıtımınıza bilgisayar gruplarını eklediğinizde, grup üyeliği yalnızca bir kez, zamanlama oluşturulurken değerlendirilir.  Grupta daha sonra yapılan değişiklikler yansıtılmaz.  Bu soruna geçici bir çözüm olarak, zamanlanmış güncelleştirme dağıtımını silin ve yeniden oluşturun.

> [!NOTE]
> Azure Market’ten dağıtılan Windows VM’leri varsayılan olarak Windows Update hizmetinden otomatik güncelleştirmeleri alacak şekilde ayarlanmıştır.  Çalışma alanınıza bu çözümü veya Windows VM’lerini ekledikten sonra bu davranış değişmez.  Güncelleştirmeleri bu çözümle etkin olarak yönetmiyorsanız, varsayılan davranış (güncelleştirmeleri otomatik olarak uygulama) geçerli olur.  

Azure Market’ten edinilebilen isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan sanal makineler için Azure’da dağıtılmış [Red Hat Update Infrastructure (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) erişimi için kaydedilir.  Diğer tüm Linux dağıtımları, ilgili dağıtımın desteklediği yöntemler izlenerek çevrimiçi dosya deposundan güncelleştirilmelidir.  

### <a name="viewing-update-deployments"></a>Güncelleştirme dağıtımlarını görüntüleme
Var olan Güncelleştirme Dağıtımlarının listesini görüntülemek için **Güncelleştirme Dağıtımı** kutucuğuna tıklayın.  Bunlar duruma göre gruplandırılır: **Zamanlanmış**, **Çalışıyor** ve **Tamamlandı**.<br><br> ![Güncelleştirme Dağıtımları Zamanlama Sayfası](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Her Güncelleştirme Dağıtımı için gösterilen özellikler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme Dağıtımının adı. |
| Zamanlama |Zamanlama türü.  Sağlanan seçenekler *Bir Kerelik*, *Haftalık Yineleme* ve *Aylık Yineleme*’dir. |
| Başlangıç Zamanı |Güncelleştirme Dağıtımının başlatılmak üzere zamanlandığı tarih ve saat. |
| Süre |Güncelleştirme Dağıtımının çalışmasına izin verilen dakika sayısı.  Bu süre içinde tüm güncelleştirmeler yüklenmezse, kalan güncelleştirmeler sonraki Güncelleştirme Dağıtımına kadar beklemelidir. |
| Sunucular |Güncelleştirme Dağıtımından etkilenen bilgisayar sayısı.  |
| Durum |Güncelleştirme Dağıtımının geçerli durumu.<br><br>Olası değerler şunlardır:<br>- Başlatılmadı<br>- Çalışıyor<br>- Tamamlandı |

Aşağıdaki tabloda gösterilen sütunları içeren ayrıntı ekranını görüntülemek için tamamlanmış bir Güncelleştirme Dağıtımı seçin.  Güncelleştirme Dağıtımı henüz başlamadıysa bu sütunlar doldurulmaz.<br><br> ![Güncelleştirme Dağıtımı Sonuçlarına genel bakış](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Sütun | Açıklama |
| --- | --- |
| **Bilgisayarlar Görünümü** | |
| Windows Bilgisayarları |Güncelleştirme Dağıtımındaki Windows bilgisayarı sayısını durumlarına göre listeler.  Güncelleştirme Dağıtımında bir duruma sahip tüm güncelleştirme kayıtlarını döndüren günlük araması gerçekleştirmek için ilgili duruma tıklayın. |
| Linux Bilgisayarları |Güncelleştirme Dağıtımındaki Linux bilgisayarı sayısını durumlarına göre listeler.  Güncelleştirme Dağıtımında bir duruma sahip tüm güncelleştirme kayıtlarını döndüren günlük araması gerçekleştirmek için ilgili duruma tıklayın. |
| Bilgisayar Yükleme Durumu |Güncelleştirme Dağıtımında rol alan bilgisayarları ve başarıyla yüklenen güncelleştirme yüzdesini listeler. Tüm eksik ve kritik güncelleştirmeleri döndüren bir günlük araması gerçekleştirmek için girişlerden birine tıklayın. |
| **Güncelleştirmeler Görünümü** | |
| Windows Güncelleştirmeleri |Güncelleştirme Dağıtımı’na dahil edilen Windows güncelleştirmelerini ve her güncelleştirme için yükleme durumunu listeler.  Belirli bir güncelleştirmenin tüm güncelleştirme kayıtlarını döndüren bir günlük araması çalıştırmak için bir güncelleştirme seçin veya dağıtımın tüm güncelleştirme kayıtlarını döndüren bir günlük araması çalıştırmak için duruma tıklayın. |
| Linux Güncelleştirmeleri |Güncelleştirme Dağıtımı’na dahil edilen Linux güncelleştirmelerini ve her güncelleştirme için yükleme durumunu listeler.  Belirli bir güncelleştirmenin tüm güncelleştirme kayıtlarını döndüren bir günlük araması çalıştırmak için bir güncelleştirme seçin veya dağıtımın tüm güncelleştirme kayıtlarını döndüren bir günlük araması çalıştırmak için duruma tıklayın. |

### <a name="creating-an-update-deployment"></a>Güncelleştirme Dağıtımı oluşturma
Yeni bir Güncelleştirme Dağıtımı oluşturmak için ekranın üst kısmındaki **Ekle** düğmesine tıklayarak **Yeni Güncelleştirme Dağıtımı** sayfasını açın.  Aşağıdaki tabloda gösterilen özelliklerin değerlerini belirtmeniz gerekir.

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
| Saat Dilimi |Başlangıç saati için kullanılacak saat dilimi. |
| Zamanlama Türü | Zamanlama türü.  Sağlanan seçenekler *Bir Kerelik*, *Haftalık Yineleme* ve *Aylık Yineleme*’dir.  
| Başlangıç Zamanı |Güncelleştirme dağıtımının başlatılacağı tarih ve saat. **Not:** Hemen dağıtım yapmanız gerekiyorsa, dağıtımın çalıştırılabileceği en yakın zaman şu andan 30 dakika sonrasıdır. |
| Süre |Güncelleştirme Dağıtımının çalışmasına izin verilen dakika sayısı.  Bu süre içinde tüm güncelleştirmeler yüklenmezse, kalan güncelleştirmeler sonraki Güncelleştirme Dağıtımına kadar beklemelidir. |
| Bilgisayarlar |Güncelleştirme Dağıtımına dahil edilecek ve hedeflenecek bilgisayarların veya bilgisayar gruplarının adları.  Açılır listeden bir veya daha fazla giriş seçin. |

<br><br> ![Yeni Güncelleştirme Dağıtımı Sayfası](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Zaman aralığı
Varsayılan olarak, Güncelleştirme Yönetimi çözümünde analiz edilen verilerin kapsamı son 1 gün içinde oluşturulan tüm bağlı yönetim gruplarından belirlenir.

Verileri zaman aralığını değiştirmek için panonun üst kısmındaki **Veri temeli**’ni seçin. Son 7 gün, 1 gün veya 6 saat içinde oluşturulan veya güncelleştirilen kayıtları seçebilirsiniz. Ya da **Özel**’i seçip özel bir tarih aralığı belirtebilirsiniz.

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Güncelleştirme Yönetimi çözümü, OMS deposunda iki tür kayıt oluşturur.

### <a name="update-records"></a>Güncelleştirme kayıtları
Her bilgisayara yüklenen veya gerekli olan her bir güncelleştirme için **Güncelleştirme** türünde bir kayıt oluşturulur. Güncelleştirme kayıtları aşağıdaki tabloda gösterilen özelliklere sahiptir.

| Özellik | Açıklama |
| --- | --- |
| Tür |*Güncelleştirme* |
| SourceSystem |Güncelleştirmenin yüklenmesini onaylayan kaynak.<br>Olası değerler şunlardır:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Linux Sunucuları (Paket Yöneticilerinden getirilir) |
| Onaylandı |Güncelleştirme yüklemesinin onaylanıp onaylanmadığını belirtir.<br> Düzeltme eki uygulama işlemi OMS tarafından yönetilmediğinden, Linux sunucuları için bu seçenek şu anda isteğe bağlıdır. |
| Windows Sınıflandırması |Güncelleştirme sınıflandırması.<br>Olası değerler şunlardır:<br>-    Uygulamalar<br>- Kritik Güncelleştirmeler<br>- Tanım Güncelleştirmeleri<br>- Özellik Paketleri<br>- Güvenlik Güncelleştirmeleri<br>- Hizmet Paketleri<br>- Güncelleştirme Paketleri<br>- Güncelleştirmeler |
| Linux Sınıflandırması |Güncelleştirme sınıflandırması.<br>Olası değerler şunlardır:<br>- Kritik Güncelleştirmeler<br>- Güvenlik Güncelleştirmeleri<br>- Diğer Güncelleştirmeler |
| Bilgisayar |Bilgisayarın adı. |
| InstallTimeAvailable |Yükleme zamanına, aynı güncelleştirmenin yüklü olduğu diğer aracılardan ulaşılıp ulaşılamadığını belirtir. |
| InstallTimePredictionSeconds |Aynı güncelleştirmeyi yükleyen diğer aracılara göre saniye cinsinden tahmini yükleme süresi. |
| KBID |Güncelleştirmeyi açıklayan KB makalesi kimliği. |
| ManagementGroupName |SCOM aracıları için yönetim grubunun adı.  Diğer aracılar için bu değer AOI-<workspace ID> şeklindedir. |
| MSRCBulletinID |Güncelleştirmeyi açıklayan Microsoft güvenlik bülteni kimliği. |
| MSRCSeverity |Microsoft güvenlik bülteninin önem derecesi.<br>Olası değerler şunlardır:<br>- Kritik<br>- Önemli<br>- Orta |
| İsteğe bağlı |Güncelleştirmenin isteğe bağlı olup olmadığını belirtir. |
| Ürün |Güncelleştirmenin ait olduğu ürünün adı.  Makaleyi bir tarayıcıda açmak için **Görüntüle**’ye tıklayın. |
| PackageSeverity |Bu güncelleştirmede giderilen güvenlik açığının Linux distro satıcıları tarafından bildirilen önem derecesi. |
| PublishDate |Güncelleştirmenin yüklendiği tarih ve saat. |
| RebootBehavior |Güncelleştirmenin yeniden başlatmaya zorlayıp zorlamayacağını belirtir.<br>Olası değerler şunlardır:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Güncelleştirmenin düzeltme sayısı. |
| SourceComputerId |Bilgisayarı benzersiz olarak tanımlayan GUID. |
| TimeGenerated |Kaydın son güncelleştirildiği tarih ve saat. |
| Başlık |Güncelleştirme başlığı. |
| UpdateID |Güncelleştirmeyi benzersiz olarak tanımlayan GUID. |
| UpdateState |Güncelleştirmenin bu bilgisayarda yüklü olup olmadığını belirtir.<br>Olası değerler şunlardır:<br>- Yüklü: Güncelleştirme bu bilgisayarda yüklü.<br>- Gerekli - Güncelleştirme yüklü değil ve bu bilgisayarda gerekli. |

**Update** türünde kayıtlar döndüren bir günlük araması yaptığınızda, arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesi görüntülemek üzere **Güncelleştirmeler** görünümünü seçebilirsiniz. **Eksik ve uygulanan güncelleştirmeler** ile **Gerekli ve isteğe bağlı güncelleştirmeler** kutucuklarındaki girişlere tıklayarak görünümün kapsamını bu güncelleştirme kümesine ayarlayabilirsiniz. Kayıtları tek tek döndürmek için **Liste** veya **Tablo** görünümünü seçin.<br>

![Güncelleştirme Kayıt Türü ile Günlük Arama Güncelleştirme Görünümü](./media/oms-solution-update-management/update-la-view-updates.png)  

**Tablo** görünümünde, KB makalesiyle bir tarayıcı açmak üzere herhangi bir kayda ait **KBID** öğesine tıklayabilirsiniz. Bunun yapılması, ilgili güncelleştirmenin ayrıntıları hakkında hızlıca bilgi almanızı sağlar.<br>

![Güncelleştirme Kayıt Türü Kutucukları ile Günlük Arama Tablosu](./media/oms-solution-update-management/update-la-view-table.png)

**Liste** görünümünde KBID’nin yanındaki **Görünüm** bağlantısına tıklayarak KB makalesini açabilirsiniz.<br>

![Güncelleştirme Kayıt Türü Kutucukları ile Günlük Arama Listesi](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>UpdateSummary kayıtları
Her Windows aracı bilgisayarı için **UpdateSummary** türünde bir kayıt oluşturulur. Bilgisayarda yapılan her güncelleştirme taramasında kayıt güncelleştirilir. **UpdateSummary** kayıtları aşağıdaki tabloda gösterilen özelliklere sahiptir.

| Özellik | Açıklama |
| --- | --- |
| Tür |UpdateSummary |
| SourceSystem |OpsManager |
| Bilgisayar |Bilgisayarın adı. |
| CriticalUpdatesMissing |Bilgisayarda eksik olan kritik güncelleştirme sayısı. |
| ManagementGroupName |SCOM aracıları için yönetim grubunun adı. Diğer aracılar için bu değer AOI-<workspace ID> şeklindedir. |
| NETRuntimeVersion |Bilgisayarda yüklü .NET çalışma zamanı sürümü. |
| OldestMissingSecurityUpdateBucket |Bu bilgisayardaki en eski eksik güvenlik güncelleştirmesinin yayımlanmasından bu yana geçen süreyi kategorilere ayıran demet.<br>Olası değerler şunlardır:<br>- Eski<br>-    180 gün önce<br>- 150 gün önce<br>-    120 gün önce<br>- 90 gün önce<br>- 60 gün önce<br>-    30 gün önce<br>-    En Son |
| OldestMissingSecurityUpdateInDays |Bu bilgisayardaki en eski eksik güvenlik güncelleştirmesinin yayımlanmasından bu yana geçen gün sayısı. |
| OsVersion |Bilgisayarda yüklü işletim sistemi sürümü. |
| OtherUpdatesMissing |Bilgisayarda eksik olan diğer güncelleştirmelerin sayısı. |
| SecurityUpdatesMissing |Bilgisayarda eksik olan güvenlik güncelleştirmelerinin sayısı. |
| SourceComputerId |Bilgisayarı benzersiz olarak tanımlayan GUID. |
| TimeGenerated |Kaydın son güncelleştirildiği tarih ve saat. |
| TotalUpdatesMissing |Bilgisayarda eksik olan toplam güncelleştirme sayısı. |
| WindowsUpdateAgentVersion |Bilgisayardaki Windows Update aracısının sürüm numarası. |
| WindowsUpdateSetting |Bilgisayarın önemli güncelleştirmeleri yükleme yöntemiyle ilgili ayar.<br>Olası değerler şunlardır:<br>- Devre dışı<br>- Yüklemeden önce bildir<br>- Zamanlanmış yükleme |
| WSUSServer |Bilgisayar, kullanacak için yapılandırılmışsa WSUS sunucusunun URL’si. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan güncelleştirme kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
| --- | --- |
| Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false &#124; measure count() by Computer |Güncelleştirmelere gerek duyan Windows tabanlı sunucu bilgisayarları |
| Type:Update OSType=Linux UpdateState!="Not needed" &#124; measure count() by Computer |Güncelleştirmelere gerek duyan Linux sunucuları | 
| Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Eksik güncelleştirmeleri olan tüm bilgisayarlar |
| Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |Belirli bir bilgisayarda eksik güncelleştirmeler (değeri kendi bilgisayarınızın adıyla değiştirin)|
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |Eksik kritik güncelleştirmeleri veya güvenlik güncelleştirmeleri olan tüm bilgisayarlar | 
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |Güncelleştirmelerin el ile uygulandığı makinelerde gerekli olan kritik güncelleştirmeler veya güvenlik güncelleştirmeleri |
| Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |Kritik güncelleştirmeleri veya gerekli güvenlik güncelleştirmeleri eksik olan makineler için hata olayları |
| Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Eksik güncelleştirme paketleri olan tüm bilgisayarlar | 
| Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |Tüm bilgisayarlardaki ayrı eksik güncelleştirmeler | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Title, UpdateRunName |Bir güncelleştirme çalıştırmasında güncelleştirmeleri başarısız olan Windows tabanlı sunucu bilgisayarı | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Product, UpdateRunName |Bir güncelleştirme çalıştırmasında güncelleştirmeleri başarısız olan Linux sunucusu | 
| Type=UpdateSummary &#124; measure count() by WSUSServer |WSUS bilgisayar üyeliği | 
| Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |Otomatik güncelleştirme yapılandırması | 
| Type=UpdateSummary WindowsUpdateSetting=Manual |Otomatik güncelleştirmenin devre dışı olduğu bilgisayarlar | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |Paket güncelleştirmesi mevcut olan tüm Linux makinelerinin listesi | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |Kritik veya Güvenlik türünde güvenlik açığını ele alan paket güncelleştirmesine sahip tüm Linux makinelerinin listesi | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" |Bir güncelleştirmenin kullanılabilir olduğu tüm paketlerin listesi | 
| Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |Kritik veya Güvenlik türünde güvenlik açığını ele alan güncelleştirmeye sahip tüm paketlerin listesi | 
| Type:UpdateRunProgress &#124; measure Count() by UpdateRunName |Hangi dağıtımların bilgisayarlarda değişiklik yaptığını gösteren liste | 
| Type:UpdateRunProgress UpdateRunName="DeploymentName" &#124; measure Count() by Computer |Bu güncelleştirme çalıştırmasında güncelleştirilmiş olan bilgisayarlar (değeri kendi Güncelleştirme Dağıtımı adınızla değiştirin) | 
| Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |Herhangi bir güncelleştirmenin mevcut olduğu tüm "Ubuntu" makinelerinin listesi |

## <a name="integrate-with-system-center-configuration-manager"></a>System Center Configuration Manager ile tümleştirme

PC, sunucu ve mobil cihazları yönetmek için System Center Configuration Manager'a yatırım yapmış müşteriler aynı zamanda yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve olgunluğundan yararlanmaktadır.

OMS Güncelleştirme Yönetimi çözümünü Sytem Center Configuration Manager ile nasıl tümleştireceğinizi öğrenmek için bkz. [System Center Configuration Manager'ı OMS Güncelleştirme Yönetimi ile tümleştirme](../automation/oms-solution-updatemgmt-sccmintegration.md).

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, Güncelleştirme Yönetimi çözümüyle ilgili sorunları gidermeye yardımcı olacak bilgiler sağlanır.

### <a name="how-do-i-troubleshoot-onboarding-issues"></a>Ekleme sorunlarını nasıl giderebilirim?
Çözümü veya bir sanal makineyi eklemeye çalışırken sorun yaşarsanız, olay kimliği 4502 olan olaylar ve **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** içeren olay iletisi için **Uygulama ve Hizmet Günlükleri\Operations Manager** olay günlüğünü denetleyin.  Aşağıdaki tabloda belirli hata iletileri ve her birinin olası çözümü vurgulanmıştır.  

| İleti | Neden | Çözüm |   
|----------|----------|----------|  
| Yama Yönetimi için Makine Kaydedilemiyor,<br>Kayıt Özel Durumla Başarısız Oldu<br>System.InvalidOperationException: {"Message":"Makine zaten<br>farklı bir hesaba kaydedildi. "} | Makine, Güncelleştirme Yönetimi için zaten başka bir çalışma alanına eklendi | Karma runbook grubunu silerek [eski yapıtları temizleyin](../automation/automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Yama Yönetimi için Makine Kaydedilemiyor,<br>Kayıt Özel Durumla Başarısız Oldu<br>System.Net.Http.HttpRequestException: İstek gönderilirken bir hata oluştu. ---><br>System.Net.WebException: Temel alınan bağlantı<br>kapatıldı: Alma işlemi sırasında<br>beklenmeyen bir hata oluştu. ---> System.ComponentModel.Win32Exception:<br>İstemci ve sunucu iletişim kuramıyor,<br>çünkü ortak bir algoritmaya sahip değiller | Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](../automation/automation-offering-get-started.md#network-planning)|  
| Yama Yönetimi için Makine Kaydedilemiyor,<br>Kayıt Özel Durumla Başarısız Oldu<br>Newtonsoft.Json.JsonReaderException: Pozitif sonsuz değer ayrıştırılırken hata oluştu. | Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](../automation/automation-offering-get-started.md#network-planning)| 
| <wsid>.oms.opinsights.azure.com hizmeti tarafından sunulan sertifika<br>Microsoft hizmetleri için kullanılan bir sertifika yetkilisi<br>tarafından verilmemiş. Lütfen<br>ağ yöneticinize başvurarak<br>TLS/SSL iletişimini engelleyen bir proxy çalıştırıp çalıştırmadıklarına bakın. |Proxy/Ağ Geçidi/Güvenlik Duvarı iletişimi engelliyor | [Ağ gereksinimlerini gözden geçirin](../automation/automation-offering-get-started.md#network-planning)|  
| Yama Yönetimi için Makine Kaydedilemiyor,<br>Kayıt Özel Durumla Başarısız Oldu<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Otomatik olarak imzalanan sertifika oluşturulamadı. ---><br>System.UnauthorizedAccessException: Erişim reddedildi. | Otomatik olarak imzalanan sertifika oluşturma hatası | Sistem hesabının<br>klasöre okuma erişiminin olduğunu doğrulayın:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Güncelleştirme dağıtımlarının sorunlarını nasıl giderebilirim?
Bu çözümü destekleyen OMS çalışma alanıyla bağlantı Otomasyon hesabınızın İşler dikey penceresindeki zamanlanmış güncelleştirme dağıtımına dahil olan güncelleştirmeleri dağıtmaktan sorumlu runbook’un sonuçlarını görüntüleyebilirsiniz.  **Patch-MicrosoftOMSComputer** runbook’u, belirli bir yönetilen bilgisayarı hedefleyen bir alt runbook’tur; ayrıntılı Akışın gözden geçirilmesi bu dağıtımla ilgili ayrıntılı bilgileri sağlar.  Çıkışta, hangi gerekli güncelleştirmelerin uygulanabilir olduğu, indirme durumu, yükleme durumu ve ek ayrıntılar gösterilir.<br><br> ![Güncelleştirme Dağıtımı iş durumu](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Daha fazla bilgi için bkz. [Otomasyon runbook’u çıkışı ve iletileri](../automation/automation-runbook-output-and-messages.md).   

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı güncelleştirme verilerini görüntülemek için [Log Analytics](../log-analytics/log-analytics-log-searches.md)’te Günlük Aramalarını kullanın.
* Yönetilen bilgisayarlarınızın güncelleştirme uyumluluğunu gösteren [kendi panolarınızı oluşturun](../log-analytics/log-analytics-dashboards.md).
* Bilgisayardan eksik kritik güncelleştirmeler algılandığında veya bilgisayarda otomatik güncelleştirmeler devre dışı olduğunda [uyarılar oluşturun](../log-analytics/log-analytics-alerts.md).  
