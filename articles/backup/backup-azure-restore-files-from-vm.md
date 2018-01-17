---
title: "Azure yedekleme: Dosya ve klasörleri Azure VM yedekten kurtarma | Microsoft Docs"
description: "Dosyaları bir Azure sanal makinesi kurtarma noktasından kurtarın"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: "öğe düzeyinde kurtarma; Dosya Kurtarma Azure VM yedekten; Azure VM geri yükleme dosyaları"
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: d1ebda145b7e355bd9763025dece742d2a23239b
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Dosyaları Azure sanal makinesi yedeklemeden Kurtar

Azure yedekleme geri yükleme yeteneği sağlar [Azure sanal makineleri (VM'ler) ve diskleri](./backup-azure-arm-restore-vms.md) olarak da bilinen geri yükleme noktaları Azure VM yedeklemelerden. Bu makalede, dosyaları ve klasörleri Azure VM yedekten kurtarma açıklanmaktadır. Dosya ve klasörleri geri yükleme, yalnızca Azure Resource Manager modelini kullanarak dağıtılmış ve bir kurtarma Hizmetleri kasası korumalı VM'ler için kullanılabilir.

> [!Note]
> Şifrelenmiş bir VM yedeğinin dosya kurtarma desteklenmez.
>

## <a name="mount-the-volume-and-copy-files"></a>Birim ve kopyalama dosyaları bağlama

Dosya ve klasörleri geri yükleme noktasından geri yüklemek için sanal makineye gidin ve geri yükleme noktası seçin. 

1. Oturum [Azure portal](http://portal.Azure.com) sol menüde tıklatıp **sanal makineleri**. Sanal makineler listesinden, bu sanal makinenin panosunu açmak için sanal makineyi seçin. 

2. Sanal makinenin menüye tıklayın **yedekleme** yedekleme panosunu açın.

    ![Açık kurtarma Hizmetleri kasasına yedekleme öğesi](./media/backup-azure-restore-files-from-vm/open-vault-from-vm.png)

3. Yedekleme Pano menüsünü tıklatın **dosya kurtarma** kendi menüsünü açın.

    ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Gelen **kurtarma noktası seçin** açılır menüsünde, kullanmak istediğiniz dosyaları içeren kurtarma noktasını seçin. Varsayılan olarak, en son kurtarma noktası zaten seçilidir.

5. Kurtarma noktasından dosya kopyalamak için kullanılan yazılım indirmek için tıklayın **karşıdan yürütülebilir** (için Windows Azure VM) veya **karşıdan yükleme komut dosyası** (için Linux Azure VM). 

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure yürütülebilir dosya veya komut dosyası yerel bilgisayara yükler.

    ![ileti yürütülebilir dosya veya komut dosyası için indirme](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Yürütülebilir dosya veya komut yönetici olarak çalıştırmak için bilgisayarınıza indirmeyi kaydedin önerilir.

6. Yürütülebilir dosya veya komut parola korumalıdır ve bir parola gerektirir. İçinde **dosya kurtarma** menüsünde parola belleğe yüklemek için Kopyala düğmesini tıklatın.

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. Yükleme konumundan (genellikle yüklemeleri klasör) yürütülebilir dosya veya komut dosyasını sağ tıklatın ve yönetici kimlik bilgileriyle çalıştırın. İstendiğinde, parolayı yazın ya da bellekten parolayı yapıştırın ve Enter tuşuna basın. Geçerli parola girildikten sonra komut dosyasını kurtarma noktasına bağlanır.

    ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/executable-output.png)

    Sınırlı erişimi olan bir bilgisayarda bir betik çalıştırırsanız, erişimi olduğundan emin olun:

    - download.microsoft.com
    - [Azure VM yedeklemeler için kullandığınız azure uç noktaları](backup-azure-arm-vms-prepare.md#establish-network-connectivity)
    - Giden bağlantı noktası 3260

    Linux için komut dosyası kurtarma noktasına bağlanmak için 'open-iSCSI' ve 'lshw' bileşenleri gerektirir. Bileşenleri betiğin çalıştırıldığı bilgisayarda mevcut değilse, komut dosyası bileşenleri yüklemek için izin ister. İzin sağlamak gerekli bileşenleri yüklemek için.
    
    Download.microsoft.com erişimi betik çalıştırdığı makine ve kurtarma noktası verileri arasında güvenli bir kanal oluşturmak için kullanılan bileşenleri yüklemek için gereklidir.         

    Yedeklenen VM olarak aynı (veya uyumlu) işletim sistemine sahip herhangi bir makinede komut dosyasını çalıştırın. Bkz: [uyumlu işletim sistemi tablo](backup-azure-restore-files-from-vm.md#system-requirements) uyumlu işletim sistemleri için. Korumalı Azure sanal makine Windows depolama alanları (için Windows Azure VM) veya LVM/RAID diziler (için Linux VM'ler) kullanıyorsa, aynı sanal makineye yürütülebilir dosya veya komut dosyası çalışamaz. Bunun yerine, uyumlu bir işletim sistemi ile diğer herhangi bir makinede yürütülebilir dosya veya komut dosyasını çalıştırın.
 

### <a name="identifying-volumes"></a>Birimleri tanımlama

#### <a name="for-windows"></a>Windows için

Yürütülebilir dosyayı çalıştırmak, işletim sistemini yeni birimlere bağlar ve sürücü harfi atar. Bu sürücüleri göz atmak için Windows Gezgini'nde veya dosya Gezgini'ni kullanın. Birimlere atanan sürücü harfleri özgün sanal makine olarak aynı harf olmayabilir, ancak, birim adı korunur. Örneğin, özgün sanal makine birimde ise "veri diski (E:`\`)", toplu yerel bilgisayarda eklenebilecek "veri diski ('Herhangi bir harf':`\`). Komut çıktısında dosya/klasör bulana kadar bahsedilen tüm birimler üzerinden göz atın.  
       
   ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>Linux için

Linux komut dosyası çalıştırdığı klasörüne kurtarma noktası birimleri bağlanır. Ekli diskleri, birimleri ve karşılık gelen bağlama yollarını buna uygun olarak gösterilir. Bu yolları bağlama kök düzeyinde erişime sahip kullanıcılar tarafından görülebilir. Komut çıktısında belirtilen birimler göz atın.

  ![Linux dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-the-connection"></a>Bağlantı kesiliyor

Dosyaları tanımlama ve yerel depolama konumuna kopyalanması sonra Kaldır (veya çıkarın) ek sürücüler. Üzerinde sürücülerin kaldırılması **dosya kurtarma** Azure portalında menüsünü **çıkarın diskleri**.

![Diskleri çıkarın](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Diskleri kaldırılan işlendikten sonra size başarılı olduğunu bildiren bir ileti alırsınız. Bağlantının diskleri kaldırabilmeniz yenilemek birkaç dakika sürebilir.

Kurtarma noktası için bağlantı zarar görmesi sonra Linux OS karşılık gelen bağlama yolları otomatik olarak kaldırmaz. Bağlama yolları "artık" birimler olarak var ve görünür ancak, erişim/yazma dosyaları olduğunda hata throw. Bunlar el ile kaldırılabilir. Çalıştırdığınızda, komut dosyası, herhangi bir önceki kurtarma noktasını varolan böyle herhangi bir birimi tanımlar ve onay temizler.

## <a name="special-configurations"></a>Özel yapılandırmalar

### <a name="dynamic-disks"></a>Dinamik diskler

Korumalı Azure VM birine veya ikisine de aşağıdaki özelliklere sahip birim varsa, aynı VM yürütülebilir komut dosyası çalıştırılamaz. 

  - (Yayılmış ve şeritli birimler) birden çok diske yayılan birimler
  - Dinamik diskler üzerindeki hataya dayanıklı birimler (yansıtılmış veya RAID-5 birimler) 

Bunun yerine, herhangi bir bilgisayarda uyumlu bir işletim sistemi ile yürütülebilir komut dosyasını çalıştırın.

### <a name="windows-storage-spaces"></a>Windows depolama alanları

Windows depolama alanları depolamayı sanallaştırmanızı olanak tanıyan bir Windows teknolojisidir. Windows depolama alanları ile endüstri standardı diskleri depolama havuzları halinde gruplandırabilirsiniz. Ardından, kullanılabilir alanı bu depolama havuzlarını depolama alanları adı verilen sanal diskler oluşturmak için kullanın.

Korumalı Azure VM Windows depolama alanları kullanıyorsa, aynı VM yürütülebilir komut dosyası çalıştırılamaz. Bunun yerine, uyumlu bir işletim sistemi ile diğer herhangi bir makinede çalıştırılabilir komut dosyasını çalıştırın.

### <a name="lvmraid-arrays"></a>LVM/RAID dizileri

Linux mantıksal birim Yöneticisi (LVM) ve/veya yazılım RAID diziler birden çok disk mantıksal birimleri yönetmek için kullanılır. Korumalı Linux VM LVM ve/veya RAID dizileri kullanıyorsa, aynı VM üzerinde komut dosyası çalıştırılamaz. Bunun yerine uyumlu bir işletim sistemi ile diğer herhangi bir makinede komut dosyasını çalıştırmak ve hangi korumalı VM dosya sistemi destekler.

Aşağıdaki komut dosyası çıktısı LVM ve/veya RAID dizileri diskleri ve birimleri bölüm türü ile görüntüler.

   ![Linux LVM çıkış menüsü](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
Bu bölümler çevrimiçi duruma getirmek için aşağıdaki bölümlerde komutları çalıştırın. 

**LVM bölümler için**

Fiziksel bir birimi altında birim grup adlarını listelemek için.
```
$ pvs <volume name as shown above in the script output> 
```
Tüm mantıksal birimleri, adlarının ve yollarının bir birim grubundaki listelemek için.

```
$ lvdisplay <volume-group-name from the pvs command’s results> 
```

Tercih ettiğiniz yolu mantıksal birimlere bağlamak için.

```
$ mount <LV path> </mountpath>
```



**RAID diziler için**

Aşağıdaki komut, tüm RAID disklerini hakkındaki ayrıntıları görüntüler.

```
$ mdadm –detail –scan
```
 İlgili RAID disk olarak görüntülenir`/dev/mdm/<RAID array name in the protected VM>`

RAID disk fiziksel birim varsa bağlama komutunu kullanın.
```
$ mount [RAID Disk Path] [/mountpath]
```

RAID disk içinde yapılandırılmış başka bir LVM varsa, sonra LVM bölümler için yukarıdaki yordamı kullanır ancak RAID Disk adı yerine birim adı kullanın

## <a name="system-requirements"></a>Sistem gereksinimleri

### <a name="for-windows"></a>Windows için

Aşağıdaki tabloda, sunucu ve bilgisayar işletim sistemleri arasındaki uyumluluk gösterir. Dosyaları kurtarırken dosyalarını bir önceki veya sonraki bir işletim sistemi sürümüne geri yükleyemezsiniz. Örneğin, bir dosyayı bir Windows Server 2016 VM'den Windows Server 2012 veya Windows 8 bilgisayarı geri yükleyemezsiniz. Aynı sunucu işletim sistemi veya uyumlu bir istemci işletim sistemi için bir sanal makineden dosyaları geri yükleyebilirsiniz.   

|Sunucu işletim sistemi | Uyumlu istemci işletim sistemi  |
| --------------- | ---- |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux"></a>Linux için

Linux dosyaları geri yüklemek için kullanılan bilgisayarın işletim sistemi dosya sistemi korunan sanal makinenin desteklemesi gerekir. Komut dosyasını çalıştırmak için bir bilgisayar seçerken, bilgisayarda uyumlu bir işletim sistemi ve aşağıdaki tabloda tanımlanan sürümlerinden birini kullanan emin olun:

|Linux İşletim Sistemi | Sürümler  |
| --------------- | ---- |
| Ubuntu | 12.04 ve üstü |
| CentOS | 6.5 ve üstü  |
| RHEL | 6.7 ve üstü |
| Debian | 7 ve üstü |
| Oracle Linux | 6.4 ve üstü |
| SLES | 12 ve üstü |
| openSUSE | 42.2 ve üstü |

Komut dosyası yürütme ve güvenli bir şekilde kurtarma noktasına bağlanmak için Python ve bash bileşenleri de gerektirir.

|Bileşen | Sürüm  |
| --------------- | ---- |
| Bash | 4 ve üstü |
| python | 2.6.6 ve üstü  |
| TLS | 1.2 desteklenmesi gereken  |

## <a name="troubleshooting"></a>Sorun giderme

Sanal makinelerden dosyalar kurtarılırken sorunlarınız varsa, ek bilgi için aşağıdaki tabloya bakın.

| Hata iletisi / senaryosu | Olası neden | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Exe çıktı: *hedef bağlanma özel durumu* |Komut dosyası kurtarma noktası erişebilir değil | Makine önceki erişim gereksinimlerine uygun olup olmadığını denetleyin. |  
|   Exe çıktı: *hedef zaten İSCSI oturumu aracılığıyla oturum açıldı.* | Komut dosyası, zaten aynı makinede yürütülen ve sürücüleri bağlı | Kurtarma noktası birimleri zaten eklenmiş. Bunlar orijinal VM ile aynı sürücü harflerini takılması değil. Kullanılabilir tüm birimleri dosyanız için dosya Gezgini'nde göz atın |
| Exe çıktı: *diskleri portal/aşıldı 12 hr sınırı çıkarılmış için bu komut dosyası geçersiz. Yeni bir betik portalından indirin.* | Diskleri portal ya da 12 hr sınırı aşıldı çıkarıldı |    Bu belirli exe artık geçersiz ve çalıştırılamaz. Bu kurtarma noktasını zaman dosyalara erişmek isterseniz yeni bir exe portalını ziyaret edin|
| Exe çalıştırdığı makinede: yeni birimleri çıkarma düğmesine tıklandığında sonra çıkarılmış değil |    İSCSI Başlatıcısı makinede değil yanıt/yenileme bağlantısı hedef için ve önbellek Bakımı |    Çıkarma düğmesine basıldığında sonra için bazı dakika bekleyin. Yeni birimlere hala çıkarılmış değil, tüm birimler üzerinden göz atın. Bu bağlantıyı yenilemek için Başlatıcı zorlar ve birim diskin kullanılabilir olmadığından bir hata iletisi ile çıkarıldı|
| Exe çıktı: komut dosyası başarıyla çalıştırıldı ancak "Yeni birimlerin bağlı" komut dosyası çıktı görüntülenmez | Bu geçici bir hatadır   | Birimleri zaten eklenmiş. Göz atmak için Explorer'ı açın. Her komut dosyaları çalıştırmak için aynı makine kullanıyorsanız, makinenin yeniden başlatılması göz önünde bulundurun ve liste sonraki exe çalıştırmalarında görüntülenmesi gerekir. |
| Linux özel: İstenen birimleri görüntülemek için | Komut dosyası çalıştırdığı makine işletim sistemini temel alınan dosya sistemi korumalı VM tanımıyor olabilir | Kurtarma noktası kilitlenme tutarlı veya dosya tutarlı olup olmadığını denetleyin. Dosya tutarlı, başka bir komut dosyasını çalıştırmak, işletim sistemi makine, korumalı VM'in filesystem tanır |
| Windows özel: İstenen birimleri görüntülemek için | Diskleri ekli ancak birimleri değil yapılandırılmamış | Disk yönetimi ekranından kurtarma noktasıyla ilgili ek diskleri belirleyin. Bu diskleri olarak çevrimdışı olan durumu çevrimiçi disk üzerinde sağ tıklayarak artırmayı deneyin ve 'Çevrimiçi' tıklayın|
