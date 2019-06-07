---
title: Öğretici - Azure’da Windows sanal makinelerini izleme ve güncelleştirme | Microsoft Docs
description: Bu öğreticide, bir Windows sanal makinesinde önyükleme tanılamaları ile performans ölçümlerinin nasıl izleneceğini ve paket güncelleştirmelerinin nasıl yönetileceğini öğreneceksiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: aa423fc441c50c774a9670feec64d0f844a4f5ec
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755308"
---
# <a name="tutorial-monitor-and-update-a-windows-virtual-machine-in-azure"></a>Öğretici: İzleme ve azure'da Windows sanal makinesi güncelleştirme

Azure izlemesi Azure VM'lerinden önyükleme ve performans verilerini toplamak, bu verileri Azure depolama içinde depolamak ve portal, Azure PowerShell modülü ve Azure CLI aracılığıyla erişilebilir duruma getirmek için aracıları kullanır. Güncelleştirme yönetimi, Azure Windows sanal makineleriniz için güncelleştirme ve yamaları yönetmenize olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * VM konak ölçümlerini görüntüleme
> * Tanılama uzantısını yükleme
> * VM ölçümlerini görüntüleme
> * Uyarı oluşturma
> * Windows güncelleştirmelerini yönetme
> * Değişiklikleri ve sayımı izleme
> * Gelişmiş izlemeyi ayarlama

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bu öğreticide Azure izlemesi ve güncelleştirme yönetimini yapılandırmak için, Azure'da bir Windows VM'sine ihtiyacınız vardır. İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Artık VM oluşturma [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte *EastUS* konumunda *myVM* adlı bir VM oluşturulur. Henüz yoksa, *myResourceGroupMonitorMonitor* kaynak grubu ve destekleyici ağ kaynakları oluşturulur:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

Kaynakların ve sanal makinenin oluşturulması birkaç dakika sürer.

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Windows sanal makinelerinin önyüklemesi yapıldığında, önyükleme tanılama aracısı sorun giderme amacıyla kullanılabilecek bir ekran çıktısı yakalar. Bu özellik varsayılan olarak etkindir. Yakalanan ekran görüntüleri de varsayılan olarak oluşturulan bir Azure depolama hesabında depolanır.

Önyükleme tanılama verilerini [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/az.compute/get-azvmbootdiagnosticsdata) komutuyla alırsınız. Aşağıdaki örnekte, önyükleme tanılamaları *c:\* sürücüsünün kök dizinine indirilir.

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName "myResourceGroupMonitor" -Name "myVM" -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Windows VM’sinin, Azure’da etkileşimde bulunduğu ayrılmış bir Konak VM'si vardır. Konağa ait ölçümler otomatik olarak toplanır ve Azure portalında görüntülenebilir.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. Konak VM’nin performansını görmek için VM dikey penceresinde **Ölçümler**’e tıklayın ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir Konak ölçümünü seçin.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleme

Temel konak ölçümleri kullanılabilir, ancak daha ayrıntılı bilgileri ve VM’ye özel ölçümleri görüntülemek için Azure tanılama uzantısını VM’ye yüklemeniz gerekir. Azure tanılama uzantısı, VM’den ek izleme ve tanılama verilerinin alınmasına izin verir. Bu performans ölçümlerini görüntüleyebilir ve VM performansına bağlı uyarılar oluşturabilirsiniz. Tanılama uzantısı Azure portalından şu şekilde yüklenir:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. **Tanılama ayarları**’na tıklayın. Listede *Önyükleme tanılaması*’nın önceki bölümde zaten etkinleştirildiği görüntülenir. *Temel ölçümler* için onay kutusuna tıklayın.
3. **Konuk düzeyinde izlemeyi etkinleştir** düğmesine tıklayın.

    ![Tanılama ölçümlerini görüntüleme](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>VM ölçümlerini görüntüleme

VM ölçümlerini, konak VM ölçümlerini görüntülediğiniz gibi görüntüleyebilirsiniz:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. VM’nin performansını görüntülemek için VM dikey penceresinde **Ölçümler**’e tıklayın ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir tanılama ölçümünü seçin.

    ![VM ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine bağlı uyarılar oluşturabilirsiniz. Uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya mevcut boş disk alanı belirli bir miktarın altına düştüğünde bildirim almak için kullanılabilir. Uyarılar Azure portalında görüntülenebilir veya e-posta ile gönderilebilir. Ayrıca oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook’larını veya Azure Logic Apps’i tetikleyebilirsiniz.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. Önce VM dikey penceresinde **Uyarı kuralları**’na ve ardından uyarılar dikey penceresinin üstündeki **Ölçüm uyarısı ekle** seçeneğine tıklayın.
3. Uyarınız için *myAlertRule* gibi bir **Ad** girin
4. CPU yüzdesi beş dakika boyunca 1,0’ı aştığında bir uyarı tetiklemek için diğer varsayılan ayarların tümünü seçili bırakın.
5. İsteğe bağlı olarak e-posta bildirimi göndermek için *E-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyebilirsiniz. Varsayılan eylem olarak portalda bir bildirim sunulur.
6. **Tamam** düğmesine tıklayın.

## <a name="manage-windows-updates"></a>Windows güncelleştirmelerini yönetme

Güncelleştirme yönetimi, Azure Windows sanal makineleriniz için güncelleştirme ve yamaları yönetmenize olanak sağlar.
VM’nizden doğrudan güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin VM’ye başarılı bir şekilde uygulandığından emin olmak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştirme

Sanal makineniz için Güncelleştirme yönetimini etkinleştirme:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. VM ekranında **İşlemler** bölümünden **Güncelleştirme yönetimi**'ne tıklayın. **Güncelleştirme Yönetimini Etkinleştirme** ekranı açılır.

Bu VM için Güncelleştirme yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir.
Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı, Güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.
Azure Otomasyonu, güncelleştirme yapılması gereken VM'lerde güncelleştirme indirme ve uygulama gibi ek işlemleri gerçekleştirmek için VM'ler üzerinde runbook'lar çalıştırmanızı sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve Otomasyon karma runbook çalışanı ile sağlanıp sağlanmadığını da kontrol eder.
Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır.

Log Analytics çalışma alanını ve Otomasyon hesabı seçin ve tıklayın **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon](../../automation/automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Konumu, Log Analytics çalışma alanını ve Otomasyon hesabı'e tıklayın yapılandırma **etkinleştirme**. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme](./media/tutorial-monitoring/manageupdates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir. Çözüm etkinleştirildikten sonra VM'deki güncelleştirmeleri eksik hakkında bilgi Azure İzleyici günlüklerine akar. Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. Güncelleştirme değerlendirmesi tamamlandıktan sonra, **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görürsünüz.

 ![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak VM için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

Yeni bir güncelleştirme dağıtımı oluşturmak için Seç **güncelleştirme dağıtımı zamanla**. **Yeni güncelleştirme dağıtım** sayfası açılır. Aşağıdaki tabloda açıklanan özellikler için değerleri girin ve ardından **Oluştur**:

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim sistemi| Linux veya Windows|
| Grupları güncelleştirmek için |Azure makineler için abonelik, kaynak grupları, konumları ve etiketleri, dağıtımınızdaki dahil etmek için Azure vm'leri dinamik bir grup oluşturmak için bir birleşimini temel bir sorgu tanımlarsınız. </br></br>Azure olmayan makineler için mevcut bir Azure olmayan makine dağıtımına dahil edilecek bir grubu seçmek için arama kaydedilmiş seçin. </br></br>Daha fazla bilgi için bkz: [dinamik gruplar](../../automation/automation-update-management.md#using-dynamic-groups)|
| Güncelleştirilecek makineler |İçeri aktarılan grubu, kayıtlı bir aramayı seçin veya makine açılan listeden seçin ve tek bir makine seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure İzleyici günlüklerine bilgisayar grupları oluşturma farklı yöntemleri hakkında bilgi edinmek için bkz: [Azure İzleyici günlüklerine bilgisayar grupları](../../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|Gereksinim duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri Ekle/Dışla|Bu açılır **dahil edin/dışlayın** sayfası. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. Ekleme nasıl ele alındığını daha fazla bilgi için bkz: [ekleme davranışı](../../automation/automation-update-management.md#inclusion-behavior) |
|Zamanlama ayarları|Her iki kez başlatın ve saati seçin veya yineleme için yineleme|
| Ön betiklerini + sonrası betikleri|Önce ve sonra dağıtımınız betiklerin seçin|
| Bakım penceresi |Güncelleştirmeler için dakika sayısı. Değeri 30 dakika ile en fazla 6 saat değerinden küçük olamaz |
| Denetim yeniden başlatma| Yeniden başlatma işlemlerini nasıl işleneceğini belirler. Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Güncelleştirme dağıtımları da bir program aracılığıyla oluşturulabilir. REST API ile bir güncelleştirme dağıtımı oluşturmak nasıl öğrenmek için bkz. [yazılım güncelleştirme yapılandırmaları - oluşturma](/rest/api/automation/softwareupdateconfigurations/create). Haftalık bir güncelleştirme dağıtımı oluşturmak için kullanılan bir örnek runbook yoktur. Bu runbook hakkında daha fazla bilgi için bkz: [bir kaynak grubundaki bir veya daha fazla sanal makineleri için haftalık bir güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanan** tablosunda oluşturduğunuz dağıtım zamanlaması görüntülenir.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.
Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/tutorial-monitoring/manageupdates-view-results.png)

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve VM’deki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu
* **Başarısız**: Güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı** kutucuğuna tıklayın.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="monitor-changes-and-inventory"></a>Değişiklikleri ve sayımı izleme

Yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili stok durumunu sorgulayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

### <a name="enable-change-and-inventory-management"></a>Değişiklik ve Sayım yönetimini etkinleştirme

Sanal makineniz için Değişiklik ve Sayım yönetimini etkinleştirme:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. Sanal makine ekranında, **İşlemler** bölümünde **Sayım** veya **Değişiklik izleme**'ye tıklayın. **Değişiklik İzleme ve Sayımı Etkinleştir** ekranı açılır.

Konumu, Log Analytics çalışma alanını ve Otomasyon hesabı'e tıklayın yapılandırma **etkinleştirme**. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir. Çözümler menüde ayrı yerlerde olsa da aynıdır. Biri etkinleştirildiğinde, sanal makinenizde her ikisi de etkinleştirilir.

![Değişiklik ve Sayım izlemeyi etkinleştirme](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra, veriler görüntülenmeden önce sanal makinede sayımın toplanması biraz zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izleme

Sanal makinenizde **İŞLEMLER** bölümünden **Değişiklik İzleme**'yi seçin. **Ayarları Düzenle**’ye tıklayın, **Değişiklik İzleme** sayfası görüntülenir. İzlemek istediğiniz ayar türünü seçin ve **+ Ekle**’ye tıklayarak ayarları yapılandırın. Windows için kullanılabilir seçenekler şunlardır:

* Windows Kayıt Defteri
* Windows Dosyaları

Değişiklik İzleme hakkında ayrıntılı bilgi için bkz. [Sanal makinedeki değişiklik sorunlarını giderme](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Sayımı görüntüleme

Sanal makinenizde **İŞLEMLER** bölümünden **Sayım**’ı seçin. **Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürüm, yayımcı, son yenilenme zamanı yer alır.

![Sayımı görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik günlüklerini ve değişiklikleri izleme

VM'nizdeki **Değişik izleme** sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev **Azure Etkinlik günlüğü** sayfasını açar. Değişiklik izleme özelliğini VM'nizin Azure etkinlik günlüğü verilerine bağlamak için **Bağlan**'ı seçin.

Bu ayar etkin durumdayken VM'nizin **Özet** sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. **Değişiklik izleme** sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Her bir olay seçilerek olayla ilgili ayrıntılı bilgiler görüntülenebilir.

![Etkinlik günlüğünde değişiklikleri görüntüleme](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir. Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler. Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder. Bu türler Linux daemon'ları, dosyalar, Windows Kayıt Defteri anahtarları, yazılımlar ve Windows hizmetleridir. Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

[Azure Otomasyonu](../../automation/automation-intro.md) tarafından sağlanan Güncelleştirme Yönetimi ve Değişiklik ve Sayım gibi çözümleri kullanarak sanal makinenizin daha gelişmiş izlemesini gerçekleştirebilirsiniz.

Log Analytics çalışma alanına eriştiğinizde, **AYARLAR** bölümünden **Gelişmiş ayarlar**’ı seçerek çalışma alanı anahtarını ve çalışma alanı tanıtıcısını bulabilirsiniz. Kullanım [kümesi AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) Microsoft Monitoring agent uzantısını VM'ye eklemek için komutu. Aşağıdaki örnekte yer alan değerleri, Log Analytics çalışma alanı anahtarınızı ve çalışma alanı kimliğinizi yansıtacak şekilde güncelleştirin.

```powershell
$workspaceId = "<Replace with your workspace Id>"
$key = "<Replace with your primary key>"

Set-AzVMExtension -ResourceGroupName "myResourceGroupMonitor" `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName "myVM" `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $workspaceId} `
  -ProtectedSettings @{"workspaceKey" = $key} `
  -Location "East US"
```

Birkaç dakika sonra yeni sanal makineyi Log Analytics çalışma alanında görmeniz gerekir.

![Log Analytics çalışma alanı dikey penceresi](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Güvenlik Merkezi'yle sanal makineleri yapılandırdınız ve gözden geçirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Kaynak grubu ve sanal makine oluşturma
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * Konak ölçümlerini görüntüleme
> * Tanılama uzantısını yükleme
> * VM ölçümlerini görüntüleme
> * Uyarı oluşturma
> * Windows güncelleştirmelerini yönetme
> * Değişiklikleri ve sayımı izleme
> * Gelişmiş izlemeyi ayarlama

Azure güvenlik merkezi hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](./tutorial-azure-security.md)
