---
title: Azure'da bir kurtarma Hizmetleri kasasını silme
description: Bir kurtarma Hizmetleri kasasını silme işlemini açıklamaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: 01c20ce84f5c97b3a0ac437fe602861085b5052c
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827896"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede nasıl silineceğini açıklar bir [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası. Bu bağımlılıklar kaldırarak ve sonra da bir kasayı silme için yönergeler içerir.


## <a name="before-you-start"></a>Başlamadan önce

Korumalı sunucular veya yedekleme yönetim sunucuları kasayla ilişkili gibi bağımlılıkları olan bir kurtarma Hizmetleri kasası silinemiyor.<br/>
(Diğer bir deyişle, koruma durduruldu ancak yedekleme verileri bile), yedekleme verilerini içeren kasa silinemez.

Bağımlılıklar içeren bir kasayı silme, şu hata görüntülenir:

![Kasa hatası Sil](./media/backup-azure-delete-vault/error.png)

Bir kasa düzgün bir şekilde silmek için sırayla belirtilen adımları izleyin:
- Korumayı durdurma ve yedekleme verilerini silme
- Korumalı sunucular veya yedekleme yönetim sunucuları Sil
- Kasayı silin


## <a name="delete-backup-data-and-backup-items"></a>Yedekleme verilerini ve yedekleme öğeleri

Daha fazla okuma devam etmeden önce **[bu ](#before-you-start)** bölümü olan bağımlılıkları anlamanız ve kasa silme işlemi.

### <a name="for-protected-items-in-cloud"></a>Bulut için korumalı öğeler

Korumayı durdurma ve yedekleme verilerini sil uygulayın aşağıda:

1. Portal > Kurtarma Hizmetleri kasası > yedekleme öğeleri, bulutta korumalı öğeleri seçin.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Sağ tıklayın ve seçmek gereken her öğe için **yedeklemeyi Durdur**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png)

3. İçinde **yedeklemeyi Durdur** > **bir seçenek belirleyin**seçin **yedekleme verilerini Sil**.
4. Öğesinin adını yazın ve tıklayın **yedeklemeyi Durdur**.
   - Bu öğeyi silmek istediğiniz doğrular.
   - **Yedeklemeyi Durdur** doğruladıktan sonra düğmesini etkinleştirir.
   - Korumak ve verileri silme, kasayı silmek mümkün olmayacaktır.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Denetleme **bildirim** ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). Tamamlandıktan sonra hizmeti şu ileti görüntülenir: **Yedekleme durduruluyor ve silme yedek verileri için "*yedekleme öğesi*"** . **İşlem başarıyla tamamlandı**.
6. Tıklayın **Yenile** üzerinde **yedekleme öğeleri** yedekleme öğesi kaldırılıp kaldırılmadığını denetlemek için menü.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>MARS Aracısı

Korumayı durdurma ve yedekleme verilerini silmek için aşağıda listelenen sırayla adımları uygulayın:

