---
title: Yönetme ve Azure Backup hizmeti ile Azure VM yedeklemeleri izleme
description: Yönetme ve izleme Azure Backup hizmeti ile Azure VM yedeklemesi hakkında bilgi edinin.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: da686e53f711877642a33806719a0d71050e721b
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669389"
---
# <a name="manage-azure-vm-backups"></a>Azure VM yedeklemelerini yönetme

Bu makalede, Azure ile yedeklenen Vm'leri yönetme işlemi açıklanır [Azure Backup hizmeti](backup-overview.md) yedeklemeler ve portal panosunda yedekleme uyarıları bilgileri özetler.


Azure portalında kurtarma Hizmetleri kasası Pano kasası dahil olmak üzere hakkındaki bilgilere erişim sağlar:

* En son geri yükleme noktası da olan en son yedekleme.
* Yedekleme İlkesi
* Tüm yedekleme anlık görüntülerinin toplam boyutu
* Yedekleme için etkin sanal makine sayısı

Panoyu kullanarak yedeklemeleri yönetebilir ve tek tek sanal makineleri aşağı araştırıp bulma. achine & yedekleme kasası panosunda açma ile başlar. 

![Kaydırıcı ile tam görünümü](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-in-the-dashboard"></a>Panodaki Vm'leri görüntüleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın. 

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Kullanım kolaylığı için kasası listesini kasaya sağ tıklayın > **panoya Sabitle**.
4. Kasa panosunda açın. 
    ![Kasa panosunu ve ayarlar dikey penceresi açın](./media/backup-azure-manage-vms/full-view-rs-vault.png)

4. Üzerinde **yedekleme öğeleri** 'a tıklayın **Azure sanal makineler**.

    ![Yedekleme öğeleri Aç kutucuğu](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. İçinde **yedekleme öğeleri** dikey penceresinde, her öğe için son yedekleme işini görebilirsiniz. Bu örnekte, bir bu kasa tarafından korunan sanal makinenin, demovm-markgal yoktur.  

    ![Yedekleme öğeleri kutucuğu](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

 
6. Kasa öğesi panosundan oluşturmak veya yedekleme ilkelerini değiştirebilir, geri yükleme noktalarını görüntülemek, isteğe bağlı yedekleme, Dur çalıştırın ve VM'lerin korumayı sürdürmek, Kurtarma noktalarını silin ve geri yükleme çalıştırın.

    ![Yedekleme öğeleri panoyla ayarlar dikey penceresi](./media/backup-azure-manage-vms/item-dashboard-settings.png)



## <a name="manage-backup-policies"></a>Yedekleme ilkelerini yönetme
1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **tüm ayarlar** .

    ![Yedekleme İlkesi dikey penceresi](./media/backup-azure-manage-vms/all-settings-button.png)
2. İçinde **ayarları**, tıklayın**yedekleme İlkesi**e.
3. Gelen **yedekleme ilkesi seçmek** menüsü:

   * İlkeleri değiştirmek için farklı bir ilke seçin ve **Kaydet**. Yeni ilke hemen kasaya uygulanır.
   * Bir ilke oluşturmak için Seç **Yeni Oluştur**. [Daha fazla bilgi](backup-azure-arm-vms-prepare.md#configure-a-backup-policy)

     ![Sanal makine yedekleme](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin
Koruma için yapılandırıldıktan sonra isteğe bağlı bir sanal makinenin yedekleme oluşturabilirsiniz.
- İlk yedekleme beklemede, isteğe bağlı yedekleme kurtarma Hizmetleri kasasında sanal makinenin tam bir kopyasını oluşturur.
- İlk Yedekleme tamamlandıktan, isteğe bağlı yedekleme yalnızca değişiklikler önceki anlık görüntüden kurtarma Hizmetleri Kasası'na gönderir. Diğer bir deyişle, sonraki yedeklemeler her zaman artımlı.
- İsteğe bağlı yedekleme bekletme aralığı günlük yedekleme noktası ilkesinde belirtilen bekletme değerdir. Sonra hiçbir günlük yedekleme noktası seçtiyseniz haftalık yedekleme noktası kullanılır.


İsteğe bağlı yedekleme tetiklemek için:

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **Şimdi Yedekle**.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-button.png)

 2. Tıklayın **Evet** yedekleme işini başlatmak için.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-check.png)

 
 Yedekleme işi bir kurtarma noktası oluşturur. Kurtarma noktası bekletme aralığını sanal makineyle ilişkilendirilmiş ilkesinde belirtilen bekletme aralığı ile aynıdır. Kasa panosunda, işin ilerleme durumunu izlemek için tıklatın **yedekleme işleri** Döşe.  

## <a name="stop-protecting-a-vm"></a>Bir VM'yi korumayı durdurursanız

Sanal makineleri korumayı durdurmanın iki yolu vardır:

- Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin. Bu durumda VM geri yükleme olanağınız olmayacaktır.
- Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktalarını bırakma. Kurtarma noktalarını depolama alanında bırakmanın bir maliyeti yoktur. Gerekirse VM'ye geri yükleyebilirsiniz ancak, Kurtarma noktalarını bırakmanın avantajı vardır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup/) fiyatlandırma ayrıntıları hakkında.

Şunlara dikkat edin:

- Kurtarma noktalarını değiştirmeden bırakırsanız, noktaları yedekleme ilkesine uygun olarak temizlenir. Tüm kurtarma noktalarını temizlenir kadar korumalı örnek ve tüketilen depolama alanı için ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup/) fiyatlandırma hakkında daha fazla.
- Süreleri dolduğunda bekletme ilkesi uyarınca olsa da, Kurtarma noktaları olduğu gibi bırakın, açıkça yedekleme verilerini silene kadar Azure yedekleme her zaman bir son kurtarma noktasını korur.
- Durdurma yedekleme olmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olmaya başlar. Yine, eski kurtarma noktalarını ilkesine göre dolacak, ancak yedeklemeyi Durdur veriler silinene kadar her zaman bir son kurtarma noktası korunur.



Bir sanal makine için korumayı durdurmak için:

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **yedeklemeyi Durdur**.
2. Korumak veya yedekleme verileri silmek ve gerektiğinde onaylayın isteyip istemediğinizi seçin. Gerektiği şekilde doğrulayın ve isteğe bağlı olarak bir açıklama sağlayın. Öğe adından emin değilseniz, adını görüntülemek için ünlem işareti gelin.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/retain-or-delete-option.png)

 Bir bildirim iletisi, yedekleme işleri durduruldu bilmenizi sağlar.


## <a name="resume-protection-of-a-vm"></a>Bir sanal makinenin korumasını sürdürme

VM durdurulduğunda yedekleme verileri korunur, korumasını sürdürebilirsiniz. Ardından yedekleme verileri silindi durumunda sürdüremezsiniz.

metin

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **yedeklemeyi Sürdür**.

2. Bağlantısındaki [yedekleme ilkelerini yönetme](backup-azure-manage-vms.md#manage-backup-policies) sanal makine için ilkeyi atamak için. farklı bir ilke ile sanal makine başlangıçta korunan ilkesinden seçebilirsiniz.
3. Sanal makine için yedekleme İlkesi uygulandıktan sonra aşağıdaki iletiyi görürsünüz.

    ![Başarıyla korumalı VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

Sırasında bir VM ile ilişkili yedekleme verilerini silmeniz **yedeklemeyi Durdur** işi veya yedekleme sonrasında dilediğiniz zaman işi tamamlandı.

- Hatta kurtarma noktalarını silmeden önce birkaç gün veya birkaç hafta beklemek yararlı olabilir.
- Kurtarma noktalarını geri yüklemekten farklı olarak, yedekleme verilerini silerken belirli kurtarma noktalarının silinmesini seçemezsiniz. Yedekleme verilerinizi silmeyi seçtiyseniz, öğeye ilişkilendirilmiş tüm kurtarma noktalarını silersiniz.

Bu yordam, VM için yedekleme işinin durdurulmuş veya devre dışı varsayar.


1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **silme yedekleme**.

    ![VM Türü](./media/backup-azure-manage-vms/delete-backup-buttom.png)

2. Kurtarma noktalarını silme isteğinizi onaylamak için öğesinin adını yazın.

    ![Doğrulama Durdur](./media/backup-azure-manage-vms/item-verification-box.png)

4. Geçerli öğe için yedekleme verileri silmek için tıklayın ![durdurma yedekleme düğmesi](./media/backup-azure-manage-vms/delete-button.png)

    Bir bildirim iletisi, yedekleme verileri silindi bilmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- [Hakkında bilgi edinin](backup-azure-vms-first-look-arm.md) VM ayarlarından Azure VM'lerin yedeklenmesi.
- [Hakkında bilgi edinin](backup-azure-arm-restore-vms.md) Vm'leri geri yükleme. 
- [Hakkında bilgi edinin](backup-azure-monitor-vms.md) Azure VM yedeklemeleri izleme.
 
