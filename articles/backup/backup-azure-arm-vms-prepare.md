---
title: "Azure yedekleme: sanal makineleri yedeklemek hazırlama | Microsoft Docs"
description: "Azure sanal makineleri yedeklemek için ortamınızı hazır olduğundan emin olun."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: yedeklemeleri; Yedekleme;
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/3/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: b8a770323d115390d323352826457eee62be5f6f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Resource Manager ile dağıtılan sanal makineleri yedeklemek için ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Resource Manager modeli](backup-azure-arm-vms-prepare.md)
> * [Klasik modeli](backup-azure-vms-prepare.md)
>
>

Bu makalede, bir Resource Manager tarafından dağıtılan sanal makinesini (VM) yedeklemek için ortamınızı hazırlama için adımları sağlar. Yordamda gösterildiği adımları Azure Portalı'nı kullanın.  

Azure Backup hizmeti Vm'lerinizi koruma için kasa (kasa ve kurtarma Hizmetleri kasaları yedekleme) iki tür vardır. Bir yedekleme kasası Klasik dağıtım modeli kullanılarak dağıtılan sanal makineleri korur. Bir kurtarma Hizmetleri kasası korur **hem Klasik dağıtılan, hem de Resource Manager tarafından dağıtılan VM'ler**. Resource Manager tarafından dağıtılan VM korumak için bir kurtarma Hizmetleri kasası kullanmanız gerekir.

> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bkz: [Azure sanal makineleri yedeklemek için ortamınızı hazırlama](backup-azure-vms-prepare.md) Klasik dağıtımı ile çalışma hakkında ayrıntılar için model VM'ler.
>
>

Koruyabilir veya bir Resource Manager tarafından dağıtılan sanal makineyi (VM) geri önce şu önkoşulların mevcut olması emin olun:

* Bir kurtarma Hizmetleri kasası oluşturma (veya var olan bir kurtarma Hizmetleri kasası tanımlama) *VM ile aynı konumda*.
* Bir senaryo seçmek, yedekleme ilkesi tanımlama ve korunacak öğeleri tanımlama.
* Sanal makine üzerinde VM Aracısı yüklemesini inceleyin.
* Ağ bağlantısını denetleyin
* Uygulama tutarlı yedekleme ortamınızı özelleştirmek istediğiniz durumda Linux VM'ler için yedeklemeler izleyin lütfen [anlık görüntü öncesi ve anlık görüntü sonrası betiklerini yapılandırma adımları](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Bu koşullar zaten ortamınızda mevcut sonra devam biliyorsanız [VM'ler Makalenizi yedekleme](backup-azure-vms.md). Ayarlayın veya çek, bu Önkoşullar hiçbirini gerekiyorsa, bu makalede önkoşul hazırlamak için adımlarda size yol gösterir.

