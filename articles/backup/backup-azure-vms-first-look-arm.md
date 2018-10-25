---
title: 'İlk bakış: Azure sanal makinelerini bir kurtarma hizmetleri kasasıyla koruma'
description: Sanal makineleri bir kurtarma hizmetleri kasasıyla koruyun. Verilerinizi korumak için, Resource Manager tarafından dağıtılan, Klasik modunda dağıtılan VM'lerin, Premium Depolama VM'lerinin, Şifrelenmiş VM’lerin ve Yönetilen Diskler üzerindeki VM’lerin yedeklemelerini kullanın. Bir kurtarma hizmetleri kasası oluşturun ve kaydedin. Azure'da VM'leri kaydedin, ilke oluşturun ve VM'leri koruyun.
services: backup
author: markgalioto
manager: carmonm
keyword: backups; vm backup
ms.service: backup
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
keywords: yedeklemeleri; VM yedeklemesi
ms.openlocfilehash: a30b4081bf01a76c6d89e7557fbb1b40baa86fbc
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985904"
---
# <a name="back-up-azure-virtual-machines-to-recovery-services-vault"></a>Azure sanal makinelerini kurtarma Hizmetleri kasasına yedekleme

Bu makalede, sanal makineler işlem menüsünden bir sanal makine veya kurtarma Hizmetleri kasası için korumanın nasıl yapılandırılacağı açıklanmaktadır. Kurtarma Hizmetleri kasaları şunları korur:

* Azure Resource Manager tarafından dağıtılan VM'ler
* Klasik VM'ler
* Standart depolama VM'leri
* Premium depolama VM'leri
* Yönetilen Diskler üzerinde çalışan sanal makineler
* Azure Disk Şifrelemesi kullanılarak şifrelenmiş VM’ler
* Windows sanal makinelerinin VSS, Linux sanal makinelerinin özel anlık görüntü öncesi ve anlık görüntü sonrası betikler kullanılarak uygulamada tutarlı yedeklenmesi

