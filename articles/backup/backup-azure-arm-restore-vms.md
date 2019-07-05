---
title: 'Azure yedekleme: Azure portalını kullanarak sanal makineleri geri yükleme'
description: Bir Azure sanal makinesi, Azure portalını kullanarak bir kurtarma noktasından geri yükleme
services: backup
author: geethalakshmig
manager: vijayts
keywords: yedeklemeyi geri yükleme; geri yükleme; kurtarma noktası;
ms.service: backup
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: geg
ms.openlocfilehash: 62e10f382882e70d488f9814cb00c2b86b8b9691
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460228"
---
# <a name="restore-azure-vms"></a>Azure VM'lerini geri yükleme

Bu makalede Azure VM veri depolanan kurtarma noktalarından geri yükleme [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasaları.



## <a name="restore-options"></a>Geri yükleme seçenekleri

Azure Backup, birkaç VM'yi geri yüklemek için yol sağlar.

**Geri yükleme seçeneği** | **Ayrıntılar**
--- | ---
**Yeni VM oluşturma** | Hızlı bir şekilde oluşturur ve temel VM çalışır duruma geri yükleme noktasından alır.<br/><br/> VM için bir ad belirtin, burada yer alır ve geri yüklenen VM için bir depolama hesabı belirtin sanal ağ (VNet) ve kaynak grubu seçin.
**Diski geri yükleme** | Yeni bir VM oluşturmak için kullanılabilen sanal makine diskini geri yükler.<br/><br/> Azure Backup, bir VM oluşturma ve özelleştirme yardımcı olması için bir şablon sağlar. <br/><br> Geri yükleme işi, indirin ve özel sanal makine ayarlarını belirtmek için ve kullanabilirsiniz bir VM oluşturun, bir şablon oluşturur.<br/><br/> Diskler, belirttiğiniz depolama hesabına kopyalanır.<br/><br/> Alternatif olarak, mevcut bir VM'ye disk ekleme veya PowerShell kullanarak yeni bir VM oluşturun.<br/><br/> Bu seçenek, VM özelleştirme, yedekleme sırasında var olmayan yapılandırma ayarlarını eklemek veya şablon veya PowerShell kullanarak yapılandırılması gereken ayarları eklemek istiyorsanız yararlıdır.
**Varolan** | Bir diski geri yükleme ve var olan sanal makine diski değiştirmek için kullanın.<br/><br/> Geçerli VM mevcut olması gerekir. Silinmiş, bu seçenek kullanılamaz.<br/><br/> Azure Backup, diski değiştirmeden önce var olan sanal makinenin anlık görüntüsünü alır ve belirttiğiniz hazırlama konumu depolar. Mevcut disk sanal Makineye bağlı seçili geri yükleme noktası ile değiştirilir.<br/><br/> Anlık görüntü, kasaya kopyalanır ve bekletme ilkesi uygun olarak korunur. <br/><br/> Replace varolan şifrelenmemiş yönetilen sanal makineler için desteklenir. Yönetilmeyen diskler için desteklenmeyen [VM genelleştirilmiş](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource), için veya Vm'leri [özel görüntüleri kullanılarak oluşturulan](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Geri yükleme noktası daha az veya daha geçerli VM disk varsa disk geri yükleme noktası sayısı yalnızca VM yapılandırması yansıtır.<br/><br/>


> [!NOTE]
> Ayrıca, belirli dosyaları ve klasörleri Azure VM'de kurtarabilirsiniz. [Daha fazla bilgi edinin](backup-azure-restore-files-from-vm.md).
>
> Çalıştırıyorsanız [en son sürümü](backup-instant-restore-capability.md) Azure Backup Azure Vm'leri (anında geri yükleme bilinir) için anlık görüntüler için yedi güne kadar da saklanır ve yedekleme verileri kasaya gönderilmeden önce anlık görüntülerden VM geri yükleyebilirsiniz. VM son yedi güne ait bir yedekten geri yüklemek istiyorsanız, anlık görüntüden ve vault'tan geri yüklemek daha hızlıdır.

## <a name="storage-accounts"></a>Depolama hesapları

Depolama hesapları hakkında bazı Ayrıntılar:

- **VM oluşturma**: VM, yeni bir VM oluşturduğunuzda, belirttiğiniz depolama hesabında yer alır.
- **Diski geri yükleme**: Bir diski geri yüklediğinizde, disk, belirttiğiniz depolama hesabına kopyalanır. Geri yükleme işi indirebilir ve özel sanal makine ayarlarını belirtin kullanacağınız bir şablon oluşturur. Bu şablon belirtilen depolama hesabında yer alır.
- **Disk değiştirme**: Bir disk üzerinde var olan bir VM'yi değiştirdiğinizde, Azure Backup diski değiştirmeden önce var olan sanal makinenin anlık görüntüsünü alır. Anlık görüntü, belirttiğiniz hazırlama konumu (depolama hesabı) depolanır. Bu depolama hesabı, anlık görüntü geri yükleme işlemi sırasında geçici olarak depolamak için kullanılır ve daha sonra kolayca kaldırılabilen Bunu yapmak için yeni bir hesap oluşturmanızı öneririz.
- **Depolama hesabı konumu** : Depolama hesabı, kasa ile aynı bölgede olmalıdır. Yalnızca bu hesapları görüntülenir. Konumda depolama hesabı yoksa oluşturmanız gerekir.
- **Depolama türü** : BLOB Depolama desteklenmiyor.
- **Depolama yedekliliği**: Bölgesel olarak yedekli depolama (ZRS) desteklenmez. Hesabı için çoğaltma ve yedeklilik bilgileri hesabı adından sonra parantez içinde gösterilmektedir. 
- **Premium depolama**:
    - Premium olmayan Vm'leri geri yüklerken, premium depolama hesapları desteklenmez.
    - Vm'leri yönetilen geri yükleme, yapılandırılmış ağ kuralları ile premium depolama hesapları desteklenmez.


## <a name="before-you-start"></a>Başlamadan önce

Bir VM'yi geri yüklemek için (yeni bir VM oluşturun) doğru rol tabanlı erişim denetimi (RBAC) sahip olduğunuzdan emin olun [izinleri](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) VM geri yükleme işlemi için.

İzinleriniz yoksa, şunları yapabilirsiniz [bir diski geri yükleme](#restore-disks), ve ardından disk geri yüklendikten sonra [şablonu](#use-templates-to-customize-a-restored-vm) yeni bir VM oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulan.



## <a name="select-a-restore-point"></a>Bir geri yükleme noktası seçin

1. Geri yüklemek istediğiniz VM ile ilişkili kasaya tıklayın **yedekleme öğeleri** > **Azure sanal makine**.
2. Bir VM'ye tıklayın. Sanal makine Panosu'ndan üzerinde varsayılan olarak, son 30 güne ait kurtarma noktaları görüntülenir. 30 gün veya tarih, zaman aralıkları ve anlık görüntü tutarlılık farklı türde göre kurtarma noktalarından bulmak için filtre daha eski kurtarma noktalarını görüntüleyebilirsiniz.
3. VM'yi geri yüklemek için **VM geri yükleme**.

    ![Geri yükleme noktası](./media/backup-azure-arm-restore-vms/restore-point.png)

4. Kurtarma için kullanılacak bir geri yükleme noktası seçin.

## <a name="choose-a-vm-restore-configuration"></a>Bir VM geri yükleme yapılandırması seçin

1. İçinde **geri yükleme Yapılandırması**, geri yükleme seçeneği seçin:
    - **Yeni Oluştur**: Yeni bir VM oluşturmak istiyorsanız bu seçeneği kullanın. Basit ayarları içeren bir VM oluşturmak veya bir diski geri yükleme ve özelleştirilmiş bir VM oluşturun.
    - **Varolan**: Mevcut bir VM üzerindeki değiştirmek istiyorsanız bu seçeneği kullanın.

        ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/restore-configuration.png)

2. Uygulamanızın seçili geri yükleme seçeneği için ayarları belirtin.

## <a name="create-a-vm"></a>VM oluşturma

Biri olarak [geri yükleme seçenekleri](#restore-options), bir VM hızlıca geri yükleme noktasından temel ayarlarla oluşturabilirsiniz.

1. İçinde **geri yükleme Yapılandırması** > **Yeni Oluştur** > **geri yükleme türü**seçin **sanal makine oluşturma** .
2. İçinde **sanal makine adı**, abonelikte mevcut olmayan bir sanal makine belirtin.
3. İçinde **kaynak grubu**, yeni VM için mevcut bir kaynak grubunu seçin veya genel olarak benzersiz bir ada sahip yeni bir tane oluşturun. Zaten bir ad atarsanız, Azure VM adıyla aynı gruba atar.
4. İçinde **sanal ağ**, hangi VM yerleştirilecek sanal ağı seçin. Abonelikle ilişkili tüm Vnet'lerin görüntülenir. Alt ağ seçin. İlk alt ağ, varsayılan olarak seçilidir.
5. İçinde **depolama konumu**, VM için depolama hesabı belirtin. [Daha fazla bilgi edinin](#storage-accounts).

    ![Yapılandırma Sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. İçinde **geri yükleme Yapılandırması**seçin **Tamam**. İçinde **geri**, tıklayın **geri** geri yükleme işlemi tetiklemek için.


## <a name="restore-disks"></a>Diskleri geri yükle

Biri olarak [geri yükleme seçenekleri](#restore-options), geri yükleme noktasından bir disk oluşturabilirsiniz. Ardından diskle aşağıdakilerden birini yapabilirsiniz:

- Ayarları özelleştirmek ve VM dağıtımı tetiklemek için geri yükleme işlemi sırasında oluşturulan şablonu kullanın. Varsayılan şablon ayarları düzenleyin ve VM Dağıtım Şablonu gönderin.
- [Geri yüklenen diski](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) var olan bir sanal makineye.
- [Yeni VM oluşturma](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#create-a-vm-from-restored-disks) PowerShell kullanarak geri yüklenen disklerden.

1. İçinde **geri yükleme Yapılandırması** > **Yeni Oluştur** > **geri yükleme türü**seçin **diskleri geri**.
2. İçinde **kaynak grubu**, geri yüklenen diskleri için mevcut bir kaynak grubunu seçin veya genel olarak benzersiz bir ada sahip yeni bir tane oluşturun.
3. İçinde **depolama hesabı**, VHD'lerin kopyalanacağı için hesabı belirtin. [Daha fazla bilgi edinin](#storage-accounts).

    ![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. İçinde **geri yükleme Yapılandırması**seçin **Tamam**. İçinde **geri**, tıklayın **geri** geri yükleme işlemi tetiklemek için.

VM geri yükleme sırasında Azure Backup depolama hesabı kullanmaz. Ancak durumunda, **diskleri geri** ve **anında geri yükleme**, depolama hesabı, şablonu depolamak için kullanılır.

### <a name="use-templates-to-customize-a-restored-vm"></a>Geri yüklenen VM özelleştirmek için şablonları kullanma

Disk geri yüklendikten sonra oluşturulan şablonu özelleştirme ve yeni bir VM oluşturmak için geri yükleme işleminin bir parçası olarak kullanın:

1. Açık **geri yükleme işinin ayrıntılarını** ilgili proje için.

2. İçinde **geri yükleme işinin ayrıntılarını**seçin **şablonu Dağıt** şablon dağıtımını başlatmak için.

    ![Geri yükleme işi detaya gitme](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

3. Şablonda sağlanan VM ayarı özelleştirmek için tıklatın **şablonu Düzen**. Daha fazla özelleştirmeler eklemek istiyorsanız, tıklayın **parametreleri Düzenle**.
    - [Daha fazla bilgi edinin](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) özel bir şablondan kaynakları dağıtma hakkında daha fazla.
    - [Daha fazla bilgi edinin](../azure-resource-manager/resource-group-authoring-templates.md) şablon yazma hakkında.

   ![Şablon dağıtımı'nı yükleme](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. VM için özel değerler girin; kabul **hüküm ve koşullar** tıklatıp **satın alma**.

   ![Şablon dağıtımı gönderin](./media/backup-azure-arm-restore-vms/submitting-template1.png)


## <a name="replace-existing-disks"></a>Var olan diskleri değiştirin

Biri olarak [geri yükleme seçenekleri](#restore-options), varolan bir sanal makine diski seçilen geri yükleme noktası ile değiştirin. [Gözden geçirme](#restore-options) tüm geri yükleme seçenekleri.

1. İçinde **geri yükleme Yapılandırması**, tıklayın **varolan**.
2. İçinde **geri yükleme türü**seçin **disk/sn yerine**. Var olan VM diskleri için kullanılan değiştirme olacak geri yükleme noktası budur.
3. İçinde **hazırlama konumu**, geçerli yönetilen disklerin anlık görüntüleri geri yükleme işlemi sırasında nereye kaydedileceğini belirtin. [Daha fazla bilgi edinin](#storage-accounts).

   ![Geri yükleme yerine var olan Yapılandırma Sihirbazı](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="restore-vms-with-special-configurations"></a>Özel yapılandırmaları olan Vm'leri geri yükleme

Vm'leri geri yükleme gerekebilir yaygın senaryolar vardır.

**Senaryo** | **Rehber**
--- | ---
**Hibrit kullanım teklifi kullanarak Vm'leri geri yükleme** | Bir Windows VM kullanıyorsa [karma kullanım Avantajı'nı (HUB) lisans](../virtual-machines/windows/hybrid-use-benefit-licensing.md)diskleri geri yükle ve belirtilen şablonu kullanarak yeni bir VM oluşturun (ile **lisans türü** kümesine **Windows_Server**) , veya PowerShell.  Bu ayar, VM oluşturduktan sonra da uygulanabilir.
**Bir Azure veri merkezi olağanüstü durum sırasında Vm'leri geri yükleme** | Azure Backup, GRS kasası kullanıyorsa ve sanal makine için birincil veri merkezinde arıza eşleştirilmiş veri merkezine geri yükleme yedeklenen sanal makineleri destekler. Eşleştirilmiş veri merkezine bir depolama hesabı seçin ve normal olarak geri yükleyin. Azure Backup, geri yüklenen VM'yi oluşturmak için eşleştirilmiş konumunda işlem hizmeti kullanır. [Daha fazla bilgi edinin](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md) datacenter dayanıklılığı hakkında.
**Tek bir etki alanı denetleyicisi VM'SİNİN tek bir etki alanında geri yükleme** | VM diğer VM'ler gibi geri yükleyin. Aşağıdakilere dikkat edin:<br/><br/> Active Directory açısından bakıldığında, diğer VM'ler gibi Azure vm'dir.<br/><br/> Dizin Hizmetleri Geri Yükleme Modu'nda (DSRM), ayrıca tüm Active Directory Kurtarma senaryolarına uygun olacak şekilde kullanılabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/virtualized-domain-controllers-hyper-v) sanallaştırılmış etki alanı denetleyicileri için yedekleme ve geri yükleme konusunda.
**Birden çok etki alanı denetleyicisi tek etki alanındaki Vm'leri geri yükleme** | Diğer etki alanı denetleyicileri aynı etki alanında ağ üzerinden ulaşılabilir değilse, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Etki alanındaki son kalan etki alanı denetleyicisi olduğundan ya da kurtarma yalıtılmış bir ortamda gerçekleştirilen kullanan bir [orman kurtarma](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery).
**Birden çok ormandaki etki alanlarında bulunan bir geri yükleme** | Öneririz bir [orman kurtarma](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery).
**Tam geri yükleme** | Azure VM ve şirket içi hiper arasındaki başlıca fark, Azure'da hiçbir VM konsol olduğunu ' dir. Bir konsol tam kurtarma (BMR) kullanarak kurtarma gibi belirli senaryolar için gerekli değildir-türü yedekleme. Ancak, kasadan VM geri yükleme, BMR için tam yerini almıştır.
**Özel ağ yapılandırmaları ile Vm'leri geri yükleme** | Özel ağ yapılandırmaları kullanarak iç veya dış Yük Dengeleme, birden çok NIC veya birden çok ayrılmış IP adresleri kullanarak Vm'leri içerir. Bu Vm'leri kullanarak geri yükleme [disk seçeneği geri](#restore-disks). Bu seçenek belirtilen depolama hesabına VHD bir kopyasını yapar ve sonra bir VM oluşturabilirsiniz bir [iç](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/) veya [dış](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/) yük dengeleyici, [birden çok NIC](../virtual-machines/windows/multiple-nics.md), veya [birden çok ayrılmış IP adresleri](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md), yapılandırmanız uygun olarak.
**NIC/alt ağ üzerinde ağ güvenlik grubu (NSG)** | Azure VM yedeklemesi vnet, alt ağ ve NIC düzeyinde yedekleme ve geri yükleme, NSG bilgileri destekler.
**Bölgeye sabitlenmiş VM'ler** | Azure Backup, yedekleme ve geri yükleme ayrılmış sabitlenmiş vm'leri destekler. [Daha fazla bilgi edinin](https://azure.microsoft.com/global-infrastructure/availability-zones/)

## <a name="track-the-restore-operation"></a>Geri yükleme işlemi İzle
Geri yükleme işlemini tetikleme sonra yedekleme hizmeti izleme için bir iş oluşturur. Azure Backup portalında işle ilgili bildirimler görüntüler. Görünmüyorsa, tıklayarak **bildirimleri** bunları göz atabilirsiniz.

![Tetiklenen geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification1.png)

 Geri yükleme gibi izleyin:

1. İş için işlemleri görüntülemek için bildirimleri köprüye tıklayın. Alternatif olarak, kasaya tıklayın **yedekleme işleri**ve ardından ilgili VM'ye tıklayın.

    ![VM'lerin listesini bir kasada](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

2. Geri yükleme ilerleme durumunu izlemek için herhangi bir geri yükleme işi durumu tıklayın **sürüyor**. Bu, geri yükleme ilerleme durumu hakkında bilgiler gösteren bir ilerleme çubuğu görüntüler:

    - **Geri yükleme süresini tahmin**: Başlangıçta geri yükleme işlemini tamamlamak için geçen süre sağlar. İşlemi ilerledikçe, geçen süreyi azaltır ve geri yükleme işlemi tamamlandığında sıfır oluncaya.
    - **Geri yükleme yüzdesi**. İşlemi yapana geri yükleme işlemi yüzdesini gösterir.
    - **Aktarılan baytların sayısını**: Yeni bir sanal makine oluşturarak geri yükleme, aktarılacak bayt sayısı karşı aktarılan bayt gösterir.

## <a name="post-restore-steps"></a>Geri yükleme sonrası adımlar

Bir sanal makine geri yüklendikten sonra dikkat edilecek noktalar vardır:

- Yedekleme yapılandırması sırasında mevcut uzantılar yüklendi, ancak etkin değil. Bir sorun görürseniz, uzantıları yeniden yükleyin.
- Yedeklenen VM'ye statik bir IP adresi varsa, geri yüklenen VM çakışmayı önlemek için dinamik IP adresi gerekir. Yapabilecekleriniz [geri yüklenen VM'ye statik bir IP adresi ekleyin](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
- Geri yüklenen VM bir kullanılabilirlik yok ayarlayın. Geri Yükleme disk seçeneği kullanmanız durumunda yapabilecekleriniz [zadat skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md) sağlanan şablon veya PowerShell kullanarak diskten bir VM oluşturduğunuzda.
- Bir cloud-init tabanlı Linux dağıtımı, Ubuntu gibi kullanıyorsanız, güvenlik nedeniyle geri yüklemeden sonra parolayı engellenir. Geri yüklenen VM'ye VMAccess uzantısını kullanın [parola sıfırlama](../virtual-machines/linux/reset-password.md). Geri yüklemeden sonra parola sıfırlama gerekmez, bu dağıtımlarında, SSH anahtarları kullanmanızı öneririz.


## <a name="backing-up-restored-vms"></a>Geri yüklenen sanal makinelerini yedekleme

- Aynı kaynak grubuna Yedeklenen özgün VM ile aynı ada sahip bir VM geri yüklediyseniz, yedekleme geri yüklemeden sonra sanal makinede devam ediyor.
- Sanal makine farklı kaynak grubuna geri veya geri yüklenen sanal makine için farklı bir ad belirttiğiniz, geri yüklenen VM için yedekleme kümesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Geri yükleme işlemi sırasında sorunlarla karşılaşırsanız [gözden](backup-azure-vms-troubleshoot.md#restore) sık karşılaşılan sorunlar ve hatalar.
- Sanal makine geri yüklendikten sonra öğrenin [sanal makineleri yönetme](backup-azure-manage-vms.md)
