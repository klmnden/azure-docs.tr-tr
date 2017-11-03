---
title: Klasik Azure VM'ye bir disk ekleme | Microsoft Docs
description: "Klasik dağıtım modeli kullanılarak oluşturulmuş bir Windows sanal makineye bir veri diski ekleyin ve onu başlatın."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: 7847a2485cd57d895c022afb12ef08f37fe5775d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Klasik dağıtım modeliyle oluşturulan bir Windows sanal makinesine veri diski ekleme
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

Bu makalede Azure portal kullanarak bir Windows sanal makine için Klasik dağıtım modeliyle oluşturulan yeni ve mevcut diskleri ekleme gösterilmiştir.

Ayrıca [bir Linux VM Azure portalında bir veri diski ekleme](../../linux/attach-disk-portal.md).

Bir disk eklemeden önce bu ipuçlarını gözden geçirin:

* Sanal makinenin boyutunu, iliştirebilirsiniz kaç tane veri diskleri denetler. Ayrıntılar için bkz [sanal makineler için Boyutlar](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Premium depolama kullanmak için DS serisi veya GS serisi bir sanal makine gerekir. Bu sanal makinelerle Premium ve standart depolama hesapları arasından diskleri kullanabilirsiniz. Premium depolama belirli bölgelerde kullanılabilir. Ayrıntılar için bkz [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Yeni bir disk için bunu eklediğinizde Azure bunu oluşturduğundan ilk oluşturmanız gerekmez.

Ayrıca [Powershell kullanarak bir veri diskini](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md).

## <a name="find-the-virtual-machine"></a>Sanal makine bulunamadı
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Panoda listelenen kaynak sanal makineyi seçin.
3. Sol bölmede altında **ayarları**, tıklatın **diskleri**.

    ![Disk Ayarları'nı açın](./media/attach-disk/virtualmachinedisks.png)

Ya da eklemek için aşağıdaki yönergeleri tarafından devam bir [yeni disk](#option-1-attach-a-new-disk) veya bir [mevcut disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Seçenek 1: Eklemek ve yeni bir disk başlatma

1. Üzerinde **diskleri** dikey penceresinde tıklatın **Attach yeni**.
2. Varsayılan ayarları gözden geçirin, gerektiği gibi güncelleştirin ve ardından **Tamam**.

   ![Disk ayarlarını gözden geçirin](./media/attach-disk/attach-new.png)

3. Azure disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenen **veri diskleri**.

### <a name="initialize-a-new-data-disk"></a>Yeni bir veri diski başlatın

1. Sanal makineye bağlanın. Yönergeler için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Sanal makineye oturum açtıktan sonra açmak **Sunucu Yöneticisi'ni**. Sol bölmede seçin **dosya ve depolama hizmetleri**.

    ![Sunucu Yöneticisi'ni açma](../media/attach-disk-portal/fileandstorageservices.png)

3. Seçin **diskleri**.
4. **Diskleri** bölüm diskleri listeler. Genellikle, bir sanal makine disk 0, 1 disk ve disk 2 ' var. Disk 0 işletim sistemi diski olduğundan, disk 1 geçici diski olduğundan ve disk 2 yeni sanal makineye bağlı veri disktir. Veri diski olarak bölüm listeler **bilinmeyen**.

 Diske sağ tıklayın ve seçin **başlatma**.

5. Disk başlatıldığında tüm veriler silinecek bildirim. Tıklatın **Evet** uyarısını kabul ve diski başlatın. Tamamlandıktan sonra bölüm olarak listelenmez. **GPT**. Diski yeniden sağ tıklatın ve seçin **yeni birim**.

6. Varsayılan değerleri kullanarak Sihirbazı tamamlayın. Sihirbaz tamamlandığında, **birimleri** bölümde yeni birimi listelenir. Disk artık çevrimiçi ve verilerini depolamak hazır olur.

    ![Birimi başarıyla başlatıldı](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>Seçenek 2: var olan bir diski kullanıma açın
1. Üzerinde **diskleri** dikey penceresinde tıklatın **Attach varolan**.
2. Altında **varolan bir diski İlişti**, tıklatın **konumu**.

   ![Varolan bir diski kullanıma açın](./media/attach-disk/attachexistingdisksettings.png)
3. Altında **depolama hesapları**, .vhd dosyası tutan kapsayıcı ve hesabı seçin.

   ![VHD konumunu bulma](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. .Vhd dosyasını seçin.
5. Altında **varolan bir diski İlişti**, seçtiğiniz dosya altında listelenen **VHD dosyasını**. **Tamam** düğmesine tıklayın.
6. Azure disk sanal makineye iliştirir sonra sanal makinenin disk ayarları altında listelenen **veri diskleri**.

## <a name="use-trim-with-standard-storage"></a>Standart depolama ile kullanmak üzere KIRPMA

Standart depolama (HDD) kullanırsanız, KIRPMA etkinleştirmeniz gerekir. Yalnızca gerçekten kullandığınız depolama için faturalandırılır şekilde KIRPMA diskte kullanılmayan blokları atar. KIRPMA kullanarak büyük dosyaların silinmesi neden kullanılmayan blokları dahil, maliyetleri kaydedebilirsiniz.

KIRPMA ayarını denetlemek için bu komutu çalıştırabilirsiniz. Windows VM ve türü bir komut istemi açın:

```
fsutil behavior query DisableDeleteNotify
```

Komut 0 döndürürse, KIRPMA doğru şekilde etkinleştirilir. 1 değeri döndürülürse, KIRPMA etkinleştirmek için aşağıdaki komutu çalıştırın:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı D: kullanması gerekirse, verileri depolamak için sürücü, şunları yapabilirsiniz [Windows geçici disk sürücü harfini değiştirin](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>Ek kaynaklar
[Diskleri ve sanal makineler için VHD'ler hakkında](../../virtual-machines-linux-about-disks-vhds.md)
