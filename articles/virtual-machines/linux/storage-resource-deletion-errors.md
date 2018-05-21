---
title: Linux sanal makineleri üzerinde Azure Resource Manager dağıtımında depolama kaynak silme hatalarında sorun giderme | Microsoft Docs
description: Depolama kaynaklarını içeren silerken sorunlarını gidermek nasıl VHD'ler bağlı.
keywords: SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines-linux
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 823c85f620dcd7c96d0005d39b1a12a7a9fba938
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Depolama kaynak silme hatalarıyla ilgili sorunları giderme

Bazı senaryolarda, aşağıdakilerden birini karşılaşabileceğiniz bir Azure depolama hesabı, kapsayıcı ya da Azure Resource Manager dağıtımında blob silmeye çalışırken hataları oluşur:

>**Depolama hesabı 'StorageAccountName' silinemedi. Hata: Depolama hesabına ait yapıtlar kullanımda nedeniyle silinemiyor.**

>**# Kaldırıldığından dışında # silinemedi:<br>VHD'ler: kapsayıcıda şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**

>**# Dışında # BLOB'lar silinemedi:<br>BlobName.vhd: blob üzerindeki şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**

Azure Vm'lerinde kullanılan VHD'ler, sayfa bloblarını Azure standart veya premium storage hesabında depolanan .vhd dosyalarıdır. Azure diskleri hakkında daha fazla bilgi için bkz: [yönetilmeyen ve Microsoft Azure Linux VM'ler için disk depolama yönetilen hakkında](about-disks-and-vhds.md). 

Azure bozulmasını önlemek için bir VM öğesine bağlı bir disk silinmesini önler. Ayrıca, kapsayıcıları ve bir VM öğesine bağlı bir sayfa blob'u olan depolama hesaplarını silinmesini önler. 

Bu hatalardan biri alırken bir depolama hesabı, kapsayıcı veya blob silmek için bir işlemdir: 
1. [Bir VM'ye bağlı BLOB'ları tanımlayın](#step-1-identify-blobs-attached-to-a-vm)
2. [Sil VM ile birlikte bağlı **işletim sistemi diski**](#step-2-delete-vm-to-detach-os-disk)
3. [Tüm detach **veri diskleri** kalan sanal makine gelen](#step-3-detach-data-disk-from-the-vm)

Bu adımları tamamladıktan sonra depolama hesabı, kapsayıcısı veya blob silme yeniden deneyin.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>1. adım: bir VM'ye bağlı blob tanımlayın

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Senaryo 1: bir blob – silme ekli VM tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**ve silmek için blob gidin.
3. Varsa blob **kira durumu** olan **Leased**, sonra sağ tıklatın ve seçin **Düzenle meta veri** Blob meta verileri bölmesini açmak için. 

    ![Portal ekran depolama hesabı BLOB'ları ve sağ tıklayın > "Vurgulanmış meta Düzenle"](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Blob meta verileri bölmesinde denetleyin ve değeri kayıt **MicrosoftAzureCompute_VMName**. Bu değer VHD bağlı olduğu VM adıdır. (Bkz **önemli** Bu alan yoksa)
5. Blob meta verileri bölmesinde denetleyin ve değerini kaydedin **MicrosoftAzureCompute_DiskType**. Ekli disk işletim sistemi veya veri diski ise bu değeri tanımlar (bkz **önemli** Bu alan yoksa). 

     ![Depolama "Blob meta verileri" bölmesiyle açık portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Blob disk türü ise **OSDisk** izleyin [2. adım: işletim sistemi diski kullanımdan çıkarmak için Sil VM](#step-2-delete-vm-to-detach-os-disk). Aksi durumda, blob disk türü ise **DataDisk** adımları [3. adım: veri diskini sanal makineden](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Varsa **MicrosoftAzureCompute_VMName** ve **MicrosoftAzureCompute_DiskType** görünmez blob meta verileri blob açıkça kiralanmış ve bir VM öğesine bağlı değil belirtir. Kira ilk bozmadan kiralık BLOB'lar silinemez. Kira sonu, blob üzerindeki sağ tıklatın ve seçmek için **sonu kira**. Bir VM öğesine bağlı olmayan kiralık BLOB'ları blob silinmesini engellemek, ancak kapsayıcı veya depolama hesabı silme engellemez.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>2. Senaryo: bir kapsayıcı - silme VM'ler için bağlı olan tüm blob(s) kapsayıcıdaki tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**, silinecek kapsayıcı bulun.
3. Kapsayıcı açmak için tıklatın ve onun içindeki blobları listesi görüntülenir. Blob türüne sahip tüm BLOB'lar tanımlamak = **sayfa blobu** ve kiralama durum = **Leased** bu listeden. İzleyin [Senaryo 1](#step-1-identify-blobs-attached-to-a-vm) her bu BLOB'lar ile ilişkilendirilmiş VM tanımlamak için.

    ![Depolama hesabı BLOB'ları ve "kira vurgulanmış durumuyla" "Leased" ile portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-disks-sm.png)

4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [adım 3](#step-3-detach-data-disk-from-the-vm) sanal makine ile silmek için **OSDisk** ve ayırma **DataDisk**. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Senaryo 3: depolama silme hesap - VM'ler için bağlı olan tüm blob(s) depolama hesabındaki tanımlama
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**.

    ![Depolama hesabı kapsayıcıları ve "kira vurgulanmış durumuyla" "Leased" ile portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-containers-sm.png)

3. İçinde **kapsayıcıları** bölmesinde, tüm kapsayıcıları tanımlamak nerede **kira durumu** olan **Leased** ve izleyin [Senaryo 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) her  **Kiralanmış** kapsayıcı.
4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [adım 3](#step-3-detach-data-disk-from-the-vm) sanal makine ile silmek için **OSDisk** ve ayırma **DataDisk**. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>2. adım: İşletim sistemi diski kullanımdan çıkarmak için VM silme
VHD bir işletim sistemi diski ise, VHD'nin silinebilmesi için önce VM silmeniz gerekir. Başka bir eylem, bu adımları tamamladıktan sonra aynı VM ekli veri diskleri için gerekli olacaktır:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **sanal makineleri**.
3. VHD bağlı olduğu VM seçin.
4. Hiçbir şey etkin olarak sanal makinenin kullandığından emin olun ve sanal makine artık gerekir.
5. Üstündeki **sanal makine ayrıntıları** bölmesinde, **silmek**ve ardından **Evet** onaylamak için.
6. VM silinmesi gerekir, ancak VHD korunabilir. Ancak, VHD'yi bir VM'ye bağlı artık veya üzerinde bir kira sahip. Yayımlanacak kira için birkaç dakika sürebilir. Blob konumuna hem de kira serbest olduğunu doğrulamak için Gözat **Blob özellikleri** bölmesinde **kiralama durum** olmalıdır **kullanılabilir**.

## <a name="step-3-detach-data-disk-from-the-vm"></a>3. adım: VM veri diski kullanımdan çıkarın
VHD bir veri diski varsa, kira kaldırmak için VM VHD'den bağlantısını kesin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **sanal makineleri**.
3. VHD bağlı olduğu VM seçin.
4. Seçin **diskleri** üzerinde **sanal makine ayrıntıları** bölmesi.
5. Silinecek veri diski seçin VHD iliştirilir. VHD URL'si denetleyerek disk hangi blob bağlı olduğu belirleyebilirsiniz.
6. Blob konumun yolunu denetlemek için disk üzerinde tıklatarak doğrulayabilirsiniz **VHD URİ'si** alan.
7. Seçin **Düzenle** tepesindeki **diskleri** bölmesi.
8. Tıklatın **simgesi detach** silinecek veri diski.

     ![Depolama "Blob meta verileri" bölmesiyle açık portalı ekran görüntüsü](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. **Kaydet**’i seçin. Disk şimdi sanal makineden ayrıldı ve VHD artık kiralanmış. Yayımlanacak kira için birkaç dakika sürebilir. Blob konumuna hem de kiralamasının serbest bırakıldığını doğrulamak için Gözat **Blob özellikleri** bölmesinde **kiralama durum** değeri olmalıdır **kilitli değil** veya **Kullanılabilir**.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm

