---
title: Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme
description: Bu konuda, Azure sanal makineleri için güncelleştirmelerin nasıl yönetileceği açıklanmaktadır.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1f34255bdbcc8761f1c68adbb2f1828521f789e4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-updates-for-multiple-machines"></a>Birden çok makine için güncelleştirmeleri yönetme

Güncelleştirme yönetimini kullanarak Windows ve Linux makineleriniz için güncelleştirmeleri ve yamaları yönetebilirsiniz. [Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan şunları yapabilirsiniz:

- Sanal makine ekleme.
- Kullanılabilir güncelleştirmelerin durumunu değerlendirme.
- Gerekli güncelleştirmeleri yüklemeyi zamanlama.
- Güncelleştirme yönetiminin etkin olduğu tüm sanal makinelere güncelleştirmelerin başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirme.

## <a name="prerequisites"></a>Önkoşullar

Güncelleştirme yönetimini kullanmak için şunlar gerekir:

- Azure Otomasyonu Farklı Çalıştır hesabı. Hesap oluşturma hakkında yönergeler için bkz. [Azure Otomasyonu ile çalışmaya başlama](automation-offering-get-started.md).

- Desteklenen işletim sistemlerinden birinin yüklü olduğu bir sanal makine veya bilgisayar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güncelleştirme yönetimi aşağıdaki işletim sistemlerinde desteklenir.

### <a name="windows"></a>Windows

- Windows Server 2008 ve üzeri ile Windows Server 2008 R2 SP1 ve üzerine göre güncelleştirme dağıtımları. Nano Sunucu desteklenmiyor.

  Windows Server 2008 R2 SP1'e yönelik güncelleştirme dağıtımı desteği için .NET Framework 4.5 ve Windows Management Framework 5.0 veya sonraki bir sürümü gerekir.

- Windows istemci işletim sistemleri desteklenmez.

Windows aracıları Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.

> [!NOTE]
> System Center Configuration Manager, Windows aracısını eşzamanlı olarak yönetemez.
>

### <a name="linux"></a>Linux

- CentOS 6 (x86/x64) ve 7 (x64)

- Red Hat Enterprise 6 (x86/x64) ve 7 (x64)

- SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)

- Ubuntu 12.04 LTS ve daha yeni (x86/x64)

> [!NOTE]
> Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Daha fazla bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.

Bu çözüm, rapor birden fazla günlük analizi çalışma alanları için yapılandırılmış Linux için bir OMS Aracısı desteklemiyor.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Azure sanal makineleri için güncelleştirme yönetimini etkinleştirme

Azure Portal'da Automation hesabınızı açın ve seçin **güncelleştirme yönetimi**.

Pencerenin üst kısmındaki **Azure VM Ekle**’yi seçin.

![Azure VM sekme ekleme](./media/manage-update-multi/update-onboard-vm.png)

Eklemek için bir sanal makine seçin. **Güncelleştirme Yönetimini Etkinleştir** iletişim kutusu görüntülenir. Seçin **etkinleştirmek** onboarding için sanal makine. Onboarding tamamlandığı zaman, güncelleştirme yönetimi, sanal makine için etkinleştirilir.

![Güncelleştirme Yönetimini Etkinleştir iletişim kutusu](./media/manage-update-multi/update-enable.png)

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Azure olmayan sanal makineler ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme

Azure olmayan Windows sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında yönergeler için [Azure’da Windows bilgisayarlarını Log Analytics hizmetine bağlama](../log-analytics/log-analytics-windows-agent.md) konusunu inceleyin.

Azure olmayan Linux sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında yönergeler için [Azure’da Linux bilgisayarlarını Log Analytics’e bağlama](../log-analytics/log-analytics-agent-linux.md) konusunu inceleyin.

## <a name="view-computers-attached-to-your-automation-account"></a>Otomasyon hesabınıza bağlı bilgisayarları görüntüleme

Makinelerinizin güncelleştirme yönetimini etkinleştirdikten sonra **Bilgisayarlar**’a tıklayarak bilgilerini görüntüleyebilirsiniz. Bilgisayar bilgileri gibi *adı*, *Uyumluluk*, *ortam*, *işletim sistemi türü*, *kritik ve güvenlikle ilgili güncelleştirmeleri* , *Diğer güncelleştirmeleri*, ve *Güncelleştirme Aracısı hazırlık* kullanılabilir.

  ![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-computers-tab.png)

Güncelleştirme yönetimi için yakın zamanda etkinleştirilmiş bilgisayarlar henüz değerlendirilmemiş olabilir. Bu bilgisayarların uyumluluk durumu *Değerlendirilmedi* şeklindedir.  Uyumluluk durumu değerlerinin listesi aşağıda verilmiştir:

- Uyumlu – Eksik kritik güncelleştirmeleri veya güvenlik güncelleştirmeleri olan bilgisayarlar.

