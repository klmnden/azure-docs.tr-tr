---
title: Yönetme ve Azure Backup hizmetini kullanarak Azure VM yedeklemeleri izleme
description: Yönetme ve Azure Backup hizmetini kullanarak Azure VM yedeklemeleri izleme hakkında bilgi edinin.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sogup
ms.openlocfilehash: add2c72535b5be0edcbc00c077dfe20a6deaa3e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434239"
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

5. Üzerinde **yedekleme öğeleri** kutucuk seçin **Azure sanal makineler**.

    ![Yedekleme öğeleri kutucuğu açın](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. Üzerinde **yedekleme öğeleri** dikey penceresinde, korumalı VM'lerin listesini görüntüleyebilirsiniz. Bu örnekte, bir sanal makine kasaya korur: demobackup.  

    ![Yedekleme öğeleri dikey penceresini görüntüleyin](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. Kasa öğenin panosundan yedekleme ilkelerini değiştirme, isteğe bağlı yedekleme, Dur çalıştırın veya VM'lerin korumayı sürdürmek, yedekleme verilerini sil, geri yükleme noktalarını görüntülemek ve geri yükleme çalıştırın.

    ![Yedekleme öğeleri panosunu ve ayarlar dikey penceresi](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Bir VM için yedekleme ilkesini yönetme

Bir yedekleme ilkesi yönetmek için:

1. [Azure Portal](https://portal.azure.com/) oturum açın. Kasa panosunda açın.
2. Üzerinde **yedekleme öğeleri** kutucuk seçin **Azure sanal makineler**.

    ![Yedekleme öğeleri kutucuğu açın](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. Üzerinde **yedekleme öğeleri** dikey penceresinde, korumalı VM'ler ve en son geri yükleme noktaları süresiyle son yedekleme durumu listesini görüntüleyebilirsiniz.

    ![Yedekleme öğeleri dikey penceresini görüntüleyin](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Kasa öğenin panodan bir yedekleme ilkesi seçebilirsiniz.

   * İlkeleri geçiş, farklı bir ilke seçin ve ardından **Kaydet**. Yeni ilke hemen kasaya uygulanır.

     ![Bir yedekleme ilkesi seçin](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin
Kendi korumasını ayarladıktan sonra sanal makinenin bir isteğe bağlı yedekleme çalıştırabilirsiniz. Bu ayrıntılar göz önünde bulundurun:

- İlk yedekleme beklemede, isteğe bağlı yedekleme kurtarma Hizmetleri Kasası'nda VM tam bir kopyasını oluşturur.
- İlk yedekleme tamamlandığında, bir isteğe bağlı yedekleme yalnızca değişiklikler önceki anlık görüntüden kurtarma Hizmetleri Kasası'na gönderir. Diğer bir deyişle, sonraki yedeklemeler her zaman artımlı.
- İsteğe bağlı yedekleme bekletme aralığı, belirttiğiniz zaman, yedeklemeyi tetiklemek bekletme değerdir.

İsteğe bağlı yedekleme tetiklemek için:

1. Üzerinde [kasa öğesi panosunda](#view-vms-on-the-dashboard)altında **korumalı öğesi**seçin **yedekleme öğesi**.

    ![Yedekleme şimdi seçeneği](./media/backup-azure-manage-vms/backup-now-button.png)

2. Gelen **Yedekleme Yönetimi türü**seçin **Azure sanal makine**. **Yedekleme öğesi (Azure sanal makine)** dikey penceresi görünür.
3. Bir VM seçip **artık yedekleme** isteğe bağlı yedekleme oluşturmak için. **Şimdi Yedekle** dikey penceresi görünür.
4. İçinde **tut** alanında, tutulacak yedekler için bir tarih belirtin.

    ![Şimdi Yedekle Takvim](./media/backup-azure-manage-vms/backup-now-check.png)

5. Seçin **Tamam** yedekleme işini çalıştırmak için.

Kasa panosunda, işin ilerleme durumunu izlemek için **yedekleme işleri** Döşe.

## <a name="stop-protecting-a-vm"></a>Bir VM'yi korumayı durdurursanız

Bir sanal makine korumayı durdurmanın iki yolu vardır:

* **Korumayı durdurma ve yedekleme verilerini koru**. Bu seçenek, VM'nizi korumaya gelen Gelecek tarihli tüm yedekleme işlerini durdurur; Ancak, Azure Backup hizmeti yedeklenmiş kurtarma noktalarını korur.  Kasada kurtarma noktalarını tutmak ücret ödemem gerekir (bkz [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/) Ayrıntılar için). Gerekirse VM'ye geri yüklenmesi mümkün olacaktır. Sanal makine korumayı sürdürmek karar sonra kullanabileceğiniz *yedeklemeyi Sürdür* seçeneği.
* **Korumayı durdurma ve yedekleme verilerini silme**. Bu seçenek, VM'nizi korumaya gelen Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin. VM'yi geri yükleme ya da kullanmak mümkün olmayacaktır *yedeklemeyi Sürdür* seçeneği.

>[!NOTE]
>Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur. Eski kurtarma noktalarını ilkesine göre dolacak, ancak bir son kurtarma noktası her zaman yedekleri durdurun ve verileri silmek kadar tutulacak.
>

### <a name="stop-protection-and-retain-backup-data"></a>Korumayı durdurma ve yedekleme verilerini koru

Korumayı durdurun ve verileri sanal makinenin korumak için:

1. Üzerinde [öğenin Pano kasası](#view-vms-on-the-dashboard)seçin **yedeklemeyi Durdur**.
2. Seçin **yedekleme verilerini koru**ve gerektiği şekilde Seçiminizi onaylayın. İsterseniz bir açıklama ekleyin. Öğenin adından emin değilseniz, adını görüntülemek için ünlem işareti gelin.

    ![Yedekleme verilerini koru](./media/backup-azure-manage-vms/retain-backup-data.png)

Bir bildirim, yedekleme işlerini durdurduğunuzdan olduğunu bilmenizi sağlar.

### <a name="stop-protection-and-delete-backup-data"></a>Korumayı durdurma ve yedekleme verilerini silme

Korumayı durdurma ve bir sanal makinenin verilerini silmek için:

1. Üzerinde [öğenin Pano kasası](#view-vms-on-the-dashboard)seçin **yedeklemeyi Durdur**.
2. Seçin **yedekleme verilerini Sil**ve gerektiği şekilde Seçiminizi onaylayın. Yedekleme öğesinin adını girin ve isterseniz bir açıklama ekleyin.

    ![Yedekleme verilerini silme](./media/backup-azure-manage-vms/delete-backup-data1.png)

## <a name="resume-protection-of-a-vm"></a>Bir sanal makinenin korumasını sürdürme

Tercih etmiş [korumasını Durdur ve yedekleme verilerini koru](#stop-protection-and-retain-backup-data) seçeneği sırasında VM korumasını durdurun, ardından kullanabilirsiniz **yedeklemeyi Sürdür**. Seçerseniz bu seçenek kullanılamaz [korumasını Durdur ve yedekleme verilerini Sil](#stop-protection-and-delete-backup-data) seçeneği veya [yedekleme verilerini Sil](#delete-backup-data).

Bir sanal makine için korumayı sürdürmek için:

1. Üzerinde [öğenin Pano kasası](#view-vms-on-the-dashboard)seçin **yedeklemeyi Sürdür**.

2. Bağlantısındaki [yedekleme ilkelerini yönetme](#manage-backup-policy-for-a-vm) VM için ilkeyi atamak için. Sanal makinenin ilk koruma ilkesini seçin gerek yoktur.
3. VM yedekleme ilkesini uyguladıktan sonra aşağıdaki iletiyi görürsünüz:

    ![Başarıyla korunan bir sanal makine belirten ileti](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

Bir sanal makinenin yedekleme verilerini silmek için iki yolu vardır:

- Kasa öğesi panosunda yedeklemeyi Durdur seçin ve yönergeleri izleyin [korumasını Durdur ve yedekleme verilerini Sil](#stop-protection-and-delete-backup-data) seçeneği.

  ![Yedeklemeyi Durdur seçin](./media/backup-azure-manage-vms/stop-backup-buttom.png)

- Kasa öğesi panosunda, yedekleme verilerini sil seçin. Bu seçeneğin etkinleştirilmesi için seçtiyseniz [korumasını Durdur ve yedekleme verilerini koru](#stop-protection-and-retain-backup-data) seçeneği sırasında VM korumasını durdurun

  ![Delete yedeği seçin](./media/backup-azure-manage-vms/delete-backup-buttom.png)

  - Üzerinde [kasa öğesi panosunda](#view-vms-on-the-dashboard)seçin **yedekleme verilerini Sil**.
  - Kurtarma noktalarını silmek istediğinizi onaylamak için yedekleme öğesinin adını yazın.

    ![Yedekleme verilerini silme](./media/backup-azure-manage-vms/delete-backup-data1.png)

  - Öğe için yedekleme verileri silmek için işaretleyin **Sil**. Bir bildirim iletisi, yedekleme verileri silindi bilmenizi sağlar.

  > [!NOTE]
  > Yedekleme verilerini sildiğinizde tüm ilişkili kurtarma noktalarını silin. Silmek için belirli kurtarma noktalarının seçemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [Azure Vm'leri sanal makinenin ayarlarını yedekleme](backup-azure-vms-first-look-arm.md).
- Bilgi edinmek için nasıl [Vm'leri geri yükleme](backup-azure-arm-restore-vms.md).
- Bilgi edinmek için nasıl [Azure VM yedeklemeleri izlemek](backup-azure-monitor-vms.md).
