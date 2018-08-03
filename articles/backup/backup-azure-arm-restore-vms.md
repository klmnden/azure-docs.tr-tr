---
title: 'Azure yedekleme: Azure portalını kullanarak sanal makineleri geri yükleme'
description: Bir Azure sanal makinesi, Azure portalını kullanarak bir kurtarma noktasından geri yükleme
services: backup
author: markgalioto
manager: carmonm
keywords: yedeklemeyi geri yükleme; geri yükleme; kurtarma noktası;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: markgal
ms.openlocfilehash: 872bfc0027fd5b69bb42f391c036f7116789f529
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431156"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Sanal makineleri geri yükleme için Azure portalını kullanma
Tanımlı aralıklarla verilerinizin anlık görüntülerini alarak verilerinizi koruyun. Bu anlık görüntüler, Kurtarma noktaları olarak bilinir ve kurtarma Hizmetleri kasalarında depolandıkları. Onarım veya bir sanal makine (VM) yeniden gerekliyse, kaydedilmiş kurtarma noktalarının birini VM geri yükleyebilirsiniz. Bir kurtarma noktasından geri yüklediğinizde, şunları yapabilirsiniz:

* Yedeklenen VM'niz zaman içinde nokta gösterimi ise yeni bir VM oluşturun.
* Diskleri geri yükleme ve geri yüklenen VM özelleştirme işlemi ile birlikte gelen şablonu kullanabilir veya tek tek dosya kurtarma yapın. 

Bu makalede, bir VM için yeni bir sanal makine geri yükleme ya da tüm yedeklenen diskleri geri açıklanmaktadır. Tek tek dosya kurtarma için bkz: [dosyaları bir Azure VM yedekten kurtarma](backup-azure-restore-files-from-vm.md).

