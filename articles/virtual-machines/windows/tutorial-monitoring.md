---
title: Azure izleme ve güncelleştirme ve Windows sanal makineleri | Microsoft Docs
description: Öğretici - izlemek ve güncelleştirmek Azure PowerShell ile Windows sanal makine
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9f8f8cb7fd267e25c83ecceb98b5faa8848fb126
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>İzleme ve Azure PowerShell ile Windows sanal makine güncelleştirmesi

Azure izleme portalı, Azure PowerShell modülü ve Azure CLI aracılığıyla erişilebilir Azure VM'lerin önyükleme ve performans verilerini toplamak ve bu verileri Azure depolama alanında depolamak için aracıları kullanır. Güncelleştirme yönetimi, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir VM üzerinde önyükleme tanılamayı etkinleştir
> * Önyükleme tanılamasını görüntüleme
> * VM konak metrikleri görüntüleyin
> * Tanılama uzantısını yükleyin
> * VM ölçümlerini görüntüleme
> * Bir uyarı oluşturabilir.
> * Windows güncelleştirmelerini yönetme
> * İzleyici değişiklikleri ve stok
> * Gelişmiş izlemeyi ayarlama

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, bu [betik örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilir. Öğreticide çalışırken, kaynak grubu, VM adını ve konumunu değiştirmek gerektiğinde.

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Windows sanal makineleri yedeklemek önyükleme gibi önyükleme Tanılama Aracı sorun giderme amacıyla kullanılabilir ekran çıkışına yakalar. Bu özellik varsayılan olarak etkindir. Yakalanan ekran görüntüleri de varsayılan olarak oluşturulan bir Azure depolama hesabında depolanır.

Önyükleme tanılama verilerini alma [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) komutu. Aşağıdaki örnekte, önyükleme tanılaması kök dizinine yüklenir * c:\* sürücü.

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Bir Windows VM adanmış bir konak VM ile etkileşime giren Azure sahiptir. Ölçümleri ana bilgisayar için otomatik olarak toplanır ve Azure Portalı'nda görüntülenebilir.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. Tıklatın **ölçümleri** VM dikey ve altında ana ölçümleri birini seçin **kullanılabilir ölçümler** konak VM nasıl gerçekleştirmekte görmek için.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleme

Temel ana ölçümleri kullanılabilir, ancak daha ayrıntılı ve VM özgü ölçümleri, VM Azure tanılama uzantısını yüklemeniz gerekir. Azure tanılama uzantısı, VM’den ek izleme ve tanılama verilerinin alınmasına izin verir. Bu performans ölçümlerini görüntüleyebilir ve VM performansına bağlı uyarılar oluşturabilirsiniz. Tanılama uzantısı Azure portalından şu şekilde yüklenir:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. **Tanılama ayarları**’na tıklayın. Listede *Önyükleme tanılaması*’nın önceki bölümde zaten etkinleştirildiği görüntülenir. *Temel ölçümler* için onay kutusuna tıklayın.
3. Tıklatın **Konuk düzeyinde izlemeyi etkinleştir** düğmesi.

    ![Tanılama ölçümlerini görüntüleme](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>VM ölçümlerini görüntüleme

VM ölçümlerini, konak VM ölçümlerini görüntülediğiniz gibi görüntüleyebilirsiniz:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. VM’nin performansını görüntülemek için VM dikey penceresinde **Ölçümler**’e tıklayın ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir tanılama ölçümünü seçin.

    ![VM ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine bağlı uyarılar oluşturabilirsiniz. Uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya mevcut boş disk alanı belirli bir miktarın altına düştüğünde bildirim almak için kullanılabilir. Uyarılar Azure portalında görüntülenebilir veya e-posta ile gönderilebilir. Ayrıca oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook’larını veya Azure Logic Apps’i tetikleyebilirsiniz.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. Önce VM dikey penceresinde **Uyarı kuralları**’na ve ardından uyarılar dikey penceresinin üstündeki **Ölçüm uyarısı ekle** seçeneğine tıklayın.
3. Uyarınız için *myAlertRule* gibi bir **Ad** girin
4. CPU yüzdesi beş dakika boyunca 1,0’ı aştığında bir uyarı tetiklemek için diğer varsayılan ayarların tümünü seçili bırakın.
5. İsteğe bağlı olarak e-posta bildirimi göndermek için *E-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyebilirsiniz. Varsayılan eylem olarak portalda bir bildirim sunulur.
6. **Tamam** düğmesine tıklayın.

## <a name="manage-windows-updates"></a>Windows güncelleştirmelerini yönetme

Güncelleştirme yönetimi, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar.
VM’nizden doğrudan güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin VM’ye başarılı bir şekilde uygulandığından emin olmak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştirme

Güncelleştirme yönetimi, VM için etkinleştir:

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

Çözümü etkinleştirmek için Log Analytics çalışma alanını ve otomasyon hesabını seçip **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon](../../automation/automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme](./media/tutorial-monitoring/manageupdates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir. Çözüm etkinleştirildikten sonra VM'deki eksik güncelleştirmeler hakkında bilgiler Log Analytics'e aktarılır. Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. Güncelleştirme değerlendirme tamamlandıktan sonra eksik güncelleştirmelerin listesini gördüğünüz **eksik güncelleştirmeler** sekmesi.

 ![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak VM için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **Güncelleştirme sınıflandırması**: Güncelleştirme dağıtımının dağıtıma dahil olan yazılım türlerini seçin. Sınıflandırma türleri şunlardır:
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

  ![Güncelleştirme Zamanlama Ayarları ekranı](./media/tutorial-monitoring/manageupdates-schedule-win.png)

* **Bakım penceresi (dakika)** - Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin.  Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanan** tablosunda oluşturduğunuz dağıtım zamanlaması görüntülenir.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için VM otomatik olarak yeniden başlatılır.

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

## <a name="monitor-changes-and-inventory"></a>İzleyici değişiklikleri ve stok

Yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili stok durumunu sorgulayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

### <a name="enable-change-and-inventory-management"></a>Etkin değişiklik ve envanter yönetimi

Etkin değişiklik ve envanter yönetimi, VM için:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. VM ekranında içinde **Operations** 'yi tıklatın **stok** veya **değişiklik izleme**. **Değişiklik izleme etkinleştirmek ve stok** ekranı açılır.

Kullanılacak konumu, Log Analytics çalışma alanını ve Otomasyon hesabını yapılandırdıktan sonra **Etkinleştir**'e tıklayın. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir. Eventhough çözümleri menüsünde ayrı, aynı çözüm olur. Bir etkinleştirme, VM için her ikisini de sağlar.

![Değişiklik ve izleme envanterini etkinleştir](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra veri görünmeden önce stok VM toplanacak sırada biraz zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izle

VM seçin üzerinde **değişiklik izleme** altında **OPERATIONS**. Tıklatın **ayarlarını Düzenle**, **değişiklik izleme** sayfası görüntülenir. İzleme ve ardından istediğiniz ayar türünü seçin **+ Ekle** ayarlarını yapılandırmak için. Windows için kullanılabilir seçenekler şunlardır:

* Windows Registry
* Windows dosyaları

Değişiklik izleme Bkz hakkında ayrıntılı bilgi için [sorun giderme VM üzerindeki değişiklikler](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Envanterini görüntüleme

VM seçin üzerinde **stok** altında **OPERATIONS**. **Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar yazılım adı, sürüm, yayımcı, son yenilendi saati içerir.

![Envanterini görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik Günlükleri ve değişiklikleri izleme

VM'nizdeki **Değişik izleme** sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev **Azure Etkinlik günlüğü** sayfasını açar. Değişiklik izleme özelliğini VM'nizin Azure etkinlik günlüğü verilerine bağlamak için **Bağlan**'ı seçin.

Bu ayar etkin durumdayken VM'nizin **Özet** sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. **Değişiklik izleme** sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Her olay olay hakkında ayrıntılı bilgileri görüntülemek için seçilebilir.

![Etkinlik günlüğünde değişiklikleri görüntüleme](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir. Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler. Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder. Bu türler Linux daemon'ları, dosyalar, Windows Kayıt Defteri anahtarları, yazılımlar ve Windows hizmetleridir. Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Güncelleştirme yönetimi ve değişiklik ve envanter tarafından sağlanan gibi çözümlerinden kullanarak VM'yi izleme daha gelişmiş yapabileceğiniz [Azure Otomasyonu](../../automation/automation-intro.md).

Günlük analizi çalışma alanına erişim sahip olduğunuzda, çalışma alanı anahtarı ve çalışma tanımlayıcısı seçerek bulabilirsiniz **Gelişmiş ayarları** altında **ayarları**. Kullanım [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) VM'ye Microsoft Monitoring agent uzantısı eklemek için komutu. Değişken değerleri güncelleştirmek günlük analizi çalışma alanı anahtarı ve çalışma alanı kimliği yansıtmak için örnek aşağıda

```powershell
$workspaceId = "<Replace with your workspace Id>"
$key = "<Replace with your primary key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $workspaceId} `
  -ProtectedSettings @{"workspaceKey" = $key} `
  -Location eastus
```

Birkaç dakika sonra yeni VM günlük Anaytics çalışma alanında görmeniz gerekir.

![OMS dikey penceresi](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide yapılandırılmış ve sanal makineleri Azure Güvenlik Merkezi ile gözden. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Bir kaynak grubu ve VM oluşturma
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * Konak ölçümlerini görüntüleme
> * Tanılama uzantısını yükleyin
> * VM ölçümlerini görüntüleme
> * Bir uyarı oluşturabilir.
> * Windows güncelleştirmelerini yönetme
> * İzleyici değişiklikleri ve stok
> * Gelişmiş izlemeyi ayarlama

Azure Güvenlik Merkezi hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](./tutorial-azure-security.md)