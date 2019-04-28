---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 06e6e491fa1e9a047527efb78149855b125771ef
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543809"
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme
## <a name="overview"></a>Genel Bakış
Azure depolama blobları anlık olanağı sağlar. Anlık görüntüler, o noktasında blob durumu yakalar. Bu makalede, anlık görüntüler kullanılarak sanal makine disklerinin yedeklerini bakımını bir senaryo açıklanmaktadır. Azure yedekleme ve kurtarma hizmeti kullanmamayı seçin ve sanal makine diskleriniz için özel bir yedekleme stratejisi oluşturmak istiyorsanız bu yöntemi kullanabilirsiniz.

Azure sanal makine diskleri sayfa blobları Azure Depolama'da depolanır. Biz bu makalede sanal makine diskleri için bir yedekleme stratejisi açıklayan olduğundan, sayfa blobları bağlamında anlık görüntüleri diyoruz. Anlık görüntüleri hakkında daha fazla bilgi için bkz [bir blobun anlık görüntüsünü oluşturma](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Anlık görüntü nedir?
Blob anlık görüntüsü, bir noktada sürede yakalanan bir blobun salt okunur bir sürümüdür. Anlık görüntü oluşturulduktan sonra okunabildiğini, kopyalanan, veya silinmiş, ancak değiştirilmedi. Anlık görüntüler, zaman içinde bir anda göründüğü gibi bir blob yedeklemek için bir yol sağlar. REST sürümü 2015-04-05 kadar tam bir anlık görüntü kopyalama özelliği vardı. Sürüm 2015-07-08 REST ve üstü ile siz de artımlı anlık kopyalayabilirsiniz.

## <a name="full-snapshot-copy"></a>Tam bir anlık görüntü kopyalama
Anlık görüntüler, başka bir depolama hesabına temel blob yedeklerini tutmak için bir blob kopyalanabilir. Blob önceki bir sürüme geri yükleme gibi olan kendi temel blob üzerinde anlık görüntü de kopyalayabilirsiniz. Anlık görüntü bir depolama hesabından diğerine kopyalandığında temel sayfa blobu olarak aynı kaplar. Bu nedenle, tüm anlık görüntüler bir depolama hesabından diğerine kopyalama yavaş ve kadar alan hedef depolama hesabı kullanır.

> [!NOTE]
> Başka bir hedef için temel bir blobu kopyalarsanız, BLOB anlık görüntüleri onunla birlikte kopyalanmaz. Benzer şekilde, temel bir blobu bir kopyasının üzerine, temel blob ile ilişkili anlık görüntüleri etkilenmez ve temel blob adla değişmeden kalır.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Anlık görüntülerini kullanarak diskleri yedekleme
Sanal makine diskleriniz için bir yedekleme stratejisi disk veya sayfa blob düzenli anlık görüntülerini alabilir ve bunları başka bir depolama hesabı kullanarak kopyalama araçları gibi [kopya blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) işlemi veya [AzCopy](../articles/storage/common/storage-use-azcopy.md). Farklı bir ada sahip bir hedef sayfa blobu için anlık görüntü kopyalayabilirsiniz. Sonuçta elde edilen hedef sayfa blob'u yazılabilir sayfa blobu ve değil bir anlık görüntü ' dir. Bu makalede, biz anlık görüntülerini kullanarak sanal makine disklerinin yedeklerini atılması gereken adımlar açıklanmaktadır.

### <a name="restore-disks-using-snapshots"></a>Anlık görüntüleri kullanarak diskleri geri yükle
Yedekleme anlık görüntülerinin birinde önceden yakalanmış olan ve kararlı bir sürüm diskinizi geri yüklemek için zaman olduğunda, bir anlık görüntü üzerinde temel sayfa blob'u kopyalayabilirsiniz. Taban sayfası için anlık görüntü yükseltildikten sonra blob anlık görüntüsü kalır ancak kaynağı hem okunabilir ve yazılabilir bir kopya ile yazılır. Bu makalenin sonraki bölümlerinde, biz diskinizi önceki bir sürümünü, anlık görüntüden geri yüklemek için adımları açıklar.

### <a name="implementing-full-snapshot-copy"></a>Uygulama tam anlık görüntü kopyalama
Aşağıdakileri yaparak bir tam anlık görüntü kopyalama uygulayabilirsiniz.

