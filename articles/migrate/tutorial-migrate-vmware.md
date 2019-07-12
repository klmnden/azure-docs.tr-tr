---
title: Geçiş şirket içi VMware Vm'lerini aracısız Azure geçişi sunucusu geçişi ile azure'a | Microsoft Docs
description: Gerçekleştirme ve şirket içi VMware vm'lerinin Azure Geçişi'ı kullanarak azure'a aracısız geçiş açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 3510c0505a5a3c1353642baf5060a83d13fdd43a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809130"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>VMware sanal makinelerini (aracısız) Azure'a geçirme

Bu makalede Azure geçişi Server Geçiş Aracı ile aracısız geçiş kullanarak şirket içi VMware Vm'leri Azure'a geçirme gösterilmektedir.

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve AWS/GCP VM örneklerini azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Değerlendirmek ve VMware Vm'lerini Azure geçişi Server değerlendirme ve geçiş kullanarak Azure'a geçirme gösterilmektedir, serideki üçüncü öğreticidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal makineleri geçiş için hazırlayın.
> * Azure geçiş Server Geçiş Aracı ekleyin.
> * Geçirmek istediğiniz sanal makineleri keşfedin.
> * Vm'leri çoğaltmaya başlayın.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için geçiş testi çalıştırma.
> * Tam VM geçişi çalıştırın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="migration-methods"></a>Geçiş yöntemi

VMware Vm'lerini Azure geçişi Server Geçiş Aracı kullanarak Azure'a geçirebilirsiniz. Bu araç, birkaç VMware VM geçişi için seçenek sunar:

- Aracısız çoğaltma kullanarak geçiş. Herhangi bir şey yükler gerek kalmadan sanal makineleri geçirin.
- Çoğaltma için bir aracı geçişle. VM çoğaltma için bir aracı yükleyin.

Aracısız veya aracı tabanlı geçişi kullanmak isteyip istemediğinizi karar vermek için bu makaleleri gözden geçirin:

