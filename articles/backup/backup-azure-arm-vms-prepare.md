---
title: 'Azure yedekleme: sanal makineleri yedeklemek hazırlama'
description: Azure'da sanal makineleri yedeklemek için ortamınızı hazırlandığından emin olun.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: yedeklemeleri; Yedekleme;
ms.service: backup
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: 086399f669b704a0ae2c9f719906e7efa672b5b1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262521"
---
# <a name="prepare-to-back-up-azure-vms"></a>Azure sanal makinelerini yedeklemek hazırlama

Bu makalede, Azure Resource Manager tarafından dağıtılan bir bir sanal makinesini (VM) için ortamınızı hazırlamak için adımları sağlar. Yordamları gösterilen adımlar, Azure portalını kullanın. Bir sanal Makine'yi yedeklediğinizde, yedekleme verileri veya kurtarma noktaları, Kurtarma Hizmetleri yedekleme kasasında depolanır.



Koruma (veya yedekleme önce) bir Resource Manager tarafından dağıtılan sanal makine şu önkoşulların mevcut olduğundan emin olun:

* Oluşturma veya kurtarma Hizmetleri kasası tanımlama *aynı bölgede sanal makineniz*.
* Bir senaryo seçmek, yedekleme ilkenizi tanımlayın ve korunacak öğeleri tanımlama.
* Sanal makinede VM Aracısı (uzantı) yüklemesini denetleyin.
* Ağ bağlantısını denetleyin.
* Linux VM'ler için uygulamayla tutarlı yedeklemeler için yedekleme ortamınızı özelleştirmek istiyorsanız, izleyin [anlık görüntü öncesi ve anlık görüntü sonrası betiklerini yapılandırma adımları](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Bu koşullar, ortamınızda zaten mevcutsa devam [Vm'lerinizi yedekleyin](backup-azure-arm-vms.md) makalesi. Ayarlayın veya herhangi biri şu önkoşulları denetleyin gerekiyorsa, bu makalede adımlarında size yol gösterir.

## <a name="supported-operating-systems-for-backup"></a>Yedekleme için desteklenen işletim sistemleri

 * **Linux**: Azure Backup'ı destekleyen [bir Azure onayladığı bir dağıtım listesini](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), CoreOS Linux ve 32-bit işletim sistemi hariç. Dosyaları geri yükleme destekleyen Linux işletim sistemleri listesi için bkz: [dosyaları sanal makine yedekten kurtarma](backup-azure-restore-files-from-vm.md#for-linux-os).

    > [!NOTE]
    > Diğer Getir your-kendi Linux'unu dağıtımları VM Aracısı sanal makinede kullanılabilir olduğu sürece, çalışma ve Python desteği bulunduğu. Ancak, bu dağıtımları desteklenmez.
    >
 * **Windows Server**, **Windows istemci**: Windows Server 2008 R2 veya Windows 7'de, eski sürümleri desteklenmez.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Yedekleme ve bir VM geri yüklenirken uygulanan sınırlamalar
Ortamınızı hazırlama önce bu sınırlamalar anladığınızdan emin olun:

* 16'dan fazla veri diski içeren sanal makineleri yedekleme desteklenmez.
* Linux Linux birleşik anahtar Kurulum (LUKS) şifreleme ile şifrelenmiş VM'ler için yedekleme desteklenmez.
* Küme Paylaşılan birimleri (CSV) veya genişleme dosya sunucusu yapılandırması içeren VM'lerin yedeklenmesi önerilmemektedir. Bu yapıldığında, CSV yazıcılar hata beklenir. Bunlar, bir anlık görüntü görevi sırasında küme yapılandırmasında bulunan tüm sanal makineler içeren gerektirir. Azure Backup, çoklu VM tutarlılığı desteklememektedir.
* Yedekleme verileri, bir VM'ye bağlı ağ sürücülerini içermez.
* Geri yükleme sırasında mevcut bir sanal makinenin değiştirilmesi desteklenmez. VM'in mevcut olduğunda VM geri yükleme girişimi geri yükleme işlemi başarısız olur.
* Bölgeler arası yedeklemek ve geri yükleme desteklenmez.
* Yukarı arka yapılandırılırken emin **güvenlik duvarları ve sanal ağlar** depolama hesabı ayarlarını tüm ağlardan erişime izin verin.
* Depolama hesabınız için güvenlik duvarı ve sanal ağ ayarlarını yapılandırdıktan sonra seçili ağlar için seçin **güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver** Azure Backup hizmeti etkinleştirmek için bir özel durum olarak ağla kısıtlı depolama hesabına erişim. Öğe düzeyinde kurtarma ağla kısıtlı depolama hesapları için desteklenmiyor.
* Tüm genel bölgelerde Azure sanal makineleri yedekleyebilir. (Bkz [denetim listesi](https://azure.microsoft.com/regions/#services) desteklenen bölgelerin.) Aradığınız bölge bugün desteklenmiyorsa, kasa oluşturma sırasında aşağı açılan listede görünmez.
* Birden çok DC yapılandırmasının bir parçasıdır (DC) sanal makine bir etki alanı denetleyicisini geri yükleme yalnızca PowerShell aracılığıyla desteklenir. Daha fazla bilgi için bkz. [DC çoklu etki alanı denetleyicisini geri yükleme](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Anlık görüntü yazma Hızlandırıcısı etkinleştirilmiş disk üzerinde desteklenmiyor. Bu kısıtlama, sanal makinenin tüm disklerinin bir uygulamayla tutarlı anlık görüntü gerçekleştirmek için Azure Backup hizmeti yeteneğini engeller.
* Aşağıdaki özel ağ yapılandırmaları olan bir sanal makineler geri yalnızca PowerShell aracılığıyla desteklenir. Geri yükleme işlemi tamamlandıktan sonra kullanıcı arabiriminde geri yükleme iş akışı aracılığıyla oluşturulan sanal makineler bu ağ yapılandırmaları yoktur. Daha fazla bilgi için bkz. [özel ağ yapılandırmalarını geri Vm'lerle](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Sanal makineler altında Yük Dengeleyiciyi yapılandırma (iç ve dış)
  * Birden çok ayrılmış IP adreslerine sahip sanal makineler
  * Birden çok ağ bağdaştırıcısı ile sanal makineler

  > [!NOTE]
  > Azure Backup'ın destekledikleri [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), Microsoft Azure sanal makineler için kalıcı depolama için yeni bir tür. Üzerinde yönetilen diskler için desteklenir [Azure VM yedekleme yığını v2'ye](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Bir VM için kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası, yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası Ayrıca, korunan sanal makinelerle ilişkili yedekleme ilkeleri içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Üzerinde **Hub** menüsünde **Gözat**, Anahtar'a tıklayın ve **kurtarma Hizmetleri**. Yazmaya başladığınızda, giriş kaynakların listesini filtrelenir. Seçin **kurtarma Hizmetleri kasaları**.

    ![Kutuya yazarak ve sonuçlarda "Kurtarma Hizmetleri kasaları" seçme](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi görünür.
1. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde **Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    **Kurtarma Hizmetleri kasaları** bölmesi açılır. Bilgilerini sağlamak için ister **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    !["Kurtarma Hizmetleri kasaları" bölmesi](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
1. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ile 50 karakter içeren bir ad yazın. Bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.
1. Seçin **abonelik** kullanılabilir abonelik listesini görmek için. Hangi aboneliğin emin değilseniz varsayılan (veya önerilen) aboneliği. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden çok Azure aboneliği ile ilişkili olduğunda.
1. Seçin **kaynak grubu** kullanılabilir kaynak grubu listesini görmek veya **yeni** yeni bir kaynak grubu oluşturmak için. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).
1. Seçin **konumu** kasa için coğrafi bölgeyi seçin. Kasa korumak istediğiniz sanal makinelerle aynı bölgede *olmalıdır*.

   > [!IMPORTANT]
   > Sanal makinenizin bulunduğu konumu emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki sanal makineler listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma Hizmetleri kasası oluşturmanız gerekir. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
   >
   >

1. **Oluştur**’u seçin. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst alandaki durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız görmüyorsanız seçin **Yenile**.

    ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Kasanızı oluşturduğunuza göre, artık depolama çoğaltmayı nasıl ayarlayacağınızı öğrenebilirsiniz.

## <a name="set-storage-replication"></a>Depolama çoğaltma
Depolama çoğaltma seçeneğini coğrafi olarak yedekli depolama ile yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Seçenek ayarı birincil yedeklemeniz için coğrafi olarak yedekli depolama alanı olarak bırakın. Kalıcı olarak ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin.

Depolama çoğaltma ayarını düzenlemek için:

1. Üzerinde **kurtarma Hizmetleri kasaları** bölmesinde kasanızı seçin.
    Kasanız, seçtiğinizde **ayarları** (olan kasa adını en üstünde) bölmesi ve kasa ayrıntıları bölmesi açık.

   ![Kasanız yedekleme kasalarının listesinden seçin.](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

1. Üzerinde **ayarları** bölmesinde dikey ekranı aşağı kaydırarak kaydırıcıyı **Yönet** bölümünde ve seçin **Yedekleme Altyapısı**. İçinde **genel** bölümünden **yedekleme yapılandırması**. Üzerinde **yedekleme yapılandırması** bölmesinde, kasanız için depolama çoğaltma seçeneğini belirleyin. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir.

   ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Azure bir birincil yedek depolama uç noktası olarak kullanıyorsanız coğrafi olarak yedekli depolamayı kullanmaya devam edin. Azure'ı bir birincil olmayan yedekleme alanı uç noktası olarak kullanıyorsanız, yerel olarak yedekli depolamayı seçin. Depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage Çoğaltmaya genel bakış](../storage/common/storage-redundancy.md).

1. Depolama çoğaltma türü değişirse seçin **Kaydet**.

Kasanız için depolama seçeneğini belirledikten sonra VM'yi kasa ile ilişkilendirmek hazırsınız. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Yedekleme hedefi seçme, ilke ayarlama ve korunacak öğeleri tanımlama
Bir kurtarma Hizmetleri kasası ile bir sanal makineyi kaydetmeden önce aboneliğe eklenmiş herhangi bir yeni sanal makineyi tanımlamak için bulma işlemini çalıştırın. Bulma işlemi Azure Abonelikteki sanal makinelerin listesi için sorgular. Yeni sanal makineler bulunamazsa, portal bulut hizmeti adı ve ilişkili bölgesi görüntüler. Azure portalında *senaryo* olduğundan kurtarma Hizmetleri kasasında girin. *İlke* kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir.

1. Açık bir Kurtarma Hizmetleri kasanız zaten varsa 2. adıma geçin. Açık kurtarma Hizmetleri kasası yoksa açın [Azure portalında](https://portal.azure.com/). Üzerinde **Hub** menüsünde **diğer hizmetler**.

   a. Kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste girişinizi filtreler. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

      ![Kutuya yazarak ve sonuçlarda "Kurtarma Hizmetleri kasaları" seçme](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      Kurtarma Hizmetleri kasalarının listesi görünür. Aboneliğinizde hiçbir kasaları varsa, bu liste boştur.

      ![Kurtarma Hizmetleri kasaları listesinin görünümü](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

      **Ayarları** bölmesinde seçilen kasa Panosu kasası ve açın.

      ![Ayarlar bölmesini ve kasa Panosu](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
1. Kasa Panosu menüsünden seçin **yedekleme**.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

   **Yedekleme** ve **yedekleme hedefi** bölmeleri açın.

1. Üzerinde **yedekleme hedefi** bölmesinde, **iş yükünüz çalıştığı?** olarak **Azure** ve **neleri yedeklemek istiyorsunuz?** olarak  **Sanal makine**. Sonra **Tamam**’ı seçin.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Bu adım, VM uzantısını kasa ile kaydeder. **Yedekleme hedefi** bölmeyi kapatır ve **yedekleme İlkesi** bölmesi açılır.

   !["Yedekleme" ve "Yedekleme İlkesi" bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
1. Üzerinde **yedekleme İlkesi** bölmesinde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin.

   ![Yedekleme ilkesini seçme](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Seçin **Tamam** yedekleme ilkesini kasa ile ilişkilendirmek için.

   **Yedekleme İlkesi** bölmeyi kapatır ve **sanal makineleri** bölmesi açılır.
1. Üzerinde **sanal makineleri** bölmesinde seçin ve belirtilen ilkeyle ilişkilendirilecek sanal makineleri seçin **Tamam**.

   !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   Seçilen sanal makine doğrulanır. Beklenen sanal makineleri görmüyorsanız kurtarma Hizmetleri kasasıyla aynı Azure bölgesindeki sanal makinelerin olduğunu kontrol edin. Sanal makinelerin yine de görmüyorsanız, bunlar zaten başka bir kasa ile korunmayan olduğunu denetleyin. Kasa panosunda, Kurtarma Hizmetleri kasası mevcut olduğu bölgenizi görebilirsiniz.

1. Kasa için tüm ayarları tanımladığınız göre **yedekleme** bölmesinde **yedeklemeyi etkinleştir**. Bu adım, ilkeyi kasaya ve Vm'lere dağıtır. Bu adım, sanal makine için ilk kurtarma noktasını oluşturmaz.

   !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedekleme başarıyla etkinleştirildikten sonra yedekleme ilkeniz zamanlamaya göre çalışır. Sanal makinelerini şimdi görmek için bir isteğe bağlı yedekleme işi oluşturmak istiyorsanız [yedekleme işini tetikleme](./backup-azure-vms-first-look-arm.md#initial-backup).

Sanal makineyi kaydetmeden sorunlarla karşılaşırsanız, aşağıdaki bilgileri VM Aracısı'nı yükleme ve ağ bağlantısı bakın. Azure'da oluşturulan sanal makineleri koruyorsanız, aşağıdaki bilgileri büyük olasılıkla gerekmez. Ancak, sanal makinelerinizi Azure'a geçiş, VM Aracısı düzgün şekilde yüklenir ve sanal makinenizi sanal ağ ile iletişim kurabildiğinden emin olun.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı sanal makineye yükleyin.
Backup uzantısının çalışması, Azure için [VM Aracısı](../virtual-machines/extensions/agent-windows.md) Azure sanal makinesinde yüklü olmalıdır. VM'niz Azure Marketi'nden oluşturulmuşsa VM Aracısı sanal makinede zaten mevcuttur.

Aşağıdaki bilgiler nerede durumlar için sağlanan *değil* kullanarak bir VM Azure Market'te oluşturulan. **Örneğin, bir VM'nin bir şirket içi veri merkezlerinden geçişi. Böyle bir durumda, VM Aracısı sanal makineyi korumak için yüklü olması gerekir.**

**Not**: VM Aracısı'nı yükledikten sonra de Azure PowerShell'i Azure VM aracısı yüklü olan bilmesi ProvisionGuestAgent özelliğini güncelleştirmek için kullanmanız gerekir.

Azure VM'yi yedekleme konusunda sorun varsa, Azure VM Aracısı sanal makinede düzgün yüklendiğini kontrol etmek için aşağıdaki tabloyu kullanın. Tablo, Windows ve Linux Vm'leri için VM Aracısı hakkında ek bilgi sağlar.

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |[Aracı MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. |<li> Son yükleme [Linux Aracısı](../virtual-machines/extensions/agent-linux.md). Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. Aracı dağıtım deponuzdan yüklemenizi öneririz. Biz **önermediğiniz** doğrudan github'dan yükleme Linux VM Aracısı.  |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. Aracı dağıtım deponuzdan güncelleştirme öneririz. Biz **önermediğiniz** doğrudan github'dan güncelleştirme Linux VM Aracısı.<br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

### <a name="backup-extension"></a>Backup uzantısı
VM Aracısı sanal makineye yüklendikten sonra Azure Backup hizmeti yedekleme uzantısını VM Aracısı'na yükler. Yedekleme hizmeti sorunsuz bir şekilde yükseltmeleri ve düzeltme ekleri yedekleme uzantısı.

VM'nin çalışır durumda olup olmadığını Backup hizmeti yedekleme uzantısını yükler. Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak, Backup hizmeti, uzantının yüklenememiş ve kapalı dahi VM'yi yedeklemeye devam eder. Bu olarak bilinir *çevrimdışı VM*. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.

## <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma
VM anlık görüntülerini yönetmek için yedek Dahili hat Azure genel IP adreslerine bağlantısı gerekir. Doğru internet bağlantısı olmadan zaman aşımı sanal makinenin HTTP istekleri ve yedekleme işlemi başarısız olur. Örneğin--yerde--bir ağ güvenlik grubu (NSG) aracılığıyla erişim kısıtlamalarını dağıtımınız varsa, yedekleme trafiği için bir yol sağlamak için aşağıdaki seçeneklerden birini seçin:

* [Beyaz liste Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).
* Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtın.

Hangi seçeneğin kullanılacağını verirken, yönetilebilirlik, ayrıntılı bir denetim ve maliyet arasında tercihlerdir.

| Seçenek | Avantajları | Dezavantajları |
| --- | --- | --- |
| Beyaz liste IP aralıkları |Ek maliyet olmadan.<br><br>Erişim bir NSG içinde açmak için kullanmak **kümesi AzureNetworkSecurityRule** cmdlet'i. |Etkilenen yönetmek için karmaşık IP aralıklarını zamanla değişir.<br><br>Yalnızca depolama ve Azure'nın tam erişim sağlar. |
| Bir HTTP Ara sunucusunu kullanacak |Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir.<br><br>Vm'leri tek noktası internet erişimi.<br><br>Azure IP adresi değişiklikleri tabi değildir. |Ara yazılımla VM çalıştırmaya yönelik ek maliyet. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Beyaz liste Azure veri merkezi IP aralıkları
Azure veri merkezi IP aralıklarını güvenilir listeye bakın [Azure Web sitesi](https://www.microsoft.com/download/details.aspx?id=41653) IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için.

Kullanarak belirli bir bölgenin depolama bağlantılara izin vermek [hizmet etiketleri](../virtual-network/security-overview.md#service-tags). Depolama hesabına erişime izin veren kuralın internet erişimini engelleyen bir kural daha yüksek önceliğe sahip olduğundan emin olun.

![Bir bölge için depolama etiketlerle NSG](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Aşağıdaki video, hizmet etiketleri yapılandırmak için adım adım yordam boyunca size yardımcı olur:

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!NOTE]
> Depolama hizmet etiketleri ve bölgelerin listesi için bkz. [depolama için hizmet etiketleri](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>VM yedeklemeleri için bir HTTP Proxy'si kullanın
Bir VM'yi yedekleme yapıyorsanız, yedekleme uzantısını VM üzerinde bir HTTPS API'sini kullanarak Azure Depolama'ya anlık görüntü yönetimi komutları gönderir. Genel internet erişimi için yapılandırılan tek bileşen olduğundan HTTP Ara sunucusu yedekleme uzantısı trafiği yönlendirmek.

> [!NOTE]
> Kullanmanız gereken belirli ara yazılım önerilmemektedir. Yapılandırma adımları ile uyumlu bir proxy izleyen çekme emin olun.
>
>

Aşağıdaki örnek resimde üç yapılandırma adımlarının bir HTTP Ara sunucusunu kullanacak şekilde gerekli gösterir:

* Uygulama VM yolları için ortak Internet aracılığıyla VM proxy tüm HTTP trafiğini bağlı.
* VM proxy Vm'lerden sanal ağda gelen trafiğe izin verir.
* Ara sunucuya VM giden internet trafiğine izin veren güvenlik kuralı NSF kilitleme adlı ağ güvenlik grubu gerekir.

Genel internet ile iletişim kurmak için bir HTTP proxy kullanmak için aşağıdaki adımları tamamlayın.

> [!NOTE]
> Bu adımları, bu örnekte belirli adlarını ve değerlerini kullanın. Ne zaman, girme (veya yapıştırarak) kodunuzla, Ayrıntılar, dağıtımınız için adları ve değerleri kullanın.

#### <a name="step-1-configure-outgoing-network-connections"></a>1. adım: giden ağ bağlantılarını yapılandırma
###### <a name="for-windows-machines"></a>Windows makineleri için
Bu yordamı yerel sistem hesabı için proxy sunucusu yapılandırmasını ayarlar.

1. İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
1. Yükseltilmiş isteminden aşağıdaki komutu çalıştırarak Internet Explorer'ı açın:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

1. Internet Explorer'da Git **Araçları** > **Internet Seçenekleri** > **bağlantıları** > **LANAyarları**.
1. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın.
1. Internet Explorer'ı kapatın.

Aşağıdaki betik, bir makine genelindeki proxy yapılandırmasını ayarlar ve tüm giden HTTP veya HTTPS trafiği için kullanır. Bir proxy sunucusunda geçerli bir kullanıcı hesabı (yerel sistem hesabı değil) ayarladıysanız, bunları için SYSTEMACCOUNT uygulamak için bu betiği kullanın.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Proxy sunucusu oturum açma "(407) Ara sunucu kimlik doğrulaması gerekli" gözlemlerseniz, kimlik Doğrulamanızın doğru ayarlandığını kontrol edin.
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

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>2. adım: proxy sunucusunda gelen bağlantılara izin verin
1. Proxy sunucusunda Windows Güvenlik Duvarı'nı açın. Güvenlik Duvarı erişmek için en kolay yolu için arama gerçekleştirmektir **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
1. İçinde **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** iletişim kutusunda sağ **gelen kuralları** seçip **yeni kural**.
1. Yeni gelen kuralı sihirbazında, üzerinde **kural türü** sayfasında **özel** seçeneğini işaretleyip **sonraki**.
1. Üzerinde **Program** sayfasında **tüm programlar** seçip **sonraki**.
1. Üzerinde **protokol ve bağlantı noktaları** sayfasında aşağıdaki bilgileri girin ve seçin **sonraki**:
   * İçin **protokol türü**seçin **TCP**.
   * İçin **yerel bağlantı noktası**seçin **belirli bağlantı noktaları**. Aşağıdaki kutuya yapılandırılmış olan proxy bağlantı noktası numarasını belirtin.
   * İçin **uzak bağlantı noktası**seçin **tüm bağlantı noktaları**.

Sonuna ulaşana kadar Sihirbazı için varsayılan ayarları kabul edin. Ardından bu kural, bir ad verin.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>3. adım: bir özel durum kuralı için bir NSG ekleyin.
Aşağıdaki komut, NSG ile bir özel durum ekler. Bu özel bağlantı noktası 80 (HTTP) veya 443 (HTTPS) herhangi bir Internet adresi 10.0.0.5 üzerinde herhangi bir bağlantı noktasından TCP trafiğine izin verir. Genel internet üzerindeki belirli bir bağlantı noktası gerekiyorsa, bu bağlantı noktasına eklediğinizden emin olun ```-DestinationPortRange```.

Bir Azure PowerShell komut isteminde aşağıdaki komutu girin:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](https://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
VM'yi yedekleme için ortamınızı hazırladığınız, mantıksal sonraki ilk adımınız bir yedekleme oluşturmaktır. Planlama makaleyi VM'lerin yedeklenmesi hakkında daha ayrıntılı bilgi sağlar.

* [Sanal makineleri yedekleme](backup-azure-arm-vms.md)
* [VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md)
* [Sanal makine yedeklemelerini yönetme](backup-azure-manage-vms.md)
