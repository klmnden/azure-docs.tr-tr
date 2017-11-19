---
title: Sanal makineleri yedekten geri | Microsoft Docs
description: "Bir Azure sanal makinesi bir kurtarma noktasından geri yüklemeyi öğrenin"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "yedeklemeyi geri; geri yükleme; kurtarma noktası;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: 5f6e5dd9d4fb96376762300856b594d772d84af8
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Azure’da sanal makineleri geri yükleme
> [!div class="op_single_selector"]
> * [Azure portalında sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
> * [Sanal makineleri Klasik portalda geri yükleme](backup-azure-restore-vms.md)
>
>

Bir sanal makine yeni bir sanal makineye aşağıdaki adımlarla bir Azure yedekleme kasasında depolanan yedeklerden geri yükleyin.

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir. Sonra <br/> **30 Kasım 2017**, artık yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmayacaktır. <br/> Tarafından **30 Kasım 2017**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="restore-workflow"></a>İş akışını geri yükle
### <a name="step-1-choose-an-item-to-restore"></a>1. adım: geri yüklemek için bir öğe seçin
1. Gidin **korunan öğeler** sekmesinde ve yeni bir sanal makineye geri yüklemek istediğiniz sanal makineyi seçin.

    ![Korumalı öğeler](./media/backup-azure-restore-vms/protected-items.png)

    **Kurtarma noktası** sütununda **korunan öğeler** sayfa söyleyin, bir sanal makine için kurtarma noktası sayısı. **En yeni kurtarma noktası** sütunu belirtir, en son yedekleme, bir sanal makine geri yükleyebilirsiniz.
2. Tıklatın **geri** açmak için **öğeyi geri** Sihirbazı.

    ![Bir öğeyi geri yükle](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>2. adım: bir kurtarma noktası seçin
1. İçinde **bir kurtarma noktası seçin** ekran, geri yükleyebilir, yeni kurtarma noktasından veya önceki bir noktaya süre. Sihirbazı açıldığında seçilen varsayılan seçenektir *en yeni kurtarma noktası*.

    ![Bir kurtarma noktası seçin](./media/backup-azure-restore-vms/select-recovery-point.png)
2. Zaman içinde önceki bir nokta seçmek için Seç **tarihi seçin** seçeneğini açılır listede ve bir tarih, Takvim denetimi tıklayarak **takvim simgesini**. Denetim kurtarma noktalarına sahip tüm tarihleri açık bir gri gölge ile doldurulur ve kullanıcı tarafından seçilebilen.

    ![Bir tarih seçin](./media/backup-azure-restore-vms/select-date.png)

    Takvim denetiminde bir tarihi tıklatın sonra kurtarma kullanılabilir tarih kurtarma noktaları tabloda gösterilen işaret eder. **Zaman** sütun anlık görüntünün alındığı saati belirtir. **Türü** sütunu görüntüler [tutarlılık](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) kurtarma noktası. Tablo üstbilgisi o gün parantez içinde kullanılabilir kurtarma noktası sayısını gösterir.

    ![Kurtarma noktaları](./media/backup-azure-restore-vms/recovery-points.png)
3. Kurtarma noktası seçin **kurtarma noktaları** tablo ve sonraki ekranına geri dönmek için İleri okuna tıklayın.

### <a name="step-3-specify-a-destination-location"></a>3. adım: bir hedef konum belirtin
1. İçinde **seçin geri örneği** ekran sanal makinenin geri yükleneceği yeri ayrıntılarını belirtin.

   * Sanal makine adı belirtin: verilen bulut hizmetinde, sanal makine adı benzersiz olmalıdır. Var olan VM aşırı yazma desteklemiyoruz.
   * VM için bir bulut hizmeti seçin: Bu bir VM oluşturmak için zorunludur. Var olan bir bulut hizmetini kullanabilir veya yeni bir bulut hizmeti oluşturmayı seçebilirsiniz.

        Herhangi bir bulut hizmeti adının çekilir küresel olarak benzersiz olmalıdır. Genellikle, bir bulut hizmeti adının [buluthizmeti] biçiminde bir genel kullanıma yönelik URL ile ilişkili alır. cloudapp.net. Azure ad zaten kullanılmış, yeni bir bulut hizmeti oluşturmak izin vermez. Yeni bir bulut hizmeti oluşturmayı seçerseniz, sanal makine VM adını çekilen çalışması ilişkili bulut hizmetine uygulanacak benzersiz olmalıdır – aynı adı verilir.

        Biz yalnızca bulut Hizmetleri ve geri yükleme örneği ayrıntıları herhangi bir benzeşim grupları ile ilişkili olmayan sanal ağları görüntüler. [Daha fazla bilgi edinin](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. VM için bir depolama hesabı seçin: Bu VM oluşturmak için zorunludur. Azure yedekleme kasası ile aynı bölgede var olan depolama hesapları arasından seçim yapabilirsiniz. Bölge olarak yedekli veya Premium depolama türü olan depolama hesapları desteklemiyoruz.

    Desteklenen yapılandırmaya sahip depolama hesabı varsa, lütfen geri yükleme işlemi başlamadan önce desteklenen yapılandırmasının bir depolama hesabı oluşturun.

    ![Sanal ağ seçin](./media/backup-azure-restore-vms/restore-sa.png)
3. Bir sanal ağı seçin: sanal makine için sanal ağ (VNET) VM oluşturma zamanında seçilmelidir. Geri yükleme UI kullanılabilmesi için bu abonelik içindeki tüm sanal ağları gösterir. Geri yüklenen VM için bir VNET seçmek için zorunlu değildir – VNET uygulanmamış olsa bile Internet üzerinden geri yüklenen sanal makineye Bağlan kuramaz.

    Ardından seçili bulut hizmetine bir sanal ağ ile ilişkili ise sanal ağ değiştirilemez.

    ![Sanal ağ seçin](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Bir alt ağ seçin: sanal ağ alt ağa sahip olmaması durumunda, ilk alt ağ varsayılan olarak seçilir. Tercih ettiğiniz alt açılır seçeneklerden birini seçin. Ağları uzantısı'nda alt ağ ayrıntılarını Git [portal giriş sayfasının](https://manage.windowsazure.com/)gidin **sanal ağlar** ve sanal ağı seçin ve alt ağ ayrıntıları görmek için yapılandırma detaya.

    ![Alt ağ seçin](./media/backup-azure-restore-vms/select-subnet.png)
5. Tıklatın **gönderme** ayrıntıları göndermesi ve geri yükleme işi Oluşturma Sihirbazı'nda simgesi.

## <a name="track-the-restore-operation"></a>Geri yükleme işlemini izlemek
Geri Yükleme Sihirbazı'nı içine tüm bilgileri girin ve gönderdikten sonra Azure yedekleme geri yükleme işlemini izlemek için bir iş oluşturmak çalışacaktır.

![Bir geri yükleme işi oluşturma](./media/backup-azure-restore-vms/create-restore-job.png)

Proje oluşturma başarılı olursa, iş oluşturulduğunu belirten bir bildirim bildirim görürsünüz. Tıklayarak daha fazla bilgi alabilirsiniz **işi görüntüle** gideceksiniz düğmesi **işleri** sekmesi.

![Geri yükleme işi oluşturuldu](./media/backup-azure-restore-vms/restore-job-created.png)

Geri yükleme işlemi tamamlandıktan sonra tamamlandı olarak işaretlenecek **işleri** sekmesi.

![Geri yükleme işi tamamlandı](./media/backup-azure-restore-vms/restore-job-complete.png)

Sanal makine geri yükledikten sonra özgün VM'de varolan uzantılarını yeniden yüklemeniz gerekebilir ve [uç noktalarını değiştirmek](../virtual-machines/windows/classic/setup-endpoints.md) Azure portalında sanal makine için.

## <a name="post-restore-steps"></a>Geri yükleme sonrası adımlar
Güvenlik nedenleriyle Ubuntu gibi bir bulut init göre Linux dağıtım kullanıyorsanız, parola engellenir geri yükleme sonrası. Lütfen geri yüklenen VM üzerinde VMAccess uzantısını kullanmak [parola sıfırlama](../virtual-machines/linux/classic/reset-access.md). Parola post geri yükleme sıfırlama önlemek için bu dağıtım üzerinde SSH anahtarları kullanmanızı öneririz.

## <a name="backup-for-restored-vms"></a>Geri yüklenen VM'ler için yedekleme
İlk olarak VM yedeklemesi yedeklenen aynı adla aynı bulut hizmetine VM geri yükledikten, yedekleme VM post geri yükleme devam eder. Farklı bir bulut hizmeti için VM geri veya geri yüklenen VM için farklı bir ad belirtilen varsa, bu yeni VM olarak kabul edilir ve geri yüklenen VM için Kurulum yedekleme gerekiyor.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Bir VM sırasında Azure veri merkezi olağanüstü durum geri yükleme
Azure yedekleme geri yükleme durumunda burada VM'ler deneyimleri olağanüstü durum çalıştırıyorsanız ve yedekleme Kasası'coğrafi olarak yedekli olacak şekilde yapılandırılmış birincil veri merkezi Vm'leri eşleştirilmiş veri merkezine yedeklenen sağlar. Bu tür senaryoları sırasında eşlenmiş veri merkezinde mevcut olan bir depolama hesabı seçmeniz gerekir ve geri yükleme işleminin geri kalanında aynı kalır. Azure yedekleme, geri yüklenen sanal makine oluşturmak için işlem hizmetinden eşleştirilmiş coğrafi kullanır. Daha fazla bilgi edinmek [Azure veri merkezi dayanıklılık](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Etki alanı denetleyicisi sanal makineleri geri yükleme
Etki alanı denetleyicisi (DC) sanal makinelerinin Azure yedekleme ile desteklenen bir senaryodur. Ancak, geri yükleme işlemi sırasında dikkatli olunması gerekir. Doğru geri yükleme işlemi, etki alanı yapısına bağlıdır. En basit durumda tek bir etki alanında tek bir DC sahip. Daha sık üretim yükleri, tek bir etki alanı birden çok DC'ler, belki de DC'lere ile şirket içinde gerekir. Son olarak, birden çok etki alanı içeren bir ormanda olabilir.

Bir Active Directory açısından herhangi bir VM modern desteklenen hiper yönetici üzerinde Azure VM gibidir. Şirket içi hiper ile en önemli fark, hiçbir VM konsolu ile Azure kullanılabilir olduğunu ' dir. Bir konsol tam kurtarma (BMR) türü yedekleme kullanarak kurtarma gibi belirli senaryolar için gereklidir. Ancak, VM geri yükleme yedekleme kasasından tam bir BMR yerini alır. Active Directory Geri Yükleme Modu'nda (DSRM), ayrıca tüm Active Directory Kurtarma senaryolarına uygun şekilde kullanılabilir. Daha fazla bilgi için lütfen denetleyin [yedekleme ve geri yükleme hakkında önemli noktalar sanallaştırılmış etki alanı denetleyicileri için](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) ve [için Active Directory orman Kurtarma planlaması](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Tek bir etki alanındaki tek DC
VM (tüm diğer VM gibi) Azure'dan geri yüklenebilir portal veya PowerShell kullanarak.

### <a name="multiple-dcs-in-a-single-domain"></a>Tek bir etki alanındaki birden çok DC'leri
Aynı etki alanının diğer DC'ler ağ üzerinden erişilebilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Etki alanındaki son kalan DC olduğu veya kurtarma yalıtılmış bir ağda gerçekleştirilir, orman kurtarma yordamı gelmelidir.

### <a name="multiple-domains-in-one-forest"></a>Bir ormandaki birden çok etki alanları
Aynı etki alanının diğer DC'ler ağ üzerinden erişilebilir olduğunda, etki alanı denetleyicisi gibi herhangi bir VM geri yüklenebilir. Ancak, diğer tüm durumlarda bir orman kurtarma önerilir.

<!--- WK: this following original supportability statement is incorrect, taking it out.
The challenge arises because DSRM mode is not present in Azure. So to restore such a VM, you cannot use the Azure portal. The only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use the Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about the [USN rollback problem](https://technet.microsoft.com/library/dd363553) and the strategies suggested to fix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Özel ağ yapılandırmaları ile sanal makineleri geri yükleme
Azure yedekleme, sanal makinelerin özel ağ yapılandırmaları izlemek için yedeklemeyi destekler.

* Yük Dengeleyici (iç ve dış) altında yer alan VM'ler
* Birden çok ayrılmış IP ile sanal makineleri
* Birden çok NIC içeren VM'ler

Bu yapılandırmalar, bunları geri yükleme sırasında ilgili önemli noktalar aşağıdaki zorunlu kılabilir.

> [!TIP]
> Lütfen PowerShell tabanlı geri yükleme akış VM'ler post geri yükleme özel ağ yapılandırmasını yeniden oluşturmak için kullanın.
>
>

### <a name="restoring-from-the-ui"></a>Kullanıcı Arabiriminden geri yükleme:
Kullanıcı Arabiriminde, geri yükleme sırasında **her zaman yeni bir bulut hizmeti seçin**. Lütfen geri yükleme akışı sırasında yalnızca kullandığı zorunlu parametreler portal olduğundan VM'ler kullanıcı arabirimini kullanarak geri Not bunlar sahip özel ağ yapılandırması kaybedersiniz. Diğer bir deyişle, geri yükleme VM'ler yük dengeleyici veya çoklu yapılandırması olmadan normal VM'ler olacaktır NIC veya birden çok ayrılmış IP.

### <a name="restoring-from-powershell"></a>Powershell'den geri yükleme:
PowerShell yalnızca VM diskleri yedekten geri yükleyin ve sanal makine oluşturma özelliğine sahiptir. Yukarıda belirtilen özel ağ yapılandırmaları gerektiren sanal makineler geri yüklerken yardımcı olur.

Tam olarak sanal makine post geri yükleme diskleri yeniden oluşturmak için aşağıdaki adımları izleyin:

1. Yedekleme kasası kullanarak diskleri geri [Azure yedekleme PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Yük Dengeleyici için gerekli yapılandırması oluşturma / VM oluşturmak için istenen PowerShell cmdlet'lerini kullanarak, birden çok NIC/birden çok ayrılmış IP yapılandırması.

   * VM bulut hizmetiyle oluşturmak [iç yük dengeleyici](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Bağlanmak için VM oluşturma [Internet'e yönelik Yük Dengeleyici](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * VM oluşturma [birden çok NIC](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * VM oluşturma [birden çok ayrılmış IP](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Sonraki adımlar
* [Sorun giderme](backup-azure-vms-troubleshoot.md#restore)
* [Sanal makineleri yönetme](backup-azure-manage-vms.md)
