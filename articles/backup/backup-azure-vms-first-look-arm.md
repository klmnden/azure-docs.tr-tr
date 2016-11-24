---
title: "İlk Bakış: Azure sanal makinelerini kurtarma hizmetleri kasasıyla koruma | Microsoft Belgeleri"
description: "Sanal makineleri bir kurtarma hizmetleri kasasıyla koruyun. Verilerinizi korumak için, Resource Manager tarafından dağıtılan, Klasik modunda dağıtılan VM&quot;lerin ve Premium Storage VM&quot;lerinin yedeklemelerini kullanın. Bir kurtarma hizmetleri kasası oluşturun ve kaydedin. Azure&quot;da VM&quot;leri kaydedin, ilke oluşturun ve VM&quot;leri koruyun."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/10/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: 85b291e3916d1274fefc71bc0c1f12cac2920bb4
ms.openlocfilehash: 77b4f6e5ee18cb3772487820bc72d7794f82162f


---
# <a name="first-look-protect-azure-vms-with-a-recovery-services-vault"></a>İlk bakış: Azure sanal makinelerini bir kurtarma hizmetleri kasasıyla koruma
> [!div class="op_single_selector"]
> * [VM'leri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md)
> * [VM'leri yedekleme kasasıyla koruma](backup-azure-vms-first-look.md)
>
>

Bu öğretici, bir kurtarma hizmetleri kasası oluşturmaya ve bir Azure sanal makinesini (VM) yedeklemeye yönelik adımlar boyunca size yol gösterir. Kurtarma hizmetleri kasaları şunları korur:

* Azure Resource Manager tarafından dağıtılan VM'ler
* Klasik VM'ler
* Standart depolama VM'leri
* Premium depolama VM'leri
* Azure Disk Şifrelemesi kullanılarak BEK ve KEK ile şifrelenmiş VM’ler

Premium depolama VM'lerini koruma ile ilgili daha fazla bilgi için bkz. [Premium Storage VM'lerini Yedekleme ve Geri Yükleme](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup)

> [!NOTE]
> Bu öğretici, Azure aboneliğinizde zaten bir VM'niz olduğunu ve yedekleme hizmetinin VM'ye erişmesine izin vermek üzere gerekli işlemleri yaptığınızı varsayar.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Korumak istediğiniz VM sayısına bağlı olarak farklı başlangıç noktalarından başlayabilirsiniz; birden çok sanal makineyi tek bir işlemde yedeklemek için Kurtarma Hizmetleri kasasına gidip yedeklemeyi kasa panosundan başlatın. Tek bir VM’niz varsa ve bunu yedeklemek istiyorsanız, yedekleme işlemini doğrudan VM yönetimi dikey penceresinden gerçekleştirebilirsiniz.

## <a name="configure-backup-from-vm-management-blade"></a>VM yönetimi dikey penceresinden Yedeklemeyi yapılandırma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Diğer Hizmetler**’e tıklayıp kaynak listesine **Sanal makineler** yazın.  Sanal makinelerin listesi görünür. Sanal makine listesinden yedeklemek istediğiniz bir sanal makine seçin. Bunu yaptığınızda sanal makine yönetimi dikey penceresi açılır. 
 ![VM Yönetimi dikey penceresi](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)
 
3. VM yönetimi dikey penceresinde Ayarlar’ın sol alt tarafında bunulan "Yedekle" seçeneğine tıklayın.
![VM yönetimi dikey penceresindeki Yedekle seçeneği](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

4. Bunu yaptığınızda Yedeklemeyi Etkinleştirme dikey penceresi açılır. Bu dikey pencere iki giriş bekler: VM’lerin yedeklerini depolamak için kullanılan bir Azure Backup kaynağı olan Kurtarma Hizmetleri kasası veya yedeklemelerin zamanlamasını ve yedek kopyaların ne kadar tutulacağını belirten bir yedekleme ilkesi. Bu dikey pencere varsayılan seçeneklerle sunulur. Bu seçenekleri yedekleme gereksinimlerine uygun olarak özelleştirebilirsiniz. 
![Yedekleme Sihirbazını Etkinleştirme](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Kurtarma Hizmetleri kasası için mevcut bir kasayı seçebilir veya yeni bir Kasa oluşturabilirsiniz. Yeni bir kasa oluşturuyorsanız bu kasa sanal makineyle aynı Kaynak Grubu’nda oluşturulur ve kasanın konumu sanal makineyle aynıdır. Farklı değerlerle bir Kurtarma Hizmetleri kasası oluşturmak istiyorsanız 3. Adım’daki Yedekle seçeneğine tıklamadan önce [bir kurtarma hizmetleri kasası oluşturun](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm) ve bu dikey pencerede o kasayı seçin. 

6. Yedekleme ilkesi dikey penceresinde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin ve **Tamam**'a tıklayın.
    ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları, ayrıntılar içinde listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Açılır menü anlık görüntünün alınma zamanını değiştirme seçeneği de sağlar. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). **Tamam**'a tıkladığınızda yedekleme ilkesi sanal makineyle ilişkilendirilir.
    
7. Sanal makinede Yedekleme yapılandırmak için "Yedeklemeyi Etkinleştir"e tıklayın. Bunu yaptığınızda bir dağıtım tetiklenir. 
![Yedeklemeyi Etkinleştir düğmesi](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Yapılandırmanın ilerleme durumunu bildirimler aracılığıyla izleyebilirsiniz. 
![Yedeklemeyi Etkinleştirme bildirimi](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Yedekleme yapılandırmaya yönelik dağıtım tamamlandıktan sonra VM yönetimi dikey penceresindeki “yedekle” seçeneğine tıklarsanız, yedeklenen VM’ye karşılık gelen Yedekleme Öğesi dikey penceresine götürülürsünüz.
![VM Yedekleme Öğesi Görünümğ](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

## <a name="configure-backup-from-recovery-services-vault-view"></a>Kurtarma Hizmetleri kasasından Yedekleme Yapılandırma Görünümü
Tamamlayacağınız gelişmiş adımlar aşağıda verilmiştir.  

1. Bir VM için kurtarma hizmetleri kasası oluşturun.
2. Bir senaryo seçmek, İlke ayarlamak ve korunacak öğeleri tanımlamak için Azure portalı kullanın.
3. İlk yedeklemeyi çalıştırın.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Bir VM için kurtarma hizmetleri kasası oluşturma
Kurtarma hizmetleri kasası, zaman içinde oluşturulan tüm yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma hizmetleri kasası ayrıca, korumalı VM'lere uygulanan yedekleme ilkesini de içerir.

> [!NOTE]
> VM'leri yedekleme işlemi, yerel bir işlemdir. Bir konumdaki VM'leri, başka bir konumda bulunan bir kurtarma hizmetleri kasasına yedekleyemezsiniz. Bu nedenle, yedeklenecek VM'ler içeren her Azure konumu için en az bir kurtarma hizmetleri kasası ilgili konumda mevcut olmalıdır.
>
>

Kurtarma hizmetleri kasası oluşturmak için:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Kurtarma hizmetleri kasalarının listesi görüntülenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)
4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Kullanılabilir abonelik listesini görmek için **Abonelik** seçeneğine tıklayın. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.
6. Kullanılabilir Kaynak grubu listesini görmek için **Kaynak grubu** seçeneğine, yeni bir Kaynak grubu oluşturmak için de **Yeni** seçeneğine tıklayın. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md)
7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Kasa korumak istediğiniz sanal makinelerle aynı bölgede **olmalıdır**.

   > [!IMPORTANT]
   > VM'nizin hangi konumda bulunduğundan emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki Virtual Machines listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma hizmetleri kasası oluşturun. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerekmez; kurtarma hizmetleri kasası ve Azure Backup hizmeti bunu otomatik olarak gerçekleştirir.
   >
   >
8. **Oluştur**'a tıklayın. Kurtarma hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalda sağ üst alandaki durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/rs-list-of-vaults.png)

Kasanızı oluşturduğunuza göre, artık depolama çoğaltmayı nasıl ayarlayacağınızı öğrenebilirsiniz.

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Bu, birincil yedeklemenizse seçeneği coğrafi olarak yedekli depolamaya ayarlanmış şekilde bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar dikey penceresini açmak için kasanızı seçin. **Ayarlar** dikey penceresi açılmazsa kasa panosunda **Tüm ayarlar** seçeneğine tıklayın.
2. **Ayarlar** dikey penceresinde, **Yedekleme Yapılandırması** dikey penceresini açmak için **Yedekleme Altyapısı** > **Yedekleme Yapılandırması**'na tıklayın. **Yedekleme Yapılandırması** dikey penceresinde, kasanıza yönelik depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Yedekleme hedefi seçme, ilke ayarlama ve korunacak öğeleri tanımlama
VM'yi bir kasaya kaydetmeden önce, aboneliğe eklenmiş olan yeni sanal makinelerin tanımlandığından emin olmak için bulma işlemini çalıştırın. Bu işlem, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte abonelikteki sanal makinelerin listesi için Azure'ı sorgular. Azure portalda, senaryo, kurtarma hizmetleri kasasına yerleştireceğiniz içeriğe başvuruda bulunur. İlke, kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir.

1. Açık bir kurtarma hizmetleri kasanız zaten varsa 2. adıma geçin. Açık kurtarma hizmetleri kasanız yoksa ancak Azure portaldaysanız Hub menüsünde **Gözat**'a tıklayın.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

     Kurtarma hizmetleri kasalarının listesi görünür.
   * Kurtarma hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.

     ![Kasa dikey penceresini açma](./media/backup-azure-vms-first-look-arm/vault-settings.png)
2. Kasa panosu menüsünden Yedekleme dikey penceresini açmak için **Yedekle** seçeneğine tıklayın.

    ![Yedekleme dikey penceresini açma](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Dikey pencere açıldığında, Backup hizmeti, abonelikteki yeni VM'leri arar.

    ![VM'leri bulma](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)
3. Yedekleme dikey penceresinde, Yedekleme Hedefi dikey penceresini açmak için **Yedekleme hedefi** seçeneğine tıklayın.

    ![Senaryo dikey penceresini açma](./media/backup-azure-vms-first-look-arm/select-backup-goal-one.png)
4. Yedekleme Hedefi dikey penceresinde **İş yükünüz nerede çalışıyor?** seçeneğini Azure olarak, **Neleri yedeklemek istiyorsunuz?** seçeneğini ise Sanal makine olarak ayarlayın ve ardından **Tamam**'a tıklayın.

    Yedekleme Hedefi dikey penceresi kapanır ve Yedekleme ilkesi dikey penceresi açılır.

    ![Senaryo dikey penceresini açma](./media/backup-azure-vms-first-look-arm/select-backup-goal-two.png)
5. Yedekleme ilkesi dikey penceresinde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin ve **Tamam**'a tıklayın.

    ![Yedekleme ilkesini seçme](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları, ayrıntılar içinde listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Açılan menü ayrıca, anlık görüntünün alınma zamanını 19:00'a değiştirme seçeneğini de sağlar. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). **Tamam**'a tıklamanızın ardından, yedekleme ilkesi kasayla ilişkilendirilir.

    Daha sonra, kasa ile ilişkilendirilecek VM'leri seçin.
6. Belirtilen ilke ile ilişkilendirilecek sanal makineleri seçin ve **Seç**'e tıklayın.

    ![İş yükünü seçme](./media/backup-azure-vms-first-look-arm/select-vms-to-backup-new.png)

    İstenen VM'yi görmüyorsanız bunun Kurtarma Hizmetleri kasası ile aynı Azure konumunda bulunduğundan emin olun.
7. Kasa için tüm ayarları tanımladığınıza göre, Yedekleme dikey penceresinde sayfanın alt kısmındaki **Yedeklemeyi Etkinleştir** seçeneğine tıklayın. Bu, ilkeyi kasaya ve VM'lere dağıtır.

    ![Yedeklemeyi Etkinleştirme](./media/backup-azure-vms-first-look-arm/enable-backup-settings-new.png)

## <a name="initial-backup"></a>İlk yedekleme
Sanal makine üzerinde bir yedekleme ilkesinin dağıtılmış olması, verilerin yedeklendiği anlamına gelmez. Varsayılan olarak, zamanlanmış birinci yedekleme (yedekleme ilkesinde tanımlandığı şekilde) ilk yedeklemedir. İlk yedekleme gerçekleştirilene kadar, **Yedekleme İşleri** dikey penceresindeki Son Yedekleme Durumu **Uyarı (ilk yedekleme bekleme)** olarak gösterilir.

![Yedekleme beklemede](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

İlk yedeklemeniz kısa süre içinde başlamazsa **Şimdi Yedekle** seçeneğini çalıştırmanız önerilir.

**Şimdi Yedekle** seçeneğini çalıştırmak için:

1. Kasa panosunda, **Backup** kutucuğunda **Azure Virtual Machines**'e tıklayın <br/>
    ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    **Yedekleme Öğeleri** dikey penceresi açılır.
2. **Yedekleme Öğeleri** dikey penceresinde, yedeklemek istediğiniz kasaya sağ tıklayıp **Şimdi yedekle**'ye tıklayın.

    ![Ayarlar simgesi](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Yedekleme işi tetiklenir. <br/>

    ![Tetiklenmiş yedekleme işi](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. İlk yedeklemenizin tamamlanıp tamamlanmadığını görmek için kasa panosu üzerinde **Yedekleme İşleri** kutucuğundaki **Azure sanal makineleri**'ne tıklayın.

    ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Yedekleme İşleri dikey penceresi açılır.
4. Yedekleme işleri dikey penceresinde tüm işlerin durumunu görebilirsiniz.

    ![Yedekleme İşleri kutucuğu](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Azure Backup hizmeti, yedekleme işleminin parçası olarak, her VM'deki yedekleme uzantısına tüm yazma işlemlerini boşaltmaya ve tutarlı bir anlık görüntü almaya yönelik bir komut verir.
   >
   >

    Yedekleme işi tamamlandığında, durum *Tamamlandı* olur.

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı'nı sanal makineye yükleme
Bu bilgiler, gerekmesi halinde kullanılabilmesi için sağlanmıştır. Backup uzantısının çalışması için Azure VM Aracısı'nın Azure sanal makinesine yüklenmesi gerekir. Ancak VM'niz Azure galerisinden oluşturulmuşsa VM Aracısı, sanal makine üzerinde zaten mevcuttur. Şirket içi veri merkezlerinden geçişi sağlanan VM'lerde VM Aracısı yüklü olmaz. Böyle bir durumda, VM Aracısı'nın yüklenmesi gerekir. Azure VM'yi yedekleme konusunda sorun yaşarsanız Azure VM Aracısı'nın sanal makineye doğru şekilde yüklenip yüklenmediğini kontrol edin (aşağıdaki tabloya bakın). Özel bir VM oluşturuyorsanız sanal makine sağlanmadan önce [**VM Aracısı'nı yükle** onay kutusunun seçili olduğundan emin olun](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[VM Aracısı](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) ve [nasıl yükleneceği](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) konusunda bilgi edinin.

Aşağıdaki tabloda, Windows ve Linux VM'ler için VM Aracısı ile ilgili ek bilgiler sağlanmıştır.

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |<li>[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. <li>Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx). |<li> En son [Linux aracısını](https://github.com/Azure/WALinuxAgent) GitHub'dan yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. <li> Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx). |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. <br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

### <a name="backup-extension"></a>Backup uzantısı
VM Aracısı sanal makineye yüklendikten sonra, Azure Backup hizmeti yedekleme uzantısını VM Aracısı'na yükler. Azure Backup hizmeti, ek kullanıcı müdahalesi olmadan sorunsuz bir şekilde yedekleme uzantısını yükseltir ve uzantıya düzeltme eki uygular.

Backup uzantısı, VM'nin çalışır durumda olup olmamasından bağımsız olarak Backup hizmeti tarafından yüklenir. Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak Azure Backup hizmeti, uzantının yüklenememiş olması ve VM'nin kapalı olması durumunda dahi VM'yi yedeklemeye devam eder. Buna Çevrimdışı VM adı verilir. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.

## <a name="troubleshooting-information"></a>Sorun giderme bilgileri
Bu makaledeki bazı görevleri yerine getirirken sorun yaşıyorsanız lütfen [Sorun giderme rehberine](backup-azure-vms-troubleshoot.md) başvurun.

## <a name="pricing"></a>Fiyatlandırma
Azure VM yedeklemesi Korunan Örnekler modeline göre ücretlendirilecektir. [Yedekleme Fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/) hakkında daha fazla bilgi edinin

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).



<!--HONumber=Nov16_HO2-->


