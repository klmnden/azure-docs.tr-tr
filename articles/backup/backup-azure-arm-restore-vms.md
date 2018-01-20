---
title: "Azure yedekleme: Azure portalını kullanarak sanal makineleri geri yükleme | Microsoft Docs"
description: "Bir Azure sanal makinesi, Azure portalını kullanarak bir kurtarma noktasından geri yükleme"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "yedeklemeyi geri; geri yükleme; kurtarma noktası;"
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: d420e0a39edf2af4bb050dd735dd7b4d1e604d6f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Sanal makineler geri yüklemek için Azure portalını kullanın
Verilerinizin anlık görüntüleri tanımlanan aralıklarla gerçekleştirerek verilerinizi koruyun. Bu anlık görüntüleri kurtarma noktaları olarak bilinir ve kurtarma Hizmetleri kasalarının depolandıkları. Onarmak veya bir sanal makine (VM) yeniden oluşturmak gerekliyse, kaydedilmiş kurtarma noktaları hiçbirini VM geri yükleyebilirsiniz. Bir kurtarma noktası geri yüklediğinizde, şunları yapabilirsiniz:

* Yedeklenen VM zaman içinde nokta gösterimidir yeni bir VM oluşturun.
* Diskler, geri yükleyin ve geri yüklenen VM özelleştirmek için işlem ile birlikte gelen şablon kullanın veya tek tek dosya kurtarma yapın. 

Bu makalede, bir VM için yeni bir VM geri yükleyin veya tüm yedeklenen diskleri geri açıklanmaktadır. Tek tek dosya kurtarma için bkz: [dosyaları bir Azure VM yedeklemeden kurtarmak](backup-azure-restore-files-from-vm.md).