- [1. adım: Yedekleme öğeleri MARS yönetim konsolundan silin](#step-1-delete-backup-items-from-mars-management-console)
- [2. adım: Azure Backup aracısını Portalı'ndan kaldırma](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>1\. adım: Yedekleme öğeleri MARS yönetim konsolundan silin

Sunucunun kullanılabilir olmaması nedeniyle bu adımı gerçekleştirmek bulamıyorsanız, ardından Microsoft Destek'e başvurun.

- MARS yönetim konsolunu başlatın, Git **eylemleri** bölmesi ve **yedeklemeyi zamanla**.
- Gelen **değiştirme veya bir zamanlanmış yedekleme durdurma** Sihirbazı seçeneğini **Bu yedekleme zamanlaması kullanmayı bırakmak ve depolanan tüm yedekleri silmek** tıklatıp **sonraki**.

    ![Değiştirme veya zamanlanmış yedekleme durdurma](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Gelen **bir zamanlanan yedeklemeyi Durdur** Sihirbazı'nı tıklatın **son**.

    ![Zamanlanan yedeklemeyi Durdur](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Güvenlik PIN'i girmeleri istenir. Gerçekleştirmek PIN oluşturmak için aşağıdaki adımları:
  - Azure Portal’da oturum açın.
  - Gözat **kurtarma Hizmetleri kasası** > **ayarları** > **özellikleri**.
  - Altında **güvenlik PIN'i**, tıklayın **Oluştur**. Bu PIN'i kopyalayın. (Bu PIN, geçerli yalnızca beş dakika)
- Yönetim Konsolu'nda (istemci uygulaması) PIN yapıştırın ve **Tamam**.

  ![Güvenlik PIN'i](./media/backup-azure-delete-vault/security-pin.png)

- İçinde **değişiklik yedekleme ilerleme** görürsünüz Sihirbazı *silinen yedekleme verileri 14 gün için tutulur. Bu tarihten sonra yedekleme verileri kalıcı olarak silinecek.*  

    ![Yedekleme Altyapısı Sil](./media/backup-azure-delete-vault/deleted-backup-data.png)

Şirket içinden veya silinen yedekleme öğeleri, portaldan sonraki adımları tamamlayın.

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>2\. adım: Azure Backup Aracısı Portalı'ndan kaldırma

Olun [1. adım](#step-1-delete-backup-items-from-mars-management-console) devam etmeden önce tamamlanır:

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

> [!NOTE]
> Aşağıdaki hata sonra listelenen adımları ilk görüyorsanız [yedekleme öğeleri yönetim konsolundan silinmesi](#step-1-delete-backup-items-from-mars-management-console).
>
>![silme işlemi başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Yönetim konsolundan yedekleri silmek için bu adımları gerçekleştirmek bulamıyorsanız, örneğin, sunucunun yönetim konsoluyla olmadığından Microsoft desteğine başvurun.

7. Denetleme **bildirim** ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). Tamamlandıktan sonra hizmeti şu ileti görüntülenir: **Yedekleme durduruluyor ve silme yedek verileri için "*yedekleme öğesi*"** . **İşlem başarıyla tamamlandı**.
8. Tıklayın **Yenile** üzerinde **yedekleme öğeleri** yedekleme öğesi kaldırılıp kaldırılmadığını denetlemek için menü.


### <a name="for-mabs-agent"></a>MABS Aracısı

Korumayı durdurma ve yedekleme verilerini silmek için aşağıda listelenen sırayla adımları uygulayın:

- [1. adım: Yedekleme öğeleri MABS yönetim konsolundan silin](#step-1-delete-backup-items-from-mabs-management-console)
- [2. adım: Azure yedekleme yönetim sunucuları Portalı'ndan kaldırma](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>1\. adım: Yedekleme öğeleri MABS yönetim konsolundan silin

Sunucunun kullanılabilir olmaması nedeniyle bu adımı gerçekleştirmek bulamıyorsanız, ardından Microsoft Destek'e başvurun.

**Yöntem 1** gerçekleştirmek korumasını durdurun ve yedekleme verilerini silmek için aşağıdaki adımları:

1.  DPM Yönetici Konsolu'nda **koruma** gezinti çubuğunda.
2.  Görüntü bölmesinde, kaldırmak istediğiniz koruma grubu üyesini seçin. Seçmek için sağ tıklama **Grup üyeleri koruma Durdur** seçeneği.
3.  Gelen **korumayı Durdur** iletişim kutusunda **korumalı verileri Sil** > **Sil çevrimiçi depolama** onay kutusunu ve ardından **Durdur Koruma**.

    ![Depolama birimini Sil](./media/backup-azure-delete-vault/delete-storage-online.png)

Korumalı üye durumu şimdi olarak değiştirildiğinde **etkin olmayan çoğaltma kullanılabilir**.

5. Etkin olmayan koruma grubuna sağ tıklayıp **etkin olmayan korumayı Kaldır**.

    ![Etkin olmayan korumayı Kaldır](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Gelen **etkin olmayan korumayı Kaldır** penceresinde **Sil çevrimiçi depolama** tıklatıp **Tamam**.

    ![Disk ve çevrimiçi çoğaltmaları Kaldır](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Yöntem 2** başlatma **MABS Yönetim** Konsolu. İçinde **veri koruma yöntemini seçin** bölümünden beklemediğiniz **çevrimiçi koruma istiyorum**.

  ![Veri koruma yöntemini seçin](./media/backup-azure-delete-vault/data-protection-method.png)

Şirket içinden veya silinen yedekleme öğeleri, portaldan sonraki adımları tamamlayın.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>2\. adım: Azure yedekleme yönetim sunucuları Portalı'ndan kaldırma

Olun [1. adım](#step-1-delete-backup-items-from-mabs-management-console) devam etmeden önce tamamlanır:

1. Kasa Panosu menüsünden tıklayın **Yedekleme Altyapısı**.
2. Tıklayın **yedekleme yönetim sunucuları** sunucuları görüntülemek için.

    ![Kasa panosunu açmak için seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Öğeye sağ tıklayın > **Sil**.
4. Üzerinde **Sil** menüsünde sunucunun adını yazın ve tıklayın **Sil**.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Aşağıdaki hata sonra listelenen adımları ilk görüyorsanız [yedekleme öğeleri yönetim konsolundan silinmesi](#step-2-from-portal-remove-azure-backup-management-servers).
>
>![silme işlemi başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Yönetim konsolundan yedekleri silmek için bu adımları gerçekleştirmek bulamıyorsanız, örneğin, sunucunun yönetim konsoluyla olmadığından Microsoft desteğine başvurun.

5. Denetleme **bildirim** ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). Tamamlandıktan sonra hizmeti şu ileti görüntülenir: **Yedekleme durduruluyor ve silme yedek verileri için "*yedekleme öğesi*"** . **İşlem başarıyla tamamlandı**.
6. Tıklayın **Yenile** üzerinde **yedekleme öğeleri** yedekleme öğesi kaldırılıp kaldırılmadığını denetlemek için menü.


## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

1. Tüm bağımlılıkları kaldırıldığında, kaydırma **Essentials** kasa menüsünden bölmesinde.
2. Tüm olmayan doğrulayın **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeleri hala Kasası'nda görünüp görünmediğini [kaldırılmalarına](#delete-backup-data-and-backup-items).

3. Kasa panosunda Kasası'nda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**. Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Azure Resource Manager istemcisini kullanarak kurtarma Hizmetleri kasasını silme

Kurtarma Hizmetleri kasasını silmek için bu seçenek, yalnızca tüm bağımlılıkları kaldırılır ve hala alıyorsanız önerilir *kasa silme hatası*.



- Gelen **Essentials** kasa menüsünden bölmesinde doğrulayın herhangi olmayan **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Yedekleme öğeleri varsa, ardından adımları gerçekleştirmek [yedekleme verilerini ve yedekleme öğeleri](#delete-backup-data-and-backup-items).
- Yeniden deneme [portalından kasayı silme](#delete-the-recovery-services-vault).
- Tüm bağımlılıkları kaldırılır ve hala alıyorsanız *kasa silme hatası* ; aşağıda verilen adımları gerçekleştirmek için sonra ARMClient aracını kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Gelen chocolatey'i yükleme [burada](https://chocolatey.org/) ve çalıştırma ARMClient yüklemek için aşağıdaki komutu:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Azure hesabınızda oturum açın ve şu komutu çalıştırın:

    `ARMClient.exe login [environment name]`

3. Azure portalında, abonelik kimliği ve kaynak grubu adı silmek istediğiniz kasayı toplayın.

ARMClient komut hakkında daha fazla bilgi için bu başvuru [belge](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silmek için Azure Resource Manager istemcisini kullanma

1. Abonelik kimliği, kaynak grubu adı ve kasa adını kullanarak aşağıdaki komutu çalıştırın. Komutu çalıştırdığınızda, herhangi bir bağımlılığın yoksa, kasa siler.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Kasa boş değilse, "Bu kasa içinde mevcut kaynaklar bulunduğundan kasa silinemiyor" hatasını alırsınız. Korumalı öğeleri kaldırmak için / kapsayıcı bir kasa içinde şunları yapın:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Azure portalında, kasa silinir doğrulayın.


## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasaları.<br/>
[Hakkında bilgi edinin](backup-azure-manage-windows-server.md) kurtarma Hizmetleri kasalarını izleme ve yönetme.
