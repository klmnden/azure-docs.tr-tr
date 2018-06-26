---
title: Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme
description: Bu makalede, güncelleştirmelerinin Azure sanal makineleri için nasıl yönetileceği açıklanmaktadır.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 59a00f5605f7664148b65f2ec9a88fbaa9057ccf
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946066"
---
# <a name="manage-updates-for-multiple-machines"></a>Birden çok makine için güncelleştirmeleri yönetme

Güncelleştirme yönetimi çözümü güncelleştirmelerini ve düzeltme eklerini için Windows ve Linux sanal makineleri yönetmek için kullanabilirsiniz. [Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan şunları yapabilirsiniz:

- Yerleşik sanal makineler
- Kullanılabilir güncelleştirmeleri durumunu değerlendirme
- Gerekli güncelleştirmeleri yüklemeyi zamanla
- Güncelleştirme yönetimi etkin olduğu tüm sanal makineler için güncelleştirmeler başarıyla uygulandığını doğrulamak için dağıtım sonuçları gözden geçirin

## <a name="prerequisites"></a>Önkoşullar

Güncelleştirme yönetimi kullanmak için size gerekenler:

- Azure Otomasyonu Farklı Çalıştır hesabı. Bir oluşturmayı öğrenmek için bkz: [Azure Automation ile çalışmaya başlama](automation-offering-get-started.md).
- Desteklenen işletim sistemlerinden birinin yüklü olduğu bir sanal makine veya bilgisayar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güncelleştirme yönetimi aşağıdaki işletim sistemlerinde desteklenir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Destekler, yalnızca değerlendirmeleri güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve sonraki sürümler     |Windows PowerShell 4.0 veya üstü gereklidir. ([Karşıdan WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1 daha fazla güvenilirlik için önerilir. ([Karşıdan WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 12.04 LTS, 14.04 LTS ve 16.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

> [!NOTE]
> Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Daha fazla bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.

Bu çözüm, birden çok Azure günlük analizi çalışma raporu için yapılandırılmış Linux için Operations Management Suite (OMS) aracıyı desteklemiyor.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Güncelleştirme yönetimi için Azure sanal makineleri etkinleştirme

Azure portalında, Automation hesabınızı açın ve ardından **güncelleştirme yönetimi**.

Seçin **Azure VM'ler eklemek**.

![Azure VM sekme ekleme](./media/manage-update-multi/update-onboard-vm.png)

Eklemek için bir sanal makine seçin. 

Altında **güncelleştirme yönetimini etkinleştirme**seçin **etkinleştirmek** onboarding için sanal makine.

![Güncelleştirme Yönetimini Etkinleştir iletişim kutusu](./media/manage-update-multi/update-enable.png)

Hazırlama tamamlandığında, güncelleştirme yönetimi, sanal makine için etkinleştirilir.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Güncelleştirme yönetimi Azure olmayan sanal makineleri ve bilgisayarlar için etkinleştir

Azure Windows sanal makineleri ve bilgisayarlar için güncelleştirme yönetimi etkinleştirme konusunda bilgi edinmek için [Azure günlük analizi hizmeti bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md).

Azure Linux sanal makineleri ve bilgisayarlar için güncelleştirme yönetimi etkinleştirme konusunda bilgi edinmek için [Linux bilgisayarlarınızı günlük Analizi'ne bağlamak](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Otomasyon hesabınıza bağlı bilgisayarları görüntüle

Makineleriniz için güncelleştirme yönetimini etkinleştirdikten sonra seçerek makine bilgilerini görüntüleyebilirsiniz **bilgisayarlar**. Bilgileri görebilirsiniz *makine adı*, *uyumluluk durumu*, *ortam*, *işletim sistemi türü*, *kritik ve güvenlik güncelleştirmeleri yüklü*, *yüklü diğer güncelleştirmeleri*, ve *Güncelleştirme Aracısı hazırlık* bilgisayarlarınız için.

  ![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-computers-tab.png)

Son güncelleştirme yönetimi için etkin bilgisayar henüz değerlendirdikten değil. Bu bilgisayarlar için uyumluluk durumu durumu **değil uygunluk**. Uyumluluk durumu için olası değerler listesi aşağıdadır:

- **Uyumlu**: eksik kritik olmayan veya güvenlik güncelleştirmeleri olan bilgisayarlar.

- **Uyumlu**: en az bir kritik eksik olan bilgisayarlar veya güvenlik güncelleştirmesi.

- **Değil uygunluk**: güncelleştirme değerlendirme verilerini beklenen zaman çerçevesi içinde bilgisayardan alınan kurmadı. Linux bilgisayarlar için beklenen zaman çerçevesinde son 3 saat içinde ' dir. Windows bilgisayarları için beklenen zaman çerçevesinde son 12 saat içinde ' dir.

Aracının durumunu görüntülemek için bağlantıdaki seçin **güncelleştirme ARACISI hazırlık** sütun. Bu seçeneğin belirlenmesi açılır **karma çalışanı** bölmesinde ve karma çalışanı durumunu gösterir. Aşağıdaki resimde, güncelleştirme yönetimi için uzun bir süre için bağlı olmayan bir aracı örneği gösterilmiştir:

![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="collect-data"></a>Veri toplama

Sanal makineler ve bilgisayarlarda yüklü aracılar güncelleştirmeler hakkında veri toplar. Aracıları Azure güncelleştirme yönetimi için verileri gönderin.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Güncelleştirme yönetimi Windows aracılardan sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmeleri yüklemesini başlatır. |
| Linux aracıları |Evet |Güncelleştirme yönetimi Linux aracılarını sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımları gerekli güncelleştirmeleri yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Güncelleştirme yönetimi aracıları bağlı Yönetim grubundaki sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure Storage hesabı |Hayır |Azure depolama sistem güncelleştirmeleri hakkında bilgi içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Bir tarama günde her yönetilen bir Windows bilgisayar çalıştırır. Windows API 15 dakikada adlı sorgulamak için durum değişip değişmediğini belirlemek son güncelleştirme zamanı. Durum değiştirdiyseniz, bir Uyumluluk taraması başlatır. Bir tarama her yönetilen Linux bilgisayar için 3 saatte çalışır.

Yönetilen bilgisayarların güncelleştirilmiş verileri görüntülemek Pano 6 saat arasındaki 30 dakika sürebilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlaması ve hizmet penceresi ile hizalar bir dağıtım zamanlaması. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

Bir veya daha fazla sanal makine için yeni bir güncelleştirme dağıtım altında zamanlamak için **güncelleştirme yönetimi**seçin **zamanlama güncelleştirme dağıtımı**.

İçinde **yeni güncelleştirme dağıtım** bölmesinde, aşağıdaki bilgileri belirtin:

- **Ad**: güncelleştirme dağıtımı tanımlamak için benzersiz bir ad girin.
- **İşletim sistemi**: seçin **Windows** veya **Linux**.
- **Güncelleştirilecek makineler**: güncelleştirmek istediğiniz sanal makineleri seçin. Makine Hazırlık gösterilen **güncelleştirme ARACISI hazırlık** sütun. Güncelleştirme dağıtımı zamanlama önce bilgisayarın sistem durumunu görebilirsiniz.

  ![Yeni güncelleştirme dağıtım bölmesi](./media/manage-update-multi/update-select-computers.png)

- **Güncelleştirme sınıflandırması**: yazılım güncelleştirmesi dağıtıma dahil türlerini seçin. Sınıflandırma türleri açıklaması için bkz: [güncelleştirme sınıflandırmaları](automation-update-management.md#update-classifications). Sınıflandırma türleri şunlardır:
  - Kritik güncelleştirmeler
  - Güvenlik güncelleştirmeleri
  - Güncelleştirme paketleri
  - Özellik paketleri
  - Hizmet paketleri
  - Tanım güncelleştirmeleri
  - Araçlar
  - Güncelleştirmeler

- **Dışlanacak güncelleştirmeleri**: Bu seçeneğin belirlenmesi açılır **hariç** sayfası. KB makalelerini veya hariç tutmak için paket adlarını girin.

- **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilirsiniz. Farklı bir saat de belirtebilirsiniz.

   Ayrıca, dağıtımın bir kez veya yinelenen bir zamanlamaya göre gerçekleşeceğini belirtebilirsiniz. Yinelenen bir zamanlama ayarlamak altında ayarlamak için **yineleme**seçin **yinelenen**.

   ![Zamanlama Ayarları iletişim kutusu](./media/manage-update-multi/update-set-schedule.png)
- **Bakım penceresi (dakika)**: gerçekleşmesi için Güncelleştirme dağıtımı istediğiniz süreyi belirtin. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

Zamanlamayı yapılandırmayı tamamladığınızda, seçin **oluşturma** düğmesi durumu panoya geri dönün. **Zamanlanmış** tablo oluşturduğunuz dağıtım zamanlaması gösterir.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için sanal makine otomatik olarak yeniden başlatılır.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.

Dağıtım o anda çalışıyorsa, **Sürüyor** durumundadır. Dağıtım başarıyla tamamlandıktan sonra durum değişikliklerini **başarılı**.

Dağıtımdaki bir veya daha fazla güncelleştirme başarısız olursa durum **Kısmen başarısız** olur.

![Güncelleştirme dağıtımının durumu](./media/manage-update-multi/update-view-results.png)

Bir güncelleştirme dağıtımının panosunu görmek için tamamlanan dağıtımı seçin.

**Güncelleştirme sonuçları** bölmesinde güncelleştirmeleri ve sanal makine için dağıtım sonuçları toplam sayısını gösterir. Sağ taraftaki tabloda her bir güncelleştirme ve yükleme sonuçları ayrıntılı bir dökümünü sağlar. Yükleme sonuçları aşağıdaki değerlerden biri olabilir:

- **Girişiminde bulunulmadı**: tanımlanmış bakım penceresinde göre yeterli zaman kullanılabilir olduğu için güncelleştirme yüklenmedi.
- **Başarılı**: Güncelleştirme başarılı oldu.
- **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef sanal makinedeki güncelleştirme dağıtımı yönetir runbook'un iş akışı görmek için çıkış döşemesini seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Günlükleri, çıkış ve hatalar da dahil olmak üzere güncelleştirme yönetimi hakkında daha fazla bilgi için bkz: [azure'da güncelleştirme yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md).
