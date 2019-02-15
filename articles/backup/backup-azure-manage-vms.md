---
title: Resource Manager tarafından dağıtılan sanal makine yedeklemelerini yönetme
description: Resource Manager tarafından dağıtılan sanal makine yedeklemeleri izlemek ve yönetme hakkında bilgi edinin
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 11/28/2016
ms.author: sogup
ms.openlocfilehash: 118e32994ed6471c52726e826ecfd42620bd3a91
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269595"
---
# <a name="manage-azure-virtual-machine-backups"></a>Azure sanal makine yedeklemelerini yönetme

Bu makalede VM yedeklemelerini yönetme hakkında rehberlik sağlar ve portal panosunda yedekleme uyarıları bilgileri açıklar. Kurtarma Hizmetleri kasaları ile Vm'leri kullanarak bu makaledeki yönergeler geçerlidir. Bu makalede, sanal makinelerin oluşturulmasını kapsamaz ya da sanal makineleri korumak nasıl açıklayan yapar. Azure Resource Manager tarafından dağıtılan Vm'leri Azure kurtarma Hizmetleri kasasıyla koruma hakkında bilgi için bkz. [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Kasaları ve korumalı sanal makineleri yönetme
Azure portalında kurtarma Hizmetleri kasası Pano kasası dahil olmak üzere hakkındaki bilgilere erişim sağlar:

* en son geri yükleme noktası da olan en son yedek anlık görüntüsü
* Yedekleme İlkesi
* tüm yedekleme anlık görüntülerinin toplam boyutu
* Kasa ile korunan bir sanal makine sayısı

Bir sanal makine yedekleme ile birçok yönetim görevleri, kasa panosunda açma ile başlar. Ancak, kasalar, birden çok öğe (veya birden çok VM), belirli bir VM'nin hakkındaki ayrıntıları görüntülemek için korumak için kullanılabildiğinden, kasa öğesi panoyu açın. Aşağıdaki yordamda nasıl açılacağı gösterilmektedir *kasa panosunda* ve ardından Devam *kasa öğesi panosunda*. Kasaya ekleyin ve Pano komutuna PIN kullanarak Azure panosuna öğesi kasası işaret eden her iki yordamda "ipuçları" vardır. Panoya Sabitle kasa veya öğe için bir kısayol oluşturma yoludur. Kısayoldan sık kullanılan komutlar da yürütebilirsiniz.

> [!TIP]
> Birden çok Pano varsa ve dikey pencereleri açılır, koyu mavi kaydırıcıyı penceresinin en altında Azure panosuna geri ve İleri kaydırarak açmak için kullanın.
>
>

![Kaydırıcı ile tam görünümü](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Kurtarma Hizmetleri kasası panosunda açın:
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

    Kurtarma Hizmetleri kasalarının listesi görüntülenir.

    ![Kasaları kurtarma Hizmetleri listesi ](./media/backup-azure-manage-vms/list-o-vaults.png)

   > [!TIP]
   > Bir kasa Azure panoya sabitlemeniz halinde, kasa, Azure portalını açtığınızda hemen erişilebilir. Kasa listesinde bir kasa panoya sabitlemek için kasaya sağ tıklatın ve seçin **panoya Sabitle**.
   >
   >
3. Panosunu açmak için kasaya kasalarının listesinden seçin. Kasa panosunda kasayı seçtiğinizde ve **ayarları** dikey penceresi açılır. Aşağıdaki görüntüde, **Contoso-vault** Pano vurgulanır.

    ![Kasa panosunu ve ayarlar dikey penceresi açın](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Kasa öğesi panosunu açın
Önceki yordamda kasa panosu açılır. Kasa öğesi panosunu açmak için:

1. Kasa panosunda üzerinde **yedekleme öğeleri** 'a tıklayın **Azure sanal makineler**.

    ![Yedekleme öğeleri Aç kutucuğu](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    **Yedekleme öğeleri** son yedekleme işini her öğe için dikey penceresinde listelenir. Bu örnekte, bir bu kasa tarafından korunan sanal makinenin, demovm-markgal yoktur.  

    ![Yedekleme öğeleri kutucuğu](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Erişim Kolaylığı için bir kasa öğesi Azure panoya sabitleyebilirsiniz. Kasa öğe listesinden bir kasa öğesi sabitlemek için öğeye sağ tıklayın ve seçin **panoya Sabitle**.
   >
   >
2. İçinde **yedekleme öğeleri** dikey penceresinde, kasaya öğesi panosunu açmak için öğeye tıklayın.

    ![Yedekleme öğeleri kutucuğu](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Kasa panosunda öğesi ve kendi **ayarları** dikey penceresi açılır.

    ![Yedekleme öğeleri panoyla ayarlar dikey penceresi](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Kasa öğesi panosundan gibi birçok önemli görevleri gerçekleştirebilirsiniz:

   * ilkeleri değiştirme veya yeni bir yedekleme ilkesi oluşturma
   * geri yükleme noktalarını görüntüleyin ve tutarlılık durumlarına bakın
   * İsteğe bağlı bir sanal makine yedeklemesi
   * sanal makine korumasını durdurma
   * bir sanal makinenin korumasını sürdürme
   * Yedekleme verileri (veya kurtarma noktası) Sil
   * [Yedekleme diskleri geri yükle](backup-azure-arm-restore-vms.md#create-new-restore-disks)

Aşağıdaki yordamlar için başlangıç noktası kasa öğesi panodur.

## <a name="manage-backup-policies"></a>Yedekleme ilkelerini yönetme
1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **tüm ayarlar** açmak için **ayarları** dikey penceresi.

    ![Yedekleme İlkesi dikey penceresi](./media/backup-azure-manage-vms/all-settings-button.png)
2. Üzerinde **ayarları** dikey penceresinde tıklayın **yedekleme İlkesi** o dikey penceresini açın.

    Yedekleme sıklığı ve bekletme aralığı ayrıntıları dikey penceresinde gösterilir.

    ![Yedekleme İlkesi dikey penceresi](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Gelen **yedekleme ilkesi seçmek** menüsü:

   * İlkeleri değiştirmek için farklı bir ilke seçin ve **Kaydet**. Yeni ilke hemen kasaya uygulanır.
   * Bir ilke oluşturmak için Seç **Yeni Oluştur**.

     ![Sanal makine yedekleme](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Bir yedekleme ilkesi oluşturma yönergeleri için bkz: [yedekleme ilkesi tanımlama](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Yedekleme ilkelerini yönetirken takip ettiğinizden emin olun [en iyi uygulamalar](backup-azure-vms-introduction.md#best-practices) ideal yedekleme performansı için
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>İsteğe bağlı bir sanal makine yedeklemesi
Koruma için yapılandırıldıktan sonra isteğe bağlı bir sanal makineye bir yedekleme oluşturabilirsiniz. İlk yedekleme beklemede, isteğe bağlı yedekleme kurtarma Hizmetleri kasasında sanal makinenin tam bir kopyasını oluşturur. İlk Yedekleme tamamlandıktan, isteğe bağlı yedekleme yalnızca değişiklikler önceki anlık görüntüden kurtarma Hizmetleri Kasası'na gönderir. Diğer bir deyişle, sonraki yedeklemeler her zaman artımlı.

> [!NOTE]
> İsteğe bağlı yedekleme bekletme aralığı günlük yedekleme noktası ilkesinde belirtilen bekletme değerdir. Sonra hiçbir günlük yedekleme noktası seçtiyseniz haftalık yedekleme noktası kullanılır.
>
>

Bir sanal makinenin bir isteğe bağlı yedekleme tetiklemek için:

* Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **Şimdi Yedekle**.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-button.png)

    Portal, isteğe bağlı yedekleme işini başlatmak istediğinizden emin olur. Tıklayın **Evet** yedekleme işini başlatmak için.

    ![Yedekleme şimdi düğmesi](./media/backup-azure-manage-vms/backup-now-check.png)

    Yedekleme işi bir kurtarma noktası oluşturur. Kurtarma noktası bekletme aralığını sanal makineyle ilişkilendirilmiş ilkesinde belirtilen bekletme aralığı ile aynıdır. Kasa panosunda, işin ilerleme durumunu izlemek için tıklatın **yedekleme işleri** Döşe.  

## <a name="stop-protecting-virtual-machines"></a>sanal makine korumasını durdurma
Bir sanal makine korumasını durdurmayı seçerseniz, Kurtarma noktalarını tutmak isteyip istemediğiniz sorulur. Sanal makineleri korumayı durdurmanın iki yolu vardır:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silme, veya
* Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktalarını bırakma

Kurtarma noktalarını depolama alanında bırakmanın bir maliyeti yoktur. Ancak, Kurtarma noktalarını bırakmanın avantajı, daha sonra sanal makine geri yükleyebilmenizdir. Kurtarma noktalarını bırakmanın maliyeti hakkında bilgi için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/backup/). Tüm kurtarma noktalarını silmeyi seçerseniz, sanal makineyi geri yükleyemezsiniz.

Kurtarma noktası süresiz yedekleme öğesi bir bekletme ilkesi veya StopProtection ile Delete verilerle yeniden korumaya alınmış kadar korunur. Yeniden koruma olması durumunda kurtarma noktalarının bekletme ilişkili yeni ilkeyi belirler. Benzer şekilde yedeklemeyi Durdur yapmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olmaya başlar ve eski kurtarma noktaları, bir yedeklemeyi Durdur ile gerçekleştirdiğiniz kadar her zaman son kurtarma noktası korunur ancak bir bekletme ilkesi uyarınca dolacak verileri silin.

Bir sanal makine için korumayı durdurmak için:

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **yedeklemeyi Durdur**.

    ![Yedek düğmesini Durdur](./media/backup-azure-manage-vms/stop-backup-button.png)

    Yedeklemeyi Durdur dikey penceresi açılır.

    ![Yedekleme dikey penceresini Durdur](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Üzerinde **yedeklemeyi Durdur** dikey penceresinde tutmak mı yedekleme verilerini silmek isteyip istemediğinizi seçin. Bilgi kutusunu seçtiğiniz hakkında ayrıntılar sağlar.

    ![Korumayı Durdur](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Yedekleme verilerini koru seçerseniz, 4. adıma geçin. Yedekleme verilerini sil seçerseniz yedekleme işlerini durdurma ve kurtarma noktaları - silmek istediğinizi onaylamak öğesinin adını yazın.

    ![Doğrulama Durdur](./media/backup-azure-manage-vms/item-verification-box.png)

    Öğe adından emin değilseniz, adını görüntülemek için ünlem işareti gelin. Ayrıca, öğe altında adıdır **yedeklemeyi Durdur** dikey penceresinin üstünde.
4. İsteğe bağlı olarak sağlayan bir **neden** veya **yorum**.
5. Geçerli öğe için yedekleme işini durdurmak için tıklatın ![durdurma yedekleme düğmesi](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Bir bildirim iletisi, yedekleme işleri durduruldu bilmenizi sağlar.

    ![Korumayı durdurun onaylayın](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>bir sanal makinenin korumasını sürdürme
Varsa **yedekleme verilerini koru** seçeneği belirtildiyse sanal makine için korumayı durdurduğunuzda korumayı sürdürmek mümkündür. Varsa **yedekleme verilerini Sil** seçeneği belirtildiyse, sonra sanal makine için koruma devam edemiyor.

Sanal makine için korumayı sürdürmek için

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **yedeklemeyi Sürdür**.

    ![Korumayı Sürdür](./media/backup-azure-manage-vms/resume-backup-button.png)

    Yedekleme İlkesi dikey penceresi açılır.

   > [!NOTE]
   > Sanal makine yeniden korunuyor, farklı bir ilke ile sanal makine başlangıçta korunan ilkesinden seçebilirsiniz.
   >
   >
2. Bağlantısındaki [yedekleme ilkelerini yönetme](backup-azure-manage-vms.md#manage-backup-policies) sanal makine için ilkeyi atamak için.

    Sanal makine için yedekleme İlkesi uygulandıktan sonra aşağıdaki iletiyi görürsünüz.

    ![Başarıyla korumalı VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme
Sırasında bir sanal makineyle ilişkili yedekleme verilerini silmeniz **yedeklemeyi Durdur** işi veya yedekleme sonrasında dilediğiniz zaman işi tamamlandı. Hatta kurtarma noktalarını silmeden önce birkaç gün veya birkaç hafta beklemek yararlı olabilir. Kurtarma noktalarını geri yüklemekten farklı olarak, yedekleme verilerini silerken belirli kurtarma noktalarının silinmesini seçemezsiniz. Yedekleme verilerinizi silmeyi seçtiyseniz, öğeye ilişkilendirilmiş tüm kurtarma noktalarını silersiniz.

Aşağıdaki yordam, sanal makine için yedekleme işinin durdurulmuş veya devre dışı varsayılır. Yedekleme işini devre dışı bırakıldıktan sonra **yedeklemeyi Sürdür** ve **silme yedekleme** kasa öğesi panosunda seçenekleri kullanılabilir.

![Sürdürme ve Sil düğmeleri](./media/backup-azure-manage-vms/resume-delete-buttons.png)

İle bir sanal makinede yedekleme verilerini silmek için *yedekleme devre dışı*:

1. Üzerinde [kasa öğesi panosunda](backup-azure-manage-vms.md#open-a-vault-item-dashboard), tıklayın **silme yedekleme**.

    ![VM Türü](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    **Yedekleme verilerini Sil** dikey penceresi açılır.

    ![VM Türü](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Kurtarma noktalarını silme isteğinizi onaylamak için öğesinin adını yazın.

    ![Doğrulama Durdur](./media/backup-azure-manage-vms/item-verification-box.png)

    Öğe adından emin değilseniz, adını görüntülemek için ünlem işareti gelin. Ayrıca, öğe altında adıdır **yedekleme verilerini Sil** dikey penceresinin üstünde.
3. İsteğe bağlı olarak sağlayan bir **neden** veya **yorum**.
4. Geçerli öğe için yedekleme verileri silmek için tıklayın ![durdurma yedekleme düğmesi](./media/backup-azure-manage-vms/delete-button.png)

    Bir bildirim iletisi, yedekleme verileri silindi bilmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
* Bir kurtarma noktasından bir sanal makine yeniden oluşturma hakkında daha fazla bilgi için kullanıma [Azure Vm'leri geri yükleme](backup-azure-arm-restore-vms.md).
* Sanal makinelerinizi koruma hakkında daha fazla bilgi gerekirse bkz [ilk bakış: Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-vms-first-look-arm.md).
* Olayları izleme hakkında daha fazla bilgi için bkz. [Azure sanal makine yedeklemeleri için uyarıları izleme](backup-azure-monitor-vms.md).