##<a name="supported-operating-system-for-backup"></a>Yedekleme için desteklenen işletim sistemi
 * **Linux**: Azure Backup, [Azure tarafından onaylanan bir dağıtım listesini](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (CoreOS Linux hariç) destekler. _Diğer Getir-bilgisayarınızı-kendi-Linux dağıtımları da VM aracısının sanal makinede kullanılabilir olduğu sürece çalışır ve Python var. desteği. Ancak, biz yedekleme için bu dağıtımları desteklemiyorsanız._
 * **Windows Server**:  Windows Server 2008 R2’den eski sürümler desteklenmez.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Yedekleme ve geri yükleme VM sınırlamaları
Ortamınızı hazırlama önce lütfen sınırlamaları anlayın.

* 16'dan fazla veri diskleri içeren sanal makineleri yedekleme desteklenmiyor.
* Sanal makineler verilerle disk boyutları 1023 GB'den büyük yedekleme desteklenmiyor.

> [!NOTE]
> Özel önizleme olan VM'ler için yedeklemeler desteklemek için sahip olduğumuz > 1TB yönetilmeyen diskler. Ayrıntı için [büyük disk VM yedekleme desteği için özel Önizleme](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

* Ayrılmış bir IP adresi ve tanımlanmış hiçbir uç nokta ile sanal makineleri yedekleme desteklenmiyor.
* Yalnızca BEK kullanılarak şifrelenmiş VM'lerin yedeklenmesi desteklenmez. Linux VM'ler LUKS şifrelemesi kullanılarak şifrelenmiş yedeklemesi desteklenmiyor.
* Anlık görüntü görev sırasında küme yapılandırmasında bulunan tüm VM'ler içeren gerektiren Küme Paylaşılan Volumes(CSV) veya ölçek dosya sunucusu yapılandırması içeren VM'ler yedeğini önerilmez. Azure yedekleme çoklu VM tutarlılığını desteklemiyor. 
* Yedekleme verilerini bağlı ağ sürücülerine VM'ye ekli içermez.
* Geri yükleme sırasında mevcut bir sanal makinenin değiştirilmesi desteklenmez. VM VM mevcut olduğunda geri yüklemeye geri yükleme işlemi başarısız olur.
* Çapraz bölge yedekleme ve geri yükleme desteklenmez.
* Tüm ortak bölgelerde Azure sanal makineleri yedekleyebilirsiniz (bkz [denetim listesi](https://azure.microsoft.com/regions/#services) desteklenen bölgeler). Aradığınız bölge bugün desteklenmiyorsa, kasa oluşturma sırasında açılan listede görünmez.
* Bir etki alanı denetleyicisini geri multi-DC yapılandırmasının bir parçası olan (DC) VM yalnızca PowerShell aracılığıyla desteklenir. Daha fazla bilgi edinin [multi-DC etki alanı denetleyicisini geri](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Aşağıdaki özel ağ yapılandırmalarının sanal makineleri geri yüklenmesi yalnızca PowerShell aracılığıyla desteklenir. Geri yükleme işlemi tamamlandıktan sonra geri yükleme iş akışı kullanıcı Arabiriminde kullanılarak oluşturulan sanal makineleri bu ağ yapılandırmaları sahip olmaz. Daha fazla bilgi için bkz: [geri VM'ler özel ağ yapılandırmaları ile](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Sanal makineler yük dengeleyici yapılandırması (dahili ve harici)
  * Birden çok ayrılmış IP adreslerine sahip sanal makineler
  * Birden çok ağ bağdaştırıcısı ile sanal makineler

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Bir VM için kurtarma hizmetleri kasası oluşturma
Bir kurtarma Hizmetleri kasası yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma hizmetleri kasası oluşturmak için:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Gözat düğmesine tıklayın ve kurtarma Hizmetleri yazın. Kurtarma Hizmetleri kasası seçeneğini gördüğünüzde, Kurtarma Hizmetleri kasası dikey penceresini açmak için tıklatın.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi görüntülenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Kullanılabilir abonelik listesini görmek için **Abonelik** seçeneğine tıklayın. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Ancak kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olacaktır.
6. Kullanılabilir Kaynak grubu listesini görmek için **Kaynak grubu** seçeneğine, yeni bir Kaynak grubu oluşturmak için de **Yeni** seçeneğine tıklayın. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md)
7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Kasa korumak istediğiniz sanal makinelerle aynı bölgede **olmalıdır**.

   > [!IMPORTANT]
   > VM'nizin hangi konumda bulunduğundan emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki Virtual Machines listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturmanız gerekir. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerekmez; Kurtarma Hizmetleri kasası ve Azure Backup hizmeti bunu otomatik olarak gerçekleştirir.
   >
   >

8. **Oluştur**'a tıklayın. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalda sağ üst alandaki durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız tıklatın **yenileme** için

    ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Kasanızı oluşturduğunuza göre, artık depolama çoğaltmayı nasıl ayarlayacağınızı öğrenebilirsiniz.

## <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Bu, birincil yedeklemenizse seçeneği coğrafi olarak yedekli depolamaya ayarlanmış şekilde bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin.

Depolama çoğaltma ayarını düzenlemek için:

1. Üzerinde **kurtarma Hizmetleri kasaları** dikey penceresinde kasanızı seçin.
    Ayarlar dikey penceresinde kasanızı tıkladığınızda (*en üstünde kasasının adına sahip*) ve kasa ayrıntıları dikey penceresi açılır.

    ![Yedekleme kasalarının listesinden kasanızı seçin](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Üzerinde **ayarları** dikey penceresinde, aşağı kaydırarak için dikey kaydırıcıyı kullanın **Yönet** bölümü. Tıklatın **Yedekleme Altyapısı** kendi dikey penceresini açın. İçinde **genel** bölümünde **yedekleme yapılandırması** kendi dikey penceresini açın. **Yedekleme Yapılandırması** dikey penceresinde, kasanıza yönelik depolama çoğaltma seçeneğini belirleyin. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Depolama çoğaltma türü değiştirirseniz, tıklatın **kaydetmek**.

    ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Azure'ı birincil yedekleme alanı uç noktası olarak kullanıyorsanız coğrafi olarak yedekli depolamayı kullanmaya devam edin. Azure birincil olmayan yedekleme alanı uç noktası olarak kullanıyorsanız, yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/common/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.
    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Yedekleme hedefi seçme, ilke ayarlama ve korunacak öğeleri tanımlama
VM'yi bir kasaya kaydetmeden önce, aboneliğe eklenmiş olan yeni sanal makinelerin tanımlandığından emin olmak için bulma işlemini çalıştırın. Bu işlem, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte abonelikteki sanal makinelerin listesi için Azure'ı sorgular. Azure portalda, senaryo, kurtarma hizmetleri kasasına yerleştireceğiniz içeriğe başvuruda bulunur. İlke, kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir.

1. Açık bir Kurtarma Hizmetleri kasanız zaten varsa 2. adıma geçin. Açık kurtarma Hizmetleri kasası yoksa açın [Azure portal](https://portal.azure.com/) ve Hub menüsünde **daha fazla hizmet**.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Kurtarma Hizmetleri kasalarının listesi görünür. Aboneliğinizde hiçbir kasalarını varsa, bu listenin boş olur.

    ![Kurtarma Hizmetleri kasaları listesinin görünümü](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Kurtarma Hizmetleri kasalarının listesinden kendi panosunu açmak için bir kasa seçin.

     Ayarlar dikey ve seçilen kasa için kasa panosu açılır.

     ![Kasa dikey penceresini açma](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Kasa panosu menüsünden Yedekleme dikey penceresini açmak için **Yedekle** seçeneğine tıklayın.

    ![Yedekleme dikey penceresini açma](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Yedekleme ve Yedekleme Hedefi dikey pencereleri açılır.

    ![Senaryo dikey penceresini açma](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Yedekleme hedefi dikey penceresinde ayarlamak **, iş yükünü çalıştırdığı** Azure ve **neleri yedeklemek istiyorsunuz** sanal makineye'ye tıklayın **Tamam**.

    Bu, VM uzantısını kasa ile kaydeder. Yedekleme Hedefi dikey penceresi kapanır ve **Yedekleme ilkesi** dikey penceresi açılır.

    ![Senaryo dikey penceresini açma](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Yedekleme ilkesi dikey penceresinde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin.

    ![Yedekleme ilkesini seçme](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Yedekleme ilkesini kasa ile ilişkilendirmek için **Tamam**’a tıklayın.

    Yedekleme ilkesi dikey penceresi kapanır ve **Sanal makineleri seçin** dikey penceresi açılır.
5. **Sanal makineleri seçin** dikey penceresinde belirtilen ilkeyle ilişkilendirilecek sanal makineleri seçin ve **Tamam**'a tıklayın.

    ![İş yükünü seçme](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Seçilen sanal makine doğrulanır. Görmeyi beklediğiniz sanal makineleri görmüyorsanız kurtarma Hizmetleri kasasıyla aynı Azure konumunda bulunan ve zaten başka bir kasaya korunmayan denetleyin. Kurtarma Hizmetleri kasasının konumu kasa panosunda gösterilir.

6. Kasa için tüm ayarları tanımladığınıza göre, Yedekleme dikey penceresinde **Yedeklemeyi Etkinleştir** seçeneğine tıklayın. Bu, ilkeyi kasaya ve VM'lere dağıtır. Bu, sanal makine için ilk kurtarma noktasını oluşturmaz.

    ![Yedeklemeyi Etkinleştirme](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedekleme başarıyla etkinleştirildikten sonra yedekleme ilkeniz ilgili zamanda çalıştırılır. Şimdi, sanal makineleri geri görmek için bir talep üzerine yedekleme işini oluşturmak istiyorsanız [yedekleme işini tetiklemeden](./backup-azure-arm-vms.md#triggering-the-backup-job).

Sanal makine kaydetme sorunları varsa, aşağıdaki bilgileri VM Aracısı'nı yükleme ve ağ bağlantısına bakın. Azure üzerinde oluşturulan sanal makineleri koruyorsanız aşağıdaki bilgileri muhtemelen gerekmez. Azure'da sanal makinelerinizi geçirdiyseniz, ancak sonra emin VM Aracısı düzgün yüklenmiş ve sanal makinenizi sanal ağ ile iletişim kurabildiğinden emin olun.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı'nı sanal makineye yükleme
Backup uzantısının çalışması için Azure VM Aracısı'nın Azure sanal makinesine yüklenmesi gerekir. VM'niz Azure galerisinden oluşturulmuşsa, daha sonra VM Aracısı sanal makineye zaten. Bu bilgiler olduğunuz durumlarda sağlanır *değil* VM kullanılarak oluşturulan Azure galerisinden - örneğin bir VM bir şirket içi veri merkezinden geçişi. Böyle bir durumda, VM aracısının sanal makineyi korumak için yüklü olması gerekir. Hakkında bilgi edinin [VM Aracısı](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Azure VM'yi yedekleme konusunda sorun yaşarsanız Azure VM Aracısı'nın sanal makineye doğru şekilde yüklenip yüklenmediğini kontrol edin (aşağıdaki tabloya bakın). Aşağıdaki tabloda, Windows ve Linux VM'ler için VM Aracısı ile ilgili ek bilgiler sağlanmıştır.

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. |<li> En son yükleme [Linux Aracısı](../virtual-machines/linux/agent-user-guide.md). Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. Aracı dağıtım depodan yüklemenizi öneririz. Biz **değil önerilir** doğrudan github'dan yükleme Linux VM Aracısı.  |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. Aracı dağıtım havuzunuzdan güncelleştirme öneririz. Biz **değil önerilir** doğrudan github'dan güncelleştirme Linux VM Aracısı.<br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

### <a name="backup-extension"></a>Backup uzantısı
VM Aracısı sanal makineye yüklendikten sonra, Azure Backup hizmeti yedekleme uzantısını VM Aracısı'na yükler. Azure Backup hizmeti sorunsuz bir şekilde yükseltir ve yedekleme uzantısını düzeltme ekleri.

Backup uzantısı, VM'nin çalışır durumda olup olmamasından bağımsız olarak Backup hizmeti tarafından yüklenir. Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak Azure Backup hizmeti, uzantının yüklenememiş olması ve VM'nin kapalı olması durumunda dahi VM'yi yedeklemeye devam eder. Buna Çevrimdışı VM adı verilir. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.

## <a name="network-connectivity"></a>Ağ bağlantısı
VM anlık görüntülerini yönetmek için Azure ortak IP adreslerine bağlantısı yedekleme uzantısını gerekir. Sağ Internet bağlantısı olmadan zaman aşımı sanal makinenin HTTP istekleri ve yedekleme işlemi başarısız olur. Dağıtımınızı (örneğin ağ güvenlik grubu (NSG)) aracılığıyla yerinde erişim sınırlamaları varsa, yedekleme trafiği için açık bir yol sağlamak için bu seçeneklerden birini seçin:

* [Beyaz liste Azure veri merkezi IP aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -beyaz liste ile IP adreslerinin nasıl hakkında yönergeler için makalesine bakın.
* Yönlendirme trafiği için bir HTTP proxy sunucusu dağıtın.

Hangi seçeneği kullanacağınıza karar verirken dengelemeler yönetilebilirlik, ayrıntılı bir denetim ve maliyet arasında ' dir.

| Seçenek | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| Beyaz liste IP aralıkları |Hiçbir ek maliyet.<br><br>Bir NSG'yi erişim açmak için kullanmak <i>kümesi AzureNetworkSecurityRule</i> cmdlet'i. |Etkilenen yönetmek için karmaşık IP aralıkları zamanla değiştirin.<br><br>Azure ve yalnızca depolama tam erişim sağlar. |
| HTTP proxy |Depolama üzerinde ayrıntılı denetim proxy'de URL'lere izin.<br>Sanal makineleri tek noktası Internet erişimi.<br>Azure IP adresi değişiklikleri tabi değildir. |Bir VM ile Ara yazılım çalıştırmak için ek ücrete. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Beyaz liste Azure veri merkezi IP aralıkları
* Lütfen Azure veri merkezi IP aralıkları, beyaz liste için bkz: [Azure Web sitesi](http://www.microsoft.com/en-us/download/details.aspx?id=41653) IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için.
* Hizmet etiketleri kullanarak belirli bir bölge depolama bağlantılara izin vermek için kullanabileceğiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags). Depolama hesabına erişim izni veren, kural Internet erişimini engelleme kural daha yüksek önceliğe sahip olduğundan emin olun. 

  ![Bir bölge için depolama etiketlerle NSG](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Depolama etiketleri yalnızca belirli bölgelerde kullanılabilir ve önizlemede. Bölgelerin bir listesi için başvurmak [etiketler için depolama hizmeti](../virtual-network/security-overview.md#service-tags)

### <a name="using-an-http-proxy-for-vm-backups"></a>VM yedeklemeler için bir HTTP proxy kullanma
VM yedeklemesi, yedekleme uzantısını VM üzerinde bir HTTPS API kullanarak Azure Storage anlık görüntü yönetimi komutları gönderir. Genel internet erişimi için yapılandırılmış tek bileşen olduğundan HTTP proxy üzerinden backup uzantısı trafiği yönlendirmek.

> [!NOTE]
> Kullanılması gereken ara yazılımı için hiçbir öneri yok. Aşağıdaki yapılandırma adımları ile uyumlu bir proxy çekme emin olun.
>
>

Aşağıdaki örnek görüntü üç yapılandırma adımlarının bir HTTP Ara sunucusunu kullanmak için gerekli gösterir:

* Uygulama VM ortak Internet Proxy VM üzerinden bağlanan tüm HTTP trafiğini yönlendirir.
* Proxy VM sanal ağ Vm'lerden gelen trafiğine izin verir.
* NSF kilitleme adlı ağ güvenlik grubu (NSG) bir güvenlik kuralı izin giden Internet trafiği Proxy VM gerekir.

![NSG ile HTTP proxy dağıtım diyagramı](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

Genel Internet'e iletişim için bir HTTP proxy kullanmak için aşağıdaki adımları izleyin:

#### <a name="step-1-configure-outgoing-network-connections"></a>1. Adım Giden ağ bağlantılarını yapılandırma
###### <a name="for-windows-machines"></a>Windows makineler için
Bu, yerel sistem hesabı için proxy sunucusu yapılandırmasını Kurulum.

1. Karşıdan [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Yükseltilmiş isteminden şu komutu çalıştırın,

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Internet explorer penceresi açar.
3. Araçlar -> Internet Seçenekleri -> bağlantıları LAN Ayarları ->.
4. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın.
5. Internet Explorer'ı kapatın.

Bu bir makine genelinde proxy yapılandırmasını ayarlanır ve tüm giden HTTP/HTTPS trafiği için kullanılır.

Geçerli kullanıcı hesabında (yerel sistem hesabı değildir) bir proxy sunucu kurulum varsa, bunları için SYSTEMACCOUNT uygulamak için aşağıdaki komut dosyasını kullanın:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Proxy sunucu oturum açma "(407) Proxy kimlik doğrulaması gerekli" gözlemlerseniz, kimlik doğrulama Kurulumu doğru ayarlandığından emin olun.
>
>

###### <a name="for-linux-machines"></a>Linux makineler için
Aşağıdaki satırı ekleyin ```/etc/environment``` dosyası:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Aşağıdaki satırları ekleyin ```/etc/waagent.conf``` dosyası:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>2. Adım Gelen bağlantılar proxy sunucusuna izin ver:
1. Proxy sunucu üzerinde Windows Güvenlik Duvarı'nı açın. Güvenlik Duvarı erişmek için kolay Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı aramak için yoludur.

    ![Güvenlik Duvarı'nı açın](./media/backup-azure-vms-prepare/firewall-01.png)
2. Windows Güvenlik Duvarı iletişim kutusunda sağ **gelen kuralları** tıklatıp **yeni kural...** .

    ![Yeni bir kural oluşturun](./media/backup-azure-vms-prepare/firewall-02.png)
3. İçinde **yeni gelen kuralı Sihirbazı**, seçin **özel** seçenek için **kural türü** tıklatıp **sonraki**.
4. Seçilecek sayfasında **Program**, seçin **tüm programlar** tıklatıp **sonraki**.
5. Üzerinde **protokol ve bağlantı noktaları** sayfasında, aşağıdaki bilgileri girin ve tıklayın **sonraki**:

    ![Yeni bir kural oluşturun](./media/backup-azure-vms-prepare/firewall-03.png)

   * için *protokol türü* seçin *TCP*
   * için *yerel bağlantı noktası* seçin *belirli bağlantı noktaları*, aşağıdaki alana belirtin ```<Proxy Port>``` yapılandırılmış.
   * için *uzak bağlantı noktası* seçin *tüm bağlantı noktaları*

     Sihirbazın geri kalanı için tüm sonuna kadar tıklayın ve bu kural bir ad verin.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>3. Adım NSG'yi bir özel durum kuralı ekleyin:
Bir Azure PowerShell komut isteminde aşağıdaki komutu girin:

Aşağıdaki komut, bir özel durum NSG'yi ekler. Bu özel bağlantı noktası 80 (HTTP) veya 443 (HTTPS) herhangi bir Internet adresi 10.0.0.5 üzerinde herhangi bir bağlantı gelen TCP trafiğine izin verir. Genel Internet belirli bir bağlantı noktası gerekiyorsa, bu bağlantı noktasına eklediğinizden emin olun ```-DestinationPortRange``` de.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*Bu adımları, bu örnek için belirli adları ve değerleri kullanın. Lütfen girerken, dağıtımınıza veya kesme ve ayrıntıları kodunuza yapıştırma adlarını ve değerlerini kullanın.*

Artık ağ bağlantısına sahip bildiğinize göre VM'yi yedeklemek hazırsınız. Bkz: [Resource Manager tarafından dağıtılan Vm'leri yedekleme](backup-azure-arm-vms.md).

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
VM yedeklemesi için ortamınızı hazırlandığınıza göre sonraki mantıksal bir yedekleme oluşturmak için adımdır. Planlama makale Vm'leri yedekleme konusunda daha ayrıntılı bilgi sağlar.

* [Sanal makineleri yedekleme](backup-azure-vms.md)
* [VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md)
* [Sanal makine yedeklerini yönetme](backup-azure-manage-vms.md)