![VM yedekten geri yüklemek için üç yol](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki dağıtım modeline sahiptir: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager modelini kullanarak sanal makineleri geri yüklemek için kullanılan yordamlar ve bilgi dağıtılan sağlar.
>
>

Sanal makineden bir VM veya tüm diskleri geri yedekleme iki adımdan oluşur:

* Geri yükleme için bir geri yükleme noktası seçin.
* Geri yükleme türünü seçin, yeni bir VM oluşturmak veya diskleri geri yüklemek ve gerekli parametreleri belirtin. 

## <a name="select-a-restore-point-for-restore"></a>Geri yükleme için bir geri yükleme noktası seçin
1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

2. Azure menüsünde seçin **Gözat**. Hizmetler listesinde yazın **kurtarma Hizmetleri**. Hizmetler listesi, yazdığınız için ayarlar. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

    ![Kurtarma Hizmetleri kasası](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Abonelikteki kasalarının listesi görüntülenir.

    ![Liste kurtarma Hizmetleri kasaları](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Listeden, geri yüklemek istediğiniz VM ile ilişkili kasayı seçin. Kasa seçtiğinizde, kendi panosu açılır.

    ![Kurtarma Hizmetleri kasası seçili](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Kasa panosunda üzerinde **yedekleme öğeleri** kutucuğu, select **Azure sanal makineleri**.

    ![Kasa Panosu](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    **Yedekleme öğeleri** dikey penceresi açılır ve Azure VM'lerin listesini görüntüler.

    ![Kasa VM'ler listesinde](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Listeden panoyu açmak için bir VM seçin. VM Pano içeren bir izleme alanı açılır **geri yükleme noktaları** döşeme.

    ![Geri yükleme noktaları](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. VM Panosu menüsünden seçin **geri**.

    ![Geri Yükle düğmesi](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    **Geri** dikey pencere açılır.

    ![Dikey pencereyi geri yükle](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. Üzerinde **geri** dikey penceresinde, select **geri yükleme noktası**. **Seçin geri yükleme noktası** dikey pencere açılır.

    ![Geri yükleme noktası seçin](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Varsayılan olarak, son 30 günün tüm geri yükleme noktaları iletişim kutusu görüntüler. Kullanım **filtre** geri yükleme noktaları zaman aralığını değiştirmek için görüntülenir. Varsayılan olarak, tüm turalılığını geri yükleme noktaları görüntülenir. Değiştirme **tüm geri yükleme noktaları** belirli geri yükleme noktası tutarlılık seçmek için filtre. Geri yükleme noktası türleri hakkında daha fazla bilgi için bkz: [veri tutarlılığını](backup-azure-vms-introduction.md#data-consistency).

    Geri yükleme noktası tutarlılık seçenekleri:

     * Kilitlenme tutarlı geri yükleme noktaları
     * Uygulama tutarlı geri yükleme noktaları
     * Dosya sistemi tutarlı geri yükleme noktaları
     * Tüm geri yükleme noktaları

8. Bir geri yükleme noktası seçin ve Seç **Tamam**.

    ![Geri yükleme noktası seçenekleri](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    **Geri** dikey gösterir geri yükleme noktası ayarlanır.

9. Olduğunuz zaten vardır, giderseniz için **geri** dikey. Emin bir [geri yükleme noktası seçildiğinde](#select-restore-point-for-restore)seçip **geri yükleme yapılandırmasını**. **Geri yükleme yapılandırmasını** dikey pencere açılır.

## <a name="choose-a-vm-restore-configuration"></a>Bir VM geri yükleme yapılandırmasını seçin
Geri yükleme noktası seçtikten sonra bir VM geri yükleme yapılandırması seçin. Geri yüklenen VM yapılandırmak için Azure portal veya PowerShell kullanabilirsiniz.

1. Olduğunuz zaten vardır, giderseniz için **geri** dikey. Emin bir [geri yükleme noktası seçildiğinde](#select-restore-point-for-restore)seçip **geri yükleme yapılandırmasını**. **Geri yükleme yapılandırmasını** dikey pencere açılır.

    ![Yapılandırma Sihirbazı'nı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. Üzerinde **geri yükleme yapılandırmasını** dikey penceresinde, iki seçeneğiniz vardır:

   * **Sanal makine oluşturma**

   * **Disklerini geri yükle**

Portal sağlayan bir **hızlı Oluştur** geri yüklenen VM için seçeneği. VM yapılandırması veya yeni bir VM seçenek oluşturulmasının parçası olarak oluşturulan kaynakların adları özelleştirmek için yedeklenen diskleri geri yüklemek için PowerShell veya portal'ı kullanın. İçin tercih ettiğiniz bir VM yapılandırmasına eklemek için PowerShell komutlarını kullanın. Veya, geri yüklenen VM özelleştirmek için geri yüklenen disklerle gelen şablonu kullanabilirsiniz. Bir yük dengeleyici veya birden çok NIC sahip bir VM geri yükleme hakkında daha fazla bilgi için bkz: [özel ağ yapılandırmaları olan bir VM geri](#restore-a vm-with-special-network-configurations). Windows VM kullanıyorsa [HUB lisans](../virtual-machines/windows/hybrid-use-benefit-licensing.md), diskleri geri yükleme ve VM oluşturmak için bu makalede belirtilen PowerShell/şablonu kullanın. Belirttiğinizden emin olun **lisans türü** "HUB faydaları geri yüklenen VM üzerine kullanılabilir kredi VM oluştururken Windows_Server" olarak. 
 
## <a name="create-a-new-vm-from-a-restore-point"></a>Bir geri yükleme noktasından yeni bir VM oluşturun
1. Zaten orada değilseniz [bir geri yükleme noktası seçin](#restore-a vm-with-special-network-configurations) bir geri yükleme noktasından yeni bir VM oluşturmaya başlamadan önce. Bir geri yükleme noktası seçtikten sonra **geri yükleme yapılandırmasını** dikey penceresinde girin veya seçin değerleri aşağıdaki alanların her biri için:

    a. **Türü geri**. Bir sanal makine oluşturun.

    b. **Sanal makine adı**. VM için bir ad sağlayın. Adı kaynak grubu (bir Azure Resource Manager tarafından dağıtılan VM) veya (için Klasik VM) bulut hizmeti için benzersiz olmalıdır. Abonelikte zaten mevcut değilse VM değiştirilemiyor.

    c. **Kaynak grubu**. Varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Klasik VM geri yükleme, bu alan yeni bir bulut hizmeti adını belirtmek için kullanın. Yeni bir kaynak grubu/bulut hizmeti oluşturuyorsanız, adı genel olarak benzersiz olması gerekir. Genellikle, bir bulut hizmeti adının bir genel kullanıma yönelik URL ile ilişkilendirilen: Örneğin, [buluthizmeti]. cloudapp.net. Bulut kaynak grubu/bulut hizmeti için bir ad zaten kullanımda kullanmayı denerseniz, Azure kaynak grubu/bulut hizmeti VM adıyla aynı atar. Azure kaynak gruplarını/bulut Hizmetleri ve herhangi bir benzeşim grupları ile ilişkili olmayan VM'ler görüntüler. Daha fazla bilgi için bkz: [bölgesel bir sanal ağ için benzeşim grupları geçirmek nasıl](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Sanal ağ**. VM oluştururken sanal ağı seçin. Alan abonelikle ilişkili tüm sanal ağları sağlar. VM kaynak grubunun parantez içinde görüntülenir.

    e. **Alt ağ**. Sanal ağ alt ağlar varsa, ilk alt ağ varsayılan olarak seçilidir. Ek alt ağlar varsa, kullanmak istediğiniz alt ağ seçin.

    f. **Depolama hesabı**. Bu menü kurtarma Hizmetleri kasasıyla aynı konumda depolama hesaplarını listeler. Bölge olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda ile depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü parantez içinde görüntülenir.

    ![Yapılandırma Sihirbazı'nı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

    > [!NOTE]
    > * Resource Manager tarafından dağıtılan VM geri yüklerseniz, bir sanal ağ tanımlamanız gerekir. Bir sanal ağı klasik bir VM için isteğe bağlıdır.

    > * Yönetilen disklerle VM'ler geri yüklerseniz, seçilen depolama hesabı ömrü Azure depolama hizmeti şifrelemesi için etkin olmadığından emin olun.

    > * Seçilen depolama hesabı (premium veya standart) depolama türüne bağlı olarak, tüm diskleri geri premium veya standart diskler olacaktır. Şu anda bir karma mod disklerin geri yüklerken desteklemiyoruz.
    >
    >

2. Üzerinde **geri yükleme yapılandırmasını** dikey penceresinde, select **Tamam** geri yükleme yapılandırmasını son haline getirmek için. Üzerinde **geri** dikey penceresinde, select **geri** geri yükleme işlemi tetiklemek için.

## <a name="restore-backed-up-disks"></a>Yedeklenen disklerini geri yükle
Yedeklenen disklerden ne de mevcut farklı oluşturmak istediğiniz VM özelleştirmek için **geri yükleme yapılandırmasını** dikey penceresinde, select **geri diskleri** değeri olarak **geri türü**. Bu seçenek yedeklemeleri disklerden kopyalanacak olduğu için bir depolama hesabı sorar. Bir depolama hesabı seçtiğinizde, Kurtarma Hizmetleri kasasıyla aynı konumda paylaşan bir hesap seçin. Bölge olarak yedekli depolama hesapları desteklenmez. Kurtarma Hizmetleri kasasıyla aynı konumda ile depolama hesabı yoksa, geri yükleme işlemi başlamadan önce bir oluşturmanız gerekir. Depolama hesabının çoğaltma türü parantez içinde görüntülenir.

Geri yükleme işlemi tamamlandıktan sonra şunları yapabilirsiniz:

* [Geri yüklenen VM özelleştirmek için şablonu kullanın](#use-templates-to-customize-restore-vm)
* [Varolan bir VM'e geri yüklenen diskleri kullanın](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Geri yüklenen disklerden PowerShell kullanarak yeni bir VM oluşturma](./backup-azure-vms-automation.md#restore-an-azure-vm)

Üzerinde **geri yükleme yapılandırmasını** dikey penceresinde, select **Tamam** geri yükleme yapılandırmasını son haline getirmek için. Üzerinde **geri** dikey penceresinde, select **geri** geri yükleme işlemi tetiklemek için.

![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-the-restore-operation"></a>Geri yükleme işlemini izlemek
Geri yükleme işlemi tetiklemek sonra yedekleme hizmeti geri yükleme işlemini izlemek için bir proje oluşturur. Yedekleme hizmetinin da oluşturur ve geçici olarak bildiriminde görüntüler **bildirimleri** portal alanı. Bildirim görmüyorsanız seçin **bildirimleri** bildirimlerinizi görüntülemek için simge.

![Tetiklenen geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification.png)

Bunu işlerken işlemi görüntülemek veya tamamlandığında görüntülemek için açık **yedekleme işleri** listesi.

1. Azure menüsünde seçin **Gözat**ve Hizmetler listesinde yazın **kurtarma Hizmetleri**. Hizmetler listesi, yazdığınız için ayarlar. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

    ![Açık kurtarma Hizmetleri kasası](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Abonelikteki kasalarının listesi görüntülenir.

    ![Liste kurtarma Hizmetleri kasaları](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Listeden, geri VM ile ilişkili kasayı seçin. Kasa seçtiğinizde, kendi panosu açılır.

3. Kasa panosunda **yedekleme işlerini** kutucuğu, select **Azure sanal makineleri** kasayla ilişkili işleri görüntülemek için.

    ![Kasa Panosu](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    **Yedekleme işleri** dikey penceresi açılır ve işlerin listesini görüntüler.

    ![Bir kasadaki VM'ler listesi](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-to-customize-a-restored-vm"></a>Geri yüklenen VM özelleştirmek için şablonlar kullanın
Sonra [geri yükleme diskleri işlemi tamamlandığında](#Track-the-restore-operation), bir yapılandırma ile yeni bir VM yedekleme yapılandırması farklı oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulan şablonu kullanın. Bir geri yükleme noktasından yeni bir VM oluşturma işlemi sırasında oluşturulan kaynakların adları özelleştirmek için de kullanabilirsiniz. 

> [!NOTE]
> Şablonlar, geri yükleme diskleri 1 Mart 2017 sonra gerçekleştirilecek kurtarma noktaları için bir parçası olarak eklenir. Bunlar, yönetilmeyen disk VM'ler için uygulanabilir. Yönetilen disk VM'ler için destek gelecek sürümlerde kullanıma sunulacaktır. 
>
>

Geri yükleme diskler seçeneği bir parçası olarak oluşturulan şablon almak için:

1. İşin geri yükleme iş ayrıntılarına gidin.

2. Üzerinde **geri yükleme iş ayrıntılarını** ekran, select **dağıtma şablonu** şablon dağıtımını başlatmak için. 

     ![İş ayrıntıya geri yükleme](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
3. Üzerinde **dağıtma şablonu** özel dağıtım, kullanım şablon dağıtımı dikey [düzenleyin ve şablonu dağıtmak](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) veya daha fazla özelleştirme tarafından ilave [şablon yazma](../azure-resource-manager/resource-group-authoring-templates.md) dağıtmadan önce. 

   ![Şablon dağıtımı yükleme](./media/backup-azure-arm-restore-vms/loading-template.png)
   
4. Gereken değerleri girdikten sonra kabul **hüküm ve koşullar** seçip **satın alma**.

   ![Şablon dağıtımı gönderme](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Geri yükleme sonrası adımlar
* Güvenlik nedenleriyle bir bulut init tabanlı Linux dağıtım, Ubuntu gibi kullanırsanız, parola engellendi geri yükleme sonrası. Geri yüklenen VM üzerinde VMAccess uzantısını kullanmak [parola sıfırlama](../virtual-machines/linux/classic/reset-access-classic.md). Parola post geri sıfırlama önlemek için bu dağıtım üzerinde SSH anahtarları kullanmanızı öneririz.
* Yedekleme yapılandırması sırasında mevcut uzantıları yüklendi, ancak bunlar etkin olmaz. Bir sorun görürseniz, uzantılarını yeniden yükleyin. 
* Yedeklenen VM statik IP post geri yükleme varsa, geri yüklenen VM geri yüklenen VM oluşturduğunuzda çakışmayı önlemek için dinamik IP vardır. Ne yapabileceğiniz hakkında daha fazla bilgi [geri yüklenen VM için bir statik IP eklemek](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Geri yüklenen VM bir kullanılabilirlik değeri ayarlanmış sahip değil. Geri yükleme diskleri seçeneğini kullanmanızı öneririz [bir kullanılabilirlik kümesi ekleme](../virtual-machines/windows/tutorial-availability-sets.md) VM PowerShell veya şablonları kullanarak oluşturduğunuzda geri diskler. 


## <a name="backup-for-restored-vms"></a>Geri yüklenen VM'ler için yedekleme
Aynı kaynak grubunda Yedeklenen özgün VM ile aynı adla bir VM geri yüklediyseniz, yedekleme VM post geri yükleme devam eder. Yeni bir VM ise gibi farklı bir kaynak grubu için VM geri veya geri yüklenen VM için farklı bir ad belirttiğiniz, VM kabul edilir. Yedekleme geri yüklenen VM ayarlamanız gerekir.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>VM bir Azure veri merkezi olağanüstü durum sırasında geri yükleme
Azure yedekleme, yedeklenen VM'ler eşleştirilmiş datacenter VM'ler çalıştırdığı birincil veri merkezindeki bir olağanüstü durum karşılaştığında ve yedekleme kasası coğrafi olarak yedekli olacak şekilde yapılandırılmış durumda geri yükleme sağlar. Bu tür senaryoları sırasında eşleştirilmiş bir veri merkezinde mevcut bir depolama hesabı seçin. Geri yükleme işleminin geri kalanında aynı kalır. Yedekleme, geri yüklenen VM oluşturmak için eşleştirilmiş coğrafi işlem hizmetinden kullanır. Daha fazla bilgi için bkz: [Azure veri merkezi dayanıklılık](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restore-domain-controller-vms"></a>Etki alanı denetleyicisi sanal makineleri geri yükleme
Etki alanı denetleyicisi (DC) yedekleme VM'ler yedekleme desteklenen bir senaryo değil. Ancak, geri yükleme işlemi sırasında dikkatli olmanız gerekir. Doğru geri yükleme işlemi, etki alanı yapısına bağlıdır. En basit durumda, tek bir etki alanında tek bir DC sahip. Daha sık üretim yükleri, tek bir etki alanı birden çok DC'ler, belki de bazı DC'leri ile şirket içi sahip. Son olarak, birden çok etki alanı içeren bir ormanda olabilir. 

Bir Active Directory açısından bakıldığında, başka bir VM modern desteklenen bir hiper yöneticide Azure VM gibidir. Şirket içi hiper ile en önemli fark, hiçbir VM konsolu ile Azure kullanılabilir olduğunu ' dir. Bir konsol tam kurtarma (BMR) kullanarak kurtarma gibi belirli senaryolar için gerekli değildir-türü yedekleme. Ancak, VM geri yükleme yedekleme kasasından tam bir BMR yerini alır. Dizin Hizmetleri Geri Yükleme Modu'nda (DSRM), ayrıca tüm Active Directory Kurtarma senaryolarına uygun şekilde kullanılabilir. Daha fazla bilgi için bkz: [sanallaştırılmış etki alanı denetleyicileri için yedekleme ve geri yükleme hakkında önemli noktalar](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) ve [Active Directory orman kurtarma planlama](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Tek bir etki alanındaki tek DC
VM (tüm diğer VM gibi) Azure portalından veya PowerShell'i kullanarak geri yüklenebilir.

### <a name="multiple-dcs-in-a-single-domain"></a>Tek bir etki alanındaki birden çok DC'leri
Aynı etki alanının diğer DC'ler ağ üzerinden erişilebilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Etki alanındaki son kalan DC olduğu veya kurtarma yalıtılmış bir ağda gerçekleştirilir, orman kurtarma yordamı gelmelidir.

### <a name="multiple-domains-in-one-forest"></a>Bir ormandaki birden çok etki alanları
Aynı etki alanının diğer DC'ler ağ üzerinden erişilebilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Diğer durumlarda, orman kurtarma öneririz.

## <a name="restore-vms-with-special-network-configurations"></a>Özel ağ yapılandırmaları ile sanal makineleri geri yükleme
Ve aşağıdaki özel ağ yapılandırmaları ile sanal makineleri geri mümkündür. Ancak, bu yapılandırmalar geri yükleme sürecinden sırasında bazı ayrıcalık gerektirir:

* Yük Dengeleyici (iç ve dış) altında yer alan VM'ler
* Birden çok ayrılmış IP ile sanal makineleri
* Birden çok NIC içeren VM'ler

> [!IMPORTANT]
> VM'ler için özel ağ yapılandırması oluşturduğunuzda, geri yüklenen disklerden VM'ler oluşturmak için PowerShell kullanmanız gerekir.
>
>

Tam olarak VM'ler diske geri yükledikten sonra yeniden oluşturmak için aşağıdaki adımları izleyin:

1. Diskleri bir kurtarma Hizmetleri kasası kullanarak geri yükleme [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

2. Yük Dengeleyici için gereken VM yapılandırması oluştur / birden çok NIC/birden çok ayrılmış IP'si PowerShell cmdlet'lerini kullanarak. İstediğiniz yapılandırma ile VM oluşturmak için kullanın:

   a. Bulut hizmeti ile bir VM oluşturma bir [iç yük dengeleyici](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Bağlanmak için bir VM oluşturma bir [internet'e yönelik Yük Dengeleyici](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/).

   c. Bir VM ile oluşturma [birden çok NIC](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/).

   d. Bir VM ile oluşturma [birden çok ayrılmış IP'ler](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/).

## <a name="next-steps"></a>Sonraki adımlar
Vm'leriniz geri yükleyebilir, sanal makineler ile sık karşılaşılan hatalar hakkında bilgi için sorun giderme makalesine bakın. Ayrıca Vm'lerinizi görevlerle yönetme makaleyi göz atın.

* [Sorun giderme](backup-azure-vms-troubleshoot.md#restore)
* [Sanal makineleri yönetme](backup-azure-manage-vms.md)
