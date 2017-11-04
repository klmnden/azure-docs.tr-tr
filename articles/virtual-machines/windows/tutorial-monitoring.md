---
title: "Azure izleme ve güncelleştirme ve Windows sanal makineleri | Microsoft Docs"
description: "Öğretici - izlemek ve güncelleştirmek Azure PowerShell ile Windows sanal makine"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: a37aed8b3321d3518ffd73e09f5bb21266a7e577
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>İzleme ve Azure PowerShell ile Windows sanal makine güncelleştirmesi

Azure izleme portalı, Azure PowerShell modülü ve Azure CLI aracılığıyla erişilebilir Azure VM'lerin önyükleme ve performans verilerini toplamak ve bu verileri Azure depolama alanında depolamak için aracıları kullanır. Güncelleştirme yönetimi, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir VM üzerinde önyükleme tanılamayı etkinleştir
> * Önyükleme tanılamasını görüntüleme
> * VM konak metrikleri görüntüleyin
> * Tanılama uzantısını yükleyin
> * VM metrikleri görüntüleyin
> * Bir uyarı oluşturabilir.
> * Windows güncelleştirmelerini yönetme
> * Gelişmiş izleme işlevini ayarlama

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

Örneğin bu öğreticiyi tamamlamak için var olan bir sanal makine olması gerekir. Gerekirse, bu [komut dosyası örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilirsiniz. Öğreticide çalışırken, kaynak grubu, VM adını ve konumunu değiştirmek gerektiğinde.

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Windows sanal makineleri yedeklemek önyükleme gibi önyükleme Tanılama Aracı sorun giderme amacıyla kullanılabilir ekran çıkışına yakalar. Bu özellik varsayılan olarak etkindir. Yakalanan ekran görüntüleri de varsayılan olarak oluşturulan bir Azure depolama hesabında depolanır. 

Önyükleme tanılama verilerini alma [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) komutu. Aşağıdaki örnekte, önyükleme tanılaması kök dizinine yüklenir * c:\* sürücü. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Ana bilgisayar metrikleri görüntüleyin

Bir Windows VM adanmış bir konak VM ile etkileşime giren Azure sahiptir. Ölçümleri ana bilgisayar için otomatik olarak toplanır ve Azure Portalı'nda görüntülenebilir.

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
2. Tıklatın **ölçümleri** VM dikey ve altında ana ölçümleri birini seçin **kullanılabilir ölçümler** konak VM nasıl gerçekleştirmekte görmek için.

    ![Ana bilgisayar metrikleri görüntüleyin](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleyin

Temel ana ölçümleri kullanılabilir, ancak daha ayrıntılı ve VM özgü ölçümleri, VM Azure tanılama uzantısını yüklemeniz gerekir. Azure tanılama uzantısını ek izleme ve tanılama verilerini sanal makineden alınmasına izin verir. Bu performans ölçümleri görüntüleyebilir ve VM nasıl gerçekleştireceğini temelinde uyarılar oluşturabilir. Tanılama uzantısını Azure portalı üzerinden aşağıdaki şekillerde yüklenir:

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
2. Tıklatın **tanılama ayarları**. Liste gösterir *önyükleme tanılama* önceki bölümden zaten etkin. Onay kutusu *temel ölçümleri*.
3. Tıklatın **Konuk düzeyinde izlemeyi etkinleştir** düğmesi.

    ![Tanılama metrikleri görüntüleyin](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>VM metrikleri görüntüleyin

VM ölçümleri VM ölçümleri konak görüntülediğiniz şekilde görüntüleyebilirsiniz:

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
2. VM nasıl gerçekleştirmekte görmek için tıklatın **ölçümleri** VM dikey ve tanılama ölçümleri altında birini seçin **kullanılabilir ölçümler**.

    ![VM metrikleri görüntüleyin](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Uyarı oluşturma

Özel performans ölçümleri temelinde uyarılar oluşturabilirsiniz. Uyarıları, belirli bir eşiği veya kullanılabilir boş disk alanı ortalama CPU kullanımını aştığında, belirli bir miktar, örneğin bırakır bildirmek için kullanılır. Uyarılar Azure portalında gösterilen veya e-posta ile gönderilebilir. Oluşturulan uyarılara yanıt olarak, Azure Otomasyon çalışma kitabı veya Azure Logic Apps tetikleyebilir.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
2. Tıklatın **uyarı kuralları** VM dikey penceresinde, ardından **ölçüm uyarı Ekle** uyarıları dikey penceresi üstte.
4. Sağlayan bir **adı** , uyarının gibi *myAlertRule*
5. CPU yüzdesi için beş dakika 1.0 aştığında bir uyarıyı tetiklemek için seçilen tüm diğer varsayılan ayarları bırakın.
6. İsteğe bağlı olarak, onay kutusunu için *sahipleri, Katkıda Bulunanlar ve okuyucular e-posta* e-posta bildirimi gönderilecek. Portalda bildirim sunmak için varsayılan eylemdir.
7. **Tamam** düğmesine tıklayın.

## <a name="manage-windows-updates"></a>Windows güncelleştirmelerini yönetme

Güncelleştirme yönetimi, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar.
Doğrudan, VM'den hızlı bir şekilde kullanılabilir güncelleştirmeleri durumunu değerlendirmek, gerekli güncelleştirmeleri yüklemesini zamanlayabilir ve güncelleştirmelerini VM'ye başarıyla uygulandığını doğrulamak için dağıtım sonuçları gözden geçirin.

Fiyatlandırma bilgileri için bkz: [Otomasyon güncelleştirme yönetimi için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Güncelleştirme yönetimi etkinleştir

Güncelleştirme yönetimi, VM için etkinleştir:
 
1. Ekranın sol tarafında seçin **sanal makineleri**.
2. Listeden bir VM seçin.
3. VM ekranında içinde **Operations** 'yi tıklatın **güncelleştirme yönetimi**. **Güncelleştirme yönetimini etkinleştirme** ekranı açılır.

Güncelleştirme yönetimi bu VM için etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Günlük analizi çalışma alanı ve bağlantılı Otomasyon hesabı için doğrulama içerir ve çalışma alanında çözümüdür.

Günlük analizi çalışma alanı özellikleri ve güncelleştirme yönetimi gibi hizmetleri tarafından oluşturulan verileri toplamak için kullanılır. Çalışma alanını gözden geçirin ve birden fazla kaynaktan verileri çözümlemek için tek bir konum sağlar. Güncelleştirmeleri gerektiren sanal makinelerin başka bir eylem gerçekleştirmek için Azure Otomasyonu VM karşı betikleri gibi karşıdan yüklemek ve güncelleştirmeleri uygulamak için çalıştırmanızı sağlar.

Doğrulama işlemini de VM Microsoft İzleme Aracısı'nı (MMA) ve karma çalışanı sağlanmamışsa görmek için denetler. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi edinmek için kullanılır. 

Bu Önkoşullar karşılanmadı bir başlık çözümü etkinleştirme seçeneği veren görüntülenir.

![Güncelleştirme yönetimi yerleşik yapılandırma başlığı](./media/tutorial-monitoring/manageupdates-onboard-solution-banner.png)

Çözümü etkinleştirmek için başlığa tıklayın. Doğrulama sonrasında eksik olması için aşağıdaki önkoşullar bulunmuşsa otomatik olarak eklenir:

* [Günlük analizi](../../log-analytics/log-analytics-overview.md) çalışma
* [Otomasyon](../../automation/automation-offering-get-started.md)
* A [karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md) VM üzerinde etkin

**Güncelleştirme yönetimini etkinleştirme** ekranı açılır. Ayarları yapılandırın ve tıklatın **etkinleştirmek**.

![Güncelleştirme yönetimi çözümü etkinleştir](./media/tutorial-monitoring/manageupdates-update-enable.png)

Çözüm etkinleştirme 15 dakika kadar sürebilir ve bu süre boyunca, tarayıcı penceresini kapatmaları değil. Çözüm etkinleştirildikten sonra VM güncelleştirmeleri eksik hakkında bilgi için günlük analizi akar.
Verilerin çözümlemesi için kullanılabilir olması 6 saat arasındaki 30 dakika sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

 ![Güncelleştirme durumunu görüntüle](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik içerebilir veya güvenlik güncelleştirmeleri ve dışlama güncelleştirme paketleri.

Tıklayarak VM için yeni bir güncelleştirme dağıtımı zamanlama **zamanlama güncelleştirme dağıtımı** en üstündeki **güncelleştirme yönetimi** ekran. İçinde **yeni güncelleştirme dağıtım** ekranında, aşağıdaki bilgileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **Güncelleştirme sınıflandırması** -yazılım dağıtımdaki güncelleştirme dağıtım türlerini seçin. Sınıflandırma türleri şunlardır:
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
Dikkat **zamanlanmış** tablo oluşturduğunuz dağıtım zamanlaması gösterir.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için VM otomatik olarak yeniden başlatılır.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.
Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

   ![Belirli bir dağıtım için dağıtım durumu Pano güncelleştir](./media/tutorial-monitoring/manageupdates-view-results.png)

İçinde **güncelleştirme sonuçları** döşeme güncelleştirmeleri ve VM Dağıtım sonuçlarına toplam sayısı bir özeti verilmiştir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* **Girişiminde bulunulmadı** -yeterli zaman kullanılabilir tanımlanan bakım penceresi süreye göre olduğundan güncelleştirme yüklenmedi.
* **Başarılı** -Güncelleştirme başarılı oldu
* **Başarısız** -güncelleştirme işlemi başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Tıklatın **çıkış** döşeme güncelleştirme dağıtım hedefteki VM yönetmekten sorumlu runbook iş akışı görürsünüz.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="advanced-monitoring"></a>Gelişmiş izleme 

Kullanarak VM'yi izleme daha gelişmiş yapabileceğiniz [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Zaten yapmadıysanız, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) Operations Management Suite.

OMS portalı erişiminiz varsa, çalışma alanı tanımlayıcısı ve çalışma alanı anahtarı ayarları dikey penceresinde bulabilirsiniz. Kullanım [kümesi AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) VM'ye OMS uzantısı eklemek için komutu. Değişken değerleri güncelleştirmek OMS çalışma alanı anahtarı ve çalışma alanı kimliği yansıtmak için örnek aşağıda  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Birkaç dakika sonra yeni VM OMS çalışma alanında görmeniz gerekir. 

![OMS dikey penceresi](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yapılandırılmış ve sanal makineleri Azure Güvenlik Merkezi ile gözden. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Bir kaynak grubu ve VM oluşturma 
> * Önyükleme tanılaması VM'de etkinleştir
> * Önyükleme tanılamasını görüntüleme
> * Ana bilgisayar metrikleri görüntüleyin
> * Tanılama uzantısını yükleyin
> * VM metrikleri görüntüleyin
> * Bir uyarı oluşturabilir.
> * Windows güncelleştirmelerini yönetme
> * Gelişmiş izleme işlevini ayarlama

Azure Güvenlik Merkezi hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](./tutorial-azure-security.md)