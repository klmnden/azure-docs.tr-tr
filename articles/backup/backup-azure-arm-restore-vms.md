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
ms.openlocfilehash: ac0c727e41ad9b361ea9f558a97f16446c12ef0e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793385"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Sanal makineleri geri yükleme için Azure portalını kullanma
Tanımlı aralıklarla verilerinizin anlık görüntülerini alarak verilerinizi koruyun. Bu anlık görüntüler, Kurtarma noktaları olarak bilinir ve kurtarma Hizmetleri kasalarında depolandıkları. Onarım veya bir sanal makine (VM) yeniden gerekliyse, kaydedilmiş kurtarma noktalarının birini VM geri yükleyebilirsiniz. Bir kurtarma noktasından geri yüklediğinizde, şunları yapabilirsiniz:

* Yeni bir VM oluşturun: Yedeklenen sanal makinenizin'bir-belirli bir noktaya kurtarma noktasından.
* Diskleri geri yükle: Tek tek dosya kurtarma yapmak veya geri yüklenen VM özelleştirme işlemi ile birlikte gelen şablon kullanın.

Bu makalede, bir VM için yeni bir sanal makine geri yükleme ya da tüm yedeklenen diskleri geri açıklanmaktadır. Tek tek dosya kurtarma için bkz: [dosyaları bir Azure VM yedekten kurtarma](backup-azure-restore-files-from-vm.md).

![Sanal makine yedeklemesini geri yükleme için üç yol](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)


VM'den bir VM veya tüm diskleri geri yükleme yedeği iki adımdan oluşur:

* Geri yükleme için bir geri yükleme noktası seçin.
* Geri yükleme türünü seçin
    - 1. seçenek: Yeni VM oluşturma
    - 2. seçenek: Diskleri geri yükleyin.

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

    - Son 30 güne ait bir geri yükleme noktası kullanarak geri yüklemek için VM'ye sağ tıklayın > **VM geri yükleme**.
    - 30 günden daha eski bir geri yükleme noktası kullanarak geri yüklemek için altındaki bağlantıya tıklayın **geri yükleme noktaları**.
    - Liste ve Vm'leri ile özelleştirilmiş tarihleri filtrelemek için tıklayın **VM geri yükleme** menüsünde. Görüntülenen geri yükleme noktalarını zaman aralığını değiştirmek için filtreyi kullanın. Ayrıca, farklı türde veri tutarlılığı filtre uygulayabilirsiniz.