- [Bilgi nasıl](server-migrate-overview.md) aracısız geçiş çalışır ve [sınırlamalarını gözden geçirme](server-migrate-overview.md#agentless-migration-limitations).
- [Bu makaleyi okuyun](tutorial-migrate-vmware-agent.md) aracı tabanlı yöntemi kullanmak istiyorsanız.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. [Anlamak](migrate-architecture.md) VMware geçiş mimarisi.
2. [İlk öğreticiyi Tamamla](tutorial-prepare-vmware.md) geçiş için Azure ve VMware ayarlamak için bu bölümdeki. Özellikle, bu öğreticide şunları yapmanız gerekir:
    - [Azure'u hazırlama](tutorial-prepare-vmware.md#prepare-azure) geçiş.
    - [Şirket içi ortamı hazırlama](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) geçiş.
    
3. VMware Vm'lerini Azure geçişi Server değerlendirmesi ile geçirmeden önce değerlendirmek deneyin öneririz bunları azure'a. Değerlendirmesi, ayarlanacak [ikinci öğreticiye](tutorial-assess-vmware.md) bu seri. Sanal makineleri değerlendirme istemiyorsanız, bu öğreticide atlayabilirsiniz. Öneririz ancak değerlendirme çalışır, ancak geçişi denemeden önce bir değerlendirme çalıştırın gerekmez.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure geçişi Server Geçiş Aracı ekleme

VMware Vm'lerini değerlendirmek için ikinci bir öğreticiyi izleyin yaramadı için gerekirse [bu yönergeleri izleyin](how-to-add-tool-first-time.md) Azure geçişi projesini ayarlarsınız ve Azure geçişi Server Geçiş Aracı'nı seçin. 

İkinci öğreticide izleyen ve bir Azure geçişi projesi ayarlanmış zaten varsa, Azure geçişi Server Geçiş Aracı aşağıdaki şekilde ekleyin:

1. Azure geçişi projesinde tıklayın **genel bakış**. 
2. İçinde **keşfedin, değerlendirin ve geçiş sunucuları**, tıklayın **değerlendirin ve sunucularını geçirme**.

     ![Değerlendirmek ve sunucularını geçirme](./media/tutorial-migrate-vmware/assess-migrate.png)

3. İçinde **Geçiş Araçları**seçin **geçirmeye hazır olduğunuzda, geçiş aracı eklemek için burayı tıklatın**.

    ![Araç seçin](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Araçlar listesinden seçin **Azure geçişi: Sunucu geçiş** > **Ekle aracı**

    ![Sunucu geçiş aracı](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Azure geçişi gerecini ayarlamak

Azure geçişi sunucusu geçişi, basit bir VMware VM Gereci çalıştırır. Gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi sunucusu geçişi için VM meta verileri ve performans verilerini gönderir. Aynı gereç, Azure geçişi Server değerlendirmesi aracı tarafından da kullanılır.

VMware Vm'lerini değerlendirmek için ikinci öğreticide izlediyseniz, Bu öğretici sırasında zaten gereç ayarlayın. Bu öğreticiyi izleyin alamadık, gerecini ayarlamak artık gerekir. Bunu yapmak için: 

- OVA bir şablon dosyasını indirin ve vCenter Server'a aktarın.
- Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin. 
- İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.

Bölümündeki yönergeleri [bu makalede](how-to-set-up-appliance-vmware.md) gerecini ayarlamak için.


## <a name="prepare-vms-for-migration"></a>Sanal makineleri geçiş için hazırlama

Azure geçişi, Vm'leri Azure'a geçirilebilir emin olmak için bazı VM değişiklikler gerektirir.

- Bazı [işletim sistemleri](server-migrate-overview.md#agentless-migration-limitations), Azure geçişi hale getirir bu değişiklikleri otomatik olarak.
- Bu işletim sistemlerinden birine sahip olmayan bir VM geçiriyorsanız, VM'yi hazırlama için yönergeleri izleyin.
- Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişiklik yapmadan önce sanal Makineyi geçirirseniz, sanal makine yedekleme Azure'da başlatılmayabilir.
- VM için çoğaltma etkinleştirildikten sonra şirket içi Vm'leri üzerinde yaptığınız değişikliklerin Azure'a çoğaltılır. Değişiklikleri çoğaltıldığından emin olmak için geçiş kurtarma noktasını yapılandırma değişikliklerini şirket içinde yapılmış olan bir saatten daha geç olduğundan emin olun.


### <a name="prepare-windows-server-vms"></a>Windows Server sanal makineleri hazırlama

**Eylem** | **Ayrıntılar** | **Yönergeleri**
--- | --- | ---
Azure VM'de Windows birimleri şirket içi VM aynı sürücü harfini atamalarını kullandığınızdan emin olun. | SAN ilkesinin çevrimiçi tüm yapılandırın. | 1. VM'ye bir yönetici hesabıyla oturum açın ve bir komut penceresi açın.<br/> 2. Tür **diskpart** Diskpart yardımcı programını çalıştırmak için.<br/> 3. Tür **SAN İLKESİNİN OnlineAll =**<br/> 4. Çıkış DiskPart bırakın ve komut istemini kapatın yazın.
Azure VM için Azure seri erişim konsol etkinleştir | Bu sorun giderme konusunda yardımcı olur. VM yeniden başlatma gerekmez. Azure sanal makine disk görüntüsü kullanarak önyükleme yapmaz ve bu yeni sanal makine için yeniden başlatma için eşdeğerdir. | İzleyin [bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console#enable-serial-console-in-custom-or-older-images) etkinleştirmek için.
Hyper-V Konuk tümleştirme yükleyin | Windows Server 2003 çalıştıran makineleri geçiriyorsanız, VM işletim sisteminde Hyper-V Konuk tümleştirme hizmetlerini yükleyin. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Uzak Masaüstü | Uzak Masaüstü VM'de etkinleştirin ve Windows Güvenlik Duvarı Uzak Masaüstü erişimi herhangi bir ağ profil engellenmediğinden kontrol edin. | [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Linux Vm'leri hazırlama

**Eylem** | **Ayrıntılar** 
--- | --- | ---
Hyper-V Linux Integration Services'ı yükleme | En yeni sürümü Linux dağıtımlarını; bu varsayılan olarak dahil bulunuyor.
Gerekli Hyper-V sürücüleri içermesi için Linux init görüntüyü yeniden derleme | Bu sanal makine Azure'da önyükleme yapmaz ve yalnızca bazı dağıtımlarında gereklidir sağlar.
Azure seri konsol günlüğe yazmayı etkinleştir | Bu sorun giderme konusunda yardımcı olur. VM yeniden başlatma gerekmez. Azure sanal makine disk görüntüsü kullanarak önyükleme yapmaz ve bu yeni sanal makine için yeniden başlatma için eşdeğerdir.<br/> İzleyin [bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) etkinleştirmek için.
Cihaz eşleme dosyasını güncelleştirme | Sürekli cihaz tanımlayıcıları kullanılacak birim ilişkilendirmeleri, cihaz adına sahip cihaz eşleme dosyasını güncelleştirme
Fstab girişleri güncelleştirin | Kalıcı hacim tanımlayıcıları kullanılacak girişleri güncelleştirin.
Udev kuralını Kaldır | Mac adresini vb. temel alarak arabirimi adlarını saklar udev kuralları kaldırın.
Güncelleştirme ağ arabirimleri | DHCP tabanlı IP adresi almak için ağ arabirimlerini güncelleştirin.
SSH etkinleştir | SSH olun etkinleştirilir ve sshd hizmeti yeniden başlatıldığında otomatik olarak başlamaya ayarlanmıştır.<br/> Gelen ssh bağlantı isteklerini kodlanabilir kuralları ve işletim sistemi güvenlik duvarı tarafından engellenmediğinden emin olun.

[Bu makaleyi takip](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic) , Azure'da bir Linux VM çalıştırmaya yönelik adımları ele alır ve bazı popüler Linux dağıtımları için yönergeler içerir.  


## <a name="replicate-vms"></a>Sanal makinelerini çoğaltma

Keşfin tamamlanma ile VMware Vm'lerini azure'a çoğaltma başlayabilirsiniz.

1. Azure geçişi projesinde > **sunucuları**, **Azure geçişi: Sunucu geçiş**, tıklayın **çoğaltmak**.

    ![Sanal makinelerini çoğaltma](./media/tutorial-migrate-vmware/select-replicate.png)

2. İçinde **çoğaltmak**, > **kaynağı ayarları** > **makineleriniz sanallaştırıldı mı?** seçin **Evet, VMware vSphere ile**.
3. İçinde **şirket içi cihazının**, ayarladığınız Azure geçişi gerecinin adı seçin > **Tamam**. 

    ![Kaynak ayarları](./media/tutorial-migrate-vmware/source-settings.png)

    - Bu adım bir öğretici tamamlandığında bir gereç zaten ayarladığınızdan varsayılır.
    - Bir gereç ayarlamadıysanız, ardından yönergeleri izleyin [bu makalede](how-to-set-up-appliance-vmware.md).

4. İçinde **sanal makineler**, çoğaltmak istediğiniz makineleri seçin.
    - VM'ler için bir değerlendirme çalıştırın, VM boyutu ve disk türü (premium veya standart) önerileri ve değerlendirme sonuçlarda uygulayabilirsiniz. Bunu yapmak için **Azure geçişi değerlendirmesi geçiş ayarlarını içeri aktarma?** seçin **Evet** seçeneği.
    - Değerlendirme çalışmadığı ya da seçin, değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır** seçenekleri.
    - Değerlendirme kullanmayı seçtiyseniz, VM grubu ve değerlendirme adını seçin.

    ![Değerlendirme seçin](./media/tutorial-migrate-vmware/select-assessment.png)

5. İçinde **sanal makineler**gerektiğinde VM'ler için arama yapın ve geçirmek istediğiniz her sanal makine kontrol edin. Ardından **sonraki: Ayarları hedef**.

    ![Sanal makineleri seçin](./media/tutorial-migrate-vmware/select-vms.png)

6. İçinde **hedef ayarları**aboneliği seçin ve hedef bölge için geçirin ve geçiş sonrasında Azure Vm'lerini bulunacağı kaynak grubu belirtin. İçinde **sanal ağ**, Azure Vm'lerini katıldığı geçişten sonra Azure VNet/alt seçin.
7. İçinde **Azure hibrit avantajı**:

    - Seçin **Hayır** Azure hibrit avantajı uygulamak istemiyorsanız. Ardından **İleri**'ye tıklayın.
    - Seçin **Evet** ile etkin Yazılım Güvencesi'ni veya Windows Server abonelikleri kapsamındaki Windows Server makineleri sahip ve geçiş yaptığınız makinelere avantajı uygulamak istediğiniz. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-vmware/target-settings.png)

8. İçinde **işlem**, VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik kümesi gözden geçirin. Vm'leri uygun olmalıdır [Azure gereksinimleri](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM boyutu**: Değerlendirmesi önerileri kullanıyorsanız, VM boyutu açılan önerilen boyut içerir. Aksi takdirde Azure geçişi, Azure aboneliğinde en yakın eşleşme dayalı bir boyutu seçer. Alternatif olarak, el ile boyutundaki çekme **Azure VM boyutu**. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim sistemi diski işletim sistemi önyükleme yükleyicisi ve yükleyici sahip bir disktir. 
    - **Kullanılabilirlik kümesi**: Sanal makine geçişten sonra bir Azure kullanılabilirlik gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![VM işlem ayarları](./media/tutorial-migrate-vmware/compute-settings.png)

9. İçinde **diskleri**, VM diskleri Azure'a çoğaltılacak ve disk türünü (SSD/HDD için standart veya premium yönetilen diskler) Azure'da seçin olup olmadığını belirtin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan hariç tutabilirsiniz.
    - Diskleri hariç tutarsanız, geçişten sonra Azure sanal makinesinde mevcut olmaz. 

    ![Diskler](./media/tutorial-migrate-vmware/disks.png)

10. İçinde **gözden geçirin ve çoğaltma başlangıç**, ayarları gözden geçirin ve tıklayın **çoğaltmak** sunucular için ilk çoğaltma başlatılamadı.

> [!NOTE]
> Çoğaltma ayarları çoğaltma başlar, buna önce dilediğiniz zaman güncelleştirebilirsiniz **Yönet** > **makineleri çoğaltma**. Çoğaltma başladıktan sonra ayarları değiştirilemez.

### <a name="provisioning-for-the-first-time"></a>İlk kez sağlama

Azure geçişi projesinde çoğaltma yapıyorsanız ilk VM varsa, Azure geçişi sunucu geçiş projesi olarak aynı kaynak grubunda bu kaynakları otomatik olarak sağlar.

- **Service bus**: Azure geçişi sunucusu geçişi, çoğaltma gerecine düzenleme iletileri göndermek için hizmet veri yolu kullanır.
- **Ağ geçidi depolama hesabı**: Sunucu geçiş ağ geçidi depolama hesabına çoğaltılan VM'ler hakkındaki durum bilgilerini depolamak için kullanır.
- **Kayıt depolama hesabı**: Azure geçişi Gereci çoğaltma günlükleri VM'ler için bir günlük depolama hesabına yükler. Azure geçişi, yönetilen çoğaltma diskleri çoğaltma bilgiler geçerlidir.
- **Anahtar kasası**: Azure geçişi gereç, hizmet veri yolu için bağlantı dizelerini ve Çoğaltmada kullanılan depolama hesapları için erişim anahtarlarını yönetmek için anahtar kasasını kullanır. Anahtar kasası, hazırlandığınızda depolama hesabına erişmek için gereken izinleri ayarlamanız gerekir. [Bu izinleri gözden](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>İzleme ve izleme


- Tıkladığınızda **çoğaltmak** Başlat çoğaltma işi başlar. 
- Başlangıç çoğaltma işlemi başarıyla tamamlandığında, makineleri azure'a, ilk çoğaltma başlar.
- İlk çoğaltma sırasında bir VM anlık görüntüsü oluşturulur. Anlık görüntüden disk verilerinin azure'da yönetilen çoğaltma diskleri çoğaltılır.
- İlk çoğaltma sonlandırıldıktan sonra değişim çoğaltması başlar. Şirket içi diskler artımlı değişiklikler düzenli aralıklarla azure'da çoğaltma diskler çoğaltılır.

İş durumunu portal bildirimlerinden izleyebilirsiniz.

Tıklayarak, çoğaltma durumunu izleyebilirsiniz **sunucuları çoğaltılması** içinde **Azure geçişi: Sunucu geçiş**.
![İzleyici çoğaltma](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Değişim çoğaltması başladığında Azure tam geçişi çalıştırmadan önce VM'ler için bir geçiş testi çalıştırabilirsiniz. Bu geçiş yapmadan önce en az bir kez her makine için bunu yapmanızı öneririz.

- Test geçişi, geçiş, şirket içi makineleri etkilemeden beklendiği gibi çalıştığından denetimleri çalıştırılıyor kalması ve çoğaltmaya devam edin. 
- Test geçişi, çoğaltılan veriler (genellikle, Azure aboneliğinizde bir üretim dışı sanal ağa geçirme) kullanarak Azure VM oluşturarak geçiş benzetimini yapar.
- Azure VM çoğaltılmış test kullanma geçişi doğrulamak için uygulama testi gerçekleştirin ve tam geçişten önce tüm sorunları giderin.

Test geçişi şu şekilde yapabilirsiniz:


1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **Test geçişi sunucuları**.

     ![Geçirilen sunucularını test etme](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Test ve VM'ye sağ **Test geçirme**.

    ![Test geçişi](./media/tutorial-migrate-vmware/test-migrate.png)

3. İçinde **geçiş testi**, hangi Azure VM konumlandırılacağı geçişten sonra Azure sanal ağı seçin. Üretim dışı VNet kullanmanızı öneririz.
4. **Test geçiş** işi başlatır. Portal bildirimleri işi izleyin.
5. Geçiş tamamlandıktan sonra geçirilen Azure sanal Makinesinde görüntüleyin **sanal makineler** Azure portalında. Bir sonek makineyi adına sahip **-Test**.
6. Test tamamlandıktan sonra Azure VM'nin sağ **makineleri çoğaltma**, tıklatıp **test geçişi temiz**.

    ![Geçişi Temizle](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçiş beklendiği gibi çalıştığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi projesinde > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **sunucuları çoğaltılması**.

    ![Çoğaltma sunucuları](./media/tutorial-migrate-vmware/replicate-servers.png)

2. İçinde **makineleri çoğaltma**, VM'ye sağ tıklayın > **geçirme**.
3. İçinde **geçirme** > **sanal makineleri kapatmak ve veri kaybı olmadan planlanan bir geçiş gerçekleştirmek**seçin **Evet** > **Tamam** .
    - Varsayılan olarak Azure geçişi, şirket içi VM'yi kapatır ve son çoğaltma gerçekleştikten oluşan VM değişiklikleri eşitlemek için bir isteğe bağlı çoğaltma çalışır. Bu, veri kaybı olmadan sağlar.
    - Sanal makineyi istemiyorsanız seçin **yok**
4. VM için bir geçiş işi başlatır. Azure bildirimleri işi izleyin.
5. İş tamamlandıktan sonra görüntüleyebilir ve VM'den yönetme **sanal makineler** sayfası.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yaptıktan sonra VM'ye sağ tıklayın > **Durdur geçiş**. Bu şirket içi makine için çoğaltma durdurulur ve sanal makine için çoğaltma durumu bilgilerini temizler.
2. Azure VM yükleme [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) geçirilen makinelerdeki aracı.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure sanal makine örneği üzerinden trafiğe keser.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Kilitleme ve gelen trafik ile erişimini [Azure Güvenlik Merkezi - tam zamanında Yönetim](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
-  [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Araştırma [bulut geçişi Yolculuğunuzun](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) Azure bulut benimseme çerçevesi içinde.