* İlk olarak kullanarak temel blob anlık görüntüsünü alma [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) işlemi.
* Ardından, hedef depolama hesabı kullanarak bir anlık görüntü kopyalama [kopya blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Temel blobunuza yedekleme kopyalarını bulundurmak için bu işlemi yineleyin.

## <a name="incremental-snapshot-copy"></a>Artımlı anlık görüntü kopyalama
Yeni özelliği [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API anlık görüntüler, sayfa BLOB'ları veya diskleri yedeklemek için çok daha iyi bir yol sağlar. API değişikliklerin listesini temel blob anlık görüntüleri arasında yedekleme hesapta kullanılan depolama alanı miktarını azaltan döndürür. API, standart depolama yanı sıra Premium depolama üzerinde sayfa bloblarını destekler. Bu API'yi kullanarak Azure Vm'leri için daha hızlı ve daha verimli yedekleme çözümleri oluşturabilirsiniz. Bu API ile REST sürümü 2015-07-08 kullanılabilir ve daha yüksek olacaktır.

Artımlı anlık görüntü kopyalama bir depolama hesabından diğerine arasındaki farkı kopyalamanıza olanak sağlayan,

* Temel blob ve kendi anlık görüntü veya
* Herhangi iki temel blob anlık görüntüsü

Aşağıdaki koşulların karşılandığından emin sağlanan

* Blob, Oca 1 2016 veya sonraki sürümlerde oluşturuldu.
* Blob ile yazıldı değil [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) veya [kopya blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) iki anlık görüntü arasındaki.

**Not**: Bu özellik, Premium ve standart Azure sayfa Blobları için kullanılabilir.

Anlık görüntülerini kullanarak özel bir yedekleme stratejisi sahip olduğunuzda, anlık görüntüler bir depolama hesabından diğerine kopyalama başka bir yavaş olabilir ve kadar depolama alanı kullanabilir. Tüm anlık görüntü bir yedekleme depolama hesabına kopyalamak yerine, bir yedekleme sayfa blobu için ardışık anlık görüntü arasındaki farkı yazabilirsiniz. Bu şekilde, kopyalama ve yedeklemeleri depolamak için alanı süresini önemli ölçüde azaltılır.

### <a name="implementing-incremental-snapshot-copy"></a>Uygulama artımlı anlık görüntü kopyalama
Aşağıdakileri yaparak artımlı anlık görüntü kopyalama uygulayabilirsiniz.

* Kullanarak temel blob anlık [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Aynı veya herhangi bir Azure bölgesine diğer kullanarak hedef yedekleme depolama hesabı için anlık görüntü kopyalama [kopya blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Yedekleme sayfa blob'u budur. Yedekleme sayfa blobu anlık görüntüsünü alın ve yedekleme hesabında depolayın.
* Anlık görüntü blobu kullanarak temel blob başka bir anlık görüntüsünü alın.
* Temel blob kullanarak ilk ve ikinci anlık görüntü arasındaki fark alma [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Yeni bir parametre kullanın **prevsnapshot**, istediğiniz fark almak için anlık görüntü tarih/saat değeri belirtin. Bu parametre, mevcut olduğunda, REST yanıtı hedef anlık görüntü ve NET sayfaları dahil olmak üzere önceki anlık görüntüye arasında değiştirilmiş sayfaları içerir.
* Kullanım [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) yedekleme sayfa blob'u için bu değişiklikleri uygulamak için.
* Son olarak, yedekleme sayfa blobu anlık görüntüsünü alın ve yedekleme depolama hesabında depolayın.

Sonraki bölümde, size daha ayrıntılı olarak, artımlı anlık görüntü kopyalama kullanarak disklerinin yedeklerini nasıl koruyabilirsiniz açıklanmıştır

## <a name="scenario"></a>Senaryo
Bu bölümde, biz anlık görüntülerini kullanarak sanal makine diskleri için özel bir yedekleme stratejisi içeren bir senaryo açıklanmaktadır.

DS serisi Azure VM bağlı bir premium depolama P30 disk ile göz önünde bulundurun. P30 diski de denen *mypremiumdisk* adlı bir premium depolama hesabında depolanan *mypremiumaccount*. Adlı bir standart depolama hesabı *mybackupstdaccount* yedeğini depolamak için kullanılan *mypremiumdisk*. Bir anlık görüntüsünü tutmak istiyoruz *mypremiumdisk* her 12 saatte bir.

Bir depolama hesabı oluşturma hakkında bilgi edinmek için [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).

Azure VM'lerin yedeklenmesi hakkında bilgi edinmek için bkz [planlama Azure VM yedeklemeleri](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Artımlı anlık görüntüleri kullanarak bir diski yedeklerini korumak için adımlar
Aşağıdaki adımları anlık açıklanır *mypremiumdisk* yedeklemelerinizi ve koruyabilir *mybackupstdaccount*. Yedekleme adlı bir standart sayfa blobudur *mybackupstdpageblob*. Yedekleme sayfa blobu, her zaman son anlık görüntüsünü aynı duruma yansıtır *mypremiumdisk*.

1. Yedekleme sayfa blob'u, premium depolama disk için anlık görüntüsünü alarak oluşturma *mypremiumdisk* adlı *mypremiumdisk_ss1*.
2. Bu anlık görüntü için mybackupstdaccount adlı bir sayfa blobu kopyalama *mybackupstdpageblob*.
3. Anlık *mybackupstdpageblob* adlı *mybackupstdpageblob_ss1*kullanarak [anlık görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) ve depoladığınız *mybackupstdaccount*.
4. Yedekleme penceresi sırasında başka bir anlık görüntüsünü oluşturmak *mypremiumdisk*, örneğin *mypremiumdisk_ss2*ve depoladığınız *mypremiumaccount*.
5. İki anlık görüntü arasındaki artımlı değişikliklerin *mypremiumdisk_ss2* ve *mypremiumdisk_ss1*kullanarak [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) üzerinde *mypremiumdisk_ ss2* ile **prevsnapshot** parametre için zaman damgası *mypremiumdisk_ss1*. Bu artımlı değişiklikler için yedekleme sayfa blobu yazma *mybackupstdpageblob* içinde *mybackupstdaccount*. Artımlı değişiklikleri silinen aralıkları varsa, yedekleme sayfa blobu temizlenmelidir. Kullanım [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) artımlı değişiklikler için yedekleme sayfa blobu yazma için.
6. Yedekleme sayfa blobu anlık *mybackupstdpageblob*adlı *mybackupstdpageblob_ss2*. Önceki anlık görüntüyü silmek *mypremiumdisk_ss1* premium depolama hesabı.
7. Her yedekleme penceresi 4-6. adımları tekrarlayın. Bu şekilde, yedeklerini koruyabilirsiniz *mypremiumdisk* standart depolama hesabı içinde.

![Artımlı anlık görüntülerini kullanarak disk yedekleme](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Bir disk anlık görüntülerden geri yüklemek için adımları
Aşağıdaki adımlar, premium disk geri yükleme açıklar *mypremiumdisk* önceki bir anlık görüntüye yedekleme depolama hesabından *mybackupstdaccount*.

1. Premium diske geri yüklemek istediğiniz zaman noktası belirleyin. Anlık görüntü olduğunu düşünelim *mybackupstdpageblob_ss2*, yedekleme depolama hesabında depolanan *mybackupstdaccount*.
2. Anlık görüntüyü mybackupstdaccount içinde yükseltme *mybackupstdpageblob_ss2* yeni yedekleme temel sayfa blobu olarak *mybackupstdpageblobrestored*.
3. Olarak adlandırılan bu geri yüklenen yedekleme sayfa blob anlık *mybackupstdpageblobrestored_ss1*.
4. Geri yüklenen sayfa blobu Kopyala *mybackupstdpageblobrestored* gelen *mybackupstdaccount* için *mypremiumaccount* yeni premium disk olarak  *mypremiumdiskrestored*.
5. Anlık *mypremiumdiskrestored*adlı *mypremiumdiskrestored_ss1* ileride artımlı yedekleme yapmak için.
6. DS serisi VM için geri yüklenen diski işaret *mypremiumdiskrestored* ve eski ayırma *mypremiumdisk* VM'den.
7. Geri yüklenen diski için önceki bölümde açıklanan yedekleme işlemine başlamak *mypremiumdiskrestored*kullanarak *mybackupstdpageblobrestored* yedekleme sayfa blobu olarak.

![Disk anlık görüntülerden geri yükleme](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bir blobun anlık görüntülerini oluşturma ve sanal makine yedekleme altyapınızı planlama hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanın.

* [Bir blobun anlık görüntüsünü oluşturma](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [VM yedekleme altyapınızı planlama](../articles/backup/backup-azure-vms-introduction.md)

