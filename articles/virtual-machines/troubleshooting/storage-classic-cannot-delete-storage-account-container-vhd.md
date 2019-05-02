---
title: Azure Klasik depolama hesaplarını, kapsayıcıları veya VHD'leri sildiğinizde sorun giderme | Microsoft Docs
description: Ekli VHD'lerde içeren depolama kaynaklarını silerken sorunlarını gidermek nasıl.
services: storage
author: AngshumanNayakMSFT
tags: top-support-issue,azure-service-management
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/11/2019
ms.author: annayak
ms.openlocfilehash: 35f8a766c6d260e23ff854284d5b8ee047e64b42
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926239"
---
# <a name="troubleshoot-classic-storage-resource-deletion-errors"></a>Klasik depolama kaynağı silme hatalarını giderme
Bu makalede, Azure Klasik depolama hesabı, kapsayıcı veya *.vhd sayfa blob dosyası silinmeye çalışılırken şu hatalar biri meydana geldiğinde sorun giderme kılavuzu verilmiştir. 


Bu makalede, yalnızca klasik depolama kaynakları ile ilgili sorunları kapsar. Azure portalını kullanarak klasik bir sanal makine bir kullanıcıyı siler, PowerShell veya CLI ardından diskleri otomatik olarak silinmez. Kullanıcı, "Disk" kaynağı silme seçeneğini alır. Durumunda seçeneği seçili değilse, "Disk" kaynak depolama hesabı, kapsayıcı ve gerçek *.vhd sayfa blob dosyası silinmesini engeller.

Azure diskleri hakkında daha fazla bilgi bulunabilir [burada](../../virtual-machines/windows/managed-disks-overview.md). Azure bozulmasını önlemek için bir sanal Makineye bağlı bir disk silinmesini engeller. Ayrıca, kapsayıcılar ve bir VM'ye bağlı bir sayfa blobu olan depolama hesapları silinmesini engeller. 

## <a name="what-is-a-disk"></a>"Disk" nedir?
"Disk" kaynağı, bir sanal makine *.vhd sayfa blob dosyasına bir işletim sistemi diski veya veri diski olarak bağlamak için kullanılır. Bir işletim sistemi diski veya veri diski kaynak silinene kadar kiralama *.vhd dosyada tutmak devam eder. "Disk" kaynağı için işaret ediyorsa, görüntü gösterilen yolundaki tüm depolama kaynağı silinemez.

![Disk (Klasik) "Özelliği" bölmesindeki portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/Disk_Lease_Illustration.jpg) 


## <a name="steps-while-deleting-a-classic-virtual-machine"></a>Klasik bir sanal makine silinirken adımları 
1. Klasik sanal makineyi silin.
2. "Diskler" onay kutusunu seçtiyseniz **disk kirası** (yukarıdaki görüntüde gösterilmiştir) ilişkili sayfa blob'u *.vhd bozuk. Gerçek sayfa blob *.vhd dosya depolama hesabında varolmaya devam eder.
![Sanal makine (Klasik) "Sil" hata bölmesi açık portal ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/steps_while_deleting_classic_vm.jpg) 

3. Disklerin kira bozuk hale geldikten sonra sayfa BLOB silinebilir. Tüm "Disk" kaynak bunları mevcut silindikten sonra bir depolama hesabı veya kapsayıcı silinebilir.

>[!NOTE] 
>Kullanıcı VM ancak VHD silerse, depolama ücretleri sayfa blob *.vhd dosyada tahakkuk etmeye devam eder. Depolama hesabı, onay türü ayarlarına uygun olarak ücret olacaktır [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/) daha fazla ayrıntı için. Kullanıcı artık vhd'sinin kullanmaya çalışırsa, BT/bunları gelecekteki ücretlerden kaçınmak için silin. 

## <a name="unable-to-delete-storage-account"></a>Depolama hesabı silinemedi 

Kullanıcı, kullanıcı artık gerekli olmadığında bir Klasik depolama hesabını silmek çalıştığında, aşağıdaki davranış görebilirsiniz.