8. Geri yükleme noktası ayarları gözden geçirin:
    - Veri tutarlılığını gösterildiği [tutarlılık türü](backup-azure-vms-introduction.md#consistency-types) kurtarma noktası.
    - **Kurtarma türünü** noktası (cinsinden bir depolama hesabı, kasa veya her ikisinde. depolandığı gösterir [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/large-disk-support/) anında kurtarma noktaları hakkında.

  ![Geri yükleme noktaları](./media/backup-azure-arm-restore-vms/vm-blade1.png)
9. Bir geri yükleme noktası seçin.

10. Seçin **geri yükleme Yapılandırması**. **Geri yükleme Yapılandırması** dikey penceresi açılır.

## <a name="choose-a-vm-restore-configuration"></a>Bir VM geri yükleme yapılandırması seçin
Geri yükleme noktası seçtikten sonra bir VM geri yükleme yapılandırması seçin. Geri yüklenen VM'yi yapılandırmak için Azure portalını veya PowerShell'i kullanabilirsiniz.

### <a name="restore-options"></a>Geri yükleme seçenekleri

**Seçenek** | **Ayrıntılar**
--- | ---
**Oluşturma yeni-VM oluşturma** | Hızlı VM oluşturma ile eşdeğerdir. Temel VM çalışır duruma geri yükleme noktasından alır.<br/><br/> VM ayarlarını geri yükleme işleminin bir parçası değiştirilebilir.
**Geri yükleme yeni disk oluşturma** | Geri yükleme işleminin bir parçası özelleştirebileceğiniz bir VM oluşturur.<br/><br/> Bu seçenek, belirttiğiniz depolama hesabına VHD kopyalar.<br/><br/> -Depolama hesabı, kasa ile aynı konumda olmalıdır.<br/><br/> Uygun bir depolama hesabınız yoksa, oluşturmanız gerekir.<br/><br/> Depolama hesabı çoğaltma türü, parantez içinde gösterilir. Bölgesel olarak yedekli depolama (ZRS) desteklenmez.<br/><br/> Depolama hesabından, mevcut bir VM'ye geri yüklenen diski veya PowerShell kullanarak geri yüklenen disklerden yeni bir VM oluşturun.
**Varolan** | Bu seçenekle bir VM oluşturmanız gerekmez.<br/><br/> Geçerli VM mevcut olması gerekir. Silinmiş, bu seçenek kullanılamaz.<br/><br/> Azure Backup, var olan sanal makinenin anlık görüntüsünü alır ve belirtilen Hazırlama konumunda depolar. Var olan VM'ye bağlı diskleri, ardından seçilen geri yükleme noktası ile değiştirilir. Daha önce oluşturduğunuz anlık görüntü kasaya kopyalanır ve yedekleme saklama ilkeniz uygun olarak bir kurtarma noktası olarak depolanır.<br/><br/> Replace varolan şifrelenmemiş yönetilen sanal makineler için desteklenir. Yönetilmeyen diskler için desteklenmeyen [VM genelleştirilmiş](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource), için veya Vm'leri [özel görüntüleri kullanılarak oluşturulan](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Geri yükleme noktası daha az veya daha geçerli VM disk varsa disk geri yükleme noktası sayısı yalnızca VM yansıtır.

> [!NOTE]
> Örneğin bir iç veya dış yük dengeleyici tarafından yönetilen veya birden çok NIC veya birden çok ayrılmış IP adresleri, ağ ayarları, Gelişmiş bir VM ile geri yüklemek istiyorsanız, PowerShell ile geri yükleyin. [Daha fazla bilgi edinin](#restore-vms-with-special-network-configurations).
> Bir Windows VM kullanıyorsa [HUB lisanslama](../virtual-machines/windows/hybrid-use-benefit-licensing.md), kullanın **geri yeni disk oluşturma** seçeneğini ve ardından PowerShell ya da geri yükleme şablon ile VM oluşturmak için **lisans türü** ayarlayın **Windows_Server**. Bu ayar, VM oluşturduktan sonra da uygulanabilir.


Geri yükleme gibi ayarlama belirtin:

1. İçinde **geri**seçin bir [geri yükleme noktası](#select-a-restore-point-for-restore) > **geri yükleme Yapılandırması**.
2. İçinde **geri yükleme Yapılandırması**, yukarıdaki tabloda özetlenen ayarları göre sanal geri yüklemek istediğiniz yöntemini seçin.

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)


## <a name="create-new-create-a-vm"></a>Oluşturma yeni-VM oluşturma

1. İçinde **geri yükleme Yapılandırması** > **Yeni Oluştur** > **geri yükleme türü**seçin **sanal makine oluşturma** .
2. İçinde **sanal makine adı**, abonelikte mevcut olmayan bir sanal makine belirtin.
3. İçinde **kaynak grubu**, yeni VM için mevcut bir kaynak grubunu seçin veya genel olarak benzersiz bir ada sahip yeni bir tane oluşturun. Zaten mevcut bir ad atamanız durumunda Azure VM adıyla aynı grup atar.
4. İçinde **sanal ağ**, hangi VM yerleştirilecek sanal ağı seçin. Abonelikle ilişkili tüm Vnet'lerin görüntülenir. Alt ağ seçin. İlk alt ağ, varsayılan olarak seçilidir.
5. İçinde **depolama konumu**, VM için kullanılan depolama türünü belirtin.

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. İçinde **geri yükleme Yapılandırması**seçin **Tamam**. İçinde **geri**, tıklayın **geri** geri yükleme işlemi tetiklemek için.



## <a name="create-new-restore-disks"></a>Geri yükleme yeni disk oluşturma

1. İçinde **geri yükleme Yapılandırması** > **Yeni Oluştur** > **geri yükleme türü**seçin **diskleri geri**.
2. İçinde **kaynak grubu**, geri yüklenen diskleri için mevcut bir kaynak grubunu seçin veya genel olarak benzersiz bir ada sahip yeni bir tane oluşturun.
3. İçinde **depolama hesabı**, VHD'lerin kopyalanacağı için hesabı belirtin. Hesabı, kasa ile aynı bölgede olduğundan emin olun.

    ![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. İçinde **geri yükleme Yapılandırması**seçin **Tamam**. İçinde **geri**, tıklayın **geri** geri yükleme işlemi tetiklemek için.
5. Disk geri yüklendikten sonra VM geri yükleme işlemini tamamlamak için aşağıdakilerden birini yapabilirsiniz.

    - Oluşturulan şablon ayarlarını özelleştirmek ve VM dağıtımı tetiklemek için geri yükleme işleminin bir parçası olarak kullanın. Varsayılan şablon ayarları düzenleyin ve VM Dağıtım Şablonu gönderin.
    - Yapabilecekleriniz [geri yüklenen diski](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps) var olan bir sanal makineye.
    - Yapabilecekleriniz [yeni VM oluşturma](backup-azure-vms-automation.md#restore-an-azure-vm) PowerShell kullanarak geri yüklenen disklerden.


## <a name="replace-existing-disks"></a>Var olan diskleri değiştirin

Var olan geçerli VM diskleri seçilen geri yükleme noktası ile değiştirmek için bu seçeneği kullanın.

1. İçinde **geri yükleme Yapılandırması**, tıklayın **varolan**.
2. İçinde **geri yükleme türü**seçin **disk/sn yerine** (var olan VM diski veya diskleri değiştirecek geri yükleme noktası).
3. İçinde **hazırlama konumu**, geçerli yönetilen disklerin anlık görüntüleri nereye kaydedileceğini belirtin.

   ![Geri yükleme yerine var olan Yapılandırma Sihirbazı](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="track-the-restore-operation"></a>Geri yükleme işlemi İzle
Geri yükleme işlemini tetikleme sonra yedekleme hizmeti geri yükleme işlemini izlemek için bir iş oluşturur. Yedekleme hizmeti da oluşturur ve geçici olarak bildiriminde görüntüler **bildirimleri** portalının alan. Bildirim görmüyorsanız seçin **bildirimleri** bildirimlerinizi görüntülenecek simge.

![Tetiklenen geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Gitmek için bildirim köprüye tıklayın **BackupJobs** listesi. Tüm işlemler için belirli bir işin ayrıntılarını kullanılabilir **BackupJobs** listeler. Gidebilirsiniz **BackupJobs** seçin, yedekleme işleri tıklayarak kasa panosundan kutucuğuna **Azure sanal makine** kasayla ilişkili işleri görüntülemek için.

**Yedekleme işleri** dikey penceresi açılır ve işlerin listesini görüntüler.

![VM'lerin listesini bir kasada](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

**İlerleme çubuğu** artık kullanılabilir **geri ayrıntıları** dikey penceresi. **Geri ayrıntıları** durumu olan herhangi bir geri yükleme işi tıklayarak dikey açılabilir **sürüyor**. **İlerleme çubuğu** geri yüklemeler gibi tüm çeşitlerini kullanılabilir **Yeni Oluştur**, **Disk geri yükleme** ve **varolan**. Geri yükleme ilerleme çubuğu tarafından gerçekleştirilen ayrıntıları **geri yükleme süresini tahmin**, **geri yükleme yüzdesi** ve **aktarılan bayt sayısını**.

Aşağıda ayrıntıları verilen ilerleme çubuğu geri yükleme:

- **Geri yükleme süresini tahmin** başlangıçta geri yükleme işlemini tamamlamak için geçen süre sağlar. İşlemi ilerledikçe, geçen süreyi azaltır ve 0 kez ulaştığında geri yükleme işlemini tamamlar.
- **Geri yükleme yüzdesi** yüzde kaçını geri yükleme işlemi tamamlandığında veriler sağlar.
- **Aktarılan baytların sayısını** geri yükleme yeni VM oluşturma gerçekleştiğinde alt görevde kullanılabilir. Bu, kaç bayt sayıda ayrıntılarını karşı aktarılacak bayt sayısı aktarıldı sağlar.


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
