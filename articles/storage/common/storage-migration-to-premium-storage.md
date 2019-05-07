---
title: Azure Premium depolama Vm'leri geçirme | Microsoft Docs
description: Mevcut Vm'lerinizi Azure Premium Depolama'ya geçirin. Premium depolama, Azure sanal makinelerinde çalışan g/Ç açısından yoğun iş yükleri için yüksek performanslı, düşük gecikme süreli disk desteği sunar.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.reviewer: yuemlu
ms.subservice: common
ms.openlocfilehash: 5cfb96bd3115c8f3116a28926e93df89dff54351
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153772"
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>(Yönetilmeyen diskler) Azure Premium depolamaya geçiş

> [!NOTE]
> Bu makalede, yönetilmeyen premium diskler kullanan bir VM'ye yönetilmeyen standart diskler kullanan bir sanal Makineyi geçirmek anlatılmaktadır. Yeni VM'ler için Azure yönetilen diskler kullanın ve önceki yönetilmeyen diskleri yönetilen disklere dönüştürmeniz önerilir. Zorunda kalmamak için disklerin tanıtıcı, temel alınan depolama hesapları yönetilen. Daha fazla bilgi için lütfen bkz. bizim [yönetilen disklere genel bakış](../../virtual-machines/windows/managed-disks-overview.md).
>

Azure Premium depolama, g/Ç açısından yoğun iş yüklerini çalıştıran sanal makineler için yüksek performanslı, düşük gecikme süreli disk desteği sunar. VM diskleri, uygulamanızın Azure Premium Depolama'ya geçiş yaparak hızını avantajlarından ve bu disklerin performans alabilir.

Bu kılavuzun amacı, Premium depolama, geçerli sisteminden sorunsuz bir geçiş yapmak hazırlama yeni kullanıcıların Azure Premium Storage daha iyi yardımcı olmaktır. Kılavuz üç anahtar bileşenleri bu süreçte ele alır:

* [Premium depolamaya geçiş planlaması](#plan-the-migration-to-premium-storage)
* [Hazırlama ve sanal sabit diskleri (VHD'ler) Premium depolama alanına kopyalama](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Premium depolama kullanarak Azure sanal makine oluşturun](#create-azure-virtual-machine-using-premium-storage)

Azure Premium Depolama'ya diğer platformlardan Vm'leri geçirme veya var olan Azure Vm'lerinin standart depolamadan Premium depolamaya geçiş. Bu kılavuz, her iki iki senaryo için adımları kapsar. Senaryonuza bağlı olarak ilgili bölümdeki belirtilen adımları izleyin.

> [!NOTE]
> Özelliklere genel bir bakış ve premium SSD fiyatlandırma bulabilirsiniz: [Iaas VM'ler için bir disk türü seçin](../../virtual-machines/windows/disks-types.md#premium-ssd). Uygulamanız için en iyi performans için Azure Premium Depolama'ya yüksek IOPS gerektiren herhangi bir sanal makine disk geçiş öneririz. Yüksek IOPS, disk gerektirmiyorsa, Sabit Disk sürücülerinin (HDD'ler) yerine SSD üzerinde sanal makine disk verilerini depolayan standart depolama bulundurmak yoluyla maliyetleri sınırlayabilirsiniz.
>

Ek eylemleri okumalıdır geçiş işlemini tamamlama öncesinde ve sonrasında bu kılavuzda sağlanan adımlar gerektirebilir. Sanal ağlara veya uç noktaları yapılandırmak veya uygulamanızdaki miktar kapalı kalma süresi gerektiren uygulamanın kendisinin içindeki kod değişikliği yapmadan örneklerindendir. Bu eylemlerin her uygulama için benzersiz olan ve tam geçiş için Premium depolama mümkün olduğunca sorunsuz hale getirmek için bu kılavuzda sağlanan adımları birlikte tamamlanmalıdır.

## <a name="plan-the-migration-to-premium-storage"></a>Premium depolamaya geçiş planlaması
Bu bölümde, bu makalede geçiş adımlarında takip etmeye hazır olduğunuz ve sanal makine Disk türlerinde en iyi kararın yapmanıza yardımcı olur sağlar.

### <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliği gerekir. Yoksa, bir aylık oluşturabilirsiniz [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) abonelik veya ziyaret [Azure fiyatlandırma](https://azure.microsoft.com/pricing/) daha fazla seçenek için.
* PowerShell cmdlet'lerini çalıştırmak için Microsoft Azure PowerShell modülü gerekir. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Azure Premium depolama alanında çalışan sanal makineleri kullanmak planlama yaparken, Premium depolama özellikli VM'ler kullanmanız gerekir. Premium depolama ile uyumlu sanal makineleri, hem standart hem de Premium depolama diskleri kullanabilirsiniz. Premium depolama diskleri gelecekte daha fazla VM türleri ile kullanılabilir. Tüm kullanılabilir Azure VM disk türleri ve boyutları hakkında daha fazla bilgi için bkz. [sanal makine boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Cloud Services boyutları](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Bir Azure sanal makinesi, böylece uygulamalarınız VM başına depolama alanı en fazla 256 TB birkaç Premium depolama diskleri eklemeyi destekler. Premium depolama sayesinde, uygulamalarınızı, disk aktarım hızı VM başına okuma işlemleri için oldukça düşük gecikme süreleriyle her VM ile 2.000 MB başına 80.000 IOPS (saniyede giriş/çıkış işlemi) elde edebilirsiniz. VM'ler ve diskleri çeşitli seçenekleri var. Bu bölümde, iş yükü gereksinimlerinize uygun bir seçeneği bulmak için yardımcı olmaktır.

#### <a name="vm-sizes"></a>VM boyutları
Azure VM boyutu belirtimleri listelenen [sanal makine boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Premium depolama ile çalışır ve iş yükünüz gereksinimlerinize en uygun bir VM boyutu seçme sanal makineleri performans özelliklerini gözden geçirin. Kullanılabilir yeterli bant genişliği, VM diski trafiği yönlendirmek emin olun.

#### <a name="disk-sizes"></a>Disk boyutları
VM'nizi ile kullanılabilen diskler beş türde vardır ve her belirli IOPS ve aktarım hızı sahip sınırları. Bu limitler kapasite, performans, ölçeklenebilirlik açısından uygulamanızın ihtiyaçlarını temel VM'niz için disk türünü seçme ve en yüksek yükler yaparken dikkate.

| Premium disk türü  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Disk boyutu           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| Disk başına IOPS       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Disk başına aktarım hızı | Saniye başına 100 MB | 150 MB / saniye | Saniye başına 200 MB | Saniye başına 250 MB | Saniye başına 250 MB |

İş yükünüze bağlı olarak, ek veri diskleri sanal Makineniz için gerekli olup olmadığını belirler. Sanal makinenizde birden fazla kalıcı veri diskleri ekleyebilirsiniz. Gerekirse, kapasite ve birimin performansını artırmak için disklerde stripe. (Disk bölümleme türüyle neler olacağıyla [burada](../../virtual-machines/windows/premium-storage-performance.md#disk-striping).) Premium depolama diskleri kullanarak stripe varsa [depolama alanları][4], kullanılan her disk için tek bir sütunu yapılandırmanız gerekir. Aksi takdirde, disklerde şeritli birim genel performansını trafiği düzensiz şekilde dağıtılmasının nedeni beklenenden daha düşük olabilir. Linux Vm'leri için kullanabileceğiniz *mdadm* aynı şeyi elde etmek için yardımcı program. Makalesine bakın [Linux'ta yazılım RAID yapılandırma](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ayrıntılar için.

#### <a name="storage-account-scalability-targets"></a>Depolama hesabı ölçeklenebilirlik hedefleri
Premium depolama hesaplarına sahip ek olarak aşağıdaki ölçeklenebilirlik hedefleri [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md). Uygulama gereksinimlerinize tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı ve verilerinizi bu depolama hesabı arasında bölüm.

| Toplam hesabı kapasitesi | Yerel olarak yedekli depolama hesabı için toplam bant genişliği |
|:--- |:--- |
| Disk kapasitesi: 35TB<br />Anlık görüntü kapasitesi: 10 TB |En çok 50 Gigabit / saniye için gelen ve giden |

Premium depolama özellikleri hakkında daha fazla bilgi için kullanıma [Azure depolama ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md#premium-performance-storage-account-scale-limits).

#### <a name="disk-caching-policy"></a>Diski önbelleğe alma İlkesi
Varsayılan olarak, önbelleğe alma İlkesi disktir *salt okunur* tüm Premium veri disklerinde, ve *okuma-yazma* Premium işletim sistemi diski, VM'ye bağlı. Bu yapılandırma ayarının, uygulamanızın IOs için en iyi performans elde etmek için önerilir. (Örneğin, SQL Server günlük dosyası) yazma yoğunluklu veya salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın. Mevcut veri diskleri için önbellek ayarlarını kullanarak güncelleştirilebilir [Azure portalında](https://portal.azure.com) veya *- HostCaching* parametresinin *kümesi AzureDataDisk* cmdlet'i.

#### <a name="location"></a>Location
Azure Premium depolama kullanılabilir olduğu bir konum seçin. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumların hakkında güncel bilgi için. Depoları diskleri VM için ayrı bölge içinde olup olmadıklarını daha çok daha iyi performans sağlayabilir, depolama hesabıyla aynı bölgede yer alan VM'ler.

#### <a name="other-azure-vm-configuration-settings"></a>Diğer Azure VM yapılandırma ayarları
Bir Azure VM oluştururken, bazı VM ayarlarını yapılandırmak için istenecektir. Unutmayın, değiştirebilir veya başkalarının daha sonra ekleme sırasında birkaç ayarlar VM'nin ömrü boyunca sabittir. Bu Azure VM yapılandırma ayarlarını gözden geçirin ve bunları uygun şekilde, iş yükü gereksinimlerinize uyacak şekilde yapılandırıldığından emin olun.

### <a name="optimization"></a>İyileştirme
[Azure Premium Depolama: Yüksek performans tasarımı](../../virtual-machines/windows/premium-storage-performance.md) Azure Premium depolama kullanan yüksek performanslı uygulamalar oluşturmak için yönergeler sağlar. Performansı en iyi uygulamaları, uygulamanız tarafından kullanılan teknolojiler için geçerli bir araya geldiğinde yönergeleri izleyebilirsiniz.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Hazırlama ve sanal sabit diskleri (VHD'ler) Premium depolama alanına kopyalama
Aşağıdaki bölümde, VM'den VHD hazırlama ve Azure Depolama'ya VHD'lerin kopyalanması için yönergeler sağlar.

* [Senaryo 1: "I var olan Azure Vm'lerinin Azure Premium Depolama'ya geçiriyorum."](#scenario1)
* [Senaryo 2: "I Vm'leri diğer platformlardan Azure Premium Depolama'ya geçiriyorum."](#scenario2)

### <a name="prerequisites"></a>Önkoşullar
VHD'ler geçişe hazırlamak için gerekir:

* Bir Azure aboneliği, bir depolama hesabı ve VHD'nizi kopyalamak bu depolama hesabında bir kapsayıcı. Hedef depolama hesabını gereksinimlerinize bağlı olarak standart veya Premium depolama hesabı olabileceğini unutmayın.
* Birden fazla VM örneği oluşturmayı planlıyorsanız, VHD'ye genelleştirmek için bir araç. Örneğin, sysprep Windows veya Ubuntu için vırt-sysprep.
* Depolama hesabına VHD dosyası yüklemek için bir araç. Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) veya bir [Azure Depolama Gezgini](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Bu kılavuzda, AzCopy Aracı'nı kullanarak VHD'nizi kopyalama açıklanır.

> [!NOTE]
> AzCopy, zaman uyumlu kopyası seçeneği seçerseniz, en iyi performans için hedef depolama hesabıyla aynı bölgede olan bir Azure VM'den Bu araçlardan birini çalıştırarak VHD'nizi kopyalayın. Farklı bir bölgede bir Azure VM'den bir VHD kopyalıyorsanız performans daha yavaş olabilir.
>
> Sınırlı bant genişliği üzerinde büyük miktarda veri kopyalamak için göz önünde bulundurun [Blob depolama alanına veri aktarmak için Azure içeri/dışarı aktarma hizmetini kullanarak](../storage-import-export-service.md); bu bir Azure veri merkezine sabit disk sürücüleri sevkiyat tarafından verilerinizi aktarmanıza izin veriyor. Azure içeri/dışarı aktarma hizmeti yalnızca standart depolama hesabı için veri kopyalamak için kullanabilirsiniz. Standart depolama hesabınızdaki verilerin hale geldikten sonra kullanabilirsiniz [kopyalama Blob API'sine](https://msdn.microsoft.com/library/azure/dd894037.aspx) veya premium depolama hesabınıza veri aktarmak için AzCopy.
>
> Microsoft Azure yalnızca sabit boyutlu VHD dosyalarını desteklediğini unutmayın. VHDX dosyası veya dinamik VHD'ler desteklenmez. Dinamik VHD'yi varsa, bunu sabit boyutlu kullanarak dönüştürebilirsiniz [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) cmdlet'i.
>
>

### <a name="scenario1"></a>Senaryo 1: "I var olan Azure Vm'lerinin Azure Premium Depolama'ya geçiriyorum."
Var olan Azure sanal makineleri geçiriyorsanız, sanal Makineyi durdurun, istediğiniz VHD türü başına VHD hazırlama ve ardından VHD'yi AzCopy veya PowerShell ile kopyalayın.

VM, temiz bir duruma geçirmek için tamamen aşağı olması gerekiyor. Geçiş tamamlanana kadar bir süre kapalı olacaktır.

#### <a name="step-1-prepare-vhds-for-migration"></a>1. Adım Geçiş için VHD hazırlama
Mevcut Azure Vm'leri Premium depolama alanına geçiriyorsanız, VHD'nizi olabilir:

* Genelleştirilmiş işletim sistemi görüntüsü
* Benzersiz bir işletim sistemi diski
* Veri diski

Aşağıda VHD'nizi hazırlamaya için 3 şu senaryonun üzerinden inceleyeceğiz.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Genelleştirilmiş işletim sistemi VHD birden çok VM örnekleri oluşturmak için kullanın
Birden çok genel Azure VM örnekleri oluşturmak için kullanılan VHD yüklüyorsanız, önce sysprep yardımcı programını kullanarak VHD generalize gerekir. Bu, bulutta veya şirket içi bir VHD için geçerlidir. Sysprep tüm makineye özgü bilgileri VHD'den kaldırır.

> [!IMPORTANT]
> Bir anlık görüntüsünü alın veya genelleştiriliyor önce sanal Makineyi yedekleyin. Çalışan sysprep durdurun ve sanal makine örneği serbest bırakın. Sysprep Windows işletim sistemi VHD'si için aşağıdaki adımları izleyin. Sysprep komutu çalıştırılıyor sanal makineyi kapatmanız gerekir olduğunu unutmayın. Sysprep hakkında daha fazla bilgi için bkz. [Sysprep genel bakış](https://technet.microsoft.com/library/hh825209.aspx) veya [Sysprep teknik başvuru](https://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Yönetici olarak bir komut istemi penceresi açın.
2. Sysprep açmak için aşağıdaki komutu girin:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Sistem hazırlığı aracı, sistem girin kullanıma hazır deneyimi (OOBE) seçin, Generalize onay kutusunu işaretleyin, **kapatma**ve ardından **Tamam**, aşağıdaki resimde gösterildiği gibi. Sysprep kullanarak işletim sistemini genelleştirir ve sistemi kapat.

    ![][1]

Bir Ubuntu sanal makinesi için sysprep vırt aynı şeyi elde etmek için kullanın. Bkz: [vırt sysprep](https://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) daha fazla ayrıntı için. Açık kaynak bazıları için Ayrıca bkz: [Linux sunucu sağlama yazılım](https://www.cyberciti.biz/tips/server-provisioning-software.html) diğer Linux işletim sistemleri için.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Tek bir VM örneği oluşturmak için benzersiz bir işletim sistemi VHD'si kullanın
Makine belirli veri gerektiren bir VM üzerinde çalışan bir uygulama varsa, VHD'yi generalize değil. Olmayan genelleştirilmiş VHD, benzersiz bir Azure sanal makine örneği oluşturmak için kullanılabilir. VHD'nizi etki alanı denetleyicisi varsa, örneğin, sysprep yürütülürken, bir etki alanı denetleyicisi olarak etkisiz bulabilmesini sağlar. VM'nizi ve VHD genelleme yapmadan önce aşağıdaki sysprep çalışan etkisini üzerinde çalışan uygulamalar gözden geçirin.

##### <a name="register-data-disk-vhd"></a>Veri diski VHD kaydetme
Azure'da geçirilmesi için veri diskleri varsa, bu veri diski kullanan VM'ler kapatıldığından emin olmanız gerekir.

Azure Premium Depolama'ya VHD'yi kopyalayın ve sağlanan veri diski olarak kaydetmek için aşağıda açıklanan adımları izleyin.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD'nizi hedefi oluşturma
Vhd'lerinizi sürdürmek için bir depolama hesabı oluşturun. Vhd'lerinizi depolanacağı planlarken aşağıdaki noktaları göz önünde bulundurun:

* Premium depolama hesabı hedef.
* Depolama hesabı konumu, Premium Storage özellikli Azure son aşamasında oluşturacağınız Vm'lerini aynı olmalıdır. Yeni depolama hesabı veya gereksinimlerinize göre aynı depolama hesabı kullanılacak planı kopyalanamadı.
* Kopyalayın ve sonraki aşama için hedef depolama hesabının depolama hesabı anahtarını kaydedin.

Veri diskleri için standart depolama hesabı (örneğin, harika bir depolama alanına sahip diskleri) bazı veri disklerini korumak seçebilirsiniz, ancak, premium depolama kullanmak, üretim iş yükü için tüm veri taşıma öneririz.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>3. adım. AzCopy veya PowerShell ile VHD'yi kopyalayın
Bu iki seçeneği işlemek için kapsayıcı yolu ve depolama hesabı anahtarınızı bulmanız gerekir. Kapsayıcı yolu ve depolama hesabı anahtarını bulunabilir **Azure portalı** > **depolama**. URL gibi olacaktır kapsayıcı "https:\//myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>1. seçenek: AzCopy (zaman uyumsuz kopya) ile bir VHD'yi kopyalayın
AzCopy kullanarak, Internet üzerinden VHD kolayca karşıya yükleyebilir. VHD'ler boyutuna bağlı olarak, bu zaman alabilir. Bu seçenek kullanıldığında depolama hesabı giriş/çıkış sınırları iade etmeyi unutmayın. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) Ayrıntılar için.

1. İndirin ve AzCopy buradan yükleyin: [En güncel AzCopy sürümünü](https://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy yüklü olduğu klasöre gidin.
3. "Hedef" için "Kaynak" VHD dosyasından kopyalamak için aşağıdaki komutu kullanın.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Örnek:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    AzCopy komutta kullanılan parametreler açıklamaları aşağıda verilmiştir:

   * **/ Kaynak:  *&lt;kaynak&gt;:*** VHD içeren bir depolama kapsayıcısı URL'si ve klasör konumu.
   * **/ SourceKey:  *&lt;kaynağı hesap anahtarı&gt;:*** Kaynak depolama hesabının depolama hesabı anahtarı.
   * **/ Hedef:  *&lt;hedef&gt;:*** Depolama kapsayıcısı URL'si için VHD'yi kopyalayın.
   * **/ DestKey:  *&lt;hedef hesap anahtarı&gt;:*** Hedef depolama hesabının depolama hesabı anahtarı.
   * **/ Desen:  *&lt;dosya adı&gt;:*** Kopyalamak için VHD dosya adı belirtin.

Aracı AzCopy kullanma hakkında bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>2. seçenek: PowerShell (Synchronızed kopya) ile bir VHD'yi kopyalayın

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Ayrıca, başlangıç AzStorageBlobCopy PowerShell cmdlet'ini kullanarak VHD dosyasını kopyalayabilirsiniz. Aşağıdaki komut, VHD kopyalamak için Azure PowerShell kullanın. <> Değerleri, kaynak ve hedef depolama hesabınızdan karşılık gelen değerlerle değiştirin. Bu komutu kullanmak için hedef depolama hesabınız VHD adlı bir kapsayıcı olması gerekir. Komutu çalıştırmadan önce bir kapsayıcı mevcut değilse oluşturun.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Örnek:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Senaryo 2: "I Vm'leri diğer platformlardan Azure Premium Depolama'ya geçiriyorum."
VHD olmayan - Azure bulut depolama alanından Azure'a taşıyorsanız, VHD'yi yerel bir dizine ilk vermeniz gerekir. VHD kullanışlı depolandığı bir yerel dizinin tam kaynak yoluna sahip ve AzCopy kullanarak Azure Depolama'ya yükler.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>1. Adım VHD'yi yerel bir dizine dışarı aktarma
##### <a name="copy-a-vhd-from-aws"></a>AWS'den bir VHD'yi kopyalayın
1. AWS kullanıyorsanız, EC2 örneği bir Amazon S3 demetini VHD olarak dışarı aktarın. Amazon EC2 komut satırı arabirimi (CLI) aracı yükleme ve EC2 örneği bir VHD dosyasına aktarmak için örnek dışarı aktarma Görevi Oluştur komutu çalıştırmak Amazon EC2 örneklerinin dışarı aktarma için Amazon belgelerinde açıklanan adımları izleyin. Kullandığınızdan emin olun **VHD** diskin&#95;görüntü&#95;çalıştırırken biçimi değişkeni **oluşturma-örnek-dışarı aktarma-görev** komutu. Dışarı aktarılan VHD dosyası bu işlem sırasında belirlediğiniz Amazon S3 demetini kaydedilir.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. VHD dosyasını S3 demetini ' indirin. VHD dosyasını seçip **eylemleri** > **indirme**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Diğer Azure dışı buluttan bir VHD'yi kopyalayın
VHD olmayan - Azure bulut depolama alanından Azure'a taşıyorsanız, VHD'yi yerel bir dizine ilk vermeniz gerekir. VHD depolandığı yerel dizinin tam kaynak yolu kopyalayın.

##### <a name="copy-a-vhd-from-on-premises"></a>Şirket içinden bir VHD'yi kopyalayın
VHD bir şirket içi ortamdan geçiş yapıyorsanız, VHD'nin depolandığı tam kaynak yolu gerekir. Kaynak yolu bir konum veya dosya paylaşımı olabilir.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD'nizi hedefi oluşturma
Vhd'lerinizi sürdürmek için bir depolama hesabı oluşturun. Vhd'lerinizi depolanacağı planlarken aşağıdaki noktaları göz önünde bulundurun:

* Hedef depolama hesabı, standart veya premium depolama, uygulama gereksinim bağlı olabilir.
* Depolama hesabı bölgesini Premium Storage özellikli Azure son aşamasında oluşturacağınız Vm'lerini aynı olmalıdır. Yeni depolama hesabı veya gereksinimlerinize göre aynı depolama hesabı kullanılacak planı kopyalanamadı.
* Kopyalayın ve sonraki aşama için hedef depolama hesabının depolama hesabı anahtarını kaydedin.

Premium depolama kullanmak, üretim iş yükü için tüm verileri hareket öneririz.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>3. Adım VHD Azure depolamaya yükleme
Yerel dizinde VHD'nizi sahip olduğunuza göre Azure Depolama'ya .vhd dosyasını yüklemek için AzCopy veya AzurePowerShell kullanabilirsiniz. Her iki seçenek aşağıda verilmiştir:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>1. seçenek: .Vhd dosyasını karşıya yüklemek için Azure PowerShell Add-AzureVhd'ı kullanma

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Bir örnek \<URI > olabilir ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***. Bir örnek \<FileInfo > olabilir ***"C:\path\to\upload.vhd"***.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>2. seçenek: .Vhd dosyasını karşıya yüklemek için AzCopy kullanarak
AzCopy kullanarak, Internet üzerinden VHD kolayca karşıya yükleyebilir. VHD'ler boyutuna bağlı olarak, bu zaman alabilir. Bu seçenek kullanıldığında depolama hesabı giriş/çıkış sınırları iade etmeyi unutmayın. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) Ayrıntılar için.

1. İndirin ve AzCopy buradan yükleyin: [En güncel AzCopy sürümünü](https://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy yüklü olduğu klasöre gidin.
3. "Hedef" için "Kaynak" VHD dosyasından kopyalamak için aşağıdaki komutu kullanın.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Örnek:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    AzCopy komutta kullanılan parametreler açıklamaları aşağıda verilmiştir:

   * **/ Kaynak:  *&lt;kaynak&gt;:*** VHD içeren bir depolama kapsayıcısı URL'si ve klasör konumu.
   * **/ SourceKey:  *&lt;kaynağı hesap anahtarı&gt;:*** Kaynak depolama hesabının depolama hesabı anahtarı.
   * **/ Hedef:  *&lt;hedef&gt;:*** Depolama kapsayıcısı URL'si için VHD'yi kopyalayın.
   * **/ DestKey:  *&lt;hedef hesap anahtarı&gt;:*** Hedef depolama hesabının depolama hesabı anahtarı.
   * **/ BlobType: sayfa:** Hedef sayfa blobu olduğunu belirtir.
   * **/ Desen:  *&lt;dosya adı&gt;:*** Kopyalamak için VHD dosya adı belirtin.

Aracı AzCopy kullanma hakkında bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD'yi karşıya yüklemek için diğer seçenekler
Depolama hesabınızda aşağıdaki yollardan birini kullanarak bir VHD da karşıya yükleyebilirsiniz:

* [Azure depolama kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Depolama Gezgini Blobları karşıya yükleme](https://azurestorageexplorer.codeplex.com/)
* [Depolama içeri/dışarı aktarma hizmeti REST API Başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> İçeri/dışarı aktarma hizmeti, 7 günden uzun süre karşıya tahmin kullanmanızı öneririz. Kullanabileceğiniz [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) veri boyutu ve aktarım birimi saati tahmin etmek için.
>
> İçeri/dışarı aktarma, bir standart depolama hesabına kopyalamak için kullanılabilir. AzCopy gibi bir araç kullanarak premium depolama hesabı için standart depolama alanından kopyalamanız gerekir.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure Premium depolama kullanan sanal makineler oluşturun
VHD'yi karşıya veya istenen depolama hesabına kopyalanır sonra VHD'yi bir işletim sistemi görüntüsü veya işletim sistemi diski senaryonuza bağlı olarak kaydedin ve bundan bir VM örneği oluşturmak için bu bölümdeki yönergeleri uygulayın. Oluşturulduktan sonra VM'ye veri diski VHD eklenebilir.
Örnek geçiş betiği bu bölümün sonunda verilmektedir. Bu basit bir betik, tüm senaryolarda eşleşmiyor. Betik kendi senaryonuza ile eşleşecek şekilde güncelleştirmeniz gerekebilir. Bu betik, senaryonuz için geçerli olup olmadığını görmek için aşağıya bakın: [bir betiğe geçiş](#a-sample-migration-script).

### <a name="checklist"></a>Denetim listesi
1. Tüm kopyalama VHD diskler, tamamlanana kadar bekleyin.
2. Premium depolama, için geçirdiğiniz bölgede kullanılabilir olduğundan emin olun.
3. Kullanacağınız yeni VM serisi karar verin. Bir Premium depolama özelliğine sahip olmalıdır ve boyutu bölgede kullanılabilirliğine bağlı olarak ve gereksinimlerinize göre.
4. Kullanacağınız tam olarak VM boyutuna karar verin. VM boyutu, sahip olduğunuz veri diski sayısı destekleyecek kadar büyük olması gerekiyor. Örneğin VM, 4 veri diskleri varsa, 2 veya daha fazla çekirdek olması gerekir. Ayrıca, işleme gücü, bellek göz önünde bulundurun ve ağ bant genişliği gerekiyor.
5. Bir Premium depolama hesabı, hedef bölgede oluşturun. Bu yeni VM için kullanacağı hesaptır.
6. Geçerli sanal makine ayrıntıları kullanışlı, diskler ve karşılık gelen VHD bloblarını listesi dahil olmak üzere vardır.

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için tüm işlemlerin geçerli sistemde durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebileceğiniz tutarlı duruma alabilirsiniz. Kapalı kalma süresi geçirmek için disklerde veri miktarına bağlı olacaktır.

> [!NOTE]
> Özel bir VHD diskten bir Azure Resource Manager sanal makine oluşturuyorsanız, edinmek [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) Resource Manager var olan bir diski kullanarak VM dağıtmak için.
>
>

### <a name="register-your-vhd"></a>VHD'nizi kaydetme
İşletim sistemi VHD'den VM oluşturma veya yeni bir VM'ye veri diski eklemek için önce bunları kaydetmeniz gerekir. VHD'NİZİ'ın senaryoya bağlı olarak aşağıdaki adımları izleyin.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Birden çok Azure VM örnekleri oluşturmak için işletim sistemi VHD'si genelleştirilmiş
Genelleştirilmiş işletim sistemi görüntüsü VHD depolama hesabına yüklendikten sonra olarak kaydetmek bir **Azure VM görüntüsü** böylece bir veya daha fazla sanal makine örneği oluşturabilir. Bir Azure sanal makine işletim sistemi görüntüsü olarak VHD'nizi kaydetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. VHD kopyalandığı tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Kopyalayın ve bu yeni bir Azure VM görüntü adını kaydedin. Yukarıdaki örnekte olduğu *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Benzersiz işletim sisteminin tek bir Azure sanal makine örneği oluşturmak için VHD
Benzersiz bir işletim sistemi VHD'si depolama hesabına yüklendikten sonra olarak kaydetmek bir **Azure işletim sistemi diski** böylece bir sanal makine örneği oluşturabilir. Bir Azure işletim sistemi diski VHD'nizi kaydetmek için bu PowerShell cmdlet'lerini kullanın. VHD kopyalandığı tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Kopyalayın ve bu yeni Azure işletim sistemi diski adı kaydedin. Yukarıdaki örnekte olduğu *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Veri diski VHD'si için yeni bir Azure VM örnekleri eklenmesi
Veri diski VHD depolama hesabına yüklendikten sonra yeni DS serisi için DSv2 serisi veya GS serisi Azure sanal makine örneği eklenebilir böylece bir Azure veri diski olarak kaydedin.

Bir Azure veri diski olarak VHD'nizi kaydetmek için bu PowerShell cmdlet'lerini kullanın. VHD kopyalandığı tam kapsayıcı URL'sini sağlayın.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Kopyalayın ve bu yeni Azure veri diski adını kaydedin. Yukarıdaki örnekte olduğu *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Premium depolama özellikli VM oluşturma
Bir kez işletim sistemi görüntüsü veya işletim sistemi diski kayıtlı, yeni DS serisi, DSv2 serisi veya GS serisi VM oluşturun. İşletim sistemi görüntüsü veya kaydettiğiniz işletim sistemi disk adı kullanacaksınız. VM Premium depolama katmanından seçin. Aşağıdaki örnekte, kullanıyoruz *Standard_DS2* VM boyutu.

> [!NOTE]
> Kapasite ve performans gereksinimlerini ve kullanılabilir Azure disk boyutları eşleştiğinden emin olmak için disk boyutunu güncelleştirin.
>
>

Yeni bir VM oluşturmak için aşağıdaki adım adım PowerShell cmdlet'lerini izleyin. İlk olarak, ortak parametreleri ayarlayın:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

İlk olarak, yeni sanal makinelerinizin içinde barındıracak bir bulut hizmeti oluşturun.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Ardından, işletim sistemi görüntüsü veya işletim sistemi diski, kaydettiğiniz senaryonuza bağlı olarak, Azure sanal makine örneği oluşturun.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Birden çok Azure VM örnekleri oluşturmak için işletim sistemi VHD'si genelleştirilmiş
Kullanarak bir veya daha fazla yeni DS serisi Azure VM örnekleri oluşturma **Azure işletim sistemi görüntüsü** kaydettiğiniz. Bu işletim sistemi görüntüsü adı, aşağıda gösterildiği gibi yeni VM oluşturma sırasında VM yapılandırması belirtin.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Benzersiz işletim sisteminin tek bir Azure sanal makine örneği oluşturmak için VHD
Kullanarak yeni bir DS serisi Azure sanal makine örneği oluşturma **Azure işletim sistemi diski** kaydettiğiniz. Bu işletim sistemi diski adı, aşağıda gösterildiği gibi yeni VM oluşturma sırasında VM yapılandırması belirtin.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Bir bulut hizmeti, bölge, depolama hesabı, kullanılabilirlik kümesi ve önbelleğe alma İlkesi gibi diğer Azure sanal makine bilgilerini belirtin. Aynı konumda bu disklerin temel alınan VHD olarak tüm seçili bulut Hizmetleri, bölge ve depolama hesabı olmalıdır böylece sanal makine örneği ilişkili işletim sistemi veya veri diskleri ile birlikte bulunan olması gerektiğini unutmayın.

### <a name="attach-data-disk"></a>Veri diski ekleme
Veri diski VHD'leri kayıtlıysanız, son olarak, bunları yeni Premium depolama özellikli Azure VM'e ekleyin.

Yeni VM'ye veri diski ekleme ve önbelleğe alma ilkesi belirtmek için PowerShell cmdlet'ini kullanın. Aşağıdaki örnekte önbelleğe alma ilkesini ayarlamak *salt okunur*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Ek adımlar olması, uygulamanızı desteklemek için gerekli değil Bu kılavuzda ele.
>
>

### <a name="checking-and-plan-backup"></a>Denetleme ve yedek plan
Yeni sanal makine çalışır duruma geldikten sonra aynı oturum açma kimliğini kullanarak erişmek ve parola özgün VM ve her şeyin beklendiği gibi çalıştığını doğrulayın. Şeritli birimler dahil olmak üzere tüm ayarlar, yeni sanal Makineyi mevcut olacaktır.

Yedek plan için son adımdır ve yeni sanal makine için bakım zamanlaması uygulamanın ihtiyaçlarına göre.

### <a name="a-sample-migration-script"></a>Örnek geçiş betiği
Geçiş için birden fazla VM varsa, PowerShell betikleri aracılığıyla Otomasyon yararlı olacaktır. Bir sanal makine geçişi otomatikleştiren bir örnek betiği aşağıda verilmiştir. Not, aşağıdaki komut yalnızca örnek olarak verilmiştir; geçerli VM diskleri hakkında birkaç varsayımlar vardır. Betik kendi senaryonuza ile eşleşecek şekilde güncelleştirmeniz gerekebilir.

Varsayımların şunlardır:

* Klasik Azure Vm'leri oluşturuyorsunuz.
* Kaynak işletim sistemi diskleri ve kaynak veri diskleri aynı depolama hesabı ve aynı kapsayıcı içinde olan. İşletim sistemi diskleri ve veri diskleri aynı yerde emin değilseniz, depolama hesapları ve kapsayıcılar üzerinden VHD'ler kopyalamak için AzCopy ya da Azure PowerShell kullanabilirsiniz. Önceki adıma bakın: [AzCopy veya PowerShell ile VHD'yi kopyalayın](#copy-vhd-with-azcopy-or-powershell). Senaryonuz karşılamak için bu betiği düzenleme başka bir seçenektir, ancak daha kolay ve hızlı olduğundan AzCopy veya PowerShell kullanmanızı öneririz.

Otomasyon betiği aşağıda verilmiştir. Metin kendi bilgilerinizle değiştirin ve betik kendi senaryonuza ile eşleşecek şekilde güncelleştirin.

> [!NOTE]
> Mevcut komut dosyası kullanarak kaynak VM'NİZİN ağ yapılandırması korumaz. Geçirilen Vm'leriniz üzerinde ağ ayarlarını re-config gerekir.
>
>

```powershell
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
    https://azure.microsoft.com/documentation/articles/powershell-install-configure/
    https://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

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

    # how frequently to report the copy status in seconds
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
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module version" -ForegroundColor Yellow
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
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article https://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
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

    # exporting the source vm to a configuration file, you can restore the original VM by importing this config file
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
    $sourceStorageKey = (Get-AzStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzStorageContainer -Context $destContext -Name vhds
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
        $destOSVHD = Get-AzStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting until the OS disk is released by source VM. This may take up to several minutes."
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
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
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
            $copyState = Get-AzStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
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

    # Edit this if you want to add more customization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>En iyi duruma getirme
Geçerli VM yapılandırmanızı, özellikle de standart diskler ile çalışacak şekilde özelleştirilebilir. Örneğin, şeritli birim içinde birçok diskleri kullanarak performansını artırmak için. Örneğin, 4 disk ayrı olarak Premium depolama kullanmak yerine, tek bir disk sağlayarak maliyetini en iyi duruma getirmek mümkün olabilir. En iyi duruma getirme, geçişten sonra özel adımlar gerekir ve bir olay bazında ele alınması için bu gereksinimi ister. Ayrıca, bu işlem veritabanları ve ayarları'nda tanımlanan disk düzeni kullanan uygulamalar için de çalışmayabilir unutmayın.

##### <a name="preparation"></a>Hazırlık
1. Basit bir geçiş önceki bölümde açıklandığı gibi tamamlayın. En iyi duruma getirme, geçişten sonra yeni VM üzerinde gerçekleştirilir.
2. Yeni disk boyutu için en iyi duruma getirilmiş gerekli tanımlayın.
3. Yeni disk belirtimlerine geçerli diskler/birimler eşleme belirleyin.

##### <a name="execution-steps"></a>Yürütme adımları
1. Yeni diskler, Premium Storage VM'si doğru boyutları ile oluşturun.
2. Geçerli birim birime eşleştiren yeni diske oturumu açma VM ve veri kopyalama. Yeni bir disk için eşlemek için gereken tüm geçerli birimler için bunu yapın.
3. Ardından, yeni diskler için geçiş yapmak için uygulama ayarları değiştirin ve eski birimleri ayır.

Uygulama için daha iyi disk performansı ayarlamak için lütfen en iyi duruma getirme uygulama performans bölümüne bakın bizim [yüksek performans için tasarlama](../../virtual-machines/windows/premium-storage-performance.md) makalesi.

### <a name="application-migrations"></a>Uygulama geçişleri
Veritabanları ve diğer karmaşık uygulamalar geçiş için uygulama sağlayıcısı tarafından tanımlanan özel adımlar gerektirebilir. Lütfen ilgili uygulama belgelerine başvurun. Örneğin veritabanlarını yedekleme genellikle geçirilebilir ve geri yükleme.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineleri geçirmek için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure sanal makineleri depolama hesapları arasında geçirme](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Oluşturun ve Azure'a bir Windows Server VHD'si yükleyin.](../../virtual-machines/windows/upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Oluşturma ve Azure'a bir Linux VHD karşıya yükleme](../../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Amazon AWS geçirme sanal makineleri için Microsoft Azure](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure depolama ve Azure sanal makineler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Iaas VM'ler için bir disk türü seçin](../../virtual-machines/windows/disks-types.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: https://technet.microsoft.com/library/hh831739.aspx
