---
title: Azure'daki Linux vm'lerinde depolama kaynağı silme hatalarını giderme | Microsoft Docs
description: Ekli VHD'lerde içeren depolama kaynaklarını silerken sorunlarını gidermek nasıl.
keywords: ''
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: a1eb946d3f1b18aaa86735dedcfbaa1fd6a89621
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60922685"
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Depolama kaynağı silme hatalarını giderme

Bazı senaryolarda, aşağıdakilerden birini karşılaşabileceğiniz hatalar, bir Azure depolama hesabı, kapsayıcı veya bir Azure Resource Manager dağıtımında blob silmeye çalıştığınız sırada oluşur:

> **Depolama hesabı 'StorageAccountName' silinemedi. Hata: Depolama hesabının kullanımda, yapılarının nedeniyle silinemiyor.**
> 
> **# Dışında # kapsayıcı silinemedi:<br>VHD'ler: Kapsayıcı üzerinde şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**
> 
> **# Dışında # bloblar silinemedi:<br>BlobName.vhd: Blob üzerinde şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.**

Azure Vm'lerinde kullanılan VHD'ler, azure'daki standart veya premium depolama hesabında sayfa blobları olarak depolanan .vhd dosyalarıdır. Azure diskleri hakkında daha fazla bilgi için bkz. bizim [yönetilen disklere giriş](../linux/managed-disks-overview.md).

Azure bozulmasını önlemek için bir sanal Makineye bağlı bir disk silinmesini engeller. Ayrıca, kapsayıcılar ve bir VM'ye bağlı bir sayfa blobu olduğu depolama hesaplarında silinmesini engeller. 

