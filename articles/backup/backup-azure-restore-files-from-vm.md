---
title: 'Azure yedekleme: dosya ve klasörleri bir Azure sanal makine yedeklemesinden kurtarma'
description: Bir Azure sanal makinesi kurtarma noktasından dosya kurtarma
services: backup
author: pvrk
manager: shivamg
keywords: öğe düzeyinde kurtarma; Dosya Kurtarma Azure VM yedeklemesi; Azure VM'den dosyaları geri yükleme
ms.service: backup
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: pullabhk
ms.openlocfilehash: fecdb54af58faaf601ab74f89039a47e0d32e650
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493390"
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Azure sanal makine yedeklemesinden dosya kurtarma

Azure yedekleme, geri yükleme yeteneği sağlar [Azure sanal makineleri (VM'ler) ve diskleri](./backup-azure-arm-restore-vms.md) Azure VM yedeklerinden kurtarma noktaları olarak da bilinir. Bu makalede, bir Azure VM yedeklemesi dosyaları ve klasörleri kurtarmasına açıklanmaktadır. Dosya ve klasörleri geri yükleme, yalnızca Azure Resource Manager modeli kullanılarak dağıtılan ve bir kurtarma Hizmetleri kasası için korunan sanal makineleri için kullanılabilir.

> [!Note]
> Bu özellik, Azure Resource Manager modeli kullanılarak dağıtılan ve bir kurtarma Hizmetleri kasası için korunan sanal makineler için kullanılabilir.
> Şifrelenmiş bir VM yedeğinden dosya kurtarma desteklenmez.
>

## <a name="mount-the-volume-and-copy-files"></a>Birim ve kopyalama dosyaları bağlama

Kurtarma noktasından dosyaları veya klasörleri geri yüklemek için sanal makineye gidin ve istenen kurtarma noktası seçin.

1. Oturum [Azure portalında](http://portal.Azure.com) ve sol bölmedeki **sanal makineler**. Sanal makineler listesinden, sanal makinenin panosunu açmak için sanal makineyi seçin.

2. Sanal makinenin menüde **yedekleme** yedekleme panosunu açın.

    ![Açık kurtarma Hizmetleri kasası yedekleme öğesi](./media/backup-azure-restore-files-from-vm/open-vault-from-vm.png)

3. Yedekleme Pano menüde **dosya kurtarma** alt menüsünü açın.

    ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Gelen **kurtarma noktasını seçme** açılan menüsünde, istediğiniz dosyaları içeren kurtarma noktasını seçin. Varsayılan olarak, en son kurtarma noktası zaten seçildi.

5. Kurtarma noktasından dosyaları kopyalamak için kullanılan yazılım indirmek için tıklayın **yürütülebilir dosyayı indir** (için Windows Azure VM) veya **betiği indirin** (için Linux Azure VM).

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure, yürütülebilir veya betik yerel bilgisayara yükler.

    ![ileti yürütülebilir veya betik için indirin](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Yönetici olarak yürütülebilir veya betik çalıştırmak için indirmeyi bilgisayarınıza kaydetmek önerilir.

6. Yürütülebilir dosya veya komut parola korumalı ise ve bir parola gerektirir. İçinde **dosya kurtarma** menüsünde, parola belleğe yüklemek için Kopyala düğmesine tıklayın.

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. Yükleme konumundan (genellikle indirilenler klasörüne) yürütülebilir veya betik sağ tıklayın ve yönetici kimlik bilgileriyle çalıştırın. İstendiğinde parolayı yazın ya da parola bellekten yapıştırın ve Enter tuşuna basın. Geçerli parola girildikten sonra betik kurtarma noktasına bağlanır.

    ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/executable-output.png)

    Sınırlı erişimi olan bir bilgisayarda bir betik çalıştırırsanız, erişimi bulunduğundan emin olun:

    - download.microsoft.com
    - Kurtarma hizmeti URL'leri (Kurtarma Hizmetleri kasası bulunduğu bölgeye başvurduğu coğrafi adı)
        - <https://pod01-rec2.geo-name.backup.windowsazure.com> (Azure genel coğrafi için)
        - <https://pod01-rec2.geo-name.backup.windowsazure.cn> (Azure Çin)
        - <https://pod01-rec2.geo-name.backup.windowsazure.us> (Azure ABD kamu)
        - <https://pod01-rec2.geo-name.backup.windowsazure.de> (Azure Almanya için)
    - Giden bağlantı noktası 3260

    Linux için komut dosyası kurtarma noktasına bağlanmak için 'open-iSCSI' ve 'lshw' bileşenleri gerektirir. Bileşenleri betiğin çalıştırıldığı bilgisayarda mevcut değilse, komut dosyası bileşenleri yüklemek için izin ister. Rıza sağlamanın gerekli bileşenleri yüklemek için.

    Download.microsoft.com erişimi, betiğin çalıştırıldığı makine ve kurtarma noktası verilerinde arasında güvenli bir kanal oluşturmak için kullanılan bileşenleri yüklemek için gereklidir.

    Yedeklenen sanal makine olarak aynı (veya uyumlu) işletim sistemi olan herhangi bir makinede kod çalıştırabilirsiniz. Bkz: [uyumlu işletim sistemi tablo](backup-azure-restore-files-from-vm.md#system-requirements) uyumlu işletim sistemleri için. Korumalı bir Azure sanal makine Windows depolama alanları (Windows Azure sanal makineler için) veya LVM/RAID diziler (için Linux Vm'leri) kullanıyorsa, aynı sanal makinede yürütülebilir veya betik çalıştırılamıyor. Bunun yerine, uyumlu bir işletim sistemine sahip diğer herhangi bir makinede yürütülebilir veya betik çalıştırın.

### <a name="identifying-volumes"></a>Birimleri tanımlama

#### <a name="for-windows"></a>Windows için

Yürütülebilir dosyayı çalıştırmak, işletim sistemi yeni birimlere bağlar ve sürücü harfini atar. Bu sürücüleri göz atmak için Windows Gezgini ya da File Explorer'ı kullanabilirsiniz. Atanan birimlere sürücü harflerini özgün sanal makineyle aynı harflerini olmayabilir, ancak, birim adı korunur. Örneğin, birim özgün sanal makine üzerinde ise "veri diski (E:`\`)", toplu yerel bilgisayarda eklenebilecek "veri diski ('Herhangi bir harf':`\`). Komut çıktısında, dosya/klasör bulana kadar bahsedilen tüm birimler göz atın.  

   ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/volumes-attached.png)

#### <a name="for-linux"></a>Linux için

Linux'ta, kurtarma noktası birimleri betiğin çalıştırıldığı klasörün bağlanır. Bağlı diskleri, birimleri ve karşılık gelen yollarla bağlama uygun şekilde gösterilir. Bu yolları bağlama kök düzeyinde erişime sahip kullanıcılar tarafından görülebilir. Betik çıktısında belirtilen birimleri göz atın.

  ![Linux dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  
## <a name="closing-the-connection"></a>Bağlantı kesiliyor

Dosyalar belirleniyor ve yerel depolama konumuna kopyalayarak sonra Kaldır (veya çıkarın) ek sürücüler. Sürücüler üzerinde çıkarmak için **dosya kurtarma** Azure portalında menüsünü **diskleri çıkar**.

![Diskleri çıkarın](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Diskleri kaldırılan yaptıktan sonra size başarılı olduğunu bildiren bir ileti alırsınız. Bu bağlantının diskleri kaldırabilir miyim yenilemek birkaç dakika sürebilir.

Kurtarma noktası için bağlantı yazıyordunuz sonra Linux işletim sistemi karşılık gelen bağlama yolları otomatik olarak kaldırmaz. Bağlama yolu "artık" birimleri olarak mevcut ve görülebilir ancak, / dosyaları yazma erişimi olduğunda bir hata oluşturur. Bunlar el ile kaldırılabilir. Çalıştırdığınızda, betik, herhangi bir önceki kurtarma noktalarından varolan gibi herhangi bir birimi tanımlar ve onay temizler.

## <a name="special-configurations"></a>Özel yapılandırmalar

### <a name="dynamic-disks"></a>Dinamik diskler

Korumalı Azure VM'nin bir veya iki aşağıdaki özelliklere sahip birimler varsa, aynı sanal makinede yürütülebilir komut dosyası çalıştırılamaz.

    - (Dağıtılmış ve birimler şeritli) birden çok diske yayılma birimleri
    - Dinamik diskler hataya dayanıklı birimlerde (yansıtılmış veya RAID-5 birimler)

Bunun yerine, herhangi bir bilgisayarda uyumlu bir işletim sistemi yürütülebilir betiği çalıştırın.

### <a name="windows-storage-spaces"></a>Windows depolama alanları

Windows depolama alanları, depolama sanallaştırmanızı sağlar bir Windows teknolojisidir. Windows depolama alanları ile endüstri standardı diskleri depolama havuzları halinde gruplandırabilirsiniz. Ardından, depolama alanları olarak adlandırılan sanal diskler oluşturmak için bu depolama havuzlarında kullanılabilir alanı kullanın.

Korumalı Azure VM Windows depolama alanları kullanıyorsa, aynı sanal makinede yürütülebilir komut dosyası çalıştırılamaz. Bunun yerine, uyumlu bir işletim sistemine sahip diğer herhangi bir makinede çalıştırılabilir betiği çalıştırın.

### <a name="lvmraid-arrays"></a>LVM'yi/RAID diziler

Linux'ta, mantıksal birim Yöneticisi (LVM) ve/veya yazılım RAID diziler üzerinde birden çok disk mantıksal birimleri yönetmek için kullanılır. Korumalı Linux VM LVM ve/veya RAID diziler kullanıyorsa, aynı sanal makinede komut dosyası çalıştırılamaz. Bunun yerine uyumlu bir işletim sistemi olan diğer herhangi bir makinede betiği çalıştırmak ve korumalı VM dosya sistemini destekler.

Aşağıdaki betik çıktısı LVM ve/veya RAID diziler diskler ve birimlerle bölüm türünü görüntüler.

   ![Linux LVM çıkış menüsü](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)

Bu bölümleri çevrimiçi duruma getirmek için aşağıdaki bölümlerde komutları çalıştırın.

#### <a name="for-lvm-partitions"></a>LVM'yi bölümleri

Birim grubu adları bir fiziksel birime altında listelenecek.

```bash
#!/bin/bash
$ pvs <volume name as shown above in the script output>
```

Tüm mantıksal birimleri, adları ve yollarının bir birim grubu içinde listelemek için.

```bash
#!/bin/bash
$ lvdisplay <volume-group-name from the pvs command’s results>
```

Bağlama yolu tercih ettiğiniz mantıksal birimleri için.

```bash
#!/bin/bash
$ mount <LV path> </mountpath>
```

#### <a name="for-raid-arrays"></a>RAID diziler için

Aşağıdaki komut, tüm RAID diskler hakkındaki ayrıntıları görüntüler.

```bash
#!/bin/bash
$ mdadm –detail –scan
```

 İlgili RAID disk olarak görüntülenir `/dev/mdm/<RAID array name in the protected VM>`

RAID disk fiziksel birim varsa bağlama komutunu kullanın.

```bash
#!/bin/bash
$ mount [RAID Disk Path] [/mountpath]
```

Bunların içinde yapılandırılan başka bir LVM RAID disk varsa, ardından LVM bölümleri için yukarıdaki yordamı kullanın ancak RAID Disk adı yerine birim adı kullanın

## <a name="system-requirements"></a>Sistem gereksinimleri

### <a name="for-windows-os"></a>Windows işletim sistemi için

Aşağıdaki tabloda, sunucu ve bilgisayar işletim sistemleri uyumluluğu gösterilmektedir. Dosyaları kurtarırken, dosyaları bir önceki veya sonraki bir işletim sistemi sürümüne geri yükleyemezsiniz. Örneğin, bir dosyayı bir Windows Server 2016 sanal makinesinden Windows Server 2012 veya Windows 8 bilgisayarı geri yükleyemezsiniz. Aynı sunucu işletim sistemi veya uyumlu bir istemci işletim sistemi bir VM'den dosyaları geri yükleyebilirsiniz.

|Sunucu işletim sistemi | Uyumlu bir istemci işletim sistemi  |
| --------------- | ---- |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux-os"></a>Linux işletim sistemi için

Linux'ta, korunan sanal makinenin dosya sistemi dosyaları geri yüklemek için kullanılan bilgisayarın işletim sistemi desteklemesi gerekir. Betiği çalıştırmak için bir bilgisayar seçildiğinde, bilgisayar uyumlu bir işletim sistemine sahip ve aşağıdaki tabloda tanımlanan sürümlerinden birini kullanan emin olun:

|Linux İşletim Sistemi | Sürümler  |
| --------------- | ---- |
| Ubuntu | 12.04 ve üzeri |
| CentOS | 6.5 ve üzeri  |
| RHEL | 6.7 ve üzeri |
| Debian | 7 ve üzeri |
| Oracle Linux | 6.4 ve üzeri |
| SLES | 12 ve üzeri |
| openSUSE | 42.2 ve üzeri |

Betiği yürütün ve güvenli bir şekilde kurtarma noktasına bağlanmak için Python ve bash bileşenleri de gerektirir.

|Bileşen | Sürüm  |
| --------------- | ---- |
| Bash | 4 ve üzeri |
| python | 2.6.6 ve üzeri  |
| TLS | 1.2 desteklenir  |

## <a name="troubleshooting"></a>Sorun giderme

Sanal makinelerden dosya kurtarma sırasında sorunlarla karşılaşırsanız, ek bilgi için aşağıdaki tabloya bakın.

| Hata iletisi / senaryo | Olası neden | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Exe çıkış: *hedef bağlanan özel durumu* |Betik kurtarma noktasına erişmek mümkün değildir.    | Makine önceki erişim gereksinimlerini karşıladığı olup olmadığını denetleyin. |  
| Exe çıkış: *hedef zaten bir iSCSI oturumu oturum açıldı.* | Komut dosyasını aynı makinede zaten yürütüldü ve sürücüleri eklenmiş olması gerekir | Kurtarma noktası birimleri zaten eklenmiş olması gerekir. Bunlar orijinal VM ile aynı sürücü harflerini takılamadı değil. Dosya Gezgini'nde dosyanız için kullanılabilir tüm birimleri göz atın |
| Exe çıkış: *disklerin sınırı portal/aşıldı 12 saat çıkarıldı olduğundan, bu komut dosyası geçersiz. Portaldan yeni bir betik indirin.* |    Diskleri portalı ya da 12 saatlik sınırı aşıldı çıkarıldı | Bu belirli exe artık geçersiz ve çalıştırılamaz. Bu kurtarma noktasını zamanında dosyalara erişmek istiyorsanız, yeni bir exe için portalı ziyaret edin.|
| Exe çalıştırıldığı makinede: yeni birimlere sonra dismount düğmeye tıkladı çıkarıldı değil | Makinede iSCSI başlatıcısı, hedef bağlantı yanıt/yenileme değil ve önbellek Bakımı |    Çıkarma düğmeye basıldığında sonra için bazı dakika bekleyin. Yeni birimleri hala bağlantısı değil, lütfen tüm birimler üzerinden göz atın. Bu bağlantı yenilemek için Başlatıcı zorlar ve birim disk kullanılabilir değil bir hata iletisiyle çıkarıldı|
| Exe çıkış: Komut başarıyla çalışır ancak "Yeni birimlere bağlı" betik çıktı görüntülenmez |    Bu geçici bir hatadır    | Birimler zaten eklenmiş. Göz atmak için gezginini açın. Her komut dosyaları çalıştırmak için aynı makineye kullanıyorsanız, makinenin yeniden başlatılması göz önünde bulundurun ve sonraki exe çalıştırma listesinde görüntülenmelidir. |
| Linux özel: İstenen birimleri görüntülemek karşılaştırılamıyor | Betiğin çalıştırıldığı makinenin işletim sistemini temel alınan dosya sistemi korunan sanal makinenin algılamayabilir | Kilitlenme tutarlı veya dosya tutarlı kurtarma noktası olup olmadığını denetleyin. Dosya tutarlı, başka bir komut dosyasını çalıştırmak, işletim sistemi makine, korumalı sanal makinenin dosya sistemi tanır. |
| Windows özel: İstenen birimleri görüntülemek karşılaştırılamıyor | Diskleri eklenmiş ancak birimleri değil yapılandırılmamış | Disk yönetimi ekranında kurtarma noktası ile ilgili ek diskleri belirleyin. Bu disk, çevrimdışı olduğunda durumu diske sağ tıklayarak bunları çevrimiçi yapmayı deneyin ve 'Çevrimiçi' tıklayın|
