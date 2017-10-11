---
title: "Şirket içi azure'dan yeniden çalışma VMware sanal makinelerini | Microsoft Docs"
description: "Şirket içi siteye başarısız olan VMware vm'lerinin yük devretme sonrasında ve fiziksel sunucuları azure'a hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: dde0bb6b4f6bc10afdd7d40adc6689d42b37de81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Geri VMware sanal makineleri ve fiziksel sunucuları şirket içi siteye başarısız


Bu makalede Azure yeniden çalışma için nasıl Azure sanal makineleri şirket içi siteye. VMware sanal makinelerinizi yeniden çalışmak hazırsınız veya Windows veya Linux fiziksel sunucuları, sonra bu kullanarak makinelerinizi yeniden korunan Buradaki yönergeleri izlemeniz [başvuru](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Klasik Azure portalını kullanıyorsanız, Lütfen belirtilen yönergelerine başvurun [burada](site-recovery-failback-azure-to-vmware-classic.md) Azure Mimarisi için geliştirilmiş VMware için ve [burada](site-recovery-failback-azure-to-vmware-classic-legacy.md) eski mimari

## <a name="overview"></a>Genel Bakış
Bu bölümdeki diyagramları bu senaryo için yeniden çalışma mimarisi gösterir.

İşlem sunucusu şirket içi ve Azure ExpressRoute bağlantısı kullanarak bu mimarinin kullanın:

![ExpressRoute için mimarisi diyagramı](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Azure üzerinde işlem sunucusu olduğunda ve bir VPN veya ExpressRoute bağlantısına sahip olduğunda bu mimarinin kullanın:

![VPN mimarisi diyagramı](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Bağlantı noktaları ve geri dönme mimarisi diyagramı tam listesi için aşağıdaki görüntüye bakın:

![Yük devretme yeniden çalışma tüm bağlantı noktalarının listesi](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Azure'a yük devrettikten sonra üç aşamada, şirket içi sitenize başarısız olur:

* **Aşama 1**: Bunlar şirket içi sitenizdeki çalıştırmakta olan VMware VM'ler dön çoğaltma işlemi başlatma böylece Azure Vm'leri koruyun.
* **Aşama 2**: Azure VM'ler, şirket içi siteye çoğaltılır sonra Yük devretme Azure'dan başarısız çalıştırın.
* **3. Aşama**: verilerinizi geri başarısız oldu sonra böylece bunlar Azure'a çoğaltma işlemi başlatma geri için başarısız şirket içi sanal makineleri koruyun.

### <a name="fail-back-to-the-original-location-or-an-alternate-location"></a>Özgün konuma veya alternatif bir konuma başarısız
VMware VM başarısız olduktan sonra şirket içi hala varsa aynı kaynağa geri VM başarısız olabilir. Bu senaryoda, yalnızca farkları geri başarısız olan.

Fiziksel sunucuları üzerinde başarısız oldu, yeniden çalışma her zaman yeni bir VMware VM olur. Geri başarısız olan bir fiziksel makine önce dikkat edin:
* Bunu yeniden VMware devredildiğinde korumalı bir fiziksel makine bir sanal makine olarak geri gelecektir. Korumalı ise ve Azure'a devredilen bir Windows Server 2008 R2 SP1 fiziksel makine geri başarısız olamaz. Bir sanal makine şirket içi olarak başlatılan bir Windows Server 2008 R2 SP1 makineye geri dönecek mümkün olacaktır.
* En az bir ana hedef sunucusunu yeniden çalışmak için gereken gerekli ESX/ESXi konaklarının birlikte bulma emin olun.

Özgün VM başarısız olursa, aşağıdakiler gereklidir:

* VM bir vCenter sunucusu tarafından yönetiliyorsa, ana hedefin ESX konak VM'ler veri deposuna erişimi olmalıdır.
* VM bir ESX konağında olmakla birlikte vCenter tarafından yönetilmiyor, sanal sabit disk MT'ın ana bilgisayar tarafından erişilebilen bir veri deposu olması gerekir.
* VM'yi bir ESX konağında ise ve vCenter kullanmaz, koruyun önce bulma MT ESX konağının tamamlamanız gerekir. Geri fiziksel sunucuları çok çalıştırma işlemini gerçekleştiriyorsanız bu geçerlidir.
* (Şirket içi VM varsa) başka bir yeniden çalışma yapmadan önce silmek için seçenektir. Yeniden çalışma yeni bir VM ana hedef ESX konağı olarak aynı ana bilgisayardaki sonra oluşturur.

Farklı bir konuma başarısız olduğunda, aynı veri deposu ve aynı ESX ana bilgisayarın şirket içi ana hedef sunucusu tarafından kullanılan veri kurtarılır.

## <a name="prerequisites"></a>Ön koşullar
* Başarısız olmasına VMware Vm'lerini ve fiziksel sunucuları, bir VMware ortamı gerekir. Başarısız olan fiziksel bir sunucuya geri desteklenmiyor.
* Koruma ayarlama başlangıçta ayarladığınızda yeniden çalışmak için bir Azure ağı oluşturmuş olmanız gerekir. Yeniden çalışma Azure VM'ler şirket içi siteye bulunduğu Azure ağdan bir VPN ya da ExpressRoute bağlantı gerekir.
* Geri yönetilen bir vCenter sunucusu tarafından devretmek istediğiniz sanal makineleri yaparsanız emin, VM'ler bulma vCenter sunucuları üzerinde için gerekli izinlere sahip. Daha fazla bilgi için bkz: [çoğaltmak VMware sanal makineleri ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-vmware-to-azure-classic.md).
* Bir VM anlık görüntüler varsa, yükü başarısız olur. Anlık görüntülere veya diskleri silebilirsiniz.
* Geri dönecek önce bu bileşenlerin oluşturun:
  * **Bir işlem sunucusu oluşturma**. Oluşturun ve yeniden çalışma sırasında çalışmaya devam bir Azure VM bileşendir. Yeniden çalışma işlemi tamamlandıktan sonra VM'yi silebilirsiniz.
  * **Bir ana hedef sunucusu oluşturmak**: ana hedef sunucusu gönderir ve yeniden çalışma verilerini alır. Şirket içinde oluşturulan yönetim sunucusu varsayılan olarak yüklenen bir ana hedef sunucusu vardır. Ancak, başarısız geri trafik hacmi bağlı olarak, ayrı ana hedef sunucusu yeniden çalışma için oluşturmanız gerekebilir.
  * Linux üzerinde çalışan bir ek ana hedef sunucusu oluşturmak için ana hedef sunucusunu yüklemeden önce Linux VM yedeklemesi daha sonra açıklandığı şekilde ayarlayın.
* Bir yeniden çalışma işlemi yaptığınızda yapılandırma şirket içi sunucusudur. Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olması gerekir. Yapılandırma sunucusu veritabanı hiçbir VM içeriyorsa, yeniden çalışma başarısız olması. Bu nedenle, sunucunuzun düzenli zamanlanmış yedeklemeler yaptığınızdan emin olun. Bir olağanüstü durum geri dönme çalışmasını aynı IP adresi ile geri yüklemeniz gerekir.
* Disk.enableUUID=true ayarı kümesinde **yapılandırma parametrelerini** VMware VM ana hedef. Bu satır mevcut değilse, bunu ekleyin. Bu ayar, doğru şekilde bağlandığını böylece sanal makine disk (VMDK) dosyasına tutarlı evrensel benzersiz tanımlayıcı (UUID) sağlamak için gereklidir.
* Haberdar bir "ana hedef sunucusu, depolama vMotioned olamaz" koşulu, yeniden çalışma başarısız olmasına neden olabilir. Diskleri için kullanılabilir hale değil çünkü VM çıkarılamıyor.
* Ana hedef sunucusunda bir bekletme sürücüye adlı bir sürücü ekleyin. Bir disk ekleyip sürücüyü biçimlendirin.

## <a name="failback-policy"></a>Yeniden çalışma ilkesi
Geri şirket içi çoğaltmak için yeniden çalışma ilkesi gerekir. İlke iletme yönü ilkesi oluşturma ve yapılandırma sunucusuna otomatik olarak ilişkilendirilir otomatik olarak oluşturulur. Düzenlenebilir değil. İlke çoğaltma ayarları aşağıdaki gibidir:

* RPO eşiği = 15 dakika
* Kurtarma noktası bekletme = 24 saat
* Uygulama ile tutarlı anlık görüntü sıklığı = 60 dakika

 ![Yeniden çalışma ilkesinin çoğaltma ayarları](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu kurma
Böylece Azure sanal makinelerini verileri şirket içi ana hedef sunucuya geri gönderebilir Azure üzerinde bir işlem sunucusu yükleyin.

Korunan Klasik kaynaklar olarak, sanal makine varsa (diğer bir deyişle, Azure'da kurtarılan VM Klasik dağıtım modeli kullanılarak oluşturulmuş bir VM'dir), Azure işlem sunucusu gerekir. Dağıtım türü olarak Azure Resource Manager ile VM'ler kurtardı, işlem Sunucusu Kaynak Yöneticisi dağıtım türü olarak olması gerekir. Dağıtım türü için işlem sunucusu Dağıt Azure sanal ağı olarak seçilidir.

1. İçinde **kasa** > **ayarları** > **Site Recovery altyapısı** (altında **yönetmek**) > **Yapılandırma sunucularına** (altında **için VMware ve fiziksel makineleri**), yapılandırma sunucusu seçin.
2. Tıklatın **işlem sunucusu**.

  ![İşlem sunucusu düğmesi](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. İşlem sunucusu olarak dağıtmayı seçtiğiniz **yeniden çalışma işlem sunucusu azure'da dağıtmak**.
4. Sanal makineleri için kurtardı aboneliği seçin.
5. VM'ler için kurtardı Azure ağını seçin. İşlem sunucusu kurtarılan VM'ler ve işlem sunucusu iletişim kurabilmesi için aynı ağda olması gerekir.
6. Seçtiyseniz, bir *Klasik dağıtım modeli* ağ, Azure Market üzerinden bir VM oluşturun ve içine işlem sunucusu yükleyin.

 !["İşlem Sunucu Ekle" penceresi](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 İşlem sunucusu oluşturmakta olduğunuz gibi aşağıdaki dikkat edin:
 * Görüntü adı *Microsoft Azure Site kurtarma işlemi Server V2*. Seçin **Klasik** dağıtım modeli olarak.

       ![Select "Classic" as the Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * İşlem sunucusu yönergelerine göre yükleyin [çoğaltmak VMware sanal makineleri ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-vmware-to-azure-classic.md).
7. Seçerseniz *Resource Manager* Azure ağ, aşağıdaki bilgileri sağlayarak işlem sunucusu Dağıt:

  * Sunucuya dağıtmak istediğiniz kaynak grubunun adı
  * Sunucu adı
  * Bir kullanıcı adı ve parola sunucuya oturum açmak için
  * Sunucuya dağıtmak istediğiniz depolama hesabı
  * Alt ağı ve buna bağlanmak istediğiniz ağ arabirimi
   >[!NOTE]
   >Kendi oluşturmalısınız [ağ arabirimi](../virtual-network/virtual-networks-multiple-nics.md) (NIC) ve işlem sunucusu dağıtırken seçin.

    !["İşlem Sunucu Ekle" iletişim kutusuna bilgileri girin](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. **Tamam** düğmesine tıklayın. Bu eylem işlem Sunucusu Kurulumu sırasında bir Resource Manager dağıtım türü sanal makine oluşturur bir işi tetikler. Sunucusunu yapılandırma sunucusuna kaydetmek için kurulumu VM içindeki yönergeleri takip ederek çalıştırmak [çoğaltmak VMware sanal makineleri ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-vmware-to-azure-classic.md). İşlem sunucusu dağıtmak için bir iş ayrıca tetiklenir.

  İşlem sunucusu listelendiğini **yapılandırma sunucularına** > **sunucular ilişkilendirilen** > **işlem sunucuları** sekmesi.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > İşlem sunucusu altında görünür olmayan **VM özelliklerini**. Yalnızca görülebilir **sunucuları** için kayıtlı yönetim sunucusu sekmesindedir. İşlem sunucusu görünmesi için 10-15 dakika sürebilir.


## <a name="set-up-the-master-target-server-on-premises"></a>Ana hedef sunucusu şirket içi ayarlayın
Ana hedef sunucusu yeniden çalışma verileri alır. Sunucu şirket içi yönetim sunucusunda otomatik olarak yüklenir, ancak çok fazla veri yeniden çalıştırma işlemini gerçekleştiriyorsanız, bir ek ana hedef sunucusu ayarlamanız gerekebilir. Ayarlamak için bir ana hedef sunucusu şirket, aşağıdakileri yapın:

> [!NOTE]
> Linux ana hedef sunucusunda ayarlamak için sonraki yordama atlayın. Yalnızca CentOS 6.6 en düşük işletim sistemi ana hedef işletim sistemi kullanın.

1. Windows ana hedef sunucusunda ayarlıyorsanız, ana hedef sunucusu yüklemekte VM'den hızlı başlangıç sayfasını açın.
2. İçin Azure Site kurtarma birleşik Kurulum Sihirbazı yükleme dosyasını indirin.
3. Kurulumu çalıştırmak ve buna **başlamadan önce**seçin **dağıtım ölçeklendirmek için ek işlem sunucusu eklemek**.
4. Ne zaman yaptığınız gibi Sihirbazı tamamlayın, [yönetimi sunucusunu ayarlama](site-recovery-vmware-to-azure-classic.md). Üzerinde **yapılandırma sunucusu ayrıntılarını** sayfasında, ana hedef sunucusu IP adresini belirtin ve VM erişmek için bir parola girin.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Bir Linux VM ana hedef sunucusu olarak ayarla
Ana hedef sunucusu bir Linux VM çalıştıran yönetim sunucusu ayarlamak için CentOS 6.6 en düşük işletim sistemi yükleyin. Ardından, her SCSI sabit disk için SCSI kimliklerini almak, bazı ek paketler yükleme ve özel bazı değişiklikler uygulanır.

#### <a name="install-centos-66"></a>CentOS 6.6 yükleyin

1. CentOS 6.6 en düşük işletim sistemi yönetim sunucusuna VM yükleyin. Bir DVD sürücüsünde ISO tutmak ve sistem önyüklemesi. Sınama ortamı atlayın. Seçin **ABD İngilizcesi** dili olarak seçin **temel depolama aygıtları**, sabit sürücüde tıklatın herhangi bir önemli veri yok denetleyin **Evet**ve herhangi bir veri atın. Yönetim sunucusunun ana bilgisayar adını girin ve sunucu ağ bağdaştırıcısı seçin.  İçinde **düzenleme sistem** iletişim kutusunda **otomatik olarak bağlan**ve ardından statik bir IP adresi, ağ ve DNS ayarlarını ekleyin. Bir saat dilimini belirtin. Yönetim sunucusuna erişmek için kök parolasını girin.
2. Hangi tür yükleme istediğiniz sorulduğunda, seçin **oluşturma özel yerleşim** bölümü. **İleri**’ye tıklayın. Seçin **serbest**ve ardından **oluşturma**. Oluşturma  **/** , **/var/kilitlenme**, ve **/ev bölümleri** ile **FS türü:** **ext4**. Takas bölümü olarak oluşturma **FS türü: takas**.
3. Önceden var olan aygıtları bulunursa, bir uyarı iletisi görüntülenir. Tıklatın **biçimi** bölüm ayarları sürücüyle biçimlendirmek için. Tıklatın **değişiklik diske yazma** bölüm değişiklikleri uygulamak için.
4. Seçin **yükleme önyükleyici** > **sonraki** önyükleyici kök bölüme yüklemek için.
5. Yükleme tamamlandığında tıklatın **yeniden**.

#### <a name="retrieve-the-scsi-ids"></a>SCSI kimlikleri alma

1. Yükleme sonrasında, VM her SCSI sabit disk için SCSI kimlikleri alma. Bunu yapmak için yönetim sunucusunu VM'yi kapatın. VMware VM özelliklerinde VM girişi sağ tıklatın > **ayarlarını Düzenle** > **seçenekleri**.
2. Seçin **Gelişmiş** > **genel öğesi**ve ardından **yapılandırma parametreleri**. Makine çalışıyorsa, bu seçenek kullanılamaz. Seçeneğinin kullanılabilir olması için makine kapatılmalıdır.
3. Aşağıdakilerden birini yapın:
 * Varsa satır **disk. EnableUUID** yoksa, değer ayarlamak emin olun **True** (büyük küçük harf duyarlı). Değer zaten True olarak ayarlanırsa, iptal edin ve onu önyüklendiğinde sonra SCSI komutu bir konuk işletim sistemi içinde test edin.
 * Varsa satır **disk. EnableUUID** değil, mevcut tıklatın **Satır Ekle**ve ardından ekleyin **True** değeri. Çift tırnak işaretleri kullanmayın.

#### <a name="install-additional-packages"></a>Ek paket yüklemek için
Ek paket yükleyip yeniden açın.

1. Ana hedef sunucusu Internet'e bağlı olduğundan emin olun.
2. Karşıdan yükle ve CentOS depodan 15 paketleri yüklemek için bu komutu çalıştırın: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Kaynak makinelerden koruyorsanız Reiser ile Linux çalıştıran veya XFS dosya sistemi kök veya önyükleme aygıtı için indirin ve ek paketleri aşağıdaki gibi yükleyin:

   * \#CD /usr/local
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#RPM – ivh reiserfs 0,0 1.el6.elrepo.x86_64.rpm kmod reiserfs-yardımcı programları-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#RPM – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \#yum aygıt-Eşleyici-(ana hedef sunucusunda çok yollu paketleri etkinleştirmek için gerekli) çok yollu yükleyin

#### <a name="apply-custom-changes"></a>Özel Değişiklikleri Uygula
Yükleme sonrası adımları tamamladıysanız ve paketleri kurduktan sonra aşağıdakileri yaparak özel değişiklikleri uygulayın:

1. RHEL 6-64 birleşik aracı ikili VM'ye kopyalayın. İkili untar için bu komutu çalıştırın: `tar –zxvf <file name>`.
2. İzin vermek için bu komutu çalıştırın: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Aşağıdaki komut dosyasını çalıştırın: `# ./ApplyCustomChanges.sh`. Çalıştırın yalnızca bir kez. Başarıyla çalıştıktan sonra sunucuyu yeniden başlatın.

## <a name="run-the-failback"></a>Yeniden çalışma çalıştırın
### <a name="reprotect-the-azure-vms"></a>Azure VM'ler koruyun
1. İçinde **kasa**, **öğeleri çoğaltılan**üzerinden başarısız VM'ye sağ tıklayın ve ardından **yeniden koruma**.
2. Dikey penceresinde görebilirsiniz koruma yönünü **şirket içi Azure** zaten seçilidir.
3. İçinde **ana hedef sunucusu** ve **işlem sunucusu**, şirket içi ana hedef sunucusu ve Azure VM işlem sunucusunu seçin.
4. Diskleri şirket içi kurtarmak istediğiniz veri deposu seçin. Şirket içi VM silinir ve yeni diskler oluşturmak gerektiğinde bu seçeneği kullanın. Diskleri zaten var, ancak yine de bir değer belirtmeniz gerekiyorsa yoksay seçeneği.
5. VM geri şirket içi zaman çoğaltılır zaman içinde nokta durdurmak için bekletme sürücüsü kullanın. Saklama sürücüsünün bazı ölçütü burada listelenir. Bu ölçütler sürücü ana hedef sunucu için listelenmeyen.

  * Birim, başka bir amaç için (hedef çoğaltma ve benzeri) kullanımda olmamalıdır.
  * Birim kilit modunda olmamalıdır.
  * Birim önbellek birimi olmamalıdır. (Ana hedef yükleme bu birimde mevcut döndürmemelidir. İşlem sunucusu ve ana hedef özel yükleme birimi bekletme birimi için uygun değil. Burada, birim yüklü işlem sunucusu ve ana hedef ana hedef önbellek birimdir.)
  * Birim dosya sistemi türü, FAT ve FAT32 olmamalıdır.
  * Birim kapasitesi sıfır olmamalıdır.
  * Windows için varsayılan saklama biriminin R birimdir.
  * Linux için varsayılan saklama biriminin /mnt/retention ' dir.

6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Tıklattıktan sonra **Tamam** yükü başlamak için bir iş VM Azure'dan şirket içi siteye çoğaltmaya başlar. Üzerinde ilerleme durumunu izleyebilirsiniz **işleri** sekmesi.

Alternatif konuma kurtarma yapmak istiyorsanız, bekletme sürücüsü ve ana hedef sunucu için yapılandırılmış veri deposu seçin. Şirket içi sitede yeniden çalıştırmak, yeniden çalışma koruma planı VMware Vm'lerini aynı veri deposu ana hedef sunucusu olarak kullanın. Azure VM aynı şirket içi VM çoğaltma kurtarmak istiyorsanız, şirket içi VM zaten aynı veri deposu ana hedef sunucusu olarak bulunmalıdır. Hiçbir VM içi varsa, yeni bir yükü sırasında oluşturulur.

!["Azure şirket içi" açılır menüde seçin](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Ayrıca, bir kurtarma planı düzeyde koruyun. Bir çoğaltma grubu varsa, yalnızca bir kurtarma planı kullanarak koruyun. Bir kurtarma planı kullanarak koruyun, önceki değerleri korunan her makine için kullanın.

> [!NOTE]
> Çoğaltma grupları yeniden aynı ana hedefi ile korunmalıdır. Farklı bir ana hedef sunucularla korunuyorsa kendileri için bir ortak bir noktaya belirlenemiyor.

### <a name="run-a-failover-to-the-on-premises-site"></a>Şirket içi siteye bir yük devretmeyi çalıştırma
VM koruyun sonra bir yük devretme azure'dan şirket içi başlatabilirsiniz.

1. Çoğaltılan öğe sayfasında, sanal makineye sağ tıklayın ve ardından **planlanmamış yük devretme**.
2. İçinde **onaylayın yük devretme**, yük devretme yönünden (Azure) doğrulayın ve ardından Yük devretme için (en son veya en son uygulamayla tutarlı kurtarma noktası) kullanmak istediğiniz kurtarma noktasını seçin. Bir uygulama tutarlı bir kurtarma noktası süre önce en son noktası oluşur ve bazı veri kaybına neden olur.
3. Yük devretme sırasında Azure sanal makinelerini Site Recovery kapatır. Bu geri dönme beklendiği gibi tamamlanmış denetledikten sonra Azure sanal makinelerini beklendiği gibi kapatılmışsa emin olmak için kontrol edebilirsiniz.

### <a name="reprotect-the-on-premises-site"></a>Şirket içi site koruyun
Yeniden çalışma tamamladıktan sonra yürütme sanal makineleri Azure'da kurtarılan sağlamak için sanal makine silinir. Bunu yapmak için korunan öğeye sağ tıklayın ve ardından **yürütme**. Bu eylem, önceki kurtarılan sanal makine Azure'da kaldıran bir işi tetikler.

Yürütme tamamlandıktan sonra verilerinizi şirket içi sitede olmalıdır, ancak korunan olmaz. Azure'a çoğaltma yeniden başlatmak için aşağıdakileri yapın:

1. İçinde **kasa**, **ayarı** > **öğeleri çoğaltılan**, geri başarısız olmuş ve ardından sanal makineleri seçin **yeniden koruma**.
2. Geri Azure'a veri göndermek için kullanılması gereken işlem sunucusu değerini verir.
3. **Tamam** düğmesine tıklayın.

Yükü tamamlandıktan sonra geri Azure VM yinelenir ve bir yük devretme yapabilirsiniz.

### <a name="resolve-common-failback-issues"></a>Yeniden çalışma ile ilgili genel sorunları giderin
* Salt okunur kullanıcı vCenter bulma işlemini ve sanal makineleri koruma, başarılı ve yük devretme çalışır. Yükü sırasında datastores bulunan yük devretme başarısız olur. Bir belirti yükü sırasında listelenen datastores görmezsiniz. Bu sorunu çözmek için gerekli izinlere sahip uygun bir hesap ile vCenter kimlik bilgileri güncelleştirebilir ve işi yeniden deneyin. Daha fazla bilgi için bkz: [çoğaltmak VMware sanal makineleri ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-vmware-to-azure-classic.md)
* Bir Linux VM yeniden çalışmak ve şirket içi çalıştırın, ağ yöneticisi paket makineden kaldırıldı görebilirsiniz. Ağ Yöneticisi paket Azure'da VM kurtarıldığında kaldırıldığı bu kaldırma işlemi gerçekleşir.
* Bir VM Azure'a yük devretti ve statik IP adresiyle yapılandırılmış olduğunda, IP adresini DHCP yoluyla alınır. Şirket içi geri üzerinden başarısız olduğunda, VM IP adresini almak için DHCP kullanmaya devam eder. El ile makinede oturum açmak ve IP adresini geri bir statik adrese gerekirse ayarlayın.
* ESXi 5.5 ücretsiz sürüm veya 6 vSphere hiper yönetici ücretsiz sürümü kullanıyorsanız, yük devretme başarılı, ancak yeniden çalışma başarısız. Yeniden çalışma etkinleştirmek için her iki programın değerlendirme lisansına yükseltme.
* Yapılandırma sunucusuna işlem sunucusundan bağlanamazsa, 443 numaralı bağlantı noktasını yapılandırma server makinesinde için yapılandırma sunucusu tarafından Telnet bağlantısını denetleyin. İşlem sunucusu makine yapılandırma sunucusundan ping işlemi yapmak deneyebilirsiniz. Yapılandırma sunucusuna bağlandığında bir işlem sunucusu bir sinyal de olmalıdır.
* Alternatif bir vCenter başarısız çalışıyorsanız, yeni vCenter bulunduğundan ve ana hedef sunucusu ayrıca bulunduğundan emin olun. Tipik bir belirti datastores erişilebilir veya görünür durumda olmamasıdır **koruyun** iletişim kutusu.
* Fiziksel makine şirket içi gibi korunan bir WS2008R2SP1 makine Azure'dan şirket içi başarısız olamaz.

## <a name="fail-back-with-expressroute"></a>ExpressRoute ile başarısız
ExpressRoute bağlantısı kullanarak veya bir VPN bağlantısı üzerinden geri başarısız olabilir. ExpressRoute bağlantısı kullanmak istiyorsanız, aşağıdakilere dikkat edin:

* Kaynak makine için yük devri ve yük devretme gerçekleştikten sonra Azure sanal makinelerini bulunduğu Azure sanal ağ ExpressRoute bağlantı ayarlanması.
* Veriler, ortak bir uç noktada bir Azure depolama hesabına çoğaltılır. ExpressRoute bağlantısı kullanmak için ortak de ExpressRoute Site Recovery çoğaltma için hedef veri merkezi ile eşleme yukarı ayarlayın.
