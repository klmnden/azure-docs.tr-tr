---
title: 'Azure yedekleme: Azure portalını kullanarak sanal makineleri geri yükleme'
description: Bir Azure sanal makinesi, Azure portalını kullanarak bir kurtarma noktasından geri yükleme
services: backup
author: geethalakshmig
manager: vijayts
keywords: yedeklemeyi geri yükleme; geri yükleme; kurtarma noktası;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: 0d78ae294cea383fbe59a1f7968d8bf18b1942d1
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422965"
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
1. [Azure Portal](http://portal.azure.com/) oturum açın.

2. Azure menüsünde **tüm hizmetleri**. Hizmetler listesinde yazın **kurtarma Hizmetleri** veya Git **depolama** burada **kurtarma Hizmetleri kasaları** olan listelenen, onu seçin.

    ![Kurtarma Hizmetleri kasası](./media/backup-azure-arm-restore-vms/open-recovery-services-vault1.png)

3. Abonelikteki kasalarının listesi görüntülenir.

    ![Kasaları kurtarma Hizmetleri listesi](./media/backup-azure-arm-restore-vms/list-of-rs-vaults1.png)

4. Kurtarma Hizmetleri kasalarının listesinden geri yüklemek istediğiniz VM ile ilişkili kasayı seçin. Kasayı seçtiğinizde, kendi Pano açılır.

    ![Kurtarma Hizmetleri kasası seçilmedi](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade1.png)

5. Kasa panosunda üzerinde **yedekleme öğeleri** kutucuk seçin **Azure sanal makine**.

    ![Kasa Panosu](./media/backup-azure-arm-restore-vms/vault-dashboard1.png)

6. **Yedekleme öğeleri** Azure VM'lerin listesini içeren dikey pencere açılır.

    ![Kasadaki sanal makinelerinin listesi](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault1.png)

7. Listeden panoyu açmak için bir VM seçin. Sanal makine Panosu'ndan içeren bir izleme alan açılır **kurtarma noktaları**. Tüm VM düzeyinde işlemler ister **Şimdi Yedekle**, **dosya kurtarma**, **yedeklemeyi Durdur** bu dikey pencereden gerçekleştirilebilir.
Bu dikey penceresinden birçok yönden, geri yükleme gerçekleştirilebilir. Bu dikey pencere yalnızca son 30 günde kurtarma noktaları listelediğine dikkat edin.

    bir) sağ bu dikey pencereyi (30 günden az) kurtarma noktasına tıklayın ve başlatmak **VM geri yükleme**.

    b) Kurtarma geri dikey penceresinde burada sağlanan tıklatın kullanılabilir 30 günden fazla işaret eder.

    c) **VM geri yükleme** menüde üstbilgisi listelemek ve sanal makinelerin tercih edilen olarak özelleştirilmiş tarihleri filtrelemek için bir seçenek sağlar.

    Görüntülenen geri yükleme noktalarını zaman aralığını değiştirmek için filtreyi kullanın. Varsayılan olarak, tüm bunları listeledik geri yükleme noktaları görüntülenir. Belirli bir geri yükleme noktası tutarlılığı seçmek için tüm geri yükleme noktaları filtreyi değiştirin. Geri yükleme noktası türleri hakkında daha fazla bilgi için bkz. [veri tutarlılığı](backup-azure-vms-introduction.md#data-consistency).

    Geri yükleme noktası tutarlılığı seçenekleri:
    - Kilitlenme tutarlı geri yükleme noktaları
    - Uygulama tutarlı geri yükleme noktaları
    - Dosya sistemiyle tutarlı geri yükleme noktaları
    - Tüm geri yükleme noktaları

  ![Geri yükleme noktaları](./media/backup-azure-arm-restore-vms/vm-blade1.png)

    >  [!NOTE]
    > Kurtarma türünü, müşteri depolama hesabındaki kasası veya her ikisi içinde olup olmadığını gösterir. Daha fazla bilgi edinin [anında kurtarma noktası](https://azure.microsoft.com/blog/large-disk-support/).

8. Üzerinde **geri** dikey penceresinde **geri yükleme noktası**.

    ![Geri yükleme noktası seçin](./media/backup-azure-arm-restore-vms/select-recovery-point1.png)

    **Geri** dikey pencerede gösterilir öğesini tıklatarak geri yükleme noktası ayarlandığını **Tamam**.
9. Sizin olduğunuz zaten vardır, Git, **geri** dikey penceresi. Emin bir [geri yükleme noktası seçili](#select-a-restore-point-for-restore)seçip **geri yükleme Yapılandırması**. **Geri yükleme Yapılandırması** dikey penceresi açılır.

## <a name="choose-a-vm-restore-configuration"></a>Bir VM geri yükleme yapılandırması seçin
Geri yükleme noktası seçtikten sonra bir VM geri yükleme yapılandırması seçin. Geri yüklenen VM'yi yapılandırmak için Azure portalını veya PowerShell'i kullanabilirsiniz.

1. Sizin olduğunuz zaten vardır, Git, **geri** dikey penceresi. Emin bir [geri yükleme noktası seçili](#select-a-restore-point-for-restore)seçip **geri yükleme Yapılandırması**. **Geri yükleme Yapılandırması** dikey penceresi açılır.
2. Bu dikey pencere, şu anda iki seçenekleri bir olan içerir **Yeni Oluştur** varsayılan ve diğer olduğu olduğu **varolan** olduğu mevcut yapılandırmaları koruma yalnızca disk veya diskler değiştirmek için yerinde geri yükleme ve uzantıları.

> [!NOTE]
> Tüm sanal disklerin, ağ ayarları, yapılandırma ve uzantıları ile önümüzdeki birkaç ay içinde değiştirerek üzerinde çalışıyoruz.
>
>

 İçinde **Yeni Oluştur** yeni sanal makine veya yeni diskler, verileri geri yükler seçeneği iki seçeneğiniz vardır:

 ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/restore-configuration-create-new.png)

 - **Sanal makine oluşturma**
 - **Diskleri geri yükle**

![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)

Portal sağlar bir **hızlı Oluştur** geri yüklenen VM için seçenek. VM yapılandırması veya yeni bir VM seçenek oluşturulmasının bir parçası oluşturulan kaynakların adlarını özelleştirmek için yedeklenen diskleri geri yüklemek için PowerShell veya portal'ı kullanın. Bunları kendi seçtiğiniz VM yapılandırması için eklemek için PowerShell komutlarını kullanın. Veya geri yüklenen VM özelleştirmek için geri yüklenen diskleri ile birlikte gelen şablon kullanabilirsiniz. Birden çok NIC içeren veya bir yük dengeleyici altında olan bir VM geri yükleme hakkında daha fazla bilgi için bkz: [bir sanal özel ağ yapılandırmalarını geri](#restore-vms-with-special-network-configurations). Windows VM'nizi kullanıyorsa [HUB lisanslama](../virtual-machines/windows/hybrid-use-benefit-licensing.md), diskleri geri yükle ve VM'yi oluşturmak için bu makalede belirtildiği gibi PowerShell/şablonu kullanın. Belirttiğiniz emin **lisans türü** "geri yüklenen VM üzerindeki HUB avantajları yararlanabilmek için bir VM oluştururken Windows_Server" olarak. Bu daha sonra oluşturulmasını VM de yapılabilir unutmayın.

## <a name="create-a-new-vm-from-a-restore-point"></a>Geri yükleme noktasından yeni VM oluşturma
1. Üzerinde **geri yükleme Yapılandırması** dikey bahsedilen içinde bölümünde önce girin veya seçin değerleri aşağıdaki alanların her biri için:

    a. **Geri yükleme türü**. Sanal makine oluşturur.

    b. **Sanal makine adı**. Abonelikte mevcut değil VM adı girin.

    c. **Kaynak grubu**. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Klasik VM'yi geri yüklemek, yeni bir bulut hizmeti adını belirtmek için bu alanı kullanın. Yeni bir kaynak grubu/bulut hizmeti oluşturuyorsanız, adı genel olarak benzersiz olmalıdır. Genellikle, bulut hizmeti adı genel kullanıma yönelik URL ile ilişkilendirilen: Örneğin, [buluthizmeti]. cloudapp.net. Bulut kaynak grup/bulut hizmeti için bir ad zaten kullanımda kullanmayı denerseniz, Azure kaynak grubu/bulut hizmeti VM adıyla aynı atar. Azure kaynak grupları/cloud services ve Vm'leri herhangi bir benzeşim grupları ile ilişkili olmayan görüntüler. Daha fazla bilgi için [bölgesel sanal ağ için benzeşim grupları geçirme](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Sanal ağ**. VM oluştururken sanal ağı seçin. Alan, abonelikle ilişkili tüm sanal ağları sağlar. Sanal makinenin kaynak grubunu, parantez içinde görüntülenir.

    e. **Alt ağ**. İlk alt ağ, sanal ağ alt ağlar varsa, varsayılan olarak seçilidir. Ek alt ağlar varsa, kullanmak istediğiniz alt ağı seçin.

    f. **Depolama konumu**. Hazırlama konumu depolama hesaplarıdır. Bu menü kurtarma Hizmetleri kasasıyla aynı konumda depolama hesaplarını listeler. Bölgesel olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda olan depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü, parantez içinde görüntülenir.

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

    > [!NOTE]
    > * Bir sanal ağı klasik bir sanal makine için isteğe bağlı ve Resource Manager tarafından dağıtılan sanal makine için zorunlu ' dir.

    > * Depolama, depolama konumu hazırlama hesabı (premium veya standart) geri yükleme disk depolama türü karar sağlanan yazın. Şu anda bir karma mod disk geri yüklerken desteklemiyoruz.
    >
    >

2. Üzerinde **geri yükleme Yapılandırması** dikey penceresinde **Tamam** geri yükleme yapılandırmasını sonlandırmak için. Üzerinde **geri** dikey penceresinde **geri** geri yükleme işlemi tetiklemek için.

## <a name="restore-backed-up-disks"></a>Yedeklenen diskleri geri yükle
Türü değeri geri yüklemek **geri yükleme disk** içinde **geri yükleme Yapılandırması** özelleştirilmiş yapılandırmaları olan bir VM oluşturmak için dikey sağlar. Diskleri geri yüklerken seçilmesi için depolama hesabı kurtarma Hizmetleri kasasıyla aynı konumda olmalıdır. Kurtarma Hizmetleri kasasıyla aynı konumda olan depolama hesabı yoksa bir depolama hesabı oluşturmak için zorunludur. ZRS depolama hesapları desteklenmez. Depolama hesabı çoğaltma türü, parantez içinde görüntülenir.

POST geri yükleme işlemi, aşağıda kullanın:
* [Geri yüklenen VM özelleştirmek için şablonu kullanın](#use-templates-to-customize-restore-vm)
* [Mevcut bir VM'ye eklemek için geri yüklenen diskleri kullanın.](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Geri yüklenen disklerden PowerShell kullanarak yeni bir VM oluşturun.](./backup-azure-vms-automation.md#restore-an-azure-vm)

Üzerinde **geri yükleme Yapılandırması** dikey penceresinde **Tamam** geri yükleme yapılandırmasını sonlandırmak için. Üzerinde **geri** dikey penceresinde **geri** geri yükleme işlemi tetiklemek için.

![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

İçinde **yerde geri** sekmesinden yapılır **değiştirin mevcut**.

## <a name="replace-existing-disks-from-a-restore-point"></a>Var olan diskleri geri yükleme noktasından değiştirin
**Varolan** seçeneği yardımcı olur. geçerli VM mevcut diskleri seçilen geri yükleme noktası ile değiştirin. Bu işlem, yalnızca geçerli VM varsa gerçekleştirilebilir. Tüm sebeplerden dolayı silinmişse, bu işlem gerçekleştirilemez. Alternatif olarak, yapmanız önerilir **Yeni Oluştur** VM veya devam etmek için diskleri geri yükleme işlemleri. Değer başlatma işlemleri diskleri önce bir önlem olarak bu işlem sırasında biz verileri yedekleyin. Bu bir anlık görüntü ve ayrıca bir kurtarma noktası bekletme süresi yapılandırılan yedekleme İlkesi'nde zamanlanmış olarak kasadaki oluşturur. Diskleri geri yükleme noktası varsa, daha fazla/geçerli VM ve ardından diskleri geri yükleme noktası sayısı'den az yalnızca VM ücreti yansıtılır. **Varolan** seçenektir yönetilmeyen diskler ve şifrelenmiş VM'ler için şu anda desteklenmiyor. İçin de desteklenmeyen [VM genelleştirilmiş](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) ve kullanılarak oluşturulan VM'ler için [özel görüntüleri](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).  

 Üzerinde **geri yükleme Yapılandırması** dikey penceresinde seçilmesi gereken yalnızca giriştir **hazırlama konumu**.

   ![Geri yükleme yerine var olan Yapılandırma Sihirbazı](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

 a. **Geri yükleme türü**. Diskleri geri yükleme noktası seçilmedi mevcut VM'deki diskler değiştirecek temsil eden değiştirin.

 b. **Hazırlama konumu**. Hazırlama konumu için yönetilen diskler depolama hesaplarıdır. Bu menü kurtarma Hizmetleri kasasıyla aynı konumda depolama hesaplarını listeler. Bölgesel olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda olan depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü, parantez içinde görüntülenir.

## <a name="track-the-restore-operation"></a>Geri yükleme işlemi İzle
Geri yükleme işlemini tetikleme sonra yedekleme hizmeti geri yükleme işlemini izlemek için bir iş oluşturur. Yedekleme hizmeti da oluşturur ve geçici olarak bildiriminde görüntüler **bildirimleri** portalının alan. Bildirim görmüyorsanız seçin **bildirimleri** bildirimlerinizi görüntülenecek simge.

![Tetiklenen geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Gitmek için bildirim köprüye tıklayın **BackupJobs** listesi. Tüm işlemler için belirli bir işin ayrıntılarını kullanılabilir **BackupJobs** listeler. Gidebilirsiniz **BackupJobs** seçin, yedekleme işleri tıklayarak kasa panosundan kutucuğuna **Azure sanal makine** kasayla ilişkili işleri görüntülemek için.

**Yedekleme işleri** dikey penceresi açılır ve işlerin listesini görüntüler.

![VM'lerin listesini bir kasada](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

## <a name="use-templates-to-customize-a-restored-vm"></a>Geri yüklenen VM özelleştirmek için şablonları kullanma
Sonra [disk geri yükleme işlemini tamamladığında](#Track-the-restore-operation), yedekleme yapılandırmasından farklı bir yapılandırma ile yeni bir VM oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulmuş şablonu kullanın. Yeni bir sanal makine bir geri yükleme noktası oluşturma işlemi sırasında oluşturulan kaynakların adlarını özelleştirmek için de kullanabilirsiniz.

![Geri yükleme işi detaya gitme](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

Diskleri geri yükleme seçeneği bir parçası olarak oluşturulan şablon almak için:

1. Git **geri yükleme işinin ayrıntılarını** işin.

2. Üzerinde **geri yükleme işinin ayrıntılarını** ekranındayken **şablonu Dağıt** şablon dağıtımını başlatmak için.

3. Üzerinde **dağıtma şablonu** kullanımı şablon dağıtımı için özel dağıtım dikey [düzenleyin ve şablonu dağıtmak](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) veya daha fazla özelleştirme tarafından ekleme [bir şablon yazma](../azure-resource-manager/resource-group-authoring-templates.md) dağıtmadan önce.

  ![Şablon dağıtımı'nı yükleme](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Gereken değerleri girdikten sonra kabul **hüküm ve koşullar** seçip **satın alma**.

  ![Şablon dağıtımı gönderin](./media/backup-azure-arm-restore-vms/submitting-template1.png)

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

   c. İle bir VM [birden çok NIC](../virtual-machines/windows/multiple-nics.md).

   d. İle bir VM [birden çok ayrılmış IP'ler](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar
Vm'lerinizi geri yükleyebilirsiniz, sanal makineler ile sık karşılaşılan hatalar hakkında bilgi için sorun giderme makalesine bakın. Ayrıca, görevler ile sanal makinelerinizi yönetme makaleyi gözden geçirin.

* [Sorun giderme hataları](backup-azure-vms-troubleshoot.md#restore)
* [Sanal makineleri yönetme](backup-azure-manage-vms.md)
