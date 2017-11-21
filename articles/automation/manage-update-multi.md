---
title: "Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme | Microsoft Docs"
description: "Bu konuda, Azure sanal makineleri için güncelleştirmelerin nasıl yönetileceği açıklanmaktadır."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: f97b28d1588e959728163f7ab16d2550a79f610e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="manage-updates-for-multiple-machines"></a>Birden çok makine için güncelleştirmeleri yönetme

Güncelleştirme yönetimi, Windows ve Linux makineleriniz için güncelleştirmeleri ve yamaları yönetmenize olanak tanır.
[Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan makineleri kolayca ekleyebilir, kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve dağıtım sonuçlarını inceleyerek güncelleştirmelerin Güncelleştirme yönetimi etkin olan tüm sanal makinelere başarıyla uygulandığını doğrulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Güncelleştirme yönetimini kullanmak için şunlar gerekir:

* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Farklı Çalıştır hesabı oluşturma yönergeleri için [Azure Otomasyonu ile Çalışmaya Başlama](automation-offering-get-started.md) konusunu inceleyin.

* Desteklenen işletim sistemlerinden birinin yüklü olduğu bir sanal makine veya bilgisayar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güncelleştirme yönetimi aşağıdaki işletim sistemlerinde desteklenir.

### <a name="windows"></a>Windows

* Windows Server 2008 ve üzeri ile Windows Server 2008 R2 SP1 ve üzerine göre güncelleştirme dağıtımları.  Sunucu Çekirdeği ve Nano Sunucu yükleme seçenekleri desteklenmez.

    > [!NOTE]
    > Windows Server 2008 R2 SP1'e yönelik güncelleştirme dağıtımı desteği için .NET Framework 4.5 ve WMF 5.0 veya sonraki bir sürümü gerekir.
    > 
* Windows istemci işletim sistemleri desteklenmez.

Windows aracıları Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.

> [!NOTE]
> Windows aracısı System Center Configuration Manager tarafından eşzamanlı olarak yönetilemez.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) ve 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) ve 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)  
* Ubuntu 12.04 LTS ve daha yeni x86/x64   

> [!NOTE]  
> Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Bahsedilen yapılandırma işlemiyle ilgili bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.

> [!NOTE]
> Birden çok OMS çalışma alanına raporlayacak şekilde yapılandırılmış bir Linux için OMS Aracısı, bu çözümde desteklenmez.  
>

## <a name="enable-update-management-for-azure-virtual-machines"></a>Azure sanal makineleri için güncelleştirme yönetimini etkinleştirme

1. Azure portalında Otomasyon hesabınızı açın.
2. Ekranın sol tarafından **Güncelleştirme yönetimi**’ni seçin.
3. Ekranın üstündeki **Azure VM Ekle**’ye tıklayın.
    ![VM Ekleme](./media/manage-update-multi/update-onboard-vm.png)
4. Eklemek için bir sanal makine seçin. **Güncelleştirme Yönetimini Etkinleştirme** ekranı görüntülenir.
5. **Etkinleştir**’e tıklayın.

   ![Güncelleştirme yönetimini etkinleştirme](./media/manage-update-multi/update-enable.png)

Güncelleştirme yönetimi, sanal makineniz için etkinleştirilir.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Azure olmayan sanal makineler ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme

Azure olmayan Windows sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında yönergeler için [Azure’da Windows bilgisayarlarını Log Analytics hizmetine bağlama](../log-analytics/log-analytics-windows-agents.md) konusunu inceleyin.

Azure olmayan Linux sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında yönergeler için [Azure’da Linux bilgisayarlarını Operations Management Suite’e (OMS) bağlama](../log-analytics/log-analytics-agent-linux.md) konusunu inceleyin.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="data-collection"></a>Veri toplama

Sanal makine ve bilgisayarlarda yüklü aracılar, güncelleştirmelerle ilgili verileri toplar ve Azure güncelleştirme yönetimine gönderir.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Güncelleştirme yönetimi, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Güncelleştirme yönetimi, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Güncelleştirme yönetimi, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure depolama hesabı |Hayır |Azure Storage, sistem güncelleştirmeleri hakkında bilgi içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API’si çağrılarak son güncelleştirme zamanı sorgulanır; böylelikle durumun değişip değişmediği saptanır ve değişmişse bir uyumluluk taraması başlatılır.  Yönetilen her Linux bilgisayarı için 3 saatte bir tarama gerçekleştirilir.

Yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin panoda görüntülenmesi 30 dakika ile 6 saat arasında bir zaman alabilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak bir veya daha fazla sanal makine için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdakileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **İşletim Sistemi Türü** - Windows veya Linux'u seçin.
* **Güncelleştirilecek bilgisayarlar** - Güncelleştirmek istediğiniz sanal makineleri seçin.

  ![Güncelleştirilecek sanal makineleri seçme](./media/manage-update-multi/update-select-computers.png)

* **Güncelleştirme sınıflandırması** - Güncelleştirme dağıtımının dağıtıma dahil edeceği yazılım türlerini seçin. Sınıflandırma türleri şunlardır:
  * Kritik güncelleştirmeler
  * Güvenlik güncelleştirmeleri
  * Güncelleştirme paketleri
  * Özellik paketleri
  * Hizmet paketleri
  * Tanım güncelleştirmeleri
  * Araçlar
  * Güncelleştirmeler
* **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilir ya da farklı bir zaman belirtebilirsiniz.
   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. Yinelenen bir zamanlama ayarlamak için Yineleme altında Yinelenen seçeneğine tıklayın.

   ![Güncelleştirme Zamanlama Ayarları ekranı](./media/manage-update-multi/update-set-schedule.png)

* **Bakım penceresi (dakika)** - Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin.  Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanmış** tablosunda yeni oluşturduğunuz dağıtım zamanlamasının gösterildiğine dikkat edin.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için sanal makine otomatik olarak yeniden başlatılır.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.

![Dağıtım durumunu güncelleştirme ](./media/manage-update-multi/update-view-results.png)

Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve sanal makinedeki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* Denenmedi - tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* Başarılı - güncelleştirme başarılı oldu
* Başarısız - güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Hedef sanal makinede güncelleştirme dağıtımını yönetmekten sorumlu runbook’un iş akışını görmek için **Çıktı** kutucuğuna tıklayın.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

Günlükler, çıktı ve hata bilgileri hakkında ayrıntılı bilgi için [Güncelleştirme Yönetimi](../operations-management-suite/oms-solution-update-management.md) konusunu inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [Güncelleştirme Yönetimi](../operations-management-suite/oms-solution-update-management.md).
