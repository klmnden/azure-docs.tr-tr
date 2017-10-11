---
title: "Başarısız Klasik eski Portalı'nda Azure'dan VMware Vm'lerini yedekleme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile azure'a çoğaltılmış bir VMware sanal makinesi geri dönecek açıklar."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 3053fc622c6343898e2007b8aaafbe1fa8e6934e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Geri VMware sanal makineleri ve fiziksel sunuculardan Azure Azure Site Recovery (eski) ile vmware'e başarısız
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klasik Azure Portalı](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klasik Azure portalı (eski)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Bu makalede geri VMware sanal makineleri ve azure'dan şirket içi fiziksel sunucuların site Azure kullanarak, şirket içi siteden çoğaltılan sonra Windows/Linux başarısız açıklar [Azure Site Recovery?](site-recovery-overview.md).

Bu makalede eski bir yapılandırma ve yeni kasa için kullanılmaması gerekir.

## <a name="architecture"></a>Mimari
Bu diyagramda, yük devretme ve yeniden çalışma senaryosu temsil eder. Mavi çizgiler yük devretme sırasında kullanılan bağlantılardır. Kırmızı satırları yeniden çalışma sırasında kullanılan bağlantılardır. Oklarla çizgiler Internet üzerinden gidin.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Başlamadan önce
* VMware Vm'lerini veya fiziksel sunucular üzerinden başarısız ve Azure'da çalıştırıyor olmalıdır.
* Yalnızca arka VMware sanal makinelerini ve Windows/Linux fiziksel sunucuların azure'dan şirket içi birincil sitede VMware sanal makineleri yük devredebildiğini unutmamalısınız.  Fiziksel makinenin yeniden çalıştırma işlemini gerçekleştiriyorsanız, Azure'a yük devretmesi için bir Azure VM dönüştürür ve VMware için yeniden çalışma için bir VMware VM dönüştürür.

Yeniden çalışma nasıl ayarlanacağını aşağıda verilmiştir:

1. **Yeniden çalışma bileşenleri ayarlamak**: bir vContinuum sunucusu ayarlamanız gerekir şirket içi ve yapılandırma sunucusu Azure VM üzerine gelin. Şirket içi ana hedef sunucusuna veri göndermek için bir Azure VM gibi bir işlem sunucusu da ayarlarız. İşlem sunucusu yük devretme işlenen yapılandırma sunucusuna kaydedin. Bir şirket içi ana hedef sunucusu yükleyin. Bir Windows ana hedef sunucusu gerekiyorsa vContinuum yüklediğinizde bu otomatik olarak ayarlanır. Linux gerekiyorsa ayrı bir sunucuya el ile ayarlamanız gerekir.
2. **Koruma ve yeniden etkinleştirme**: bileşenlerini ayarladıktan sonra vContinuum, Azure vm'lerinde korumasını etkinleştirmek gerekir. Bir hazırlık denetimi VM'ler çalıştırın ve yük devretme Azure'dan şirket içi sitenize çalıştırma. Yeniden çalışma sona erdikten sonra Azure'a çoğaltma başlatmaları böylece şirket içi makineler koruyun.

## <a name="step-1-install-vcontinuum-on-premises"></a>1. adım: vContinuum şirket içi yükleme
Şirket içinde vContinuum sunucusu yükleme ve yapılandırma sunucusuna noktası gerekir.

1. [VContinuum karşıdan](http://go.microsoft.com/fwlink/?linkid=526305).
2. Ardından karşıdan [vContinuum güncelleştirme](http://go.microsoft.com/fwlink/?LinkID=533813) sürümü.
3. VContinuum en son sürümünü yükleyin. Üzerinde **Hoş Geldiniz** sayfasında **sonraki**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Sihirbazın ilk sayfasında CX sunucusu IP adresi ve CX sunucu bağlantı noktası belirtin. Seçin **HTTPS kullanmak**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Yapılandırma sunucusu IP adresi bulunamadı **Pano** yapılandırma sunucusu Azure VM'de sekmesinde.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Genel bağlantı noktası HTTPS yapılandırma sunucusu bulunamadı **uç noktaları** yapılandırma sunucusu Azure VM'de sekmesinde.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. Üzerinde **CS parola ayrıntıları** sayfasında yapılandırma sunucusu kaydolurken aşağı ettiğiniz parolayı belirtin. Hatırlamıyorsanız iade onu **C:\\Program Files (x86)\\Inmage sistemleri\\özel\\connection.passphrase** yapılandırma sunucusundaki VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. İçinde **hedef konum seçin** sayfasında belirtin tıklatıp vContinuum sunucusu yüklemek istediğiniz **yükleme**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. Yükleme tamamlandıktan sonra vContinuum başlatabilirsiniz.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>2. adım: Azure üzerinde bir işlem sunucusu yükleme
Böylece VM'ler için Azure'da bir şirket içi ana hedef sunucusuna geri veri gönderebilir Azure üzerinde bir işlem sunucusu yüklemeniz gerekir.

1. Üzerinde **yapılandırma sunucularına** Azure'da sayfasında, yeni bir işlem sunucusu eklemek için seçin.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Bir işlem sunucusu belirtin adını, bir ad ve bir yönetici olarak sanal makineye bağlanmak için parola. İşlem sunucusu kaydetmek istediğiniz yapılandırma sunucusu seçin. Bu sanal makinelerinizi korumak ve başarısız için kullanmakta olduğunuz sunucunun aynı olmalıdır.
3. İşlem sunucusu dağıtılmalıdır Azure ağını belirtin. Yapılandırma sunucusu aynı ağda olmalıdır. Dağıtımına başlamak ve benzersiz bir IP adresi seçilen alt ağ belirtin.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. İşlem sunucusu dağıtmak için bir iş tetiklendi.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. İşlem sunucusu Azure'da dağıtıldıktan sonra belirtilen kimlik bilgilerini kullanarak sunucuda oturum açabilir. İşlem sunucusu şirket içi işlem sunucusu kayıtlı aynı şekilde kaydedin.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> Yeniden çalışma sırasında kayıtlı sunucuları, Site kurtarma VM Özellikleri'nin altında görünmeyecektir. Yalnızca altında görünür **sunucuları** kayıtlı oldukları yapılandırma sunucusu sekmesinde. Yaklaşık 10-15 dakika bunlar İşlem Sunucusu sekmesinde görünür kadar sürebilir.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>3. adım: şirket içi ana hedef server yükleme
Kaynak sanal makine işletim sistemine bağlı olarak bir Linux yüklemeniz gerekir veya bir Windows ana hedef sunucusu şirket içi.

### <a name="deploy-a-windows-master-target-server"></a>Bir Windows ana hedef sunucusu dağıtma
Bir Windows ana hedef zaten vContinuum Kurulum paketlenebilir. VContinuum yüklediğinizde, ana sunucu da aynı makinede dağıtılan ve yapılandırma sunucusuna kayıtlı.

1. Dağıtımına başlamak için boş bir oluşturma makine şirket içi istediğiniz sanal makineleri Azure yedeklemeden kurtarmak ESX konağındaki.
2. En az iki disk VM'ye bağlı olduğundan emin olun – bir işletim sistemi için kullanılan ve diğer bekletme sürücüsü için kullanılır.
3. İşletim sistemini yükleyin.
4. VContinuum sunucuya yükleyin. Bu ayrıca ana hedef sunucusu yüklemesini tamamlar.

### <a name="deploy-a-linux-master-target-server"></a>Bir Linux ana hedef sunucusu dağıtma
1. Dağıtımına başlamak için boş bir oluşturma makine şirket içi istediğiniz sanal makineleri Azure yedeklemeden kurtarmak ESX konağındaki.
2. En az iki disk VM'ye bağlı olduğundan emin olun – bir işletim sistemi için kullanılan ve diğer bekletme sürücüsü için kullanılır.
3. Linux işletim sistemini yükleyin. Linux ana hedef sistem LVM kök veya bekletme depolama alanları için kullanmamanız gerekir. Bir Linux ana hedef sunucusu varsayılan olarak, varsayılan olarak LVM bölümleri/diskleri bulma önlemek için yapılandırılır.
4. Oluşturabileceğiniz bölümler:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Yürütmek aşağıda sonrası yükleme adımlarını ana hedef yüklemeye başlamadan önce.

#### <a name="post-os-installation-steps"></a>İşletim sistemi yükleme adımlarını sonrası
Her bir Linux sanal makine SCSI sabit disk için SCSI ID almak için "disk. parametre etkinleştir EnableUUID = TRUE "gibi:

1. Sanal makineyi kapatın.
2. Sol panelinde VM girişi sağ tıklatın > **ayarlarını Düzenle**.
3. Tıklatın **seçenekleri** sekmesi. Seçin **Gelişmiş\>genel öğesi** > **yapılandırma parametreleri**. **Yapılandırma parametrelerini** seçenek, yalnızca kullanılabilir makine kapatıldığında.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Olan bir satır olup olmadığını denetler **disk. EnableUUID** bulunmaktadır. Yapar ve ayarlanırsa **False** sonra kümesine **True** (büyük/küçük harfe duyarlı değildir). Varsa var ve için true, tıklatın ayarlayın **iptal** ve yukarı önyüklendiğinde sonra konuk işletim sistemi içinde SCSI komutu test edin. Varsa tıklatın yok **Satır Ekle**.
5. Disk ekleyin. İçinde EnableUUID **adı** sütun. Değerini TRUE olarak ayarlayın. Yukarıdaki değerleri çift tırnak birlikte eklemeyin.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>İndirme ve ek paketler yükleme
Not: ek paketler yükleyip önce Sistem internet bağlantısı olduğundan emin olun.

\#yum -y xfsprogs perl lsscsi rsync wget kexec-araçları yükleme

Bu komut, CentOS 6.6 depodan 15 bu paketleri yükler ve bunları yükler:

BC 1.06.95 1.el6.x86\_64. rpm

busybox 1.15.1 20.el6.x86\_64. rpm

kitaplıklar 0.158 3.2.el6.x86 elfutils\_64. rpm

Araçlar 2.0.0 280.el6.x86 kexec\_64. rpm

lsscsi 0.23 2.el6.x86\_64. rpm

lzo 2.03 3.1.el6\_5.1.x86\_64. rpm

Perl 5.10.1 136.el6\_6.1.x86\_64. rpm

Perl-modül-takılabilir-3.90-136.el6\_6.1.x86\_64. rpm

Perl-Pod-çıkışları-1,04-136.el6\_6.1.x86\_64. rpm

Perl-Pod-Basit-3.13-136.el6\_6.1.x86\_64. rpm

Perl kitaplıklar 5.10.1 136.el6\_6.1.x86\_64. rpm

Perl sürüm 0.77 136.el6\_6.1.x86\_64. rpm

rsync 3.0.6 12.el6.x86\_64. rpm

1.1.0 1.el6.x86 snappy\_64. rpm

wget 1.12 5.el6\_6.1.x86\_64. rpm

Kaynak makine için kök veya önyükleme aygıtı Reiser veya XFS dosya sistemi kullanıyorsa, paketleri aşağıdaki indirilebilen ve Linux ana hedefinin koruma önce yüklenmiş.

\#CD /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#RPM - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. rpm reiserfs-yardımcı programları-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#RPM - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Özel yapılandırma değişikliklerini uygula
Bu değişiklikleri uygulamadan önce önceki bölümde tamamladığınız emin olun ve ardından aşağıdaki adımları izleyin:

1. RHEL 6-64 birleşik aracı ikili yeni oluşturulan işletim sistemine kopyalayın.
2. İkili untar için şu komutu çalıştırın: **- zxvf hedef \<dosya adı\>**
3. İzin vermek için bu komutu çalıştırın: \# **chmod 755./ApplyCustomChanges.sh**
4. Komut dosyasını çalıştırın:  **\# ./ApplyCustomChanges.sh**. Komut dosyası yalnızca bir kez sunucusunda çalıştırın. Betik çalıştıktan sonra sunucuyu yeniden başlatın.

### <a name="install-the-linux-server"></a>Linux sunucusu yükleme
1. [Karşıdan](http://go.microsoft.com/fwlink/?LinkID=529757) yükleme dosyası.
2. Tercih ettiğiniz bir sftp istemci yardımcı programını kullanarak Linux ana hedef sanal makine dosyasını kopyalayın. Alternatif olarak Linux ana hedef sanal makineye yüklenmesi oturum ve sağlanan bağlantıdan yükleme paketini indirmek için wget kullanın
3. Linux ana hedef sunucusu kullanarak sanal makineyi üzerine günlük bir ssh istemcisi tercih ettiğiniz
4. Linux ana hedef sunucunuzu bir VPN bağlantısı üzerinden dağıttığınız Azure ağa bağlı değilseniz sonra iç IP adresi sanal makinede bulabilirsiniz sunucunun kullandığı **Pano** sekmesini tıklatın ve 22 olarak bağlantı noktası Linux ana hedef sunucusunu Secure Shell kullanarak bağlanın.
5. Ortak bir Internet bağlantısı kullanımı Linux ana hedef sunucuya bağlanıyorsanız Linux ana hedef sunucunun ortak sanal IP adresinin (sanal makinelerden **Pano** sekmesi) ve ortak uç nokta için ssh oluşturulan Linux servder oturum açmak için.
6. Çalıştırarak gzip'li Linux ana hedef sunucu yükleyicisi tar arşivinden dosyaları ayıklayın: *"– xvzf Microsoft ASR hedef\_UA\_8.2.0.0\_RHEL6 64\"* içeren dizininden Yükleyici dosyası.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Dosyaları farklı bir dizin için dizin değiştirme yükleyici ayıklanan varsa tar içeriğini arşiv ayıkladığınız. Bu dizin yolundan Çalıştır "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. Bir birincil rolü Seç isteyip istemediğiniz sorulduğunda **2 (ana hedef)**. Bir etkileşimli yükleme seçenekleri varsayılan değerlerinde bırakın.
9. Devam etmek için yükleme ve görünmesi ana bilgisayar yapılandırma arabirimi için bekleyin. Ana bilgisayar yapılandırması Linux ana hedef sunucusu için bir komut satırı yardımcı programıdır. Yeniden boyutlandırma ssh istemcisi yardımcı programı penceresinde. Seçmek için ok tuşlarını **genel** seçeneği ve ENTER tuşuna BASIN klavyenizde.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. Alanında **IP** yapılandırma sunucusunun iç IP adresi girin (içinde bulabileceğiniz **Pano** yapılandırma sunucusu VM sekmesinde) ve ENTER tuşuna basın. İçinde **bağlantı noktası** girin **22** ve ENTER tuşuna basın.
11. Bırakın **HTTPS kullan** olarak **Evet**, ve ENTER tuşuna basın.
12. Yapılandırma sunucusunda oluşturulan parolayı girin. PUTTY bir istemci için bir Windows makineden ssh Linux sanal makineye kullanıyorsanız, Pano içeriği yapıştırmak için Shift + INSERT kullanabilirsiniz. Parola Ctrl-C kullanarak yerel panoya kopyalamak ve yapıştırmak için Shift + INSERT kullanın. ENTER tuşuna basın.
13. Çıkmak ve ENTER tuşuna basın gitmek için sağ ok tuşunu kullanın. Yüklemenin tamamlanmasını bekleyin.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Herhangi bir nedenden dolayı Linux ana hedef sunucunuzu yapılandırma sunucusuna kaydetmek başarısız olursa, böylece yeniden ana bilgisayar yapılandırması yardımcı programını (önce bunun için erişim izinlerini ayarlamak için gerekir. /usr/local/ASR/Vx/bin/hostconfigcli çalıştırarak yapabilirsiniz Dizin) süper kullanıcı olarak chmod çalıştırarak.

#### <a name="validate-master-target-registration"></a>Ana hedef kayıt doğrula
Ana hedef sunucusu yapılandırma sunucusuna Azure Site Recovery kasasına başarıyla kaydedildi doğrulayabilirsiniz > **yapılandırma sunucusu** > **sunucu ayrıntıları**.

> [!NOTE]
> Sanal makine Azure'dan silinmiş olabilir veya uç noktalar düzgün şekilde yapılandırılmazsa, yapılandırma hataları alırsanız, ana hedef sunucusu kaydolduktan sonra bu ana hedef rağmen yapılandırma tarafından algılanan nedeni Ana hedef Azure'da dağıtıldığında azure dndpoints bu şirket içi ana hedef sunucusu şirket içi için doğru değil. Bu yeniden çalışma etkilemez ve bu hatalar yoksayabilirsiniz.
>
>

## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>4. adım: şirket içi site için sanal makineleri koruma
Geri dönecek önce VM'ler şirket içi siteye korumanız gerekir.

### <a name="before-you-begin"></a>Başlamadan önce
Bir VM için Azure devredildiğinde, sayfa dosyası için fazladan geçici bir sürücü ekler. Bu, genellikle gerekli değildir, ek bir sürücüdür başarısız, sayfa dosyası için adanmış bir sürücü zaten sahip olabilir olduğundan VM üzerinde tarafından. Sanal makinelerin ters koruma başlamadan önce böylece koruma bu sürücüyü çevrimdışı duruma getirmenizi sağlamak gerekir. Bunu şu şekilde yapabilirsiniz:

1. Bilgisayar Yönetimi'ni açın ve böylece diskleri çevrimiçi listeler ve makineye bağlı Depolama Yönetimi'ni seçin.
2. Makineye bağlı geçici disk seçin ve çevrimdışı duruma getirmek seçin.

### <a name="protect-the-vms"></a>Vm'leri koruma
1. Azure portalında bunlar devredilir emin olmak için sanal makinenin durumunu denetleyin.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. VContinuum makinenizde başlatın.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Tıklatın **yeni koruma** ve işlem sistem türü seçin
4. Yeni pencerede seçin açmak **işletim sistemi türü** > **Al ayrıntıları** yeniden çalışmak istediğiniz VM'ler için. İçinde **birincil sunucu ayrıntıları**tanımlamak ve korumak istediğiniz sanal makineleri seçin. VM'ler üzerindeki yük devretme önce oldukları vCenter ana bilgisayar adı altında listelenir.
5. Korumak için bir sanal makineyi seçin (ve onu üzerinden Azure'a başarısız olduğunda) bir açılır pencere sanal makine için iki giriş sağlar. Yapılandırma sunucusu için kaydedilmiş sanal makineleri iki örneğini algılar olmasıdır. Doğru VM koruyabilmeniz için şirket içi VM için giriş kaldırmanız gerekir. Burada doğru Azure VM girişi tanımlamak için Azure VM'de oturum oturum ve C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc gidin. Dosya drscout.conf içinde ana bilgisayar kimliği tanımlayın VContinuum iletişim kutusunda, VM ana bilgisayar kimliği öğrenmek bulunan giriş tutun. Diğer tüm girişleri silin. Doğru VM seçmek için IP adresine başvurabilir. IP adresi aralığı şirket içi VM olacaktır.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. VCenter sunucusu üzerinde sanal makine durdurun. Ayrıca, sanal makineleri şirket içi silebilirsiniz.
7. Ardından sanal makineleri korumak istediğiniz şirket içi MT sunucusunu belirtin. Bunu yapmak için geri dönecek istediğiniz vCenter sunucusuna bağlanın.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. VM kurtarmak istediğiniz ana bilgisayar tabanlı ana hedef sunucuyu seçin.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Her sanal makine için çoğaltma seçeneğini belirtin. Bunu yapmak için sanal makineleri kurtarılacak kurtarma tarafı veri deposu seçmeniz gerekir. Tablonun her VM için sağlamanız gereken farklı seçenekleri özetler.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **Seçeneği** | **Önerilen değer seçeneği** |
   | --- | --- |
   |  İşlem sunucusu IP adresi |Azure üzerinde dağıtılan işlem sunucusunu seçin |
   |  Bekletme boyutu (MB) | |
   |  Bekletme değeri |1 |
   |  Gün/saat |Gün |
   |  Tutarlılık aralığı |1 |
   |  Hedef veri deposu seçin |Veri deposu kullanılabilir kurtarma sitesinde. Veri deposunda yeterli alana sahip ve sanal makineyi geri yüklemek istediğiniz ESX konak kullanılabilir olması gerekir. |
10. Sanal makine şirket içi siteye yük devretme sonrasında alacağı özelliklerini yapılandırın. Özellikler aşağıdaki tabloda özetlenmiştir.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Özellik** | **Ayrıntılar** |
    | --- | --- |
    | Ağ yapılandırması |Algılanan her ağ bağdaştırıcısı seçin ve **değişiklik** sanal makine için yeniden çalışma IP adresini yapılandırmak için. |
    | Donanım yapılandırması |CPU ve bellek VM için belirtin. Ayarları korumaya çalışıyorsunuz tüm VM'ler için uygulanabilir. CPU ve bellek için doğru değerleri belirlemek için IAAS Vm'leri rol boyutunu bakın ve çekirdek sayısı ve atanan bellek bakın. |
    | Görünen ad |VCenter envanterinde görüntülenir gibi şirket içi yeniden çalışma sonra sanal makineleri adlandırabilirsiniz. Sanal makine bilgisayar konak adı varsayılan addır. VM adını tanımlamak için koruma grubu VM listesinde başvurabilirsiniz. |
    | NAT yapılandırma |Aşağıda ayrıntılı olarak ele alınan |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT ayarlarını yapılandırma
1. Sanal makinelerin korumasını etkinleştirmek için iki iletişim kanalı kurulması gerekir. İlk sanal makine ve işlem sunucusu arasında kanalıdır. Bu kanal, sanal makineden toplar ve ardından ana hedef sunucuya veri gönderen işlem sunucusuna gönderir. İşlem sunucusu ve korunması için sanal makineyi aynı Azure sanal ağda olup olmadıklarını NAT ayarlarını kullanmak gerekmez. Aksi takdirde NAT ayarlarını belirtin. İşlem Sunucusu genel IP adresi Azure'da görüntüleyebilirsiniz.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. İkinci işlem sunucusu ve ana hedef sunucusu kanalıdır. NAT kullanıp seçeneğine tabanlı VPN bağlantısı kullanarak veya internet üzerinden iletişim bağlıdır. NAt, VPN kullanıyorsanız, ancak yalnızca bir Internet bağlantısı kullanıyorsanız seçmeyin.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Şirket içi sanal makineleri belirtilen sildiyseniz henüz ve geri yine başarısız olan veri deposu eski VMDK's içeriyorsa VM yeni bir yerde oluşturulduğunu geri dönme emin olmak gerekir. Bu seçim yapmak için **Gelişmiş** ayarları ve içeri geri yüklemek için başka bir klasör belirtin **klasör adı ayarları**. Varsayılan ayarlarına sahip diğer seçenekleri bırakın. Klasör adı ayarları tüm sunuculara uygulanır.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. Sanal makineler geri şirket içi korunması için hazır olduğundan emin olmak için bir hazırlık denetimi çalıştırın.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Tamamlanmasını bekleyin. Başarılı bir şekilde tüm sanal makineler varsa, koruma planı için bir ad belirtebilirsiniz. Ardından **koruma** başlamak için.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. VContinuum ediyor izleyebilirsiniz.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Adımından sonra **korumayı etkinleştirme planlama** sonlandığında Site Recovery portalında VM koruma izleyebilirsiniz.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. VM tıklayarak ve ilerleme durumunu izleme tam durumunu görebilirsiniz.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Yeniden çalışma planı hazırlama
Böylece uygulama herhangi bir zamanda geri şirket içi siteye Devredilebilen vContinuum kullanarak bir yeniden çalışma planı hazırlayabilirsiniz. Bu kurtarma planları, Site Recovery kurtarma planlarında çok benzer.

1. VContinuum başlatın ve seçin **yönetmek planları** > **kurtarın.** Sanal makineleri vermesine kullanılan tüm planlar listesinin görebilirsiniz. Aynı planları kurtarmak için kullanabilirsiniz.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Koruma planı ve tüm içinde kurtarmak istediğiniz sanal makineleri seçin. Her VM seçtiğinizde, hedef ESX sunucusu ve kaynak VM disk dahil olmak üzere daha fazla ayrıntı görebilirsiniz. Tıklatın **sonraki** Kurtarma Sihirbazı'nı başlatın ve kurtarmak istediğiniz sanal makineleri seçin.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Birden çok seçenek göre kurtarabilirsiniz ancak şunu kullanmanızı öneririz **son etiket** seçip **tüm VM'ler için Uygula** sanal makineyi en son verileri kullanılacak emin olmak için.
4. Çalıştırma **hazırlık denetimi.** Bu doğru parametreleri VM kurtarmayı etkinleştirmek üzere yapılandırılıp yapılandırılmadığını denetler. Tıklatın **sonraki** tüm denetimler başarılı olursa. Aksi takdirde günlüğünü denetleyin ve hataları giderin.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. İçinde **VM Yapılandırması** kurtarma ayarlarını doğru ayarlandığından emin olun. Gerekirse VM ayarlarını değiştirebilirsiniz.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Kurtarılacak ve bir kurtarma düzeni belirtin sanal makinelerin listesini gözden geçirin. VM'ler bilgisayarın ana bilgisayar adını kullanarak listelendiğine dikkat edin. Bilgisayar konak adı sanal makineyi eşlemek zor olabilir.
   Adlarını eşlemek için sanal makinelere gidin **Pano** Azure ve onay VM konak adı.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Plan adı belirtin ve seçin **daha sonra kurtarmak**. İlk koruma tam olmayabilir daha sonra kurtarılamadı öneririz.
8. Tıklayın **kurtarmak** plan kaydetmek veya artık ve daha sonra değil kurtarmayı seçtiyseniz, Kurtarma tetiklemek için. Plan kaydedilen olmadığını görmek için kurtarma durumunu kontrol edebilirsiniz.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Sanal makineleri kurtarma
Plan oluşturulduktan sonra sanal makineleri kurtarabilirsiniz. Önce sanal makineleri eşitleme tamamladınız denetleyin. Çoğaltma durumunu Tamam gösteriyorsa koruma tamamlandıktan ve RPO eşik karşılanır. Sistem durumu VM Özellikleri'nde doğrulayabilirsiniz.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Kurtarma başlatmadan önce Azure sanal makineleri kapatmanız. Bu bölme beyin yoktur ve kullanıcıların yalnızca uygulamanın bir kopyası erişim sağlar.

1. Kaydedilen planı başlatın. VContinuum içinde seçin **İzleyici** planları. Bu çalıştırılmış tüm planlar listeler.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. Planda seçin **kurtarma** tıklatıp **Başlat**. Kurtarma izleyebilirsiniz. Sanal makineleri açtıktan sonra üzerinde onlara Vcenter'da bağlanabilir.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Azure için yeniden geri dönme sonra koruma
Yeniden çalışma tamamlandıktan sonra sanal makineleri yeniden korumaya istersiniz. Bunu şu şekilde yapabilirsiniz:

1. Sanal makineleri şirket içi çalışıyorsanız ve uygulamaların erişilebilir olduğunu denetleyin.
2. Azure Site Recovery portalında sanal makineleri seçin ve silebilirsiniz. Sanal makinelerin korumasını devre dışı bırakmak için seçin. Bu sanal makineleri artık korumalı sağlar.
3. Azure'da başarısız Azure sanal makineleri silin
4. VSpehere eski sanal makineyi silin. Bunlar, daha önce Azure için yük devredildi VM'ler aynıdır.
5. Site kurtarma Portalı'nda son devredilen sanal makineleri korur. Korumalı olup olmadıklarını sonra bir kurtarma planı ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Hakkında bilgi edinin](site-recovery-vmware-to-azure-classic.md) Gelişmiş dağıtıma kullanarak Azure'da VMware sanal makineleri ve fiziksel sunucuları çoğaltma.
