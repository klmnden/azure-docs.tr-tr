---
title: Bir Linux ana hedef sunucusu yeniden çalışma için bir şirket içi siteye yükleyin | Microsoft Docs
description: Azure Site Recovery kullanılarak Azure'da VMware vm'lerinin olağanüstü durum kurtarma sırasında bir Linux ana hedef sunucusu için bir şirket içi sitede yeniden çalışma için ayarlama konusunda bilgi edinin.
author: mayurigupta13
services: site-recovery
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: mayg
ms.openlocfilehash: bcfeca34eb11caaddac06971fe7f825a142586a2
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602074"
---
# <a name="install-a-linux-master-target-server-for-failback"></a>Bir Linux ana hedef sunucusu yeniden çalışma için yükleyin
Sanal makinelerinizi azure'a yük devretme sonra sanal makineleri şirket içi siteye geri dönebilirsiniz. Yeniden çalışma için sanal makine azure'dan şirket içi siteye yeniden korumanız gerekir. Bu işlem için trafiği almak için bir şirket içi ana hedef sunucusu gerekir. 

Bir Windows sanal makine, korumalı sanal makine ise, Windows ana hedef gerekir. Bir Linux sanal makinesi için bir Linux ana hedef gerekir. Oluşturma ve bir Linux ana hedef yükleme hakkında bilgi edinmek için aşağıdaki adımları okuyun.

> [!IMPORTANT]
> 9.10.0 sürümünden itibaren ana hedef sunucu, en son ana hedef sunucusu yalnızca bir Ubuntu 16.04 sunucusuna yüklenebilir. Yeni yüklemeler CentOS6.6 sunucuları üzerinde izin verilmez. Ancak eski ana hedef sunucularınızın 9.10.0 kullanarak yükseltmeye devam edebilirsiniz sürümü.
> Ana hedef sunucusunda LVM desteklenmiyor.

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Linux ana hedef yüklemek yönergeleri sağlanır.