![Sanal makine yedeklemesini geri yükleme için üç yol](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager modelini kullanarak Vm'leri geri yüklemek için kullanılan yordamlar ve bilgi dağıtılan sağlar.
>
>

VM'den bir VM veya tüm diskleri geri yükleme yedeği iki adımdan oluşur:

* Geri yükleme için bir geri yükleme noktası seçin.
* Geri yükleme türünü seçin, yeni bir VM oluşturun veya diskleri geri yükle ve gerekli parametreleri belirtin. 

## <a name="select-a-restore-point-for-restore"></a>Geri yükleme için bir geri yükleme noktası seçin
1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

1. Azure menüsünde **Gözat**. Hizmetler listesinde yazın **kurtarma Hizmetleri**. Yazdığınız için hizmet listesini ayarlar. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

    ![Kurtarma Hizmetleri kasası](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Abonelikteki kasalarının listesi görüntülenir.

    ![Kasaları kurtarma Hizmetleri listesi](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
1. Listeden geri yüklemek istediğiniz VM ile ilişkili kasayı seçin. Kasayı seçtiğinizde, kendi Pano açılır.

    ![Kurtarma Hizmetleri kasası seçilmedi](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
1. Kasa panosunda üzerinde **yedekleme öğeleri** kutucuk seçin **Azure sanal makineler**.

    ![Kasa Panosu](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    **Yedekleme öğeleri** dikey penceresi açılır ve Azure VM'lerin listesini görüntüler.

    ![Kasadaki sanal makinelerinin listesi](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
1. Listeden panoyu açmak için bir VM seçin. Sanal makine Panosu'ndan içeren bir izleme alan açılır **geri yükleme noktaları** Döşe.

    ![Geri yükleme noktaları](./media/backup-azure-arm-restore-vms/vm-blade.png)
1. Sanal makine Panosu menüsünde **geri**.

    ![Geri Yükle düğmesi](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    **Geri** dikey penceresi açılır.

    ![Dikey pencereyi geri yükle](./media/backup-azure-arm-restore-vms/restore-blade.png)
1. Üzerinde **geri** dikey penceresinde **geri yükleme noktası**. **Seçin, geri yükleme noktası** dikey penceresi açılır.

    ![Geri yükleme noktası seçin](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Varsayılan olarak, son 30 güne ait tüm geri yükleme noktalarını iletişim kutusunu görüntüler. Kullanım **filtre** geri yükleme noktalarını zaman aralığını değiştirmek için görüntülenir. Varsayılan olarak, tüm bunları listeledik geri yükleme noktaları görüntülenir. Değiştirme **tüm geri yükleme noktaları** belirli geri yükleme noktası tutarlılığı seçmek için filtre. Geri yükleme noktası türleri hakkında daha fazla bilgi için bkz. [veri tutarlılığı](backup-azure-vms-introduction.md#data-consistency).

    Geri yükleme noktası tutarlılığı seçenekleri:

     * Kilitlenme tutarlı geri yükleme noktaları
     * Uygulama tutarlı geri yükleme noktaları
     * Dosya sistemiyle tutarlı geri yükleme noktaları
     * Tüm geri yükleme noktaları

1. Geri yükleme noktası seçin ve seçin **Tamam**.

    ![Geri yükleme noktası seçenekleri](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    **Geri** dikey penceresi, geri yükleme noktası ayarlandığını gösterir.

1. Sizin olduğunuz zaten vardır, Git, **geri** dikey penceresi. Emin bir [geri yükleme noktası seçili](#select-a-restore-point-for-restore)seçip **geri yükleme Yapılandırması**. **Geri yükleme Yapılandırması** dikey penceresi açılır.

## <a name="choose-a-vm-restore-configuration"></a>Bir VM geri yükleme yapılandırması seçin
Geri yükleme noktası seçtikten sonra bir VM geri yükleme yapılandırması seçin. Geri yüklenen VM'yi yapılandırmak için Azure portalını veya PowerShell'i kullanabilirsiniz.

1. Sizin olduğunuz zaten vardır, Git, **geri** dikey penceresi. Emin bir [geri yükleme noktası seçili](#select-a-restore-point-for-restore)seçip **geri yükleme Yapılandırması**. **Geri yükleme Yapılandırması** dikey penceresi açılır.

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
1. Üzerinde **geri yükleme Yapılandırması** dikey penceresinde, iki seçeneğiniz vardır:

   * **Sanal makine oluşturma**

   * **Diskleri geri yükle**

Portal sağlar bir **hızlı Oluştur** geri yüklenen VM için seçenek. VM yapılandırması veya yeni bir VM seçenek oluşturulmasının bir parçası oluşturulan kaynakların adlarını özelleştirmek için yedeklenen diskleri geri yüklemek için PowerShell veya portal'ı kullanın. Bunları kendi seçtiğiniz VM yapılandırması için eklemek için PowerShell komutlarını kullanın. Veya geri yüklenen VM özelleştirmek için geri yüklenen diskleri ile birlikte gelen şablon kullanabilirsiniz. Birden çok NIC içeren veya bir yük dengeleyici altında olan bir VM geri yükleme hakkında daha fazla bilgi için bkz: [bir sanal özel ağ yapılandırmalarını geri](#restore-vms-with-special-network-configurations). Windows VM'nizi kullanıyorsa [HUB lisanslama](../virtual-machines/windows/hybrid-use-benefit-licensing.md), diskleri geri yükle ve VM'yi oluşturmak için bu makalede belirtildiği gibi PowerShell/şablonu kullanın. Belirttiğiniz emin **lisans türü** "geri yüklenen VM üzerindeki HUB avantajları yararlanabilmek için bir VM oluştururken Windows_Server" olarak. 
 
## <a name="create-a-new-vm-from-a-restore-point"></a>Geri yükleme noktasından yeni VM oluşturma
1. Zaten orada değilseniz [geri yükleme noktası seçin](#restore-a vm-with-special-network-configurations) geri yükleme noktasından yeni bir sanal makine oluşturmaya başlamadan önce. Bir geri yükleme noktası seçtikten sonra **geri yükleme Yapılandırması** dikey penceresinde girin veya seçin değerleri aşağıdaki alanların her biri için:

    a. **Geri yükleme türü**. Sanal makine oluşturur.

    b. **Sanal makine adı**. VM için bir ad sağlayın. Adı (bir Azure Resource Manager tarafından dağıtılan VM için) kaynak grubu veya Bulut hizmetinden (Klasik VM için) benzersiz olmalıdır. Abonelikte zaten varsa VM değiştirilemiyor.

    c. **Kaynak grubu**. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Klasik VM'yi geri yüklemek, yeni bir bulut hizmeti adını belirtmek için bu alanı kullanın. Yeni bir kaynak grubu/bulut hizmeti oluşturuyorsanız, adı genel olarak benzersiz olmalıdır. Genellikle, bulut hizmeti adı genel kullanıma yönelik URL ile ilişkilendirilen: Örneğin, [buluthizmeti]. cloudapp.net. Bulut kaynak grup/bulut hizmeti için bir ad zaten kullanımda kullanmayı denerseniz, Azure kaynak grubu/bulut hizmeti VM adıyla aynı atar. Azure kaynak grupları/cloud services ve Vm'leri herhangi bir benzeşim grupları ile ilişkili olmayan görüntüler. Daha fazla bilgi için [bölgesel sanal ağ için benzeşim grupları geçirme](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Sanal ağ**. Sanal Makineyi oluştururken sanal ağı seçin. Alan, abonelikle ilişkili tüm sanal ağları sağlar. Sanal makinenin kaynak grubunu, parantez içinde görüntülenir.

    e. **Alt ağ**. İlk alt ağ, sanal ağ alt ağlar varsa, varsayılan olarak seçilidir. Ek alt ağlar varsa, kullanmak istediğiniz alt ağı seçin.

    f. **Depolama hesabı**. Bu menü kurtarma Hizmetleri kasasıyla aynı konumda depolama hesaplarını listeler. Bölgesel olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda olan depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü, parantez içinde görüntülenir.

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

    > [!NOTE]
    > * Resource Manager tarafından dağıtılan bir sanal makine geri yüklerseniz, bir sanal ağ belirlemeniz gerekir. Bir sanal ağı klasik bir VM için isteğe bağlıdır.

    > * Vm'leri yönetilen disklerle geri yüklerseniz, seçilen depolama hesabına yaşam sürelerinin başlarında için Azure depolama hizmeti şifrelemesi etkin olmadığından emin olun.

    > * Seçilen depolama hesabına (premium veya standart), depolama türüne bağlı olarak, tüm diskleri geri premium veya standart diskler olacaktır. Şu anda bir karma mod disk geri yüklerken desteklemiyoruz.
    >
    >

1. Üzerinde **geri yükleme Yapılandırması** dikey penceresinde **Tamam** geri yükleme yapılandırmasını sonlandırmak için. Üzerinde **geri** dikey penceresinde **geri** geri yükleme işlemi tetiklemek için.

## <a name="restore-backed-up-disks"></a>Yedeklenen diskleri geri yükle
Yedeklenen disklerden ne bulunan farklı oluşturmak istediğiniz VM özelleştirmek için **geri yükleme Yapılandırması** dikey penceresinde **diskleri geri** değeri olarak **türügeriyükleme**. Bu seçenek, yedeklemeler disklerden kopyalanacak olduğu bir depolama hesabı için sorar. Bir depolama hesabı seçin, Kurtarma Hizmetleri kasasıyla aynı konumda paylaşan bir hesap seçin. Bölgesel olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda olan depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü, parantez içinde görüntülenir.

Geri yükleme işlemi tamamlandıktan sonra şunları yapabilirsiniz:

* [Geri yüklenen VM özelleştirmek için şablonu kullanın](#use-templates-to-customize-restore-vm)
* [Mevcut bir VM'ye eklemek için geri yüklenen diskleri kullanın.](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Geri yüklenen disklerden PowerShell kullanarak yeni bir VM oluşturun.](./backup-azure-vms-automation.md#restore-an-azure-vm)

Üzerinde **geri yükleme Yapılandırması** dikey penceresinde **Tamam** geri yükleme yapılandırmasını sonlandırmak için. Üzerinde **geri** dikey penceresinde **geri** geri yükleme işlemi tetiklemek için.

![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-the-restore-operation"></a>Geri yükleme işlemi İzle
Geri yükleme işlemini tetikleme sonra yedekleme hizmeti geri yükleme işlemini izlemek için bir iş oluşturur. Yedekleme hizmeti da oluşturur ve geçici olarak bildiriminde görüntüler **bildirimleri** portalının alan. Bildirim görmüyorsanız seçin **bildirimleri** bildirimlerinizi görüntülenecek simge.

![Tetiklenen geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification.png)

Açık işlem sürerken işlemi görüntülemek üzere veya sona erdiğinde görüntülemek için **yedekleme işleri** listesi.

1. Azure menüsünde **Gözat**, Hizmetler listesinde yazın **kurtarma Hizmetleri**. Yazdığınız için hizmet listesini ayarlar. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

    ![Kurtarma Hizmetleri kasasını açın](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Abonelikteki kasalarının listesi görüntülenir.

    ![Kasaları kurtarma Hizmetleri listesi](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
1. Listeden geri yüklediğiniz VM ile ilişkili kasayı seçin. Kasayı seçtiğinizde, kendi Pano açılır.

1. Kasa panosunda, **yedekleme işleri** kutucuk seçin **Azure sanal makineleri** kasayla ilişkili işleri görüntülemek için.

    ![Kasa Panosu](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    **Yedekleme işleri** dikey penceresi açılır ve işlerin listesini görüntüler.

    ![VM'lerin listesini bir kasada](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-to-customize-a-restored-vm"></a>Geri yüklenen VM özelleştirmek için şablonları kullanma
Sonra [disk geri yükleme işlemini tamamladığında](#Track-the-restore-operation), yedekleme yapılandırmasından farklı bir yapılandırma ile yeni bir VM oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulmuş şablonu kullanın. Yeni bir sanal makine bir geri yükleme noktası oluşturma işlemi sırasında oluşturulan kaynakların adlarını özelleştirmek için de kullanabilirsiniz. 

> [!NOTE]
> Şablonları, 1 Mart 2017'den sonra gerçekleştirilen kurtarma noktaları için diskleri geri yükleme bir parçası olarak eklenir. Bunlar, yönetilmeyen disk Vm'leri için geçerlidir. Yönetilen disk sanal makinelerine yönelik destek gelecek sürümlerde kullanıma sunulacaktır. 
>
>

Diskleri geri yükleme seçeneği bir parçası olarak oluşturulan şablon almak için:

1. İşin geri yükleme iş ayrıntılarına gidin.

1. Üzerinde **geri yükleme işinin ayrıntılarını** ekranındayken **şablonu Dağıt** şablon dağıtımını başlatmak için. 

     ![Geri yükleme işi detaya gitme](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
1. Üzerinde **dağıtma şablonu** kullanımı şablon dağıtımı için özel dağıtım dikey [düzenleyin ve şablonu dağıtmak](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) veya daha fazla özelleştirme tarafından ekleme [bir şablon yazma](../azure-resource-manager/resource-group-authoring-templates.md) dağıtmadan önce. 

   ![Şablon dağıtımı'nı yükleme](./media/backup-azure-arm-restore-vms/loading-template.png)
   
1. Gereken değerleri girdikten sonra kabul **hüküm ve koşullar** seçip **satın alma**.

   ![Şablon dağıtımı gönderin](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Geri yükleme sonrası adımlar
* Güvenlik nedenleriyle bir cloud-init tabanlı Linux dağıtımı, Ubuntu gibi kullanıyorsanız, parolayı engellenir geri gönderin. Geri yüklenen VM'ye VMAccess uzantısını kullanın [parola sıfırlama](../virtual-machines/linux/reset-password.md). SSH anahtarlarını sıfırlama parola post geri önlemek için bu dağıtımlarında kullanmanızı öneririz.
* Yedekleme yapılandırması sırasında mevcut uzantılar yüklü, ancak etkinleştirilmesi gerekmez. Bir sorun görürseniz, uzantıları yeniden yükleyin. 
* Yedeklenen sanal Makinenin statik IP post geri yükleme varsa, geri yüklenen VM geri yüklenen VM oluşturduğunuz sırada çakışmayı önlemek için dinamik bir IP vardır. Nasıl yapabilecekleriniz hakkında daha fazla bilgi [geri yüklenen VM'ye statik bir IP ekleme](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Geri yüklenen VM bir kullanılabilirlik değer kümesi yok. Geri yükleme diskler seçeneğini kullanmanızı öneririz [bir kullanılabilirlik kümesine ekleme](../virtual-machines/windows/tutorial-availability-sets.md) geri diskleri VM şablonları veya PowerShell kullanarak oluşturduğunuzda. 


## <a name="backup-for-restored-vms"></a>Geri yüklenen sanal makineler için yedekleme
Aynı kaynak grubuna Yedeklenen özgün VM ile aynı ada sahip bir VM geri yüklediyseniz, yedekleme, VM post geri yükleme devam eder. Yeni bir VM ise gibi sanal makine farklı kaynak grubuna geri veya geri yüklenen sanal makine için farklı bir ad belirttiğiniz, VM kabul edilir. Geri yüklenen VM için yedeklemeyi ayarlamanız gerekir.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Bir Azure veri merkezi olağanüstü durum sırasında bir VM geri yükleme
Azure yedekleme, VM'lerin çalıştığı birincil veri merkezinde bir olağanüstü durum deneyimleri ve yedekleme kasası coğrafi yedekli olacak şekilde yapılandırılmış durumda eşleştirilmiş veri merkezine yedeklenen sanal makineleri geri yükleme sağlar. Bu senaryolara sırasında eşleştirilmiş bir veri merkezinde mevcut bir depolama hesabı seçin. Geri yükleme işleminin geri kalanı aynı kalır. Yedekleme, geri yüklenen VM'yi oluşturmak için eşleştirilmiş coğrafi işlem hizmetinden kullanır. Daha fazla bilgi için [Azure veri merkezi dayanıklılık](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restore-domain-controller-vms"></a>Etki alanı denetleyicisi Vm'lerini geri yükleme
Etki alanı denetleyicisi (DC) yedekleme vm'dir yedekleme ile desteklenen bir senaryo. Bununla birlikte, geri yükleme işlemi sırasında dikkatli olmanız gerekir. Doğru geri yükleme işlemi, etki alanı yapısına bağlıdır. En basit durumda, tek bir etki alanında tek bir DC sahip. Daha sık üretim yükleri için tek bir etki alanı birden çok DC'ler, belki de bazı DC'leri ile şirket içi sizde. Son olarak, birden çok etki alanı içeren bir ormanda olabilir. 

Active Directory açısından bakıldığında, Azure VM modern desteklenen bir hiper yönetici üzerinde herhangi bir VM gibidir. Şirket içi hiper yöneticileri ile en önemli fark, Azure'da hiçbir VM konsol olduğunu ' dir. Bir konsol tam kurtarma (BMR) kullanarak kurtarma gibi belirli senaryolar için gerekli değildir-türü yedekleme. Ancak, yedekleme kasasından VM geri yükleme, BMR için tam yerini almıştır. Dizin Hizmetleri Geri Yükleme Modu'nda (DSRM), ayrıca tüm Active Directory Kurtarma senaryolarına uygun olacak şekilde kullanılabilir. Daha fazla bilgi için [sanallaştırılmış etki alanı denetleyicileri için yedekleme ve geri yükleme konuları](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) ve [Active Directory orman kurtarma için planlama](https://technet.microsoft.com/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Tek bir etki alanındaki tek DC
VM (diğer VM'ler gibi) Azure portalından veya PowerShell kullanarak geri yüklenebilir.

### <a name="multiple-dcs-in-a-single-domain"></a>Tek bir etki alanında birden fazla DC'leri
Aynı etki alanının diğer DC'leri ağ üzerinden ulaşılabilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Etki alanındaki son kalan DC olduğu veya kurtarma yalıtılmış bir ortamda gerçekleştirilen, orman kurtarma yordamı gelmelidir.

### <a name="multiple-domains-in-one-forest"></a>Bir orman içinde birden çok etki alanı
Aynı etki alanının diğer DC'leri ağ üzerinden ulaşılabilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Diğer durumlarda, orman kurtarma öneririz.

## <a name="restore-vms-with-special-network-configurations"></a>Özel ağ yapılandırmaları ile Vm'leri geri yükleme
Aşağıdaki özel ağ yapılandırmaları ile Vm'leri geri mümkündür. Ancak, bu yapılandırmaları geri yükleme işlemi devam ederken bazı yükseltilmesine gerektirir:

* Yük Dengeleyiciler (iç ve dış) altında yer alan VM'ler
* Birden çok ayrılmış IP ile Vm'leri
* Birden çok NIC içeren VM'ler

> [!IMPORTANT]
> Sanal makineler için özel ağ yapılandırması oluşturduğunuzda, geri yüklenen disklerden sanal makineler oluşturmak için PowerShell kullanmanız gerekir.
>
>

Tam olarak Vm'leri için disk geri yükledikten sonra yeniden oluşturmak için aşağıdaki adımları izleyin:

1. Geri yükleme diskleri bir kurtarma Hizmetleri kasası kullanarak [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

1. Yük Dengeleyici için gereken VM yapılandırmasını oluşturma / birden çok NIC/birden çok ayrılmış IP PowerShell cmdlet'lerini kullanarak. İstediğiniz yapılandırma ile VM oluşturmak için kullanın:

   a. Bulut hizmeti ile bir VM oluşturmak bir [iç yük dengeleyici](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Bağlanmak için bir VM oluşturma bir [internet'e yönelik Yük Dengeleyici](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/).

   c. İle bir VM [birden çok NIC](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/).

   d. İle bir VM [birden çok ayrılmış IP'ler](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/).

## <a name="next-steps"></a>Sonraki adımlar
Vm'lerinizi geri yükleyebilirsiniz, sanal makineler ile sık karşılaşılan hatalar hakkında bilgi için sorun giderme makalesine bakın. Ayrıca, görevler ile sanal makinelerinizi yönetme makaleyi gözden geçirin.

* [Sorun giderme hataları](backup-azure-vms-troubleshoot.md#restore)
* [Sanal makineleri yönetme](backup-azure-manage-vms.md)