#### <a name="azure-portal"></a>Azure portal 
Kullanıcı için Klasik depolama hesabı üzerinde gittiğinde [Azure portalında](https://portal.azure.com) tıklattığında **Sil**, kullanıcı şu iletiyi görürsünüz: 

Diskleri ile "bir sanal makineye bağlı"

![Sanal makine (Klasik) "Sil" hata bölmesi açık portal ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_attached_portal.jpg) 


Diskleri ile "bir sanal makineye eklenmemiş"

![Sanal makine (Klasik) "Sil" hata olmayan bölmesi portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_unattached_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Kullanıcı artık, Klasik PowerShell cmdlet'lerini kullanarak kullanılmakta olan bir depolama hesabını silmeye çalışır. Kullanıcı şu iletiyi görürsünüz:

> <span style="color:cyan">**Remove-AzureStorageAccount - StorageAccountName myclassicaccount**</span>
> 
> <span style="color:red">Remove-AzureStorageAccount: BadRequest: Ör bazı etkin görüntülere ve/veya diskleri, depolama hesabı myclassicaccount sahip  
> myclassicaccount. Bu görüntülerin ve/veya diskleri bu depolama hesabını silmeden önce kaldırılır emin olun.</span>

## <a name="unable-to-delete-storage-container"></a>Depolama kapsayıcısı silinemedi

Kullanıcı, kullanıcı artık gerekli olmadığında bir Klasik depolama blob kapsayıcısını silme girişiminde bulunduğunda, aşağıdaki davranış görebilirsiniz.

#### <a name="azure-portal"></a>Azure portal 
Azure portalı, kapsayıcıdaki bir *.vhd sayfa blob dosyasına işaret eden bir "Diskler" kira varsa, bir kapsayıcı silmek kullanıcı izin vermez. Disklerin kira vhd'sinin dosyasıyla bunlardaki yanlışlıkla silinmesini önlemek için tasarım gereğidir. 

![Depolama kapsayıcısı "list" bölmesindeki portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_container_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
PowerShell kullanarak silmek kullanıcı seçerse, şu hataya neden olur. 

> <span style="color:cyan">**Remove-AzureStorageContainer -Context $context -Name vhds**</span>
> 
> <span style="color:red">Remove-AzureStorageContainer : Uzak sunucu hata döndürdü: (412) burada şu anda kapsayıcı üzerindeki kira ve istekte hiçbir kiralama kimliği belirtildi... HTTP durum kodu: 412 - HTTP hata iletisi: Kapsayıcı üzerinde şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.</span>

## <a name="unable-to-delete-a-vhd"></a>Bir vhd silinemedi 

Azure sanal makinesi sildikten sonra kullanıcı (sayfa blobu) vhd dosyasını silin ve aşağıdaki ileti almak çalışır:

#### <a name="azure-portal"></a>Azure portal 
Portalda, silinmek üzere seçilen blobların listesi bağlı olarak iki deneyimler olabilir.

1. Yalnızca "Kiralanmış" BLOB'ları seçili sonra Sil düğmesini gösterilmesini.
![Kapsayıcı blob "list" bölmesindeki portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_portal.jpg)


2. "Kiralanmış" ve "Kullanılabilir" bloblar bir karışımını seçtiyseniz, "Sil" düğmesini gösterir. Ancak, bir Disk kirası olan sayfa bloblarını arkasında "Sil" işlemi bırakır. 
![Blob kapsayıcı "list" bölmesindeki açık portal ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_1.jpg)
![seçilen blob ile portal ekran görüntüsü "Sil" bölmesini açma](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_2.jpg)

#### <a name="azure-powershell"></a>Azure PowerShell 
PowerShell kullanarak silmek kullanıcı seçerse, şu hataya neden olur. 

> <span style="color:cyan">**Remove-AzureStorageBlob -Context $context -Container vhds -Blob "classicvm-os-8698.vhd"**</span>
> 
> <span style="color:red">Remove-AzureStorageBlob : Uzak sunucu hata döndürdü: (412) burada şu anda blob üzerinde kiralama ve istekte hiçbir kiralama kimliği belirtildi... HTTP durum kodu: 412 - HTTP hata iletisi: Blob üzerinde şu anda bir kira yoktur ve istekte hiçbir kiralama kimliği belirtildi.</span>


## <a name="resolution-steps"></a>Çözüm adımları

### <a name="to-remove-classic-disks"></a>Klasik disk kaldırmak için
Azure portalında aşağıdaki adımları izleyin:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  İçin Disks(classic) gidin. 
3.  Diskler sekmesine tıklayın. ![Kapsayıcı blob "list" bölmesindeki portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_disks_tab.jpg)
 
4.  Veri diskinizi seçin ve Diski Sil’e tıklayın.
 ![Kapsayıcı blob "list" bölmesindeki portalı ekran görüntüsü açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_delete_disk.jpg)
 
5.  Daha önce başarısız silme işlemini yeniden deneyin.
6.  Tek bir diske sahip olduğu sürece, bir depolama hesabı veya kapsayıcı silinemez.

### <a name="to-remove-classic-images"></a>Klasik görüntü kaldırmak için   
Azure portalında aşağıdaki adımları izleyin:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  İşletim sistemi görüntüleri (Klasik) gidin.
3.  Görüntüyü silin.
4.  Daha önce başarısız silme işlemini yeniden deneyin.
5.  Tek bir görüntü olduğu sürece, bir depolama hesabı veya kapsayıcı silinemez.
