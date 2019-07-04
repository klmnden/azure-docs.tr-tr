---
title: Azure'da bir kurtarma Hizmetleri kasasını silme
description: Bir kurtarma Hizmetleri kasasını silme işlemini açıklamaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: raynew
ms.openlocfilehash: e195d9a4b9d2bbe21848e083dbccf864188e0790
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508500"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede nasıl silineceğini açıklar bir [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası. Bu bağımlılıklar kaldırılıyor ve bir kasayı silme ve bir kasa tarafından zorla silme yönergelerini içerir.


## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce sunucusu olan bir kurtarma Hizmetleri kasası silinemiyor anlamak için kayıtlı ve yedekleme verileri tutan önemlidir.

- Düzgün bir şekilde bir kasayı silme için içerdiği sunucularının kaydını sil, kasa verileri kaldırın ve sonra kasayı silin.
- Bir kasayı silme denerseniz, yine de bağımlılıkları olan, bir hata iletisi verildikten ve kasa bağımlılıkları da dahil olmak üzere, el ile kaldırmanız gerekir:
    - Yedeklenen öğeleri
    - Korumalı sunucular
    - Yedekleme yönetim sunucuları (Azure Backup sunucusu, DPM) ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Kurtarma Hizmetleri Kasası'nda hiçbir veriyi saklamak ve kasayı silmek isteyip istemediğiniz kasa tarafından zorla silebilirsiniz.
- Bir kasayı silmeyi deneyin ancak kullanamazsanız, yedekleme verilerini almak için kasa yine de yapılandırılır.


## <a name="delete-a-vault-from-the-azure-portal"></a>Azure portalından bir kasayı silme

1. Kasa panosunda açın.  
2. Panoda tıklayın **Sil**. Silmek istediğiniz öğeyi doğrulayın.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Bir hata alırsanız, kaldırma [yedekleme öğeleri](#remove-backup-items), [altyapı sunucularını](#remove-azure-backup-management-servers), ve [kurtarma noktaları](#remove-azure-backup-agent-recovery-points)ve sonra kasayı silin.

![Kasa hatası Sil](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Azure Resource Manager istemcisini kullanarak kurtarma Hizmetleri kasasını silme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Gelen chocolatey'i yükleme [burada](https://chocolatey.org/) ve çalıştırma ARMClient yüklemek için aşağıdaki komutu:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Azure hesabınızda oturum açın ve şu komutu çalıştırın:

    `ARMClient.exe login [environment name]`

3. Azure portalında, abonelik kimliği ve kaynak grubu adı silmek istediğiniz kasayı toplayın.

ARMClient komut hakkında daha fazla bilgi için bu başvuru [belge](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silmek için Azure Resource Manager istemcisini kullanma

1. Abonelik kimliği, kaynak grubu adı ve kasa adını kullanarak aşağıdaki komutu çalıştırın. Komutu çalıştırdığınızda herhangi bir bağımlılığın yoksa, kasa siler.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Kasa boş değil, "Bu kasa içinde mevcut kaynaklar bulunduğundan kasa silinemiyor" hatasını alıyorsunuz. Korumalı öğeleri kaldırmak için / kapsayıcı bir kasa içinde şunları yapın:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Azure portalında, kasa silinir doğrulayın.


## <a name="remove-vault-items-and-delete-the-vault"></a>Kasa öğeleri kaldırıp kasayı silin

Kurtarma Hizmetleri kasası silinmeden önce tüm bağımlılıkları kaldırın.

### <a name="remove-backup-items"></a>Yedekleme öğeleri Kaldır

Bu yordam, yedekleme verilerini Azure dosyaları ' kaldırma gösteren bir örnek sağlar.

1. Tıklayın **yedekleme öğeleri** > **Azure depolama (Azure dosyaları)**

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Her Azure dosyaları öğeyi kaldırın ve sağ **yedeklemeyi Durdur**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png)


3. İçinde **yedeklemeyi Durdur** > **bir seçenek belirleyin**seçin **yedekleme verilerini Sil**.
4. Öğesinin adını yazın ve tıklayın **yedeklemeyi Durdur**.
   - Bu öğeyi silmek istediğiniz doğrular.
   - **Yedeklemeyi Durdur** doğruladıktan sonra düğmesini etkinleştirir.
   - Korumak ve verileri silme, kasayı silmek mümkün olmayacaktır.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. İsteğe bağlı olarak, neden veri silmekte olduğunuz bir neden belirtin ve yorum ekleyin.
6. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
7. İş tamamlandıktan sonra hizmeti bir ileti gönderir: **yedekleme işlemi durduruldu ve yedekleme verileri silindi**.
8. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="deleting-backup-items-from-management-console"></a>Yönetim Konsolu'nda yedekleme öğeleri silme

Yedekleme öğeleri yedekleme Altyapıdan silmek için şirket içi sunucunuza Yönetim Konsolu (MARS, Azure Backup sunucusu veya SC geri öğelerinizi burada korunan bağlı olarak DPM) gidin.

### <a name="for-mars-agent"></a>MARS Aracısı

- MARS yönetim konsolunu başlatın, Git **eylemleri** bölmesi ve **yedeklemeyi zamanla**.
- Gelen **değiştirme veya bir zamanlanmış yedekleme durdurma** Sihirbazı seçeneğini **Bu yedekleme zamanlaması kullanmayı bırakmak ve depolanan tüm yedekleri silmek** tıklatıp **sonraki**.

    ![Değiştirme veya zamanlanmış yedekleme durdurma](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Gelen **bir zamanlanan yedeklemeyi Durdur** Sihirbazı'nı tıklatın **son**.

    ![Zamanlanan yedeklemeyi Durdur](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Güvenlik PIN'i girmeleri istenir. Gerçekleştirmek PIN oluşturmak için aşağıdaki adımları:
  - Azure Portal’da oturum açın.
  - Gözat **kurtarma Hizmetleri kasası** > **ayarları** > **özellikleri**.
  - Altında **güvenlik PIN'i**, tıklayın **Oluştur**. Bu PIN'i kopyalayın. (Bu PIN'i yalnızca beş dakikalığına geçerli olan)
- Yönetim Konsolu'nda (istemci uygulaması) PIN yapıştırın ve **Tamam**.

  ![Güvenlik PIN'i](./media/backup-azure-delete-vault/security-pin.png)

- İçinde **değişiklik yedekleme ilerleme** görürsünüz Sihirbazı *silinen yedekleme verileri 14 gün için tutulur. Bu tarihten sonra yedekleme verileri kalıcı olarak silinecek.*  

    ![Yedekleme Altyapısı Sil](./media/backup-azure-delete-vault/deleted-backup-data.png)

Şirket içinden veya silinen yedekleme öğeleri, tamamlamak aşağıdaki Portal adımları:
- MARS adımları için [kaldırmak Azure Yedekleme aracısı kurtarma noktaları](#remove-azure-backup-agent-recovery-points)

### <a name="for-mabs-agent"></a>MABS Aracısı

Çevrimiçi korumayı Durdur/silme, aşağıdakilerden birini gerçekleştirmek için farklı yöntemler vardır. aşağıdaki yöntemlerden:

**Yöntem 1**

Başlatma **MABS Yönetim** Konsolu. İçinde **veri koruma yöntemini seçin** bölümünden beklemediğiniz **çevrimiçi koruma istiyorum**.

  ![Veri koruma yöntemini seçin](./media/backup-azure-delete-vault/data-protection-method.png)

**Yöntem 2**

Bir koruma grubunu silmek için önce grubun korumasını durdurmanız gerekir. Korumayı durdurma ve koruma grubunu silme işlemi etkinleştirmek için aşağıdaki yordamı kullanın.

1.  DPM Yönetici Konsolu'nda **koruma** gezinti çubuğunda.
2.  Görüntü bölmesinde, kaldırmak istediğiniz koruma grubu üyesini seçin. Seçmek için sağ tıklama **Grup üyeleri koruma Durdur** seçeneği.
3.  Gelen **korumayı Durdur** iletişim kutusunda **korumalı verileri Sil** > **Sil çevrimiçi depolama** onay kutusunu ve ardından **Durdur Koruma**.

    ![Depolama birimini Sil](./media/backup-azure-delete-vault/delete-storage-online.png)

Korumalı üye durumu şimdi olarak değiştirildiğinde **etkin olmayan çoğaltma kullanılabilir**.

5. Etkin olmayan koruma grubuna sağ tıklayıp **etkin olmayan korumayı Kaldır**.

    ![Etkin olmayan korumayı Kaldır](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Gelen **etkin olmayan korumayı Kaldır** penceresinde **Sil çevrimiçi depolama** tıklatıp **Tamam**.

    ![Disk ve çevrimiçi çoğaltmaları Kaldır](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

Şirket içinden veya silinen yedekleme öğeleri, tamamlamak aşağıdaki Portal adımları:
- Bağlantısındaki MABS ve DPM için [kaldırmak Azure yedekleme yönetim sunucuları](#remove-azure-backup-management-servers).


### <a name="remove-azure-backup-management-servers"></a>Azure yedekleme yönetim sunucuları kaldırın

Azure yedekleme yönetim sunucusu kaldırılmadan önce listelenen adımları gerçekleştirmek emin olun [yedekleme öğeleri yönetim konsolundan silinmesi](#deleting-backup-items-from-management-console).

1. Kasa Panosu menüsünden tıklayın **Yedekleme Altyapısı**.
2. Tıklayın **yedekleme yönetim sunucuları** sunucuları görüntülemek için.

    ![Kasa panosunu açmak için seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Öğeye sağ tıklayın > **Sil**.
4. Üzerinde **Sil** menüsünde sunucunun adını yazın ve tıklayın **Sil**.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)
5.  İsteğe bağlı olarak, neden veri silmekte olduğunuz bir neden belirtin ve yorum ekleyin.

> [!NOTE]
> Aşağıdaki hata sonra listelenen adımları ilk görüyorsanız [yedekleme öğeleri yönetim konsolundan silinmesi](#deleting-backup-items-from-management-console).
>
>![silme işlemi başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Yönetim konsolundan yedekleri silmek için bu adımları gerçekleştirmek bulamıyorsanız, örneğin, sunucunun yönetim konsoluyla olmadığından Microsoft desteğine başvurun.

6. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
7. İş tamamlandıktan sonra hizmeti bir ileti gönderir: **yedekleme işlemi durduruldu ve yedekleme verileri silindi**.
8. Üzerinde listesindeki bir öğeyi sildikten sonra **Yedekleme Altyapısı** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.


### <a name="remove-azure-backup-agent-recovery-points"></a>Azure Yedekleme aracısı kurtarma noktalarını kaldırmak

Azure yedekleme kurtarma noktasını kaldırmadan önce listelenen adımları gerçekleştirmek emin olun [yedekleme öğeleri yönetim konsolundan silinmesi](#deleting-backup-items-from-management-console).

1. Kasa Panosu menüsünden tıklayın **Yedekleme Altyapısı**.
2. Tıklayın **korumalı sunucuların** altyapı sunucularını görüntüleme.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. İçinde **korumalı sunucuların** Azure Backup Aracısı'nı tıklatın.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Azure Backup Aracısı kullanılarak korunan sunucular listesinde sunucuya tıklayın.

    ![belirli bir korumalı sunucuyu seçin](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Seçili sunucu panosunda **Sil**.

    ![Seçili sunucuyu silin](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Üzerinde **Sil** menüsünden sunucu adını yazın ve tıklayın **Sil**.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. İsteğe bağlı olarak, neden veri silmekte olduğunuz bir neden belirtin ve yorum ekleyin.

> [!NOTE]
> Aşağıdaki hata sonra listelenen adımları ilk görüyorsanız [yedekleme öğeleri yönetim konsolundan silinmesi](#deleting-backup-items-from-management-console).
>
>![silme işlemi başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Yönetim konsolundan yedekleri silmek için bu adımları gerçekleştirmek bulamıyorsanız, örneğin, sunucunun yönetim konsoluyla olmadığından Microsoft desteğine başvurun. 

8. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
9. Üzerinde listesindeki bir öğeyi sildikten sonra **Yedekleme Altyapısı** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.


### <a name="delete-the-vault-after-removing-dependencies"></a>Bağımlılıklar kaldırdıktan sonra kasayı silin

1. Tüm bağımlılıkları kaldırıldığında, kaydırma **Essentials** kasa menüsünden bölmesinde.
2. Tüm olmayan doğrulayın **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeler kasaya görünmeye varsa bunları kaldırın.

3. Kasa panosunda Kasası'nda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**. Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Ne yedekleme işlemi durdurur ancak verileri tut?

Yedekleme işlemi durdurur, ancak yanlışlıkla veri korumak, önceki bölümlerde açıklandığı gibi yedekleme verilerini silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasaları.
