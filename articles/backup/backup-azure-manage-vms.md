---
title: Yönetme ve Azure Backup hizmetini kullanarak Azure VM yedeklemeleri izleme
description: Yönetme ve Azure Backup hizmetini kullanarak Azure VM yedeklemeleri izleme hakkında bilgi edinin.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: 0fa221721471772b066990ec2d33f0cedb960239
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453550"
---
# <a name="manage-azure-vm-backups"></a>Azure VM yedeklemelerini yönetme

Bu makalede Azure kullanarak yedeklenen sanal makinelerin (VM'ler) yönetmek nasıl [Azure Backup hizmeti](backup-overview.md). Makaleyi de bulabilirsiniz yedekleme bilgilerini kasa panosunda özetler.


Azure portalında kurtarma Hizmetleri kasası Pano bilgi, kasaya erişim sağlar. dahil olmak üzere:

* En son geri yükleme noktası da olan en son yedekleme.
* Yedekleme ilkesi.
* Tüm yedekleme anlık görüntülerinin toplam boyutu.
* Yedeklemeler için etkin sanal makine sayısı.

Panoyu kullanarak ve tek tek sanal makineleri aşağı ayrıntılara yedekleri yönetebilirsiniz. Makine yedeklemelerini başlatmak için kasa panosunda açın.

![Kaydırıcı ile tam Pano görünümü](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Panoda Vm'leri görüntüleme

Kasa panosunda Vm'leri görüntülemek için: 

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**. Kaynak listesinde **Kurtarma Hizmetleri** yazın. Siz yazarken liste girişinizi göre filtrelenir. Seçin **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Kullanım kolaylığı için seçin ve kasa sağ **panoya Sabitle**.
4. Kasa panosunda açın.
    ![Kasa panosunu ve ayarlar dikey penceresi açın](./media/backup-azure-manage-vms/full-view-rs-vault.png)

4. Üzerinde **yedekleme öğeleri** kutucuk seçin **Azure sanal makineler**.

    ![Yedekleme öğeleri kutucuğu açın](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. Üzerinde **yedekleme öğeleri** dikey penceresinde, her öğe için son yedekleme işi görürsünüz. Bu örnekte, bir sanal makine kasaya korur: demovm markgal.  

    ![Yedekleme öğeleri dikey penceresini görüntüleyin](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)


6. Kasa öğenin panodan oluşturmak veya yedekleme ilkelerini değiştirebilir, geri yükleme noktalarını görüntülemek, isteğe bağlı yedekleme, Dur çalıştırın veya VM'lerin korumayı sürdürmek, Kurtarma noktalarını silin ve geri yükleme çalıştırın.

    ![Yedekleme öğeleri panosunu ve ayarlar dikey penceresi](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policies"></a>Yedekleme ilkelerini yönetme

Bir yedekleme ilkesi yönetmek için:

1. Üzerinde [kasa öğesi panosunda](#view-vms-in-the-dashboard)seçin **tüm ayarlar**.

    ![Tüm ayarları seçeneği](./media/backup-azure-manage-vms/all-settings-button.png)
2. İçinde **ayarları**seçin **yedekleme İlkesi**.
3. Üzerinde **yedekleme ilkesi seçmek** menüsü:

   * İlkeleri geçiş, farklı bir ilke seçin ve ardından **Kaydet**. Yeni ilke hemen kasaya uygulanır.
   * Bir ilke oluşturmak için Seç **Yeni Oluştur**. Daha fazla bilgi için [bir yedekleme ilkesi yapılandırma](backup-azure-arm-vms-prepare.md#configure-a-backup-policy).

     ![Bir yedekleme ilkesi seçin](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin
Kendi korumasını ayarladıktan sonra sanal makinenin bir isteğe bağlı yedekleme çalıştırabilirsiniz. Bu ayrıntılar göz önünde bulundurun: 
- İlk yedekleme beklemede, isteğe bağlı yedekleme kurtarma Hizmetleri Kasası'nda VM tam bir kopyasını oluşturur.
- İlk yedekleme tamamlandığında, bir isteğe bağlı yedekleme yalnızca değişiklikler önceki anlık görüntüden kurtarma Hizmetleri Kasası'na gönderir. Diğer bir deyişle, sonraki yedeklemeler her zaman artımlı.
- İsteğe bağlı yedekleme bekletme aralığı, belirttiğiniz zaman, yedeklemeyi tetiklemek bekletme değerdir.

İsteğe bağlı yedekleme tetiklemek için:

1. Üzerinde [kasa öğesi panosunda](#view-vms-in-the-dashboard)altında **korumalı öğesi**seçin **yedekleme öğesi**.

    ![Yedekleme şimdi seçeneği](./media/backup-azure-manage-vms/backup-now-button.png)

2. Gelen **Yedekleme Yönetimi türü**seçin **Azure sanal makine**. **Yedekleme öğesi (Azure sanal makine)** dikey penceresi görünür.
3. Bir VM seçip **artık yedekleme** isteğe bağlı yedekleme oluşturmak için. **Şimdi Yedekle** dikey penceresi görünür.
4. İçinde **tut** alanında, tutulacak yedekler için bir tarih belirtin.

    ![Şimdi Yedekle Takvim](./media/backup-azure-manage-vms/backup-now-check.png)

5. Seçin **Tamam** yedekleme işini çalıştırmak için.

Kasa panosunda, işin ilerleme durumunu izlemek için **yedekleme işleri** Döşe.

## <a name="stop-protecting-a-vm"></a>Bir VM'yi korumayı durdurursanız

Bir sanal makine korumayı durdurmanın iki yolu vardır:

- Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin. Bu durumda, sanal Makineyi geri yükleme olanağınız olmayacaktır.
- Gelecek tarihli tüm yedekleme işlerini durdurma ve kurtarma noktalarını tutun. Kasada kurtarma noktalarını tutmak ücret ödemem gerekir ancak gerekirse VM geri yükleme mümkün olacaktır. Daha fazla bilgi için [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

>[!NOTE]
>Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur. Eski kurtarma noktalarını ilkesine göre dolacak, ancak bir son kurtarma noktası her zaman yedekleri durdurun ve verileri silmek kadar tutulacak.
>

Bir VM için korumayı durdurmak için:

1. Üzerinde [öğenin Pano kasası](#view-vms-in-the-dashboard)seçin **yedeklemeyi Durdur**.
2. Korumak veya yedekleme verileri silmek ve gerektiğinde Seçiminizi onaylayın isteyip istemediğinizi seçin. İsterseniz bir açıklama ekleyin. Öğenin adından emin değilseniz, adını görüntülemek için ünlem işareti gelin.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/retain-or-delete-option.png)

     Bir bildirim, yedekleme işlerini durdurduğunuzdan olduğunu bilmenizi sağlar.

## <a name="resume-protection-of-a-vm"></a>Bir sanal makinenin korumasını sürdürme

VM durdurulduğunda yedekleme verileri tutarsanız, daha sonra korumayı devam edebilir. Yedekleme verilerini silmeniz halinde, koruma sürdüremezsiniz.

Bir sanal makine için korumayı sürdürmek için:

1. Üzerinde [öğenin Pano kasası](#view-vms-in-the-dashboard)seçin **yedeklemeyi Sürdür**.

2. Bağlantısındaki [yedekleme ilkelerini yönetme](#manage-backup-policies) VM için ilkeyi atamak için. Sanal makinenin ilk koruma ilkesini seçin gerek yoktur.
3. VM yedekleme ilkesini uyguladıktan sonra aşağıdaki iletiyi görürsünüz:

    ![Başarıyla korunan bir sanal makine belirten ileti](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini sil

Sırasında bir sanal makinenin yedekleme verilerini silmeniz **yedeklemeyi Durdur** işi veya yedekleme işi tamamlandıktan sonra. Yedekleme verilerini silmeden önce bu ayrıntıları göz önünde bulundurun:

- Günler veya haftalar kurtarma noktalarını silmeden önce beklenecek bir fikir olabilir.
- İşlemi farklı yedekleme verilerini silerken kurtarma noktalarını geri yüklemek için silmek için belirli kurtarma noktalarının silinmesini seçemezsiniz. Yedekleme verilerinizi silerseniz, tüm ilişkili kurtarma noktalarını silin.

Durdurmak veya sanal makinenin yedekleme işi devre dışı sonra yedekleme verileri silebilirsiniz:


1. Üzerinde [kasa öğesi panosunda](#view-vms-in-the-dashboard)seçin **silme yedekleme**.

    ![Delete yedeği seçin](./media/backup-azure-manage-vms/delete-backup-buttom.png)

1. Kurtarma noktalarını silmek istediğinizi onaylamak için yedekleme öğesinin adını yazın.

    ![Kurtarma noktalarını silmek istediğinizi onaylayın](./media/backup-azure-manage-vms/item-verification-box.png)

1. Öğe için yedekleme verileri silmek için işaretleyin **Sil**. Bir bildirim iletisi, yedekleme verileri silindi bilmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [Azure Vm'leri sanal makinenin ayarlarını yedekleme](backup-azure-vms-first-look-arm.md).
- Bilgi edinmek için nasıl [Vm'leri geri yükleme](backup-azure-arm-restore-vms.md).
- Bilgi edinmek için nasıl [Azure VM yedeklemeleri izlemek](backup-azure-monitor-vms.md).
