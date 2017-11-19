---
title: "Resource Manager tarafından dağıtılan sanal makine yedeklerini yönetme | Microsoft Docs"
description: "Resource Manager tarafından dağıtılan sanal makine yedeklerini izlemek ve yönetmek hakkında bilgi edinin"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 597d8e12377ca19b0c58eb2fc8bdb7597c1c6c07
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="manage-azure-virtual-machine-backups"></a>Azure sanal makine yedeklemelerini yönetme
> [!div class="op_single_selector"]
> * [Azure VM yedeklemeleri yönetme](backup-azure-manage-vms.md)
> * [Klasik VM yedeklemeleri yönetme](backup-azure-manage-vms-classic.md)
>
>

Bu makalede VM yedekleri yönetmeyle ilgili yönergeler sağlar ve portal panosunda kullanılabilir yedekleme uyarıları bilgileri açıklar. Bu makalede yer alan yönergeleri VM'ler ile kurtarma Hizmetleri kasaları kullanılarak uygulanır. Bu makalede, sanal makinelerin oluşturulmasını kapsamaz ya da sanal makineleri korumak nasıl açıklayan yapar. Azure Resource Manager tarafından dağıtılan Vm'leri Azure kurtarma Hizmetleri kasası ile koruma öncü için bkz: [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Kasa ve korumalı sanal makineleri yönetme
Azure portalında kurtarma Hizmetleri kasa Panosu erişim kasası hakkında bilgi sağlar:

* Ayrıca en son geri yükleme noktası olan en son yedek anlık görüntüsü
* Yedekleme İlkesi
* tüm yedekleme anlık görüntülerinin toplam boyutu
* Kasa ile korunan bir sanal makine sayısı

Bir sanal makine yedekleme ile çok sayıda yönetim görevi, kasa panosunda açma ile başlar. Ancak, kasalarını birden çok öğe (veya birden çok VM), belirli bir VM hakkındaki ayrıntıları görüntülemek için korumak için kullanılabilir olduğundan, kasa öğesi panosunu açın. Aşağıdaki yordamda nasıl açılacağı gösterilmektedir *kasa Panosu* ve ardından Devam *kasa öğesi Panosu*. Kasa ekleme ve PIN Pano komutu kullanarak Azure panoya öğesi kasa çıkış noktası her iki yordamı "ipuçları" yoktur. Panoya Sabitle kasa veya öğe bir kısayol oluşturma yoludur. Ayrıca, sık kullanılan komutlar kısayoldan yürütebilir.

> [!TIP]
> Birden çok panolar varsa ve dikey pencere açılır ve geriye Azure Pano slayt için pencerenin alt kısmında Koyu mavi kaydırıcısını kullanın.
>
>

![Kaydırıcı ile tam görünümü](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Kurtarma Hizmetleri kasası panosunda açın:
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

    Kurtarma Hizmetleri kasalarının listesi görüntülenir.

    ![Liste kurtarma Hizmetleri kasaları ](./media/backup-azure-manage-vms/list-o-vaults.png)

   > [!TIP]
   > Azure Panoya bir kasa sabitlerseniz, Azure portal'ı açtığınızda, kasa hemen erişilebilir. Kasa listesinde bir kasa panoya sabitlemek için kasaya sağ tıklatın ve seçin **panoya Sabitle**.
   >
   >
3. Kasa listesinden kendi panoyu açmak için kasaya seçin. Kasa, kasa Panosu seçtiğinizde ve **ayarları** dikey penceresini açın. Aşağıdaki görüntüde **Contoso kasa** Pano vurgulanır.

    ![Kasa panosunu ve ayarlar dikey penceresi açın](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Kasa öğesi panosunu açın
Önceki yordamda kasa panosu açılır. Kasa öğesi panosunu açmak için:

1. Kasa panosunda üzerinde **yedekleme öğeleri** döşeme, tıklatın **Azure sanal makineleri**.

    ![Yedekleme öğeleri Aç döşeme](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    **Yedekleme öğeleri** dikey penceresinde her öğe için son yedekleme işi listelenir. Bu örnekte, bir bu kasası tarafından korunan sanal makinenin, demovm-markgal yoktur.  

    ![Yedekleme öğeleri döşeme](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Erişim Kolaylığı için bir kasa öğesi Azure panoya sabitleyebilirsiniz. Kasa öğe listesinden bir kasa öğesi sabitlemek için öğeyi sağ tıklatın ve seçin **panoya Sabitle**.
   >
   >
2. İçinde **yedekleme öğeleri** dikey penceresinde, kasaya öğesi panosunu açmak için öğesini tıklatın.

    ![Yedekleme öğeleri döşeme](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Kasa öğesi panosunu ve kendi **ayarları** dikey penceresini açın.

    ![Ayarlar dikey penceresi ile yedekleme öğeleri Panosu](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Kasa öğe panodan gibi birçok önemli yönetim görevleri gerçekleştirebilirsiniz:

   * ilkeleri değiştirin veya yeni bir yedekleme İlkesi Oluştur
   * geri yükleme noktaları görüntülemek ve tutarlılık durumlarına bakın
   * Bir sanal makinenin talep üzerine yedekleme
   * Sanal makineleri koruma Durdur
   * Bir sanal makine korumayı Sürdür
   * Yedekleme verilerini (veya kurtarma noktası) silme
   * [Yedekleme disklerini geri yükle](backup-azure-arm-restore-vms.md#restore-backed-up-disks)

Aşağıdaki yordamlar için başlangıç noktası kasası öğesi panosunu ' dir.

## <a name="manage-backup-policies"></a>Yedekleme ilkelerini yönetme
1. Üzerinde [kasa öğesi Panosu](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklatın **tüm ayarları** açmak için **ayarları** dikey.

    ![Yedekleme İlkesi dikey penceresi](./media/backup-azure-manage-vms/all-settings-button.png)
2. Üzerinde **ayarları** dikey penceresinde tıklatın **yedekleme İlkesi** bu dikey penceresini açın.

    Dikey penceresinde, yedekleme sıklığı ve bekletme aralığı ayrıntıları gösterilmiştir.

    ![Yedekleme İlkesi dikey penceresi](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Gelen **yedekleme ilkesi seçin** menüsü:

   * İlkeleri değiştirmek için başka bir ilke seçin ve **kaydetmek**. Yeni ilke hemen kasaya uygulanır.
   * Bir ilke oluşturmak için seçin **Yeni Oluştur**.

     ![Sanal makine yedekleme](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Bir yedekleme ilkesi oluşturma ile ilgili yönergeler için bkz: [yedekleme ilkesi tanımlama](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Yedekleme ilkelerini yönetirken takip ettiğinizden emin olun [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) en iyi yedekleme performansı
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>Bir sanal makinenin talep üzerine yedekleme
Koruma için yapılandırıldıktan sonra isteğe bağlı bir sanal makine yedekleme devam edebilir. İlk yedekleme bekleme durumundaysa, talep üzerine yedekleme kurtarma Hizmetleri kasasında sanal makinenin tam bir kopyasını oluşturur. İlk yedekleme tamamlandığında, bir talep üzerine yedekleme yalnızca değişiklikler önceki anlık görüntüden kurtarma Hizmetleri Kasası'na gönderir. Diğer bir deyişle, sonraki yedeklemeleri her zaman artımlı.

> [!NOTE]
> Bir talep üzerine yedekleme için saklama aralığı için günlük yedekleme noktası ilkesinde belirtilen bekletme değerdir. Hiçbir günlük yedekleme noktası seçtiyseniz haftalık yedekleme noktası kullanılır.
>
>

Bir sanal makinenin bir talep üzerine yedekleme tetiklemek için:

* Üzerinde [kasa öğesi Panosu](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklatın **Şimdi Yedekle**.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-button.png)

    Portal bir talep üzerine yedekleme işini başlatmak istediğinizden emin olur. Tıklatın **Evet** yedekleme işini başlatmak için.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-check.png)

    Yedekleme işi, bir kurtarma noktası oluşturur. Kurtarma noktası bekletme aralığını sanal makineyle ilişkili İlkesi'nde belirtilen bekletme aralığı ile aynıdır. Kasa panosunda işinin ilerleme durumunu izlemek için tıklatın **yedekleme işlerini** döşeme.  

## <a name="stop-protecting-virtual-machines"></a>Sanal makineleri koruma Durdur
Bir sanal makine korumayı durdurmak seçerseniz, Kurtarma noktalarını korumak istiyorsanız istenir. Sanal makineler korumayı durdurmak için iki yolu vardır:

* sonraki tüm yedekleme işleri durdurun ve tüm kurtarma noktalarını silin veya
* sonraki tüm yedekleme işleri durdurmak, ancak kurtarma noktaları bırakın

Depolama birimindeki kurtarma noktaları bırakarak ile ilişkili bir maliyeti yoktur. Ancak, Kurtarma noktaları bırakarak yararı daha sonra sanal makineyi geri isterseniz olur. Kurtarma noktaları bırakarak, maliyeti hakkında bilgi için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/backup/). Tüm kurtarma noktalarını silmeye karar verirseniz, sanal makine geri yükleyemezsiniz.

Bir sanal makine için korumayı durdurmak için:

1. Üzerinde [kasa öğesi Panosu](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklatın **Dur yedekleme**.

    ![Yedek düğmesini Durdur](./media/backup-azure-manage-vms/stop-backup-button.png)

    Durdur yedekleme dikey pencere açılır.

    ![Yedekleme dikey penceresini Durdur](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Üzerinde **durdurmak yedekleme** dikey penceresinde korumak veya yedekleme verilerini silmek isteyip istemediğinizi seçin. Bilgileri kutusunda seçiminizi hakkında ayrıntılar sağlar.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Yedekleme verilerini korumak seçerseniz, 4. adıma atlayın. Yedekleme verilerini silmek seçerseniz yedekleme işleri durdurur ve kurtarma noktalarını - silmek istediğinizi onaylamak öğesinin adını yazın.

    ![Doğrulama Durdur](./media/backup-azure-manage-vms/item-verification-box.png)

    Öğesinin adını emin değilseniz, adını görüntülemek için ünlem işareti gelin. Ayrıca, öğenin altında adıdır **durdurmak yedekleme** dikey pencerenin üstündeki.
4. İsteğe bağlı olarak sağlayan bir **neden** veya **açıklama**.
5. Geçerli öğe için yedekleme işi durdurmak için tıklatın ![Dur yedekleme düğmesi](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Bir bildirim iletisi, yedekleme işleri durduruldu bilmenizi sağlar.

    ![Durdurma onaylayın](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>Bir sanal makine korumayı Sürdür
Varsa **yedekleme verileri koru** seçeneği seçildi sanal makine için korumayı durduğunda korumayı sürdürmek mümkündür. Varsa **yedekleme verilerini Sil** seçeneği seçildi, sonra sanal makine için koruma devam edemiyor.

Sanal makine için korumayı sürdürmek için

1. Üzerinde [kasa öğesi Panosu](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklatın **yedeklemeyi Sürdür**.

    ![Korumayı Sürdür](./media/backup-azure-manage-vms/resume-backup-button.png)

    Yedekleme İlkesi dikey penceresi açılır.

   > [!NOTE]
   > Sanal makine yeniden korurken, başka bir ilke ile sanal makine başlangıçta korunan ilkesinden seçebilirsiniz.
   >
   >
2. Adımları [yedekleme ilkelerini yönetme](backup-azure-manage-vms.md#manage-backup-policies) sanal makine için ilke atamak için.

    Yedekleme İlkesi sanal makineye uygulandıktan sonra aşağıdaki iletiyi görürsünüz.

    ![Başarıyla korumalı VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini sil
Sırasında bir sanal makineyle ilişkili yedekleme verileri silebilirsiniz **Dur yedekleme** işi, veya yedekleme sonrasında dilediğiniz zaman işi tamamlandı. Hatta gün veya hafta kurtarma noktaları silmeden önce beklenecek yararlı olabilir. Kurtarma noktaları, yedek verileri silerken geri farklı olarak, belirli bir kurtarma noktalarını silmeye seçemezsiniz. Yedekleme verilerinizi silmek isterseniz öğeyle ilişkili tüm kurtarma noktalarını silin.

Aşağıdaki yordam, sanal makine için yedekleme işi durdurulur veya devre dışı varsayar. Yedekleme işini devre dışı bırakıldığında **yedeklemeyi Sürdür** ve **Delete yedekleme** kasası öğesi Panoda seçenekleri mevcuttur.

![Devam ettirme ve silme düğmeleri](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Bir sanal makineyle yedek verileri silmek için *yedekleme devre dışı*:

1. Üzerinde [kasa öğesi Panosu](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklatın **Delete yedekleme**.

    ![VM türü](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    **Yedekleme verilerini Sil** dikey pencere açılır.

    ![VM türü](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Kurtarma noktaları silme isteğinizi onaylamak için öğeyi adını yazın.

    ![Doğrulama Durdur](./media/backup-azure-manage-vms/item-verification-box.png)

    Öğesinin adını emin değilseniz, adını görüntülemek için ünlem işareti gelin. Ayrıca, öğenin altında adıdır **yedekleme verilerini Sil** dikey pencerenin üstündeki.
3. İsteğe bağlı olarak sağlayan bir **neden** veya **açıklama**.
4. Geçerli öğe için yedekleme verileri silmek için tıklatın ![Dur yedekleme düğmesi](./media/backup-azure-manage-vms/delete-button.png)

    Bir bildirim iletisi, yedekleme verilerini silinmiş bilmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bir sanal makine bir kurtarma noktasından yeniden oluşturma hakkında daha fazla bilgi için kullanıma [geri Azure Vm'leri](backup-azure-restore-vms.md). Sanal makinelerinizi koruma bilgi gerekirse bkz [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md). Olayları izleme hakkında daha fazla bilgi için bkz: [izlemek için Azure sanal makine yedeklerini uyarıları](backup-azure-monitor-vms.md).