Bu makalenin veya sonunda yorum veya soru gönderin [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Önkoşullar

* Ana hedef dağıtmak için konak seçmek için yeniden çalışma mevcut bir şirket içi sanal makine veya yeni bir sanal makine için kullanılacak varsa belirleyin. 
    * Mevcut bir sanal makine için ana hedef konağı sanal makinenin veri depolarında erişimi olmalıdır.
    * Şirket içi sanal makine (alternatif konuma kurtarma durumunda) mevcut değilse, yeniden çalışma sanal makine ana hedef olarak aynı ana bilgisayardaki oluşturulur. Ana hedef yüklemek için herhangi bir ESXi ana seçebilirsiniz.
* Ana hedef işlem sunucusu ve yapılandırma sunucusu ile iletişim kurabilen bir ağ üzerinde olmalıdır.
* Ana hedef sürümü, önceki sürümlerinden işlem sunucusu ve yapılandırma sunucusu veya ona eşit olmalıdır. Örneğin, yapılandırma sunucusunun sürüm 9.4 sürümünden ise, ana hedef sürümünü 9.4 sürümünden veya 9.3 ancak değil 9.5 olabilir.
* Ana hedef yalnızca bir VMware sanal makinesi ve bir fiziksel sunucu olabilir.

## <a name="sizing-guidelines-for-creating-master-target-server"></a>Ana hedef sunucu oluşturma yönergeleri boyutlandırma

Ana hedef aşağıdaki boyutlandırma yönergelerine uygun olarak oluşturun:
- **RAM**: 6 GB veya daha fazla bilgi
- **İşletim sistemi disk boyutu**: 100 GB veya daha fazla (işletim sistemi yüklemek için)
- **Bekletme sürücüsü için ek disk boyutu**: 1 TB
- **CPU çekirdekleri**: 4 çekirdek ya da daha fazla bilgi

Aşağıdaki desteklenen Ubuntu çekirdekler desteklenir.


|Çekirdek serisi  |En fazla desteği  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>Ana hedef sunucusu dağıtma

### <a name="install-ubuntu-16042-minimal"></a>Ubuntu 16.04.2 yükleme en az

Aşağıdaki adımlar Ubuntu 16.04.2 64-bit işletim sistemini yüklemek için.

1.   Git [indirme bağlantısı](http://old-releases.ubuntu.com/releases/16.04.2/ubuntu-16.04.2-server-amd64.iso), en yakın yansıtma seçin ve bir Ubuntu 16.04.2 en az 64 bit ISO indirin.
DVD sürücüsüne bir Ubuntu 16.04.2 en az 64 bit ISO tutun ve sistem başlatın.

1.  Seçin **İngilizce** olarak tercih edilen dili ve ardından **Enter**.
    
    ![Dil seç](./media/vmware-azure-install-linux-master-target/image1.png)
1. Seçin **Ubuntu sunucusu yükleme**ve ardından **Enter**.

    ![Ubuntu Server yükleme seçin](./media/vmware-azure-install-linux-master-target/image2.png)

1.  Seçin **İngilizce** olarak tercih edilen dili ve ardından **Enter**.

    ![İngilizce tercih ettiğiniz dili seçin](./media/vmware-azure-install-linux-master-target/image3.png)

1. Uygun seçeneği seçin **saat dilimi** seçenekler listesini ve ardından **Enter**.

    ![Doğru saat dilimini seçin](./media/vmware-azure-install-linux-master-target/image4.png)

1. Seçin **Hayır** (varsayılan seçenek) ve ardından **Enter**.

     ![Klavye yapılandırın](./media/vmware-azure-install-linux-master-target/image5.png)
1. Seçin **İngilizce (ABD)** ülke/bölge klavye ve seçin için kaynak olarak **Enter**.

1. Seçin **İngilizce (ABD)** klavye düzeni tıklayın ve ardından olarak **Enter**.

1. Sunucunuzun konak adı girin **Hostname** kutusuna ve ardından **devam**.

1. Bir kullanıcı hesabı oluşturmak için kullanıcı adını girin ve ardından **devam**.

      ![Bir kullanıcı hesabı oluşturun](./media/vmware-azure-install-linux-master-target/image9.png)

1. Yeni kullanıcı hesabının parolasını girin ve ardından **devam**.

1.  Yeni kullanıcı için parolayı onaylayın ve ardından **devam**.

    ![Parolalar onaylayın](./media/vmware-azure-install-linux-master-target/image11.png)

1.  Giriş dizininize şifrelemek için İleri, seçer **Hayır** (varsayılan seçenek) ve ardından **Enter**.

1. Görüntülenen saat dilimini doğruysa seçin **Evet** (varsayılan seçenek) ve ardından **Enter**. Saat diliminizi yapılandırılacağını seçin **Hayır**.

1. Bölümleme yöntemi seçenekler arasından seçim **destekli - tüm disk kullanmak**ve ardından **Enter**.

     ![Bölümleme yöntemi seçeneğini belirleyin](./media/vmware-azure-install-linux-master-target/image14.png)

1.  Uygun disk seçin **bölüme Select disk** seçenekleri ve ardından **Enter**.

    ![Disk seçin](./media/vmware-azure-install-linux-master-target/image15.png)

1.  Seçin **Evet** diske ve ardından değişiklik yazılacak **Enter**.

    ![Varsayılan seçeneği seçin](./media/vmware-azure-install-linux-master-target/image16-ubuntu.png)

1.  Yapılandırma proxy'si Seçimi'nde, varsayılan seçeneği seçin, **devam**ve ardından **Enter**.
     
     ![Yükseltmeler yönetme seçin](./media/vmware-azure-install-linux-master-target/image17-ubuntu.png)

1.  Seçin **otomatik güncelleştirme** yükseltmeleri sisteminize yönetmek için seçimi seçeneğini ve ardından **Enter**.

     ![Yükseltmeler yönetme seçin](./media/vmware-azure-install-linux-master-target/image18-ubuntu.png)

    > [!WARNING]
    > Azure Site Recovery ana hedef sunucusunda Ubuntu çok belirli bir sürümünü gerektirdiğinden, yükseltmeler sanal makine için devre dışı çekirdek emin olmanız gerekir. Etkinleştirilmişse, normal herhangi bir yükseltmeyi ana hedef sunucusunda çalışmasına neden olur. Seçtiğinizden emin olun **otomatik güncelleştirme** seçeneği.

1.  Varsayılan seçenekleri seçin. SSH bağlantısı için openSSH istiyorsanız belirleyin **OpenSSH sunucu** seçeneğini belirtin ve ardından **devam**.

    ![Yazılımını seçin](./media/vmware-azure-install-linux-master-target/image19-ubuntu.png)

1. GRUB önyükleme yükleyicisi'ni yüklemek için seçimdeki seçin **Evet**ve ardından **Enter**.
     
    ![GRUB önyükleme yükleyicisi](./media/vmware-azure-install-linux-master-target/image20.png)


1. Önyükleme yükleyicisi yükleme için uygun cihazı seçin (tercihen **/dev/sda**) ve ardından **Enter**.
     
    ![Uygun bir cihaz seçin](./media/vmware-azure-install-linux-master-target/image21.png)

1. Seçin **devam**ve ardından **Enter** yüklemeyi bitirmek için.

    ![Yüklemeyi tamamlayın](./media/vmware-azure-install-linux-master-target/image22.png)

1. Yükleme tamamlandıktan sonra VM'yi yeni kullanıcı kimlik bilgileriyle oturum açın. (Bakın **10. adımı** daha fazla bilgi için.)

1. KÖK kullanıcı parolası ayarlamak için aşağıdaki ekran görüntüsünde açıklanan adımları kullanın. Ardından kök kullanıcı olarak oturum açın.

    ![KÖK kullanıcı parolası ayarlayın](./media/vmware-azure-install-linux-master-target/image23.png)


### <a name="configure-the-machine-as-a-master-target-server"></a>Makine bir ana hedef sunucusu olarak yapılandırma

KODU SCSI sabit disklerin bir Linux sanal makinesinde almak için **disk. EnableUUID = TRUE** parametresi etkinleştirilmesi gerekir. Bu parametre etkinleştirmek için aşağıdaki adımları uygulayın:

1. Sanal makineyi kapatın.

2. Sol bölmede sanal makine için girişe sağ tıklayın ve ardından **ayarlarını Düzenle**.

3. Seçin **seçenekleri** sekmesi.

4. Sol bölmede seçin **Gelişmiş** > **genel**ve ardından **yapılandırma parametrelerini** düğme ekranın sağ alt bölümünde.

    ![Açık bir yapılandırma parametresi](./media/vmware-azure-install-linux-master-target/image24-ubuntu.png) 

    **Yapılandırma parametrelerini** seçeneği kullanılamaz makine çalışırken. Bu sekme etkin hale getirmek için sanal makineyi kapatır.

5. Bir satır olup olmadığını görmek **disk. EnableUUID** zaten mevcut.

   - Değer varsa ve ayarlanmış **False**, değere değiştirin **True**. (Değerler büyük küçük harfe duyarlı değildir.)

   - Değer varsa ve ayarlanmış **True**seçin **iptal**.

   - Değer yoksa seçin **Satır Ekle**.

   - Ad sütununda ekleme **disk. EnableUUID**ve ardından değerine **TRUE**.

     ![Olup olmadığını denetleme disk. EnableUUID zaten var.](./media/vmware-azure-install-linux-master-target/image25.png)

#### <a name="disable-kernel-upgrades"></a>Çekirdek yükseltmeleri devre dışı bırak

Azure Site Recovery ana hedef sunucusu Ubuntu belirli bir sürümünü gerektirir, çekirdek yükseltmeleri sanal makine için devre dışı olduğundan emin olun. Çekirdek yükseltme etkinse, ana hedef sunucusunda çalışmasına neden olabilir.

#### <a name="download-and-install-additional-packages"></a>Ek paketleri indirme ve yükleme

> [!NOTE]
> Ek paketler yüklemek ve indirmek için Internet bağlantısına sahip olduğunuzdan emin olun. Internet bağlantısı yoksa, el ile bu Deb paketleri bulun ve bunları yüklemeniz gerekir.

 `apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx`

### <a name="get-the-installer-for-setup"></a>Kurulumu yükleyicisi'ni edinin

Ana hedef Internet bağlantısı varsa, yükleyiciyi indirmek için aşağıdaki adımları kullanabilirsiniz. Aksi takdirde, yükleyici işlem sunucusundan kopyalayın ve yükleyin.

#### <a name="download-the-master-target-installation-packages"></a>Ana hedef yükleme paketleri indirin

[En son Linux ana hedef yükleme indirme](https://aka.ms/latestlinuxmobsvc).

Linux kullanarak yüklemek için şunu yazın:

`wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz`

> [!WARNING]
> İndirin ve yükleyici giriş dizininizde sıkıştırmasını emin olun. İçin sıkıştırmasını açın, **/usr/Local**, yükleme başarısız olur.


#### <a name="access-the-installer-from-the-process-server"></a>İşlem sunucusundan yükleyici erişim

1. İşlem sunucusu, Git **C:\Program Files (x86) recovery\home\svsystems\pushinstallsvc\repository**.

2. İşlem sunucusundan gerekli yükleyici dosyasını kopyalayın ve kaydedileceği **latestlinuxmobsvc.tar.gz** giriş dizininizde.


### <a name="apply-custom-configuration-changes"></a>Özel yapılandırma değişiklikleri Uygula

Özel yapılandırma değişiklikleri uygulamak için aşağıdaki adımları kullanın:


1. İkili untar için aşağıdaki komutu çalıştırın.

    `tar -zxvf latestlinuxmobsvc.tar.gz`

    ![Çalıştırılacak komut ekran görüntüsü](./media/vmware-azure-install-linux-master-target/image16.png)

2. İzin vermek için aşağıdaki komutu çalıştırın.

    `chmod 755 ./ApplyCustomChanges.sh`


3. Betiği çalıştırmak için aşağıdaki komutu çalıştırın.
    
    `./ApplyCustomChanges.sh`

> [!NOTE]
> Yalnızca bir kez komut dosyası sunucuda çalıştırın. Ardından sunucuyu kapatın. Bir disk ekledikten sonra sonraki bölümde açıklandığı gibi sunucuyu yeniden başlatın.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Linux ana hedef sanal makineye bir bekletme diski Ekle

Bekletme diski oluşturmak için aşağıdaki adımları kullanın:

1. Linux ana hedef sanal makineyi yeni bir 1 TB disk ekleyebilir ve sonra makineyi başlatın.

2. Kullanım **çok yollu -ll** bekletme diski çok yollu kimliği öğrenmek için komut: **çok yollu -ll**

    ![Çok yollu kimliği](./media/vmware-azure-install-linux-master-target/image27.png)

3. Sürücüyü biçimlendirmek ve ardından yeni sürücüsünde bir dosya sistemi oluşturun: **mkfs.ext4 /dev/Eşleyici/< bekletme diskin çok yollu kimliği >** .
    
    ![Dosya sistemi](./media/vmware-azure-install-linux-master-target/image23-centos.png)

4. Dosya sistemi oluşturduktan sonra bekletme diski bağlayın.

    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

5. Oluşturma **fstab** sistemin her başlatıldığında bekletme sürücüsü bağlamak için giriş.
    
    `vi /etc/fstab`
    
    Seçin **Ekle** dosyayı düzenlemeye başlayabilmesi için. Yeni bir satır oluşturun ve sonra aşağıdaki metni ekleyin. Önceki komutta alınan vurgulanan çok yollu kimliği temel disk çok yollu Kimliğini düzenleyin.

    **/dev/Eşleyici/\<bekletme diskler çok yollu kimliği >/mnt/saklama ext4 rw 0 0**

    Seçin **Esc**, Anahtar'a tıklayın ve **: wq** (yazma ve Çık) Düzenleyicisi penceresini kapatın.

### <a name="install-the-master-target"></a>Ana hedef yükleyin

> [!IMPORTANT]
> Ana hedef sunucunun sürümü, önceki sürümlerinden işlem sunucusu ve yapılandırma sunucusu veya ona eşit olmalıdır. Bu koşul karşılanmazsa, yeniden koruma başarılı, ancak çoğaltma başarısız olur.


> [!NOTE]
> Ana hedef sunucusu yüklemeden önce bu maddeyi **/etc/hosts** dosyası sanal makinede yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler içeriyor.

1. Parola öğesinden kopyalayın **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** yapılandırma sunucusunda. Olarak Kaydet **passphrase.txt** aşağıdaki komutu çalıştırarak aynı yerel dizine:

    `echo <passphrase> >passphrase.txt`

    Örnek: 

       `echo itUx70I47uxDuUVY >passphrase.txt`
    

2. Yapılandırma sunucusunun IP adresini not edin. Ana hedef sunucusu yükleme ve yapılandırma sunucusu ile sunucuyu kaydetmek için aşağıdaki komutu çalıştırın.

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Örnek: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

Betik tamamlanana kadar bekleyin. Ana hedef başarıyla kaydederse, ana hedef listelenir **Site Recovery altyapısı** portal sayfası.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Ana hedef etkileşimli kurulum kullanılarak yüklemek

1. Ana hedef yüklemek için aşağıdaki komutu çalıştırın. Aracı rolü için seçin **ana hedef**.

    ```
    ./install
    ```

2. Yükleme için varsayılan konum seçin ve ardından **Enter** devam etmek için.

    ![Ana hedef yüklemesi için bir varsayılan konumu seçme](./media/vmware-azure-install-linux-master-target/image17.png)

Yükleme tamamlandıktan sonra komut satırını kullanarak yapılandırma sunucusunu kaydedin.

1. Yapılandırma sunucusunun IP adresini not edin. Sonraki adımda ihtiyacınız.

2. Ana hedef sunucusu yükleme ve yapılandırma sunucusu ile sunucuyu kaydetmek için aşağıdaki komutu çalıştırın.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Örnek: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

     Betik tamamlanana kadar bekleyin. Ana hedef başarıyla kayıtlıysa, ana hedef listelenir **Site Recovery altyapısı** portal sayfası.


### <a name="install-vmware-tools--open-vm-tools-on-the-master-target-server"></a>VMware araçlarını yükleyin / ana hedef sunucusunda açık-vm-tools

Ana hedef veri depolarını keşfedebilmesi için VMware araçları veya açık vm araçları yüklemeniz gerekir. Yeniden koruma ekran, Araçlar yüklü değilse, veri depolarında listede yok. VMware araçları yüklendikten sonra yeniden başlatmanız gerekir.

### <a name="upgrade-the-master-target-server"></a>Ana hedef sunucusunu yükseltme

Yükleyiciyi çalıştırın. Ana hedef sunucudaki aracı yüklendiğini otomatik olarak algılar. Yükseltmek için seçin **Y**.  Kurulum tamamlandıktan sonra aşağıdaki komutu kullanarak yüklü ana hedef sürümünü denetleyin:

`cat /usr/local/.vx_version`


Göreceksiniz **sürüm** alanı ana hedef uygulamanın sürüm sayısını verir.

## <a name="common-issues"></a>Sık karşılaşılan sorunlar

* Üzerinde herhangi bir ana hedef gibi yönetim bileşenleri Storage VMotion'ı kapatmayın emin olun. Sanal makine disklerinin (Vmdk) ana hedef sonra başarılı bir yeniden koruma geçerse ayrılamıyor. Bu durumda, yeniden çalışma başarısız olur.

* Ana hedef sanal makinedeki tüm anlık görüntüleri sahip olmamalıdır. Anlık görüntüler varsa, yeniden çalışma başarısız olur.

* Özel bazı NIC yapılandırmalar nedeniyle başlatma sırasında ağ arabirimini devre dışı bırakıldı ve ana hedef Aracısı başlatılamıyor. Aşağıdaki özellikler doğru şekilde ayarlandığından emin olun. Bu özellikler, Ethernet kartı dosyanın /etc/sysconfig/network-scripts/ifcfg denetleyin-eth *.
    * BOOTPROTO dhcp =
    * ONBOOT = Evet


## <a name="next-steps"></a>Sonraki adımlar
Yükleme ve ana hedef kaydını tamamladıktan sonra görünür ana hedef gördüğünüz **ana hedef** konusundaki **Site Recovery altyapısı**, yapılandırması sunucusuna genel bakış.

Artık devam edebilirsiniz [yeniden koruma](vmware-azure-reprotect.md)geri dönme çizgidir.