Şu hatalardan biriyle aldığında bir depolama hesabı, kapsayıcı veya blob silme işlemi şöyledir: 
1. Bloblar bir VM'ye tanımlayın
2. [Delete Vm'lerle bağlı **işletim sistemi diski**](#step-2-delete-vm-to-detach-os-disk)
3. [Tümünü Ayır **veri disklerini** gelen kalan VM](#step-3-detach-data-disk-from-the-vm)

Bu adımları tamamladıktan sonra depolama hesabı, kapsayıcı veya blob siliniyor. yeniden deneyin.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>1\. adım: Bir VM'ye blob tanımlayın

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Senaryo 1: Bir blob – silme ekli VM tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**ve silmek için blob gidin.
3. Varsa blobun **kiralama durumu** olduğu **kiralanmış**, sonra sağ tıklayıp **meta verileri Düzenle** Blob meta verileri bölmesini açmak için. 

    ![Portal ekran görüntüsü, depolama ile blobları hesap ve sağ tıklayın > "Vurgulanmış meta Düzenle"](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Blob meta verileri bölmesinde denetleyin ve değeri kayıt **MicrosoftAzureCompute_VMName**. Bu değer, VHD'nin bağlı olduğu VM adıdır. (Bkz **önemli** Bu alan mevcut değilse)
5. Blob meta verileri bölmesinde denetleyin ve değerini kayıt **MicrosoftAzureCompute_DiskType**. Bağlı disk işletim sistemi veya veri diski varsa, bu değeri tanımlar (bkz **önemli** Bu alan mevcut değilse). 

     ![Depolama "Blob meta verilerini" bölmesi açık portal ekran görüntüsü](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Blob disk türü ise **OSDisk** izleyin [2. adım: İşletim sistemi diskini ayırmak VM silme](#step-2-delete-vm-to-detach-os-disk). Aksi takdirde blob disk türü ise **DataDisk** adımları [3. adım: VM'den veri diski çıkarma](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Varsa **MicrosoftAzureCompute_VMName** ve **MicrosoftAzureCompute_DiskType** görünmez blob meta verilerini gösteren blob açıkça kiralanmış ve bir sanal makineye bağlı değil. İlk kiranın olmadan kiralanmış BLOB'lar silinemez. Kirayı Kes, bloba sağ tıklayın ve seçmek için **sonu kira**. Bir VM'ye bağlı olmayan kiralanmış blobları blob silinmesini önlemek, ancak kapsayıcı ya da depolama hesabının silinmesini engellemez.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Senaryo 2: Bir kapsayıcı - silme, Vm'lere eklenmiş tüm BLOB kapsayıcısındaki tanımlayın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **kapsayıcıları**, silinecek kapsayıcıyı bulun.
3. Kapsayıcı açmak için tıklayın ve içindeki blobların listesi görünür. Blob türü ile tüm blobları tanımlamak = **sayfa blobu** ve kiralama durumu = **kiralanmış** bu listeden. Senaryo 1'ın bu bloblarda her biriyle ilişkili VM tanımlamak için izleyin.

    ![Depolama hesabının BLOB'ları ve "kiralama vurgulanmış durumu" ile "Kiralanmış" portal ekran görüntüsü](./media/troubleshoot-vhds/utd-disks-sm.png)

4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [3. adım](#step-3-detach-data-disk-from-the-vm) vm'lerle silmek **OSDisk** ve ayırma **DataDisk**. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Senaryo 3: Hesap - tanımlamak, Vm'lere eklenmiş tüm BLOB Depolama hesabında depolama siliniyor
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları**. Depolama hesabına altında Git **Blob hizmeti** seçin **Blobları**.
3. İçinde **kapsayıcıları** bölmesinde, tüm kapsayıcıları tanımlamak burada **kiralama durumu** olduğu **kiralanmış** izleyin [Senaryo 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) her  **Kiralanmış** kapsayıcı.
4. İzleyin [2. adım](#step-2-delete-vm-to-detach-os-disk) ve [3. adım](#step-3-detach-data-disk-from-the-vm) vm'lerle silmek **OSDisk** ve ayırma **DataDisk**. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>2\. adım: İşletim sistemi diskini ayırmak VM silme
VHD bir işletim sistemi diski, VHD'nin silinebilmesi için önce sanal Makineyi silmeniz gerekir. Başka bir işlem bu adımları tamamladıktan sonra aynı sanal Makineye bağlı veri diskleri için gerekli olacaktır:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **sanal makineler**.
3. VHD bağlı olduğu sanal Makineyi seçin.
4. Hiçbir etkin bir şekilde sanal makine kullandığından emin olun ve artık sanal makine gerekir.
5. Üst kısmındaki **sanal makine ayrıntıları** bölmesinde **Sil**ve ardından **Evet** onaylamak için.
6. Sanal Makinenin silinmesi gerekir, ancak VHD tutulabilir. Ancak, VHD'yi bir VM'ye bağlı artık veya üzerinde bir kira sahip. Bu, kira serbest bırakılması için birkaç dakika sürebilir. Kira serbest bırakılır doğrulamak için hem de blob konumuna Gözat **Blob özellikleri** bölmesinde **kiralama durumu** olmalıdır **kullanılabilir**.

## <a name="step-3-detach-data-disk-from-the-vm"></a>3\. adım: VM'den veri diski çıkarma
VHD kirayı kaldırmaktır VM'den veri diski VHD ise ayırma:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **sanal makineler**.
3. VHD bağlı olduğu sanal Makineyi seçin.
4. Seçin **diskleri** üzerinde **sanal makine ayrıntıları** bölmesi.
5. Silinecek veri diski seçin VHD iliştirilir. Hangi blob disk VHD URL'sini kontrol ederek ekli belirleyebilirsiniz.
6. Blob konumun yolunu iade diske tıklayarak doğrulayabilirsiniz **VHD URİ'si** alan.
7. Seçin **Düzenle** üzerindeki **diskleri** bölmesi.
8. Tıklayın **simgesi ayırma** veri diskinin silinecek.

     ![Depolama "Blob meta verilerini" bölmesi açık portal ekran görüntüsü](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. **Kaydet**’i seçin. Diski sanal makineden artık kullanımdan çıkarıldı ve VHD artık kiralanır. Bu, kira serbest bırakılması için birkaç dakika sürebilir. Kira serbest olduğunu doğrulamak için hem de blob konumuna göz atın **Blob özellikleri** bölmesinde **kiralama durumu** değeri **kilitli değil** veya **Kullanılabilir**.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm

