---
title: "Azure Premium Depolama'ya geçirme VM'ler | Microsoft Docs"
description: "Varolan Vm'lerinizi Azure Premium depolama alanına geçiş. Premium Storage, Azure sanal makinelerde çalışan g/Ç kullanımı yoğun iş yükleri için yüksek performanslı, düşük gecikmeli disk desteği sağlar."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: 9c2ac737c9f4e13b4e5e4f5dba18c7a697f6e6c3
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>Azure Premium Storage (yönetilmeyen diskleri) geçirme

> [!NOTE]
> Bu makalede, yönetilmeyen premium diskleri kullanan bir VM için yönetilmeyen standart diskler kullanan bir VM geçirme açıklanmaktadır. Azure yönetilen diskleri yeni VM'ler için kullanın ve önceki yönetilmeyen disklerinizi yönetilen Diske Dönüştür öneririz. Diskleri tanıtıcı zorunda kalmamak için temel alınan depolama hesapları yönetilen. Daha fazla bilgi için lütfen bkz bizim [yönetilen diskleri genel bakış](../../virtual-machines/windows/managed-disks-overview.md).
>

Azure Premium depolama g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Uygulamanızın VM diskleri için Azure Premium Depolama'ya geçiş yaparak hızını avantajlarından ve bu disklerin performansını alabilir.

Bu kılavuzun amacı, Premium depolama kendi geçerli sisteminden sorunsuz bir geçiş yapmak hazırlama yeni kullanıcılar Azure Premium Storage daha iyi yardımcı olmaktır. Kılavuzu üç anahtar bileşenleri bu süreçte ele alır:

* [Premium depolama geçişi planlama](#plan-the-migration-to-premium-storage)
* [Hazırlama ve sanal sabit diskleri (VHD) Premium depolama alanına kopyalama](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Premium depolama kullanarak Azure sanal makine oluşturun](#create-azure-virtual-machine-using-premium-storage)

Diğer platformlarından Azure Premium Storage Vm'leri geçirme veya var olan Azure VM'ler standart depolama biriminden Premium bir depolama alanına geçiş. Bu kılavuz, her iki iki senaryo adımları kapsar. Senaryonuza bağlı olarak ilgili bölümdeki belirtilen adımları izleyin.

> [!NOTE]
> Özelliklere genel bakış ve Premium depolama, Premium Storage fiyatlandırma bulabilirsiniz: [Azure sanal makine iş yükleri için yüksek performanslı depolama](../../virtual-machines/windows/premium-storage.md). Uygulamanız için en iyi performans için Azure Premium Depolama'ya yüksek IOPS gerektiren herhangi bir sanal makine disk geçirme öneririz. Diskinizin yüksek IOPS gerektirmiyorsa SSD yerine Sabit Disk sürücülerinin (HDD'ler) üzerinde sanal makine disk verilerini depolayan standart depolama tutarak maliyetleri sınırlayabilirsiniz.
>

Bütün geçiş işlemini tamamlama öncesinde ve sonrasında bu kılavuzda sağlanan adımları ek eylemler gerekebilir. Sanal ağlar veya uç noktaların yapılandırma veya uygulamanızda miktar kapalı kalma süresi gerektirebilecek uygulamanın kendisinden kodda değişiklikler yaparak örnek olarak verilebilir. Bu eylemler her uygulama için benzersiz ve bunları birlikte tam geçiş Premium depolama alanına mümkün olduğunca sorunsuz yapmak için bu kılavuzda sağlanan adımları tamamlamanız.

## <a name="plan-the-migration-to-premium-storage"></a>Premium depolama geçişi planlama
Bu bölümde, bu makaledeki geçiş adımları uygulamaya hazır olduğunu ve VM ve Disk türlerinde en iyi kararı vermenize yardımcı olur sağlar.

### <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliği gerekir. Yoksa, bir aylık oluşturabilirsiniz [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) abonelik veya ziyaret [Azure fiyatlandırma](https://azure.microsoft.com/pricing/) daha fazla seçenek için.
* PowerShell cmdlet'lerini çalıştırmak için Microsoft Azure PowerShell modülü gerekir. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Azure Premium depolama üzerinde çalışan Vm'leri kullanmak planlama yaparken, Premium depolama yeteneğine sahip sanal makineleri kullanmanız gerekir. Premium depolama yeteneğine sahip sanal makineleri ile standart ve Premium depolama diskleri kullanabilirsiniz. Premium depolama diskleri gelecekte daha fazla VM türleriyle kullanılabilir. Tüm kullanılabilir Azure VM disk türleri ve boyutları hakkında daha fazla bilgi için bkz: [sanal makineler için Boyutlar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Cloud Services boyutları](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Bir Azure VM uygulamalarınızı en fazla 256 TB depolama VM başına böylece birkaç Premium depolama diskleri ekleme destekler. Premium depolama ile uygulamalarınızı, ikinci disk işleme VM başına okuma işlemleri için son derece düşük gecikme ile başına VM ve 2000 MB başına 80.000 IOPS (saniye başına girdi/çıktı işlemleri) elde edebilirsiniz. VM'ler ve diskleri çeşitli seçeneğiniz vardır. Bu bölümde, iş yükünüzü uygun bir seçeneği bulmak için yardımcı olmaktır.

#### <a name="vm-sizes"></a>VM boyutları
Azure VM boyutu belirtimleri listelenen [sanal makineler için Boyutlar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Premium Storage ile birlikte çalışmak ve İş yükünüzün uygun en uygun VM boyutunu seçin sanal makineleri performans özelliklerini gözden geçirin. Yeterli bant genişliği kullanılabilir disk trafiği sürücü için de kendi VM'nizi bulunduğundan emin olun.

#### <a name="disk-sizes"></a>Disk boyutları
VM ile kullanılan diskler beş tür vardır ve her belirli IOPS ve üretilen iş sahip sınırlar. Bu sınırlar yoğun yükler ve kapasite, performans, ölçeklenebilirlik açısından, uygulamanızın gereksinimlerine, VM için disk türü seçme temel yaparken dikkate.

| Premium diskler türü  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Disk boyutu           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| Disk başına IOPS       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Disk başına aktarım hızı | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye | Saniye başına 250 MB | Saniye başına 250 MB |

İş yükünüz bağlı olarak, ek veri disklerinin VM için gerekli olup olmadığını belirleyin. VM'nize birden çok kalıcı veri disk ekleyebilirsiniz. Gerekirse, kapasite ve birim performansını artırmak için bu disklere şeritler. (Disk şeritleme görmek [burada](../../virtual-machines/windows/premium-storage-performance.md#disk-striping).) Premium depolama veri diskleri kullanarak şeritler varsa [depolama alanları][4], kullanılan her disk için bir sütun ile yapılandırmanız gerekir. Aksi takdirde, disklerde şeritli birim genel performansını trafik düzensiz dağıtım nedeniyle beklenenden daha düşük olabilir. Linux VM'ler için kullandığınız *mdadm* aynı elde etmek için yardımcı programı. Makalesine bakın [yapılandırma yazılım RAID Linux'ta](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ayrıntılar için.

#### <a name="storage-account-scalability-targets"></a>Depolama hesabımın ölçeklenebilirlik hedefleri
Premium depolama hesapları sahip ek olarak aşağıdaki ölçeklenebilirlik hedefleri [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md). Uygulamanızın gereksinimleri tek bir depolama hesabı ölçeklenebilirlik hedeflerini aşarsa, birden çok depolama hesabı kullanmak için uygulamanızı oluşturmak ve bu depolama hesaplarında verilerinizi bölüm.

| Toplam hesabı kapasitesi | Yerel olarak yedekli depolama hesabı için toplam bant genişliği |
|:--- |:--- |
| Disk kapasitesi: 35TB<br />Kapasite anlık görüntü: 10 TB |Gelen + giden için saniye başına en fazla 50 Gigabit |

Premium depolama özellikleri hakkında daha fazla bilgi için kullanıma [ölçeklenebilirlik ve performans Premium depolama kullanırken hedefleri](../../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Önbellek İlkesi disk
Varsayılan olarak, ilke önbelleğe alma disktir *salt okunur* tüm Premium veri diskleri için ve *okuma-yazma* için Premium işletim sistemi diski VM'ye eklenmiş. Bu yapılandırma ayarının uygulamanızın IOs için en iyi performans elde etmek için önerilir. (SQL Server günlük dosyaları gibi) ağır yazma ya da salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın. Mevcut veri diskleri için önbellek ayarlarını kullanarak güncelleştirilebilir [Azure Portal](https://portal.azure.com) veya *- HostCaching* parametresinin *kümesi AzureDataDisk* cmdlet'i.

#### <a name="location"></a>Konum
Azure Premium Storage kullanılabilir olduğu bir konum seçin. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumları hakkında güncel bilgi. Depoları ayrı bölge içinde olup olmadığını daha çok daha iyi performans için VM diskleri sağlayabilir depolama hesabı ile aynı bölgede yer alan VM'ler.

#### <a name="other-azure-vm-configuration-settings"></a>Diğer Azure VM yapılandırma ayarları
Bir Azure VM oluştururken, belirli VM ayarlarını yapılandırmak için istenir. Değiştirme veya başkalarının daha sonra ekleme sırasında birkaç ayarlar VM ömrü boyunca sabit unutmayın. Bu Azure VM yapılandırma ayarlarını gözden geçirin ve bunları uygun şekilde, iş yükü gereksinimlerinize uyacak şekilde yapılandırıldığından emin olun.

### <a name="optimization"></a>İyileştirme
[Azure Premium Storage: Tasarım için yüksek performanslı](../../virtual-machines/windows/premium-storage-performance.md) Azure Premium Storage kullanarak yüksek performanslı uygulamalar oluşturmak için yönergeler sağlar. Performansı en iyi yöntemler, uygulamanız tarafından kullanılan teknolojiler uygulanabilir birlikte yönergeleri takip edebilirsiniz.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Hazırlama ve sanal sabit diskleri (VHD) Premium depolama alanına kopyalama
Aşağıdaki bölümde, sanal makineden VHD'ler hazırlama ve VHD Azure depolama alanına kopyalama için yönergeler sağlar.

* [Senaryo 1: "ı mevcut Azure VM'ler Azure Premium depolama alanına geçirme."](#scenario1)
* [Senaryo 2: "ı VM'ler diğer platformlarından Azure Premium depolama alanına geçirme."](#scenario2)

### <a name="prerequisites"></a>Önkoşullar
VHD'leri geçişe hazırlamak için ihtiyacınız vardır:

* Bir Azure aboneliği, bir depolama hesabı ve bir kapsayıcıda bu depolama hesabı, VHD kopyalayabilirsiniz. Hedef depolama hesabının bir standart veya Premium depolama hesabı ihtiyacınıza olabileceğini unutmayın.
* Birden çok VM örnekleri ondan oluşturmayı planlıyorsanız, VHD'yi genelleştirmek için bir araç. Örneğin, sysprep Windows ya da Sanal Küp Str sysprep Ubuntu için.
* Depolama hesabına VHD dosyasını yüklemek için bir araç. Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) veya bir [Azure storage Gezgini](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Bu kılavuzda AzCopy aracını kullanarak, VHD kopyalama açıklanmaktadır.

> [!NOTE]
> AzCopy, zaman uyumlu kopyası seçeneğiyle seçerseniz en iyi performans için bu araçlardan biri, hedef depolama hesabının aynı bölgede bir Azure VM çalıştırarak, VHD'yi kopyalayın. Bir VHD farklı bir bölgede bir Azure VM kopyalıyorsanız performansınızı daha yavaş olabilir.
>
> Sınırlı bant genişliği üzerinde büyük miktarda veri kopyalama için göz önünde bulundurun [Blob depolama alanına veri aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma](../storage-import-export-service.md); Bu, Azure veri merkezinde sabit disk sürücülerine sevkiyat tarafından veri aktarım olanak sağlar. Yalnızca standart depolama hesabı için veri kopyalamak için Azure içeri/dışarı aktarma hizmetini kullanabilirsiniz. Standart depolama hesabınızdaki veriler olduktan sonra kullanabilirsiniz [kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) veya premium depolama hesabınıza verileri aktarmak için Azcopy'yi.
>
> Microsoft Azure yalnızca sabit boyutlu VHD dosyalarını desteklediğini unutmayın. VHDX dosyaları veya dinamik VHD desteklenmez. Dinamik VHD varsa, bunu sabit boyutlu kullanarak dönüştürebilirsiniz [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet'i.
>
>

### <a name="scenario1"></a>Senaryo 1: "ı mevcut Azure VM'ler Azure Premium depolama alanına geçirme."
Mevcut Azure sanal makineleri geçiriyorsanız ve VM'yi durdurun, VHD istediğiniz VHD türü başına hazırlayın ve VHD AzCopy veya PowerShell ile kopyalayın.

VM temiz bir duruma geçirmek için tamamen aşağı olması gerekir. Geçiş tamamlanana kadar bir kapalı kalma süresi olacaktır.

#### <a name="step-1-prepare-vhds-for-migration"></a>1. Adım VHD'ler geçiş için hazırlama
Premium depolama alanına var olan Azure sanal makineleri geçiriyorsanız, VHD olabilir:

* Genelleştirilmiş işletim sistemi görüntüsü
* Benzersiz işletim sistemi diski
* Bir veri diski

Aşağıda Biz bu 3 senaryolar için VHD hazırlama yol.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Genelleştirilmiş bir işletim sistemi VHD birden çok VM örnekleri oluşturmak için kullanın
Birden çok genel Azure VM örnekleri oluşturmak için kullanılan bir VHD yüklüyorsanız, sysprep yardımcı programını kullanarak VHD generalize gerekir. Bu, şirket içi bir VHD veya bulutta geçerlidir. Sysprep herhangi bir makineye özel bilgi VHD'den kaldırır.

> [!IMPORTANT]
> Bir anlık görüntüsünü veya genelleme önce VM'yi yedekleme. Çalışan sysprep durdurun ve VM örneği serbest bırakma. Sysprep Windows işletim sistemi VHD için aşağıdaki adımları izleyin. Sysprep komutunu çalıştırarak, sanal makine kapatılamıyor gerektireceğini unutmayın. Sysprep hakkında daha fazla bilgi için bkz: [Sysprep genel bakış](http://technet.microsoft.com/library/hh825209.aspx) veya [Sysprep Teknik Başvurusu](http://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Yönetici olarak bir komut istemi penceresi açın.
2. Sysprep açmak için aşağıdaki komutu girin:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Sistem Hazırlama aracı girin sistem Out-of-Box deneyimi (OOBE) seçin, Generalize onay kutusunu işaretleyin, **kapatma**ve ardından **Tamam**, aşağıdaki resimde gösterildiği gibi. Sysprep'i kullanarak işletim sistemini genelleştirir ve sistemi kapat.

    ![][1]

Bir Ubuntu VM için Sanal Küp Str sysprep aynı elde etmek için kullanın. Bkz: [Sanal Küp Str sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) daha fazla ayrıntı için. Ayrıca bazı açık kaynak bkz [sunucu Linux sağlama yazılım](http://www.cyberciti.biz/tips/server-provisioning-software.html) diğer Linux işletim sistemleri için.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Tek bir VM örneği oluşturmak için benzersiz bir işletim sistemi VHD kullanın
Makine belirli veri gerektiren VM üzerinde çalışan bir uygulamanız varsa, VHD'yi generalize değil. Genelleştirilmiş olmayan bir VHD benzersiz bir Azure VM örneği oluşturmak için kullanılabilir. VHD etki alanı denetleyicisi varsa, örneğin, sysprep yürütülürken, bir etki alanı denetleyicisi olarak etkisiz hale getirir. VM'nizi ve VHD genelleme önce sysprep çalışan etkisini çalışan uygulamaları gözden geçirin.

##### <a name="register-data-disk-vhd"></a>Veri diski VHD kaydetme
Veri diskleri geçirilmesi Azure'da varsa, bu veri diskleri kullanan sanal makineleri kapatmak emin olmanız gerekir.

VHD Azure Premium depolama alanına kopyalayın ve sağlanan veri diski olarak kaydetmek için aşağıda açıklanan adımları izleyin.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD için hedef oluşturma
Vhd'lerinizi korumak için bir depolama hesabı oluşturun. Vhd'lerinizi depolanacağı planlarken aşağıdaki noktaları dikkate alın:

* Premium depolama hesabı hedef.
* Depolama hesap konumu Premium depolama özellikli Azure Son aşamada oluşturacak VM'ler ile aynı olması gerekir. Yeni depolama hesabı ya da gereksinimlerinize göre aynı depolama hesabı kullanmak için plan kopyalamak.
* Kopyalayın ve bir sonraki aşaması için hedef depolama hesabının depolama hesabı anahtarı kaydedin.

Veri diskleri için bir standart depolama hesabı (örneğin, için depolama alanına sahip diskler) bazı veri diskleri tutmak seçebilirsiniz, ancak, premium depolama kullanmak, üretim iş yükü için tüm verilerin taşınması önerilir.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>3. adım. VHD AzCopy veya PowerShell ile kopyalama
Bu iki seçenekten biri işlemek için kapsayıcı yolu ve depolama hesabı anahtarınızı bulmak gerekir. Kapsayıcı yolu ve depolama hesabı anahtarı bulunabilir **Azure Portal** > **depolama**. Kapsayıcı URL "gibi https://myaccount.blob.core.windows.net/mycontainer/" olacaktır.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Seçenek 1: bir VHD AzCopy (zaman uyumsuz kopya) ile kopyalama
AzCopy kullanarak, Internet üzerinden VHD kolayca karşıya yükleyebilir. VHD'leri boyutuna bağlı olarak, bu zaman alabilir. Bu seçenek kullanıldığında depolama hesabı giriş/çıkış sınırları denetlemek unutmayın. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) Ayrıntılar için.

1. AzCopy buradan yükleyip: [en güncel AzCopy sürümünü](http://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy yüklü olduğu klasöre gidin.
3. "Kaynak" VHD dosyasına "Hedef" kopyalamak için aşağıdaki komutu kullanın.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Örnek:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    AzCopy komutta kullanılan parametreler açıklamalarını şunlardır:

   * **/ Kaynak:  *&lt;kaynak&gt;:***  klasör veya VHD'yi içeren depolama kapsayıcısı URL.
   * **/ SourceKey:  *&lt;kaynak hesap anahtarı&gt;:***  kaynağı depolama hesabının depolama hesabı anahtarı.
   * **/ Taşınmaya:  *&lt;hedef&gt;:***  VHD'ye kopyalamak için depolama kapsayıcı URL'si.
   * **/ DestKey:  *&lt;hedef hesap anahtarı&gt;:***  hedef depolama hesabının depolama hesabı anahtarı.
   * **/ Deseni:  *&lt;dosya adı&gt;:***  kopyalamak için VHD dosyasının adını belirtin.

Aracı AzCopy kullanımıyla ilgili ayrıntılar için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Seçenek 2: bir VHD PowerShell (Synchronızed kopya) ile kopyalama
Ayrıca, başlangıç AzureStorageBlobCopy PowerShell cmdlet'ini kullanarak VHD dosyasını kopyalayabilirsiniz. Aşağıdaki komutu Azure PowerShell VHD kopyalamak için kullanın. Kaynak ve hedef depolama hesabınız karşılık gelen değerlerle <> değerleri değiştirin. Bu komutu kullanmak için hedef depolama hesabınız VHD adlı bir kapsayıcı olması gerekir. Kapsayıcı yoksa, bir komut çalıştırmadan önce oluşturun.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Örnek:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Senaryo 2: "ı VM'ler diğer platformlarından Azure Premium depolama alanına geçirme."
VHD için Azure olmayan - Azure bulut depolama biriminden geçiriyorsanız, yerel bir dizine VHD ilk vermeniz gerekir. VHD kullanışlı depolandığı bir yerel dizinin tam kaynak yoluna sahip ve AzCopy kullanarak Azure Storage'a yükler.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>1. Adım VHD yerel bir dizine dışarı aktarma
##### <a name="copy-a-vhd-from-aws"></a>Bir VHD'yi AWS kopyalayın
1. AWS kullanıyorsanız, EC2 örneği bir Amazon S3 demetini VHD'yi verin. Amazon EC2 komut satırı arabirimi (CLI) aracını yükleyin ve EC2 örneği bir VHD dosyasına aktarmak için örnek verme Görev Oluştur komutu çalıştırmak Amazon EC2 örnekleri dışarı aktarma için Amazon belgelerinde açıklanan adımları izleyin. Kullandığınızdan emin olun **VHD** DISK &#95; görüntü &#95; Çalıştırırken biçimi değişkeni **-örnek-export-görev oluştur** komutu. Dışarı aktarılan VHD dosyasının bu işlem sırasında belirttiğiniz Amazon S3 demetini kaydedilir.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. VHD dosyasını S3 demetini indirin. VHD dosyasını seçip **Eylemler** > **karşıdan**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Bir VHD'yi diğer Azure olmayan buluttan kopyalayın
VHD için Azure olmayan - Azure bulut depolama biriminden geçiriyorsanız, yerel bir dizine VHD ilk vermeniz gerekir. VHD depolandığı bir yerel dizinin tam kaynak yolu kopyalayın.

##### <a name="copy-a-vhd-from-on-premises"></a>Şirket içi bir VHD'yi kopyalayın
Bir şirket içi ortamından VHD geçiriyorsanız, VHD depolandığı tam kaynak yolu gerekir. Kaynak yolu, bir sunucu konumu veya dosya paylaşımı olabilir.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD için hedef oluşturma
Vhd'lerinizi korumak için bir depolama hesabı oluşturun. Vhd'lerinizi depolanacağı planlarken aşağıdaki noktaları dikkate alın:

* Hedef depolama hesabı, standart veya premium depolama uygulama ihtiyacınıza olabilir.
* Depolama hesabı bölgesi Premium depolama özellikli Azure Son aşamada oluşturacak VM'ler ile aynı olması gerekir. Yeni depolama hesabı ya da gereksinimlerinize göre aynı depolama hesabı kullanmak için plan kopyalamak.
* Kopyalayın ve bir sonraki aşaması için hedef depolama hesabının depolama hesabı anahtarı kaydedin.

Premium depolama kullanmak, üretim iş yükü için tüm veriler hareket öneririz.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>3. Adım Azure depolama alanına VHD yükleme
Yerel dizindeki, VHD sahip olduğunuza göre .vhd dosyası Azure depolama alanına yüklemek için AzCopy veya AzurePowerShell kullanabilirsiniz. Her iki seçenek aşağıda verilmiştir:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Seçenek 1: .vhd dosyasını karşıya yüklemek için Azure PowerShell Ekle-AzureVhd kullanma

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Örnek <Uri> olabilir ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***. Örnek <FileInfo> olabilir ***"C:\path\to\upload.vhd"***.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Seçenek 2: .vhd dosyasını karşıya yüklemek için AzCopy kullanma
AzCopy kullanarak, Internet üzerinden VHD kolayca karşıya yükleyebilir. VHD'leri boyutuna bağlı olarak, bu zaman alabilir. Bu seçenek kullanıldığında depolama hesabı giriş/çıkış sınırları denetlemek unutmayın. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) Ayrıntılar için.

1. AzCopy buradan yükleyip: [en güncel AzCopy sürümünü](http://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy yüklü olduğu klasöre gidin.
3. "Kaynak" VHD dosyasına "Hedef" kopyalamak için aşağıdaki komutu kullanın.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Örnek:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    AzCopy komutta kullanılan parametreler açıklamalarını şunlardır:

   * **/ Kaynak:  *&lt;kaynak&gt;:***  klasör veya VHD'yi içeren depolama kapsayıcısı URL.
   * **/ SourceKey:  *&lt;kaynak hesap anahtarı&gt;:***  kaynağı depolama hesabının depolama hesabı anahtarı.
   * **/ Taşınmaya:  *&lt;hedef&gt;:***  VHD'ye kopyalamak için depolama kapsayıcı URL'si.
   * **/ DestKey:  *&lt;hedef hesap anahtarı&gt;:***  hedef depolama hesabının depolama hesabı anahtarı.
   * **/ BlobType: sayfa:** hedef bir sayfa blob'u olduğunu belirtir.
   * **/ Deseni:  *&lt;dosya adı&gt;:***  kopyalamak için VHD dosyasının adını belirtin.

Aracı AzCopy kullanımıyla ilgili ayrıntılar için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD karşıya yükleme için diğer seçenekleri
Bir VHD depolama hesabınıza aşağıdaki anlamına gelir birini kullanarak da yükleyebilirsiniz:

* [Azure depolama kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Depolama Gezgini karşıya BLOB'ları](https://azurestorageexplorer.codeplex.com/)
* [Depolama içeri/dışarı aktarma hizmeti REST API Başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> İçeri/dışarı aktarma hizmeti 7 günden daha uzun süre karşıya tahmini varsa kullanmanızı öneririz. Kullanabileceğiniz [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) veri boyutu ve aktarım birimi saati tahmin etmek için.
>
> İçeri/dışarı aktarma, bir standart depolama hesabına kopyalamak için kullanılabilir. Premium depolama hesabı AzCopy gibi bir araç kullanarak standart depolama biriminden kopyalamanız gerekir.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure Premium Storage kullanarak VM'ler oluşturma
VHD karşıya veya istenen depolama hesabına kopyalanır sonra VHD bir işletim sistemi görüntüsü veya işletim sistemi diski senaryonuza bağlı olarak kaydettirmek ve ondan VM örneği oluşturmak için bu bölümdeki yönergeleri izleyin. Oluşturulduktan sonra veri diski VHD VM'ye bağlı.
Örnek geçiş komut dosyası, bu bölümün sonunda sağlanır. Bu basit bir komut dosyası tüm senaryoları eşleşmiyor. Kodun belirli senaryonuza ile eşleşecek şekilde güncelleştirmeniz gerekebilir. Bu komut dosyası senaryonuz için geçerli olup olmadığını görmek için aşağıya bakın: [bir örnek geçiş komut dosyası](#a-sample-migration-script).

### <a name="checklist"></a>Denetim listesi
1. Tüm kopyalama VHD diskleri olduğu tamamlanana kadar bekleyin.
2. Premium depolama, geçiş yaptığınız bölgede kullanılabilir olduğundan emin olun.
3. Kullanacağınız yeni VM dizisi karar verin. Premium depolama özelliğine sahip olmalıdır ve boyutu bölgede kullanılabilirliğine bağlı olarak ve gereksinimlerinize göre.
4. Kullanacağınız tam VM boyutu karar verin. VM boyutu sahip veri diski sayısı desteklemek için büyük olması gerekir. Örneğin 4 veri diski varsa, VM 2 veya daha fazla çekirdek olması gerekir. Ayrıca, işlemci gücü, bellek göz önünde bulundurun ve ağ bant genişliği gerekiyor.
5. Premium depolama hesabı hedef bölgede oluşturun. Yeni VM için kullanacağı hesap budur.
6. Kullanışlı, diskler ve karşılık gelen VHD BLOB'ları listesi dahil olmak üzere geçerli VM ayrıntıları vardır.

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için geçerli sistemdeki tüm işleme durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebilirsiniz tutarlı durumuna alabilir. Kapalı kalma süresi geçirmek için diskleri veri miktarına bağlıdır.

> [!NOTE]
> Özel bir VHD diskten bir Azure Kaynak Yöneticisi'ni VM oluşturuyorsanız, lütfen [bu şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) Resource Manager mevcut diski kullanarak VM dağıtmak için.
>
>

### <a name="register-your-vhd"></a>VHD kaydetme
İşletim sistemi VHD'den bir VM oluşturun veya yeni bir sanal makineye bir veri diski eklemek için önce bunları kaydetmeniz gerekir. VHD'ler senaryo bağlı olarak aşağıdaki adımları izleyin.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>İşletim sistemi birden çok Azure VM örnekleri oluşturmak için VHD genelleştirilmiş
Genelleştirilmiş işletim sistemi görüntüsü VHD depolama hesabına yüklendikten sonra olarak kaydettirmek bir **Azure VM görüntüsü** böylece bir veya daha fazla VM örnekleri oluşturabilirsiniz. Bir Azure VM işletim sistemi görüntüsü olarak, VHD'yi kaydetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. VHD için burada kopyalanmıştır tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Kopyalayın ve bu yeni Azure VM görüntü adı kaydedin. Yukarıdaki örnek *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Tek bir Azure VM örneği oluşturmak için benzersiz işletim sistemi VHD
Depolama hesabı için benzersiz OS VHD yüklendikten sonra olarak kaydettirmek bir **Azure işletim sistemi diski** böylece VM örneği oluşturabilirsiniz. VHD Azure işletim sistemi diski kaydetmek için şu PowerShell cmdlet'lerini kullanın. VHD için burada kopyalanmıştır tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Kopyalayın ve bu yeni Azure işletim sistemi diski adı kaydedin. Yukarıdaki örnek *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>VHD yeni Azure VM örnekleri için eklenecek veri diski
Veri diski VHD depolama hesabına yüklendikten sonra yeni DS serisi için DSv2 serisi veya GS serisi Azure VM örneği eklenebilecek böylece bir Azure veri diski olarak kaydedin.

Bir Azure veri diski olarak, VHD'yi kaydetmek için şu PowerShell cmdlet'lerini kullanın. VHD için burada kopyalanmıştır tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Kopyalayın ve bu yeni Azure veri diski adını kaydedin. Yukarıdaki örnek *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Premium depolama özelliğine sahip VM oluşturma
Bir kez işletim sistemi görüntüsü veya işletim sistemi diski kayıtlı, yeni DS serisi, DSv2 serisi veya GS serisi VM oluşturun. İşletim sistemi görüntüsü veya kaydettiğiniz işletim sistemi disk adı kullanacaksınız. VM Premium depolama katmanını seçin. Aşağıdaki örnekte kullanıyoruz *Standard_DS2* VM boyutu.

> [!NOTE]
> Kapasite ve performans gereksinimlerini ve kullanılabilir Azure disk boyutlarını eşleştiğinden emin olmak için disk boyutu güncelleştirin.
>
>

Yeni bir VM oluşturmak için aşağıdaki adım adım PowerShell cmdlet'leri izleyin. İlk olarak, ortak parametreleri ayarlayın:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

İlk olarak, yeni VM'ler içinde barındıracak bir bulut hizmeti oluşturun.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Ardından, senaryonuza bağlı olarak işletim sistemi görüntüsü veya kaydettiğiniz işletim sistemi diski ' Azure VM örneği oluşturun.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>İşletim sistemi birden çok Azure VM örnekleri oluşturmak için VHD genelleştirilmiş
Kullanarak bir veya daha fazla yeni DS serisi Azure VM örnekleri oluşturmak **Azure işletim sistemi görüntüsü** , kayıtlı. Bu işletim sistemi görüntüsü adı, aşağıda gösterildiği gibi yeni bir VM oluştururken, VM yapılandırması belirtin.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Tek bir Azure VM örneği oluşturmak için benzersiz işletim sistemi VHD
Kullanarak yeni bir DS serisi Azure VM örneği oluştur **Azure işletim sistemi diski** , kayıtlı. Bu işletim sistemi diski adı VM Yapılandırması'nda yeni VM aşağıda gösterildiği gibi oluştururken belirtin.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Bulut hizmeti, bölge, depolama hesabı, kullanılabilirlik kümesi ve önbellek İlkesi gibi diğer Azure VM bilgileri belirtin. Seçili bulut hizmetini, bölge ve depolama hesabı tüm aynı konumu bu disklerin temel VHD olarak olmalıdır VM örneği ilişkili işletim sistemi veya veri diski ile birlikte bulunan olması gerektiğini unutmayın.

### <a name="attach-data-disk"></a>Veri diski ekleme
Veri diski VHD'leri kaydettiyseniz, son olarak, bunları yeni Premium depolama özellikli Azure VM'e ekleyin.

Veri diski yeni VM'e ekleyin ve önbellek ilkesi belirtmek için PowerShell cmdlet'ini kullanın. Aşağıdaki örnekte önbellek İlkesi ayarlamak *salt okunur*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Ek adımlar olabilir, uygulama desteklemek için gerekli kapsanmayan bu kılavuzda.
>
>

### <a name="checking-and-plan-backup"></a>Denetleme ve yedekleme planlama
Yeni VM çalışır durumda sonra aynı oturum açma kimliğini kullanarak erişmek ve parola özgün VM ve bu her şeyin beklendiği gibi çalıştığını doğrulayın. Şeritli birimler dahil tüm ayarlar ve yeni VM mevcut olacaktır.

Yedekleme plana son adımdır ve yeni VM için bakım zamanlaması uygulamanın gereksinimlerine göre.

### <a name="a-sample-migration-script"></a>Örnek geçiş komut dosyası
Geçirmek için birden çok VM varsa, automation PowerShell komut yardımcı olacaktır. Bir VM geçişini otomatikleştiren bir örnek betiği aşağıdadır. Not yalnızca bir örnek komut dosyası aşağıda olan ve geçerli VM diskleri hakkında yapılan birkaç varsayımlar vardır. Kodun belirli senaryonuza ile eşleşecek şekilde güncelleştirmeniz gerekebilir.

Varsayımlar şunlardır:

* Klasik Azure sanal makineleri oluşturuyorsunuz.
* Kaynak işletim sistemi diski ve kaynak veri diskleri aynı depolama hesabı ve aynı kapsayıcı olan. İşletim sistemi diskleri ve veri diskleri aynı yerde değilse, depolama hesapları ve kapsayıcıları VHD'ler kopyalamak için AzCopy ya da Azure PowerShell kullanabilirsiniz. Önceki adıma bakın: [kopyalama VHD AzCopy veya PowerShell ile](#copy-vhd-with-azcopy-or-powershell). Senaryonuz karşılamak için bu komut dosyasını düzenleme başka bir seçenektir ancak daha kolay ve hızlı olduğundan AzCopy veya PowerShell kullanmanızı öneririz.

Otomasyon betiğini aşağıda verilmiştir. Metin bilgilerinizle değiştirin ve belirli senaryonuza ile eşleşmesi için komut dosyasını güncelleştirin.

> [!NOTE]
> Varolan bir komut dosyasını kullanarak, kaynak VM ağ yapılandırması korumaz. Ağ ayarları re-config geçirilen Vm'leriniz gerekir.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>En iyi duruma getirme
Geçerli VM yapılandırmanızı, özellikle de standart diskler ile çalışmak için özelleştirilebilir. Örneğin, bir şeritli birim birçok diskleri kullanarak performansını artırmak için. Örneğin, 4 disk ayrı olarak Premium depolama kullanmak yerine, tek bir disk sağlayarak maliyeti en iyi duruma getirmek mümkün olabilir. En iyi duruma getirme bu gerek bir olay temelinde işlenmesi ve özel adımlar geçişten sonra gerektiren ister. Ayrıca, bu işlem veritabanları ve ayarları'nda tanımlanan disk düzeni kullanan uygulamalar için düzgün çalışmayabilir unutmayın.

##### <a name="preparation"></a>Hazırlama
1. Basit geçiş, önceki bölümde açıklandığı gibi tamamlayın. En iyi duruma getirme geçişten sonra yeni VM üzerinde gerçekleştirilir.
2. İçin en iyi duruma getirilmiş yapılandırma yeni disk boyutları tanımlayın.
3. Yeni disk belirtimlerine geçerli diskleri/birimlerin eşleme belirler.

##### <a name="execution-steps"></a>Yürütme adımları
1. Premium depolama VM üzerinde doğru boyutta yeni diskleri oluşturun.
2. Oturum açma verileri kopyalamanız ve VM birime eşleştiren yeni diske geçerli birimden. Yeni bir diske eşlemeniz geçerli birimler için bunu yapabilirsiniz.
3. Ardından, yeni disklere geçiş yapmak için uygulama ayarlarını değiştirin ve eski birimlerin ayırın.

Uygulama için daha iyi disk performansı ayarlamak için lütfen [uygulama performansı en iyi duruma getirme](../../virtual-machines/windows/premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Uygulama geçişleri
Veritabanları ve diğer karmaşık uygulamalar geçiş için uygulama sağlayıcısı tarafından tanımlanan özel adımlar gerektirebilir. Lütfen ilgili uygulama belgelerine başvurun. Örneğin veritabanlarını yedekleme genellikle geçirilebilecek ve geri yükleme.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineleri geçirmek için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure sanal makineleri depolama hesapları arasında geçiş](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Oluşturun ve Windows Server VHD Azure'a yükleyin.](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Oluşturma ve Linux işletim sistemini içeren bir Sanal Sabit Disk karşıya yükleme](../../virtual-machines/linux/classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Geçirme sanal makinelerden Amazon AWS Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure Storage ve Azure sanal makineler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](../../virtual-machines/windows/premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
