---
title: Azure’da Linux sanal makinelerini izleme ve güncelleştirme | Microsoft Docs
description: Azure’da bir Linux sanal makinesinde önyükleme tanılamaları ile performans ölçümlerinin nasıl izleneceğini ve paket güncelleştirmelerinin nasıl yönetileceğini öğrenin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9ffd36da535a2e5ac4a355f429394dc4209348b7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Azure’da bir Linux sanal makinesini izleme ve güncelleştirme

Azure’daki sanal makinelerin (VM) düzgün bir şekilde çalıştığından emin olmak için önyükleme tanılamalarını ve performans ölçümlerini gözden geçirebilir ve paket güncelleştirmelerini yönetebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * Konak ölçümlerini görüntüleme
> * VM’de tanılama uzantısını etkinleştirme
> * VM ölçümlerini görüntüleme
> * Tanılama ölçümlerine bağlı uyarılar oluşturma
> * Paket güncelleştirmelerini yönetme
> * Gelişmiş izlemeyi ayarlama


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>VM oluşturma

Tanılama ve ölçüm özelliklerinin nasıl çalıştığını görmek için bir VM gerekir. Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroupMonitor* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Önyükleme tanılamasını etkinleştirme

Linux VM’lerde önyükleme yapılırken önyükleme tanılama uzantısı, önyükleme çıktısını yakalar ve bunu Azure depolama alanında depolar. Bu veriler VM önyükleme sorunlarını gidermek için kullanılabilir. Azure CLI kullanarak bir Linux VM’si oluşturduğunuzda önyükleme tanılamaları otomatik olarak etkinleştirilmez.

Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Depolama hesapları, genel olarak benzersiz bir ada sahip olmalıdır. Ad, 3 ile 24 karakter arasında olmalıdır ve yalnızca sayı ve küçük harf içermelidir. [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutuyla bir depolama hesabı oluşturun. Bu örnekte benzersiz bir depolama hesabı oluşturmak için rastgele bir dize kullanılmaktadır. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Önyükleme tanılamasını etkinleştirirken blob depolama kapsayıcısının URI’sı gerekir. Aşağıdaki komut, bu URI’yı döndürmek için depolama hesabını sorgular. URI değeri, *bloburi* adlı bir değişkende depolanmaktadır ve bu değişken sonraki adımda kullanılabilir.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Şimdi [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable) komutuyla önyükleme tanılamasını etkinleştirin. `--storage` değeri, önceki adımda toplanan blob URI’sıdır.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Önyükleme tanılaması etkinleştirildiğinde, VM’yi durdurduğunuz ve başlattığınızda her seferinde önyükleme işlemiyle ilgili bilgiler bir günlük dosyasına yazılır. Bu örnekte öncelikle [az vm deallocate](/cli/azure/vm#az_vm_deallocate) komutuyla VM’yi şu şekilde serbest bırakın:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Şimdi [az vm start]( /cli/azure/vm#az_vm_stop) komutuyla VM’yi şu şekilde başlatın:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

*myVM* için önyükleme tanılama verilerini [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) komutuyla şu şekilde alabilirsiniz:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Linux VM’si, Azure’da etkileşimde bulunduğu ayrılmış bir konağa sahiptir. Konağa ait ölçümler otomatik olarak toplanır ve Azure portalında şu şekilde görüntülenebilir:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
1. Konak VM’nin performansını görüntülemek için VM dikey penceresinde **Ölçümler**’e tıklayın ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir *[Konak]* ölçümünü seçin.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleme

> [!IMPORTANT]
> Bu belgede Linux Tanılama Uzantısının kullanım dışı olan 2.3 sürümü açıklanmaktadır. 2.3 sürümü 30 Haziran 2018’e kadar desteklenecektir.
>
> Bunun yerine Linux Tanılama Uzantısının 3.0 sürümü etkinleştirilebilir. Daha fazla bilgi edinmek için [belgelere](./diagnostic-extension.md) göz atın.

Temel konak ölçümleri kullanılabilir, ancak daha ayrıntılı bilgileri ve VM’ye özel ölçümleri görüntülemek için Azure tanılama uzantısını VM’ye yüklemeniz gerekir. Azure tanılama uzantısı, VM’den ek izleme ve tanılama verilerinin alınmasına izin verir. Bu performans ölçümlerini görüntüleyebilir ve VM performansına bağlı uyarılar oluşturabilirsiniz. Tanılama uzantısı Azure portalından şu şekilde yüklenir:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
1. **Tanılama ayarları**’na tıklayın. Listede *Önyükleme tanılaması*’nın önceki bölümde zaten etkinleştirildiği görüntülenir. *Temel ölçümler* için onay kutusuna tıklayın.
1. *Depolama hesabı* bölümünde, önceki bölümde oluşturulan *mydiagdata[1234]* hesabını bulup seçin.
1. **Kaydet** düğmesine tıklayın.

    ![Tanılama ölçümlerini görüntüleme](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>VM ölçümlerini görüntüleme

VM ölçümlerini, konak VM ölçümlerini görüntülediğiniz gibi görüntüleyebilirsiniz:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
1. VM’nin performansını görüntülemek için VM dikey penceresinde **Ölçümler**’e tıklayın ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir tanılama ölçümünü seçin.

    ![VM ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine bağlı uyarılar oluşturabilirsiniz. Uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya mevcut boş disk alanı belirli bir miktarın altına düştüğünde bildirim almak için kullanılabilir. Uyarılar Azure portalında görüntülenebilir veya e-posta ile gönderilebilir. Ayrıca oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook’larını veya Azure Logic Apps’i tetikleyebilirsiniz.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroup** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. Önce VM dikey penceresinde **Uyarı kuralları**’na ve ardından uyarılar dikey penceresinin üstündeki **Ölçüm uyarısı ekle** seçeneğine tıklayın.
4. Uyarınız için *myAlertRule* gibi bir **Ad** girin
5. CPU yüzdesi beş dakika boyunca 1,0’ı aştığında bir uyarı tetiklemek için diğer varsayılan ayarların tümünü seçili bırakın.
6. İsteğe bağlı olarak e-posta bildirimi göndermek için *E-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyebilirsiniz. Varsayılan eylem olarak portalda bir bildirim sunulur.
7. **Tamam** düğmesine tıklayın.

## <a name="manage-package-updates"></a>Paket güncelleştirmelerini yönetme

Güncelleştirme yönetimini kullanarak Azure Linux VM’leriniz için paket güncelleştirmelerini ve düzeltme eklerini yönetebilirsiniz. VM’nizden doğrudan güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin VM’ye başarılı bir şekilde uygulandığından emin olmak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management-preview"></a>Güncelleştirme yönetimini (Önizleme) etkinleştirme

VM’niz için güncelleştirme yönetimini etkinleştirme

1. Ekranın solundan **Sanal makineler**’i seçin.
1. Listeden bir VM seçin.
1. VM ekranında **İşlemler** bölümünden **Güncelleştirme yönetimi**'ne tıklayın. **Güncelleştirme yönetimini etkinleştirme** ekranı açılır.

Bu VM için Güncelleştirme yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir. Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

Log Analytics çalışma alanı, Güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır. Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar. Azure Otomasyonu, güncelleştirme yapılması gereken VM'lerde güncelleştirme indirme ve uygulama gibi ek işlemleri gerçekleştirmek için VM'ler üzerinde betikler çalıştırmanızı sağlar.

Doğrulama işlemi ayrıca VM'nin Microsoft Monitoring Agent (MMA) ve karma çalışan ile sağlanıp sağlanmadığını da kontrol eder. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır. 

Bu önkoşullar sağlanmadıysa, çözümü etkinleştirme seçeneği sunan bir başlık görüntülenir.

![Güncelleştirme Yönetimi ekleme yapılandırması başlığı](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Çözümü etkinleştirmek için başlığa tıklayın. Doğrulama sonrasında aşağıdaki önkoşullardan birinin karşılanmadığı belirlenirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon](../../automation/automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimini Etkinleştirme** ekranı açılır. Ayarları yapılandırın ve **Etkinleştir**’e tıklayın.

![Güncelleştirme yönetimi çözümünü etkinleştirme](./media/tutorial-monitoring/manage-updates-update-enable.png)

Çözümü etkinleştirmek 15 dakika sürebilir, bu süre boyunca tarayıcı penceresini kapatmayın. Çözüm etkinleştirildikten sonra VM'deki paket yöneticisinde eksik olan güncelleştirmelerle ilgili bilgiler Log Analytics'e aktarılır.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** çözümü etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görüntülenir. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanıza ve bakım pencerenize uygun bir dağıtım zamanlayın.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak VM için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **Hariç tutulacak güncelleştirmeler** - Güncelleştirmenin dışında tutulacak paketlerin adlarını girmek için bu seçeneği belirleyin.
* **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilir ya da farklı bir zaman belirtebilirsiniz. Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. Yinelenen bir zamanlama ayarlamak için Yineleme altında Yinelenen seçeneğine tıklayın.

  ![Güncelleştirme Zamanlama Ayarları ekranı](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Bakım penceresi (dakika)** - Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin.  Bu ayar, değişikliklerin sizin tanımladığınız bakım pencereleri içinde gerçekleştirilmesini sağlar. 

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanan** tablosunda oluşturduğunuz dağıtım zamanlaması görüntülenir.

> [!WARNING]
> Bakım penceresinde yeterli zaman kalırsa, güncelleştirmeler yüklendikten sonra VM yeniden başlatılır.

Güncelleştirme yönetimi, paketleri yüklemek için VM’nizdeki mevcut paket yöneticisini kullanır.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa, durum **Başarısız** olur.
Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/tutorial-monitoring/manage-updates-view-results.png)

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve VM’deki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı** - Güncelleştirme başarıyla indirildi ve VM’ye yüklendi
* **Başarısız** - Güncelleştirme indirilemedi veya VM’ye yüklenemedi.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı** kutucuğuna tıklayın.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="advanced-monitoring"></a>Gelişmiş izleme 

[Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)’i kullanarak VM’nizde daha gelişmiş bir izleme işlemi gerçekleştirebilirsiniz. Henüz kaydolmadıysanız Operations Management Suite’in [ücretsiz denemesi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) için kaydolabilirsiniz.

OMS portalına erişim sağladığınızda, çalışma alanı anahtarını ve çalışma alanı tanımlayıcısını Ayarlar dikey penceresinde bulabilirsiniz. <workspace-key> ve <workspace-id> öğelerini OMS çalışma alanınızdaki değerlerle değiştirdikten sonra OMS uzantısını VM’ye eklemek için **az vm extension set** komutunu kullanabilirsiniz:

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

OMS portalının Günlük Arama dikey penceresinde *myVM*’yi aşağıdaki resimde gösterilen şekilde görebilirsiniz:

![OMS dikey penceresi](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir VM için güncelleştirmeleri yapılandırdınız, gözden geçirdiniz ve yönetimini gerçekleştirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * Konak ölçümlerini görüntüleme
> * VM’de tanılama uzantısını etkinleştirme
> * VM ölçümlerini görüntüleme
> * Tanılama ölçümlerine bağlı uyarılar oluşturma
> * Paket güncelleştirmelerini yönetme
> * Gelişmiş izlemeyi ayarlama

Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](./tutorial-azure-security.md)