- Uyumlu değil – En az bir eksik kritik güncelleştirmesi veya güvenlik güncelleştirmesi olan bilgisayarlar.

- Değerlendirilmedi – Güncelleştirme değerlendirme verileri beklenen zaman çerçevesi içinde bilgisayardan alınmamıştır.  Linux bilgisayarlar için son üç saat ve Windows bilgisayarlar için son 12 saat içinde.

Aracı durumunu görmek için bağlantıya tıklayın **güncelleştirme ARACISI hazırlık** sütun. Bu karma çalışanı durumunu gösteren karma çalışanı sayfasını açar. Aşağıdaki resimde, güncelleştirme yönetimi için genişletilmiş bir süre boyunca bağlı değil bir aracı örneği gösterilmiştir.

![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** iletişim kutusu görünür. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="collect-data"></a>Veri toplama

Sanal makine ve bilgisayarlarda yüklü aracılar, güncelleştirmelerle ilgili verileri toplar ve Azure güncelleştirme yönetimine gönderir.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Güncelleştirme yönetimi, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Güncelleştirme yönetimi, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Güncelleştirme yönetimi, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure depolama hesabı |Hayır |Azure Storage, sistem güncelleştirmeleri hakkında bilgi içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API’si çağrılarak son güncelleştirme zamanı sorgulanır; böylelikle durumun değişip değişmediği saptanır. Değişmişse bir uyumluluk taraması başlatılır. Yönetilen her Linux bilgisayarı için 3 saatte bir tarama gerçekleştirilir.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** iletişim kutusunun üst kısmındaki **Güncelleştirme dağıtımı zamanla**’yı seçerek bir veya daha fazla sanal makine için yeni bir güncelleştirme dağıtımı zamanlayabilirsiniz.
**Yeni güncelleştirme dağıtım** bölmesinde aşağıdakileri belirtin:

- **Ad**: Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
- **İşletim Sistemi Türü**: Windows veya Linux'u seçin.
- **Güncelleştirilecek makineler**: güncelleştirmek istediğiniz sanal makineleri seçin. Makine Hazırlık gösterilen **güncelleştirme ARACISI hazırlık** sütun. Bu güncelleştirme dağıtımı planlama önce bilgisayarın sistem durumunu görmenizi sağlar.

  !["Yeni güncelleştirme dağıtımı" bölmesi](./media/manage-update-multi/update-select-computers.png)

- **Güncelleştirme sınıflandırması** - Güncelleştirme dağıtımının içereceği yazılım türlerini seçin. Sınıflandırma türleri açıklaması için bkz: [güncelleştirme sınıflandırmaları](automation-update-management.md#update-classifications). Sınıflandırma türleri şunlardır:
  - Kritik güncelleştirmeler
  - Güvenlik güncelleştirmeleri
  - Güncelleştirme paketleri
  - Özellik paketleri
  - Hizmet paketleri
  - Tanım güncelleştirmeleri
  - Araçlar
  - Güncelleştirmeler
- **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilirsiniz. Veya farklı bir saat belirtebilirsiniz.
   Ayrıca, dağıtımın bir kez veya yinelenen bir zamanlamaya göre gerçekleşeceğini belirtebilirsiniz. Yinelenen bir zamanlama ayarlamak için **Yineleme** altında **Yinelenen** seçeneğine tıklayın.

   ![Zamanlama Ayarları iletişim kutusu](./media/manage-update-multi/update-set-schedule.png)

- **Bakım penceresi (dakika)**: Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesini seçerek durum panosuna dönün. **Zamanlanmış** tablosunda yeni oluşturduğunuz dağıtım zamanlaması gösterilir.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için sanal makine otomatik olarak yeniden başlatılır.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** iletişim kutusundaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
Dağıtım o anda çalışıyorsa, **Sürüyor** durumundadır. Dağıtım başarıyla tamamlandıktan sonra durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme başarısız olursa durum **Kısmen başarısız** olur.

![Güncelleştirme dağıtımının durumu](./media/manage-update-multi/update-view-results.png)

Bir güncelleştirme dağıtımının panosunu görmek için tamamlanan dağıtımı seçin.

**Güncelleştirme sonuçları** bölmesinde toplam güncelleştirme sayısı ve sanal makinedeki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir. Yükleme sonuçları aşağıdaki değerlerden biri olabilir:

- Denenmedi: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
- Başarılı: güncelleştirme başarılı oldu.
- Başarısız: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef sanal makinede güncelleştirme dağıtımını yöneten runbook’un iş akışını görmek için **Çıktı** kutucuğunu seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Güncelleştirme yönetimi hakkında daha fazla bilgi edinmek için de dahil olmak üzere günlükleri, çıkış ve hataları--görmek [Azure güncelleştirme yönetimi çözümünde](../operations-management-suite/oms-solution-update-management.md).