Premium depolama VM'lerini koruma hakkında daha fazla bilgi için [Premium Storage VM'lerini Yedekleme ve Geri Yükleme](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) makalesine bakın. Yönetilen disk sanal makinelerine yönelik destek hakkında daha fazla bilgi için bkz. [Yönetilen diskler üzerindeki sanal makineleri yedekleme ve geri yükleme](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). Linux VM yedekleme için ön ve son betik çerçevesi hakkında daha fazla bilgi için bkz. [Ön betik ve son betik kullanarak uygulamayla tutarlı Linux VM yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Ve yapabileceklerinizi yedekleyemezsiniz hakkında daha fazla bilgi için bkz: [Azure Vm'lerini yedeklemek için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm).

> [!NOTE]
> Yedekleme hizmeti, geri yükleme noktası koleksiyonu depolamak için sanal makinenin kaynak grubundan ayrı bir kaynak grubu oluşturur. Müşterilerin, Backup hizmeti tarafından kullanım için oluşturduğunuz kaynak grubunda değil kilitlemek için önerilir.
Backup hizmeti tarafından oluşturulan kaynak grubu adlandırma biçimi: AzureBackupRG_`<Geo>`_`<number>`
<br>Örn: AzureBackupRG_northeurope_1
>
>

Korumak istediğiniz sanal makine sayısına bağlı olarak farklı başlangıç noktalarından başlayabilirsiniz. Tek bir işlemde birden çok sanal makine yedeklemek istiyorsanız Kurtarma Hizmetleri Kasası'na gidin ve [yedekleme işini kasa panosundan başlatın](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Tek bir sanal makineyi yedeklemek istiyorsanız [VM işlemler menüsünde yedekleme işini başlatmak](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu).

## <a name="configure-the-backup-job-from-the-vm-operations-menu"></a>Yedekleme işini VM işlemler menüsünde yapılandırın

Sanal makine işlemleri menüsünden yedekleme işini yapılandırmak için aşağıdaki adımları kullanın. Bu adımlar, yalnızca Azure portalındaki sanal makineler için geçerlidir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Tüm hizmetler**’e tıklayıp Filtre iletişim kutusuna **Sanal makineler** yazın. Siz yazarken kaynakların listesini filtrelenir. Sanal makineler seçeneğini gördüğünüzde seçin.

  ![Tüm hizmetlerdeki sanal makinelere nasıl gidileceğini gösteren ekran görüntüsü](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  Abonelikteki sanal makinelerin (VM) listesi görüntülenir.

  ![Abonelikteki VM'lerin listesi görüntülenir.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Listeden yedekleyeceğiniz VM'yi seçin.

  ![Abonelikteki VM'lerin listesi görüntülenir.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  VM'yi seçtiğinizde, sol, sanal makine yönetimi menüsü ve sanal makine Panosu, sanal makineler kaydırmalar listesini açın.

4. VM management menüsünden de **Operations** bölümünde **yedekleme**. </br>

  ![VM management menüsünde penceresindeki yedekle seçeneği](./media/backup-azure-vms-first-look-arm/vm-management-menu.png)

  Etkin yedek menüsü açılır.

  ![VM management menüsünde penceresindeki yedekle seçeneği](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup.png)

5. Kurtarma Hizmetleri kasası alanı **var olanı Seç** aşağı açılan listesinden bir kasa seçin.

  ![Yedekleme Sihirbazını Etkinleştirme](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

  Kurtarma Hizmetleri kasası yoksa veya yeni bir kasa kullanmak istiyorsanız, **Yeni oluştur**’a tıklayın ve yeni kasanın adını girin. Sanal makine ile aynı Kaynak Grubunda ve aynı bölgede yeni bir kasa oluşturulur. Farklı değerlere sahip bir Kurtarma Hizmetleri kasası oluşturmak istiyorsanız [kurtarma hizmetleri kasası oluşturma](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm) konusuna bakın.

6. Seç yedekleme İlkesi menüsünde bir ilke seçin. Seçili ilke ayrıntıları açılan menüsünün altında görünür.

  İsterseniz yeni bir ilke oluşturun veya var olan ilkeyi düzenlemek **oluşturun (veya tanımı düzenleyin) yeni bir ilke** yedekleme İlkesi Düzenleyicisi'ni açın. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Yedekleme ilkesi için değişiklikleri kaydedin ve etkinleştirme yedekleme menüye dönmek için tıklatın **Tamam**.

  ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Sanal makine için kurtarma Hizmetleri kasası ve yedekleme ilkesini uygulamak için tıklayın **yedeklemeyi etkinleştir** ilkeyi dağıtmak için. İlke dağıtmak, onu kasayla ve sanal makinelerle ilişkilendirir.

  ![Yedeklemeyi Etkinleştir düğmesi](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Yapılandırmanın ilerlemesini portalda görünen bildirimler aracılığıyla takip edebilirsiniz. Aşağıdaki örnek Dağıtımın başlatıldığını gösterir.

  ![Yedeklemeyi Etkinleştirme bildirimi](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Yapılandırmanın ilerlemesini tamamlandığında VM yönetimi menüsünü tıklatın **yedekleme** yedekleme menüsünü açın ve kullanılabilir ayrıntıları görüntüleyin.

  ![VM Yedekleme Öğesi Görünümü](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

  İlk yedekleme gerçekleştirilene kadar **yedekleme durumu**, **Uyarı (İlk yedekleme beklemede)** olarak gösterilir. Bir sonraki zamanlanmış yedekleme işi, altında oluştuğunda görmek için **özeti** ilke adına tıklayın. Yedekleme İlkesi menüsü açılır ve zamanlanmış yedekleme gösterilir.

10. Sanal makineyi korumak için tıklayın **Şimdi Yedekle**.

  ![İlk yedeklemeyi çalıştırmak Şimdi yedekle’ye tıklayın](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

  Şimdi Yedekle menüsü açılır.

  ![Şimdi Yedekle dikey penceresi](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Şimdi Yedekle menüsünde, Takvim simgesine tıklayın, bu kurtarma noktası korunur ve tıklayın son günü seçmek için takvim denetimini kullanın **Tamam**.

  ![Şimdi Yedekle kurtarma noktasının korunduğu son günü ayarlayın](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar.

## <a name="configure-the-backup-job-from-the-recovery-services-vault"></a>Yedekleme işini Kurtarma Hizmetleri kasasından yapılandırın
Yedekleme işini yapılandırmak için, aşağıdaki adımları tamamlayın.

1. Bir sanal makine için Kurtarma Hizmetleri kasası oluşturun.
2. Bir senaryo seçmek, bir Yedekleme ilkesi ayarlamak ve korunacak öğeleri tanımlamak için Azure portalını kullanın.
3. İlk yedeklemeyi çalıştırın.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Bir VM için kurtarma hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası, zaman içinde oluşturulan tüm yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası ayrıca, korumalı VM'lere uygulanan yedekleme ilkesini de içerir.

> [!NOTE]
> VM'leri yedekleme işlemi, yerel bir işlemdir. Bir bölgedeki VM'leri, başka bir bölgede bulunan bir Kurtarma Hizmetleri kasasına yedekleyemezsiniz. Bu nedenle, yedeklenecek VM'ler içeren her Azure bölgesi için en az bir kurtarma Hizmetleri kasası ilgili bölgede mevcut olmalıdır.
>
>

Kurtarma Hizmetleri kasası oluşturmak için:

1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Tüm hizmetler**’e tıklayıp Filtre iletişim kutusunda **Kurtarma Hizmetleri** yazın. Siz yazarken kaynakların listesini filtrelenir. Listede Kurtarma Hizmetleri kasaları seçeneğini gördüğünüzde buna tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Abonelikte Kurtarma Hizmetleri kasaları varsa, kasalar listelenir.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası menüsünü açar, sağlamak isteyen bir **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. **Abonelik** bölümündeki açılır menüyü kullanarak Azure aboneliğini seçin. Yalnızca bir abonelik kullanıyorsanız bu abonelik görüntülenir ve sonraki adıma atlayabilirsiniz. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.

6. **Kaynak grubu** bölümünde:

    * bir Kaynak grubu oluşturmak istiyorsanız, **Yeni oluştur**’u seçin.
    Veya
    * **Var olanı kullan**’ı seçin ve açılır menüyü kullanarak mevcut Kaynak gruplarının listesine bakın.

  Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Bu seçim, yedekleme verilerinizin gönderildiği coğrafi bölgeyi belirler.

  > [!IMPORTANT]
  > VM'nizin hangi bölgede bulunduğundan emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki Sanal Makineler listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Sonraki bölgeye geçmeden önce, kasayı ilk bölgede oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerekmez; Kurtarma Hizmetleri kasası ve Azure Backup hizmeti depolamayı otomatik olarak gerçekleştirir.
  >

8. Kurtarma Hizmetleri kasası menüsünde altındaki tıklatın **Oluştur**.

    Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür. Birkaç dakika sonra kasayı görmezseniz **Yenile**’ye tıklayın.

    ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Kasanızı Kurtarma Hizmetleri kasaları listesinde gördükten sonra, depolama yedekliliğini ayarlamaya hazır olursunuz.

Kasanızı oluşturduğunuza göre, artık depolama çoğaltmayı nasıl ayarlayacağınızı öğrenebilirsiniz.

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Kurtarma Hizmetleri kasası, birincil yedeklemeniz ise, depolama çoğaltma seçeneğini coğrafi olarak yedekli depolama şeklinde bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Gelen **kurtarma Hizmetleri kasaları** menü, yeni kasayı seçin.

  ![Kurtarma Hizmetleri kasası listesinden yeni kasayı seçin](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Kasa ayarları menü seçeneğini belirlediğinizde (*en üstünde kasanın adı olan*) ve kasa panosunda açın.

  ![Yeni kasa için depolama yapılandırmasını görüntüleme](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-update.png)

2. Yeni kasanın yönetim menüde Yönet bölümüne inin ve dikey kaydırma çubuğunu kullanın. **Yedekleme Altyapısı** yedekleme altyapısı menüsünü açmak için.

   ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-config-bkup-infra.png)

3. Yedekleme Altyapısı menüden **yedekleme yapılandırması** açmak için **yedekleme yapılandırması** menüsü.

    ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-open-infra.png)
4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

    ![depolama yapılandırması seçenekleri](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, **Coğrafi olarak yedekli** seçeneğini kullanmaya devam edin. Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Depolama yedekliliğine genel bakış](../storage/common/storage-redundancy.md) bölümünden edinebilirsiniz.


## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Yedekleme hedefi seçme, ilke ayarlama ve korunacak öğeleri tanımlama
VM'yi bir kasaya kaydetmeden önce, aboneliğe eklenmiş olan yeni sanal makinelerin tanımlandığından emin olmak için bulma işlemini çalıştırın. Bu işlem, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte abonelikteki sanal makinelerin listesi için Azure'ı sorgular. Azure portalda, senaryo, kurtarma hizmetleri kasasına yerleştireceğiniz içeriğe başvuruda bulunur. İlke, kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir.

1. Açık bir kurtarma hizmetleri kasanız zaten varsa 2. adıma geçin. Aksi takdirde **Tüm hizmetler**’e tıklayın. **Kurtarma Hizmetleri** yazın ve **Kurtarma Hizmetleri kasaları**’na tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Kurtarma hizmetleri kasalarının listesi görünür.

    ![Kurtarma Hizmetleri kasaları listesinin görünümü](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Kurtarma hizmetleri kasalarının listesinden bir kasayı seçerek kasanın panosunu açın.

     ![Kasa menüsünü açma](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Kasa panosu menüsünden Yedekleme menüsünü açmak için **Yedekle** seçeneğine tıklayın.

    ![Yedekleme menüyü Aç](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Yedekleme ve yedekleme hedefi menülerini açın.

    ![Senaryo menüsünü açma](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. Yedekleme hedefi menüsünde, gelen **iş yükünüz çalıştığı** açılan menüsünde Azure'u seçin. **Neleri yedeklemek istiyorsunuz** açılan penceresinde, Sanal makine’yi seçin ve ardından **Tamam**’a tıklayın.

    Bu eylemler, VM uzantısını kasa ile kaydeder. Yedekleme hedefi menüsünde kapatır ve **yedekleme İlkesi** menüsü açılır.

    ![Senaryo menüsünü açma](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Yedekleme İlkesi menüsünde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin.

    ![Yedekleme ilkesini seçme](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Yedekleme ilkesini kasa ile ilişkilendirmek için **Tamam**’a tıklayın.

    Yedekleme İlkesi menüsü kapatır ve **sanal makineleri** menüsü açılır.
5. İçinde **sanal makineleri** menüsünde,'a tıklayın ve belirtilen ilkeyle ilişkilendirilecek sanal makineleri seçin **Tamam**.

    ![İş yükünü seçme](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Seçilen sanal makine doğrulanır. Görmeyi beklediğiniz sanal makineleri görmüyorsanız Kurtarma Hizmetleri kasasıyla aynı Azure konumda olduklarından ve zaten koruma altında olup olmadıklarından emin olun. Kurtarma Hizmetleri kasasının konumu kasa panosunda gösterilir.

6. Kasa için tüm ayarları yedekleme menüde tanımladığınız, tıklayın **yedeklemeyi etkinleştir** ilkeyi kasaya ve Vm'lere dağıtmak için. Yedekleme ilkesinin dağıtılmasıyla, sanal makine için ilk kurtarma noktasını oluşturulmaz.

    ![Yedeklemeyi Etkinleştirme](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedekleme başarıyla etkinleştirildikten sonra yedekleme ilkeniz ilgili zamanda çalıştırılır. Ancak, ilk yedekleme işini başlatmak için devam edin.

## <a name="initial-backup"></a>İlk yedekleme
Sanal makine üzerinde bir yedekleme ilkesinin dağıtılmış olması, verilerin yedeklendiği anlamına gelmez. Varsayılan olarak, zamanlanmış birinci yedekleme (yedekleme ilkesinde tanımlandığı şekilde) ilk yedeklemedir. İlk yedekleme gerçekleştirilene kadar son yedekleme durumu **yedekleme işleri** menüsü gösterir olarak **uyarı (ilk yedekleme Beklemede)**.

![Yedekleme beklemede](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

İlk yedeklemeniz kısa süre içinde başlamazsa **Şimdi Yedekle** seçeneğini çalıştırmanız önerilir.

İlk yedekleme işini çalıştırmak için:

1. Kasa panosunda **Yedekleme Öğeleri** altındaki sayıya veya **Yedekleme Öğeleri** kutucuğuna tıklayın. <br/>
  ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  **Yedekleme Öğeleri** menüsü açılır.

  ![Öğeleri yedekleme](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Üzerinde **yedekleme öğeleri** menüsünde, öğeyi seçin.

  ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  **Yedekleme Öğeleri** listesi açılır. <br/>

  ![Tetiklenmiş yedekleme işi](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. **Yedekleme Öğeleri** listesinde üç noktaya **...** tıklayarak Bağlam menüsünü açın.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Bağlam menüsü görüntülenir.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Bağlam menüsünde **Şimdi yedekle**’ye tıklayın.

  ![Bağlam menüsü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Şimdi Yedekle menüsü açılır.

  ![Şimdi Yedekle menüsü gösterir](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Şimdi Yedekle menüsünde, Takvim simgesine tıklayın, bu kurtarma noktası korunur ve tıklayın son günü seçmek için takvim denetimini kullanın **yedekleme**.

  ![Şimdi Yedekle kurtarma noktasının korunduğu son günü ayarlayın](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

  > [!NOTE]
  > Tüm Azure Backup tarafından yedeklenen veriler, kullanılmadıkları şifrelenir [depolama hizmeti şifrelemesi (SSE)](../storage/common/storage-service-encryption.md).
  >
  >

6. İlk yedekleme durumunu izlemek için kasa panosunda **Yedekleme İşleri** kutucuğunda **Devam eden**’e tıklayın.

  ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Yedekleme işleri menüsü açılır.

  ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  İçinde **yedekleme işleri** menüsünde, tüm işlerin durumunu görebilirsiniz. VM’niz için yedekleme işinin devam edip etmediğini veya bitip bitmediğini kontrol edin. Yedekleme işi tamamlandığında, durum *Tamamlandı* olur.

  > [!NOTE]
  > Azure Backup hizmeti, yedekleme işleminin parçası olarak, her VM'deki yedekleme uzantısına tüm yazma işlemlerini boşaltmaya ve tutarlı bir anlık görüntü almaya yönelik bir komut verir.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı'nı sanal makineye yükleme
Bu bilgiler, gerekmesi halinde kullanılabilmesi için sağlanmıştır. Backup uzantısının çalışması için Azure VM Aracısı'nın Azure sanal makinesine yüklenmesi gerekir. Ancak VM'niz Azure galerisinden oluşturulmuşsa VM Aracısı, sanal makine üzerinde zaten mevcuttur. Şirket içi veri merkezlerinden geçişi sağlanan VM'lerde VM Aracısı yüklü olmaz. Böyle bir durumda, VM Aracısı'nın yüklenmesi gerekir. Azure VM’yi yedekleme konusunda sorun yaşarsanız Azure VM Aracısı'nın sanal makineye doğru şekilde yüklenip yüklenmediğini kontrol edin (aşağıdaki tabloya bakın). Özel bir VM oluşturuyorsanız sanal makine sağlanmadan önce VM Aracısı'nın yüklü olduğundan emin olun.

[VM Aracısı](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) ve [nasıl yükleneceği](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) konusunda bilgi edinin.

Aşağıdaki tabloda, Windows ve Linux VM'ler için VM Aracısı ile ilgili ek bilgiler sağlanmıştır.

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |<li>[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. <li>Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx). |<li> En son [Linux aracısını](https://github.com/Azure/WALinuxAgent) GitHub'dan yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. <li> Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx). |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. <br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

### <a name="backup-extension"></a>Backup uzantısı
VM Aracısı sanal makineye yüklendikten sonra, Azure Backup hizmeti yedekleme uzantısını VM Aracısı'na yükler. Azure Backup hizmeti, ek kullanıcı müdahalesi olmadan sorunsuz bir şekilde yedekleme uzantısını yükseltir ve uzantıya düzeltme eki uygular.

VM çalışmıyor olsa dahi, Backup hizmeti yedekleme uzantısını yükler. Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak Azure Backup hizmeti, uzantının yüklenememiş olması ve VM'nin kapalı olması durumunda dahi VM'yi yedeklemeye devam eder. Bu yedekleme türüne Çevrimdışı VM adı verilir ve kurtarma noktası *çökme tutarlıdır*.

## <a name="troubleshooting-information"></a>Sorun giderme bilgileri
Bu makaledeki bazı görevleri yerine getirirken sorun yaşıyorsanız lütfen [Sorun giderme rehberine](backup-azure-vms-troubleshoot.md) başvurun.

## <a name="pricing"></a>Fiyatlandırma
Azure VM'lerin yedeklenmesi maliyeti, korumalı örneklerinin sayısını temel alır. Korumalı bir örneğin tanımı için, bkz: [Korumalı örnek nedir](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Bir sanal makineyi yedeklemenin maliyetini hesaplamaya ilişkin bir örnek için, bkz: [Korumalı örnekler nasıl hesaplanır](backup-azure-vms-introduction.md#calculating-the-cost-of-protected-instances). [Yedekleme Fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/) hakkında bilgi için Azure Backup Fiyatlandırma sayfasına bakın.

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).
