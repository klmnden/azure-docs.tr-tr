---
title: 'Azure yedekleme: Dosya ve klasörleri bir Azure sanal makine yedeklemesinden kurtarma'
description: Bir Azure sanal makinesi kurtarma noktasından dosya kurtarma
services: backup
author: pvrk
manager: shivamg
keywords: öğe düzeyinde kurtarma; Dosya Kurtarma Azure VM yedeklemesi; Azure VM'den dosyaları geri yükleme
ms.service: backup
ms.topic: conceptual
ms.date: 3/01/2019
ms.author: pullabhk
ms.openlocfilehash: 22ada6f9bb614bdc3698c58c6aa8ec3dd5def868
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240219"
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Azure sanal makine yedeklemesinden dosya kurtarma

Azure yedekleme, geri yükleme yeteneği sağlar [Azure sanal makineleri (VM'ler) ve diskleri](./backup-azure-arm-restore-vms.md) Azure VM yedeklerinden kurtarma noktaları olarak da bilinir. Bu makalede, bir Azure VM yedeklemesi dosyaları ve klasörleri kurtarmasına açıklanmaktadır. Dosya ve klasörleri geri yükleme, yalnızca Azure Resource Manager modeli kullanılarak dağıtılan ve bir kurtarma Hizmetleri kasası için korunan sanal makineleri için kullanılabilir.

> [!Note]
> Bu özellik, Azure Resource Manager modeli kullanılarak dağıtılan ve bir kurtarma Hizmetleri kasası için korunan sanal makineler için kullanılabilir.
> Şifrelenmiş bir VM yedeğinden dosya kurtarma desteklenmez.
>

## <a name="mount-the-volume-and-copy-files"></a>Birim ve kopyalama dosyaları bağlama

Kurtarma noktasından dosyaları veya klasörleri geri yüklemek için sanal makineye gidin ve istenen kurtarma noktası seçin.

1. Oturum [Azure portalında](https://portal.Azure.com) ve sol bölmedeki **sanal makineler**. Sanal makineler listesinden, sanal makinenin panosunu açmak için sanal makineyi seçin.

2. Sanal makinenin menüde **yedekleme** yedekleme panosunu açın.

    ![Açık kurtarma Hizmetleri kasası yedekleme öğesi](./media/backup-azure-restore-files-from-vm/open-vault-for-vm.png)

3. Yedekleme Pano menüde **dosya kurtarma**.

    ![Dosya Kurtarma düğmesi](./media/backup-azure-restore-files-from-vm/vm-backup-menu-file-recovery-button.png)

    **Dosya kurtarma** menüsü açılır.

    ![Dosya Kurtarma menüsü](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Gelen **kurtarma noktasını seçme** açılan menüsünde, istediğiniz dosyaları içeren kurtarma noktasını seçin. Varsayılan olarak, en son kurtarma noktası zaten seçildi.

5. Kurtarma noktasından dosyaları kopyalamak için kullanılan yazılım indirmek için tıklayın **yürütülebilir dosyayı indir** (için Windows Azure VM) veya **betiği indirin** (Linux Azure VM için bir python betiği oluşturulur).

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
        - https:\//pod01-rec2.geo-name.backup.windowsazure.com (Azure için ortak coğrafyalar)
        - https:\//pod01-rec2.geo-name.backup.windowsazure.cn (Azure Çin için)
        - https:\//pod01-rec2.geo-name.backup.windowsazure.us (için Azure ABD kamu)
        - https:\//pod01-rec2.geo-name.backup.windowsazure.de (Azure Almanya için)
    - Giden bağlantı noktası 3260

> [!Note]
> 
> * İndirilen Dosya adı olacaktır **coğrafi-name** URL'de doldurulacak. İçin örn: İndirdiğiniz betiğin adı ile başlayan \'VMname\'\_\'geoname\'_\'GUID\', ister ContosoVM_wcus_12345678...<br><br>
> * URL şu şekilde olacaktır "https:\//pod01-rec2.wcus.backup.windowsazure.com"


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

Diskleri kaldırılan yaptıktan sonra bir ileti alırsınız. Bu bağlantının diskleri kaldırabilir miyim yenilemek birkaç dakika sürebilir.

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

|Linux OS | Sürümler  |
| --------------- | ---- |
| Ubuntu | 12.04 ve üzeri |
| CentOS | 6.5 ve üzeri  |
| RHEL | 6.7 ve üzeri |
| Debian | 7 ve üzeri |
| Oracle Linux | 6.4 ve üzeri |
| SLES | 12 ve üzeri |
| openSUSE | 42.2 ve üzeri |

> [!Note]
> Dosya Kurtarma betiği SLES 12 SP4 işletim sistemi ile makinelerde çalışan bazı sorunlar bulduk. SLES ekibiyle incelemektedir.
> Şu anda, çalışır dosya kurtarma betiği SLES 12 SP2 ve SP3 işletim sistemi sürümlerine sahip makineler üzerinde çalışıyor.
>

Betiği yürütün ve güvenli bir şekilde kurtarma noktasına bağlanmak için Python ve bash bileşenleri de gerektirir.

|Bileşen | Version  |
| --------------- | ---- |
| Bash | 4 ve üzeri |
| python | 2.6.6 ve üzeri  |
| TLS | 1.2 desteklenir  |

## <a name="troubleshooting"></a>Sorun giderme

Sanal makinelerden dosya kurtarma sırasında sorunlarla karşılaşırsanız, ek bilgi için aşağıdaki tabloya bakın.

| Hata iletisi / senaryo | Olası neden | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Exe çıktısı: *Hedefine bağlanma özel durumu* |Betik kurtarma noktasına erişmek mümkün değildir.    | Makine önceki erişim gereksinimlerini karşıladığı olup olmadığını denetleyin. |  
| Exe çıktısı: *Hedef zaten bir iSCSI oturumu oturum açıldı.* | Komut dosyasını aynı makinede zaten yürütüldü ve sürücüleri eklenmiş olması gerekir | Kurtarma noktası birimleri zaten eklenmiş olması gerekir. Bunlar orijinal VM ile aynı sürücü harflerini takılamadı değil. Dosya Gezgini'nde dosyanız için kullanılabilir tüm birimleri göz atın |
| Exe çıktısı: *Bu betik, disklerin sınırı portal/aşıldı 12 saat çıkarıldı olduğundan geçersiz. Portaldan yeni bir betik indirin.* |    Diskleri portalı ya da 12 saatlik sınırı aşıldı çıkarıldı | Bu belirli exe artık geçersiz ve çalıştırılamaz. Bu kurtarma noktasını zamanında dosyalara erişmek istiyorsanız, yeni bir exe için portalı ziyaret edin.|
| Exe olduğu makinede çalıştırın: Yeni birimleri sonra dismount düğmeye tıkladı çıkarıldı değil | Makinede iSCSI başlatıcısı değil, hedef bağlantı yanıt/yenilenmesi ve önbellek bakımını yapma. |  ' I tıklattıktan sonra **çıkarmaya**, birkaç dakika bekleyin. Yeni birimleri çıkarıldı değil, tüm birimler göz atın. Başlatıcı bağlantı yenilemek için tüm birimleri tarama zorlar ve birim disk kullanılabilir değil bir hata iletisiyle çıkarıldı.|
| Exe çıktısı: Betiği başarıyla çalıştırdıktan ancak "Yeni birimlere bağlı" betik çıktı gösterilmez |    Bu geçici bir hatadır    | Birimler zaten eklenmiş. Göz atmak için gezginini açın. Her komut dosyaları çalıştırmak için aynı makineye kullanıyorsanız, makinenin yeniden başlatılması göz önünde bulundurun ve sonraki exe çalıştırma listesinde görüntülenmelidir. |
| Belirli Linux: İstenen birimleri görüntülemek karşılaştırılamıyor | Betiğin çalıştırıldığı makinenin işletim sistemini temel alınan dosya sistemi korunan sanal makinenin algılamayabilir | Kilitlenme tutarlı veya dosya tutarlı kurtarma noktası olup olmadığını denetleyin. Dosya tutarlı, başka bir komut dosyasını çalıştırmak, işletim sistemi makine, korumalı sanal makinenin dosya sistemi tanır. |
| Belirli Windows: İstenen birimleri görüntülemek karşılaştırılamıyor | Diskleri eklenmiş ancak birimleri değil yapılandırılmamış | Disk yönetimi ekranında kurtarma noktası ile ilgili ek diskleri belirleyin. Bu disk, çevrimdışı olduğunda durumu diske sağ tıklayarak bunları çevrimiçi yapmayı deneyin ve 'Çevrimiçi' tıklayın|

## <a name="security"></a>Güvenlik

Bu bölümde, kullanıcılar özelliğinin güvenlik açısından duyarlı şekilde Azure VM yedeklerinden dosya kurtarma uygulanması için gerçekleştirilen çeşitli güvenlik önlemlerini açıklanmaktadır.

### <a name="feature-flow"></a>Özellik akış

Bu özellik, tüm sanal makine veya sanal makine sağlamaya gerek kalmadan VM verilere erişmek için oluşturulmuş diskler ve en azından aşağıdaki adımlar. VM verilerine erişim (aşağıda gösterildiği gibi çalıştırdığınızda kurtarma birimi bağlar) bir komut dosyası tarafından sağlanır ve bu nedenle, tüm güvenlik uygulamalarının temel oluşturur.

  ![Güvenlik özelliği akışı](./media/backup-azure-restore-files-from-vm/vm-security-feature-flow.png)

### <a name="security-implementations"></a>Güvenlik uygulamaları

#### <a name="select-recovery-point-who-can-generate-script"></a>(Kimin betiği oluşturabilirsiniz) kurtarma noktası seçin

Betik erişim sağlayan VM verilere kimin ilk başta oluşturabilirsiniz Devletlerde önemlidir. Azure portalında oturum açmak gereken ve olması gereken [yetkili RBAC](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) betiği oluşturmak için.

Dosya Kurtarma, VM geri yükleme ve diskleri geri yükleme için gerekli olarak yetkilendirme aynı düzeyde gerekir. Diğer bir deyişle, yalnızca yetkili kullanıcılar görünümü VM verileri betiği oluşturabilirsiniz.

Oluşturulan komut dosyası için Azure Backup hizmeti ile resmi Microsoft Sertifika imzalanır. Herhangi bir komut dosyasıyla değiştirme, imza bozuk ve betiği çalıştırmak için her türlü girişim, işletim sistemi tarafından potansiyel bir risk vurgulanır anlamına gelir.

#### <a name="mount-recovery-volume-who-can-run-script"></a>(Kimin betiği çalıştırabilirsiniz) bağlama kurtarma birimi

Yalnızca yönetici sorulması yükseltilmiş modda ve komut dosyasını çalıştırabilirsiniz. Betik yalnızca önceden oluşturulmuş bir dizi adım çalışır ve herhangi bir dış kaynaktan girdisi kabul etmiyor.

Betiği çalıştırmak için bir yalnızca yetkili kullanıcıya komut oluşturma sırasında Azure portal veya PowerShell/CLI gösterilen bir parola gerektirir. Bu betik indirir yetkili bir kullanıcı da komut dosyasını çalıştırmak için sorumlu olduğu sağlamaktır.

#### <a name="browse-files-and-folders"></a>Dosya ve klasörleri Gözat

Dosya ve klasörleri göz atmak için betik makinede iSCSI başlatıcısı kullanan ve bir iSCSI hedefi olarak yapılandırılmış olan bir kurtarma noktasına bağlanır. Burada bir senaryoları biri ya da/tüm bileşenleri taklit/sızma için nerede çalışıyor varsayabilirsiniz.

Böylece her bileşenin diğer kimlik doğrulaması Karşılıklı CHAP kimlik doğrulama mekanizması kullanırız. Bu, iSCSI hedefi ve betiğin çalıştırıldığı makinede bağlanması sahte bir hedef bağlanmak sahte bir Başlatıcısı için son derece zor olduğu anlamına gelir.

Kurtarma hizmeti ile makine arasındaki veri akışını TCP üzerinden SSL güvenli bir tünelde oluşturmaya tarafından korunur ([TLS 1.2 desteklenmesi](#system-requirements) betik çalıştırıldığı makinede)

Herhangi bir dosya erişim denetimi listesi (ACL) üst/yedeklenen sanal makine mevcut korunur bağlı dosya sisteminde de.

Betik, bir kurtarma noktası için salt okunur erişim sağlayan ve 12 saat boyunca geçerlidir. Kullanıcı erişimini daha önce kaldırmak isterse, ardından Azure Portal/PowerShell/CLI ile oturum açın ve gerçekleştirmek **diskleri çıkarma** , belirli bir kurtarma noktası için. Betik hemen geçersiz kılınır.
