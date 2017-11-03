---
title: "İzleme ve Linux sanal makineleri azure'da güncelleştirme | Microsoft Docs"
description: "Önyükleme tanılaması ve performans ölçümlerini izlemek ve azure'da bir Linux sanal makinede paket güncelleştirmelerini yönetme hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 70c17d9a8f7bf6d9106efcb56eee7cd996460c18
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Nasıl izleneceğini ve azure'da bir Linux sanal makine güncelleştirmesi

Azure, sanal makineleri (VM'ler) düzgün çalışmasını sağlamak için önyükleme Tanılaması, performans ölçümleri gözden geçirin ve paket güncelleştirmesi yönetebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önyükleme tanılaması VM'de etkinleştir
> * Önyükleme tanılamasını görüntüleme
> * Ana bilgisayar metrikleri görüntüleyin
> * VM'de tanılama uzantısını etkinleştirme
> * VM metrikleri görüntüleyin
> * Tanılama ölçümleri temel uyarı oluşturma
> * Paket güncelleştirmelerini yönetme
> * Gelişmiş izleme işlevini ayarlama


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>VM oluşturma

Tanılama ve eylem ölçümlerini görmek için bir VM gerekir. İlk olarak, bir kaynak grubu ile oluşturmak [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupMonitor* içinde *eastus* konumu.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi bir VM oluşturmak [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Önyükleme tanılaması etkinleştir

Linux VM'ler önyükleme gibi önyükleme tanılama uzantısını önyükleme çıkış yakalar ve Azure depolama alanında depolar. Bu veriler, VM önyükleme sorunlarını gidermek için kullanılabilir. Azure CLI kullanarak bir Linux VM oluştururken önyükleme tanılaması otomatik olarak etkin değildir.

Önyükleme tanılaması etkinleştirmeden önce bir depolama hesabı önyükleme günlüklerini depolamak için oluşturulması gerekir. Depolama hesapları 3 ile 24 karakter arasında olması genel olarak benzersiz bir ad olmalıdır ve yalnızca sayılar ve küçük harf içermelidir. İle depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#create) komutu. Bu örnekte, rastgele bir dize benzersiz depolama hesabı adı oluşturmak için kullanılır. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Önyükleme tanılaması etkinleştirirken, blob depolama kapsayıcısını URI gereklidir. Aşağıdaki komutu bu URI döndürmek için depolama hesabı sorgular. URI değeri değişken adlarında depolanan *bloburi*, sonraki adımda kullanılır.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Önyükleme Tanılaması ile şimdi etkinleştirmek [az vm önyükleme-tanılamayı etkinleştirin](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). `--storage` URI toplanan önceki adımda blob bir değerdir.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Önyükleme tanılaması etkin olduğunda, durdurmak ve VM başlatmak her zaman önyükleme işlemi bilgilerini bir günlük dosyasına yazılır. Bu örnekte, ilk VM ile serbest bırakma [az vm serbest bırakma](/cli/azure/vm#deallocate) gibi komut:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

VM ile Şimdi Başlat [az vm başlangıç]( /cli/azure/vm#stop) gibi komut:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Önyükleme tanılama verilerini alma *myVM* ile [az vm önyükleme tanılaması get-önyükleme-günlük](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) gibi komut:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Ana bilgisayar metrikleri görüntüleyin

Bir Linux VM ayrılmış bir ana bilgisayar ile etkileşime giren Azure sahiptir. Ölçümleri ana bilgisayar için otomatik olarak toplanır ve Azure portalında şu şekilde görüntülenebilir:

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroupMonitor**ve ardından **myVM** kaynak listesinde.
1. VM konak nasıl gerçekleştirmekte görmek için tıklatın **ölçümleri** VM dikey penceresinde herhangi birini seçip *[ana bilgisayar]* altında ölçümleri **kullanılabilir ölçümler**.

    ![Ana bilgisayar metrikleri görüntüleyin](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleyin

> [!IMPORTANT]
> Bu belge Linux tanılama kullanım dışı bırakılmış uzantısının 2.3 sürümünü açıklar. Sürüm 2.3 30 Haziran 2018 kadar desteklenmez.
>
> Linux tanılama uzantısını 3.0 sürümü yerine etkinleştirilebilir. Daha fazla bilgi için bkz: [belgelerine](./diagnostic-extension.md).

Temel ana ölçümleri kullanılabilir, ancak daha ayrıntılı ve VM özgü ölçümleri, VM Azure tanılama uzantısını yüklemeniz gerekir. Azure tanılama uzantısını ek izleme ve tanılama verilerini sanal makineden alınmasına izin verir. Bu performans ölçümleri görüntüleyebilir ve VM nasıl gerçekleştireceğini temelinde uyarılar oluşturabilir. Tanılama uzantısını Azure portalı üzerinden aşağıdaki şekillerde yüklenir:

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
1. Tıklatın **tanılama ayarları**. Liste gösterir *önyükleme tanılama* önceki bölümden zaten etkin. Onay kutusu *temel ölçümleri*.
1. İçinde *depolama hesabı* bölümünü bulun ve seçin *mydiagdata [1234]* önceki bölümde oluşturduğunuz hesabı.
1. **Kaydet** düğmesine tıklayın.

    ![Tanılama metrikleri görüntüleyin](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>VM metrikleri görüntüleyin

VM ölçümleri VM ölçümleri konak görüntülediğiniz şekilde görüntüleyebilirsiniz:

1. Azure portalında tıklatın **kaynak grupları**seçin **myResourceGroup**ve ardından **myVM** kaynak listesinde.
1. VM nasıl gerçekleştirmekte görmek için tıklatın **ölçümleri** VM dikey ve tanılama ölçümleri altında birini seçin **kullanılabilir ölçümler**.

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

## <a name="manage-package-updates"></a>Paket güncelleştirmelerini yönetme

Güncelleştirme yönetimi kullanarak Azure Linux VM'ler için paket güncelleştirmelerini ve düzeltme eklerini yönetebilirsiniz. Doğrudan, VM'den hızlı bir şekilde kullanılabilir güncelleştirmeleri durumunu değerlendirmek, gerekli güncelleştirmeleri yüklemesini zamanlayabilir ve güncelleştirmelerini VM'ye başarıyla uygulandığını doğrulamak için dağıtım sonuçları gözden geçirin.

Fiyatlandırma bilgileri için bkz: [Otomasyon güncelleştirme yönetimi için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management-preview"></a>Güncelleştirme yönetimi (Önizleme) etkinleştir

Güncelleştirme yönetimi, VM için etkinleştirme

1. Ekranın sol tarafında seçin **sanal makineleri**.
1. Listeden bir VM seçin.
1. VM ekranında içinde **Operations** 'yi tıklatın **güncelleştirme yönetimi**. **Etkinleştirmek güncelleştirme yönetimi** ekranı açılır.

Güncelleştirme yönetimi bu VM için etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Günlük analizi çalışma alanı ve bağlantılı Otomasyon hesabı için doğrulama içerir ve çalışma alanında çözümüdür.

Günlük analizi çalışma alanı özellikleri ve güncelleştirme yönetimi gibi hizmetleri tarafından oluşturulan verileri toplamak için kullanılır. Çalışma alanını gözden geçirin ve birden fazla kaynaktan verileri çözümlemek için tek bir konum sağlar. Güncelleştirmeleri gerektiren sanal makinelerin başka bir eylem gerçekleştirmek için Azure Otomasyonu VM karşı betikleri gibi karşıdan yüklemek ve güncelleştirmeleri uygulamak için çalıştırmanızı sağlar.

Doğrulama işlemini de VM Microsoft İzleme Aracısı'nı (MMA) ve karma çalışanı sağlanmamışsa görmek için denetler. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi edinmek için kullanılır. 

Bu Önkoşullar karşılanmadı bir başlık çözümü etkinleştirme seçeneği veren görüntülenir.

![Güncelleştirme yönetimi yerleşik yapılandırma başlığı](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Çözümü etkinleştirmek için başlığa tıklayın. Doğrulama sonrasında eksik olması için aşağıdaki önkoşullar bulunmuşsa otomatik olarak eklenir:

* [Günlük analizi](../../log-analytics/log-analytics-overview.md) çalışma
* [Otomasyon](../../automation/automation-offering-get-started.md)
* A [karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md) VM üzerinde etkin

**Güncelleştirme yönetimini etkinleştirme** ekranı açılır. Ayarları yapılandırın ve tıklatın **etkinleştirmek**.

![Güncelleştirme yönetimi çözümü etkinleştir](./media/tutorial-monitoring/manage-updates-update-enable.png)

Çözüm etkinleştirme 15 dakika kadar sürebilir ve bu süre boyunca, tarayıcı penceresini kapatmaları değil. Çözüm etkinleştirildikten sonra VM Paket Yöneticisi'nden eksik güncelleştirmeler hakkında bilgi için günlük analizi akar.
Verilerin çözümlemesi için kullanılabilir olması 6 saat arasındaki 30 dakika sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Sonra **güncelleştirme yönetimi** çözüm etkin olduğunda, **güncelleştirme yönetimi** ekranı görüntülenir. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

![Güncelleştirme durumunu görüntüle](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlaması ve bakım penceresi izleyen bir dağıtım zamanlaması.

Tıklayarak VM için yeni bir güncelleştirme dağıtımı zamanlama **zamanlama güncelleştirme dağıtımı** en üstündeki **güncelleştirme yönetimi** ekran. İçinde **yeni güncelleştirme dağıtım** ekranında, aşağıdaki bilgileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **Dışlanacak güncelleştirmeleri** -Update'ten dışlamak için paket adını girmek için bu seçeneği kullanın.
* **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilir ya da farklı bir zaman belirtebilirsiniz. Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. Yinelenen bir zamanlama ayarlamak için Yineleme altında Yinelenen seçeneğine tıklayın.

  ![Güncelleştirme Zamanlama Ayarları ekranı](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Bakım penceresi (dakika)** - Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin.  Bu değişiklikler, tanımlanmış bakım pencerelerinin içinde gerçekleştirilen sağlamaya yardımcı olur. 

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
Dikkat **zamanlanmış** tablo oluşturduğunuz dağıtım zamanlaması gösterir.

> [!WARNING]
> Bakım penceresinde yeterli zaman varsa güncelleştirmeler yüklendikten sonra VM otomatik olarak yeniden başlatılır.

Güncelleştirme yönetimi paketleri yüklemek için VM varolan Paket Yöneticisi'ni kullanır.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirmeleriyle hatası varsa, durumudur **başarısız**.
Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için dağıtım durumu Pano güncelleştir](./media/tutorial-monitoring/manage-updates-view-results.png)

İçinde **güncelleştirme sonuçları** döşeme güncelleştirmeleri ve VM Dağıtım sonuçlarına toplam sayısı bir özeti verilmiştir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* **Girişiminde bulunulmadı** -yeterli zaman kullanılabilir tanımlanan bakım penceresi süreye göre olduğundan güncelleştirme yüklenmedi.
* **Başarılı** -güncelleştirme başarıyla indirildi ve VM'de yüklü
* **Başarısız** -indirin veya VM yüklemek güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Tıklatın **çıkış** döşeme güncelleştirme dağıtım hedefteki VM yönetmekten sorumlu runbook iş akışı görürsünüz.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="advanced-monitoring"></a>Gelişmiş izleme 

Kullanarak VM'yi izleme daha gelişmiş yapabileceğiniz [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Zaten yapmadıysanız, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) Operations Management Suite.

OMS portalı erişiminiz varsa, çalışma alanı tanımlayıcısı ve çalışma alanı anahtarı ayarları dikey penceresinde bulabilirsiniz. Değiştir < çalışma alanı anahtarı > ve < çalışma alanı kimliği > değerlerini, OMS çalışma ve daha sonra kullanabilirsiniz **az vm uzantısı kümesi** VM'ye OMS uzantısı eklemek için:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

OMS portalı günlük arama dikey görmelisiniz *myVM* ne aşağıdaki resimde gösterildiği gibi:

![OMS dikey penceresi](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide yapılandırılmış, gözden ve güncelleştirmeler için bir VM yönetilen. Şunları öğrendiniz:

> [!div class="checklist"]
> * Önyükleme tanılaması VM'de etkinleştir
> * Önyükleme tanılamasını görüntüleme
> * Ana bilgisayar metrikleri görüntüleyin
> * VM'de tanılama uzantısını etkinleştirme
> * VM metrikleri görüntüleyin
> * Tanılama ölçümleri temel uyarı oluşturma
> * Paket güncelleştirmelerini yönetme
> * Gelişmiş izleme işlevini ayarlama

Azure Güvenlik Merkezi hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](./tutorial-azure-security.md)
