---
title: Azure'da bir kurtarma Hizmetleri kasasını silme
description: Bir kurtarma Hizmetleri kasasını silme işlemini açıklamaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: raynew
ms.openlocfilehash: 94d66e28f8edbda6c41dcceaf427d7d7d869c90f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620126"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede nasıl silineceğini açıklar bir [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası. Bu bağımlılıklar kaldırılıyor ve bir kasayı silme ve bir kasa tarafından zorla silme yönergelerini içerir.


## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce sunucusu olan bir kurtarma Hizmetleri kasası silinemiyor anlamak için kayıtlı ve yedekleme verileri tutan önemlidir.

- Düzgün bir şekilde bir kasayı silme için da sunucularının kaydını sil, kasa verileri kaldırın ve sonra kasayı silin.
- Yine de bağımlılıklar içeren bir kasayı silme denerseniz, bir hata iletisi görüntülenir. ve kasa bağımlılıkları da dahil olmak üzere, el ile kaldırmanız gerekir:
    - Yedeklenen öğeleri
    - Korumalı sunucular
    - Yedekleme yönetim sunucuları (Azure Backup sunucusu, DPM) ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Kurtarma Hizmetleri Kasası'nda hiçbir veriyi saklamak ve kasayı silmek isteyip istemediğiniz kasa tarafından zorla silebilirsiniz.
- Bir kasayı silmeyi deneyin ancak kullanamazsanız, yedekleme verilerini almak için kasa yine de yapılandırılır.


## <a name="delete-a-vault-from-the-azure-portal"></a>Azure portalından bir kasayı silme

1. Kasa panosunda açın.  
2. Panoda tıklayın **Sil**. Silmek istediğiniz öğeyi doğrulayın.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Bir hata alırsanız, kaldırma [yedekleme öğeleri](#remove-backup-items), [altyapı sunucularını](#remove-backup-infrastructure-servers), ve [kurtarma noktaları](#remove-azure-backup-agent-recovery-points)ve sonra kasayı silin.

![Kasa hatası Sil](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Force tarafından kurtarma Hizmetleri kasasını silme

Bir kasa tarafından zorla PowerShell ile silebilirsiniz. Zorla silme kasasını ve ilişkili tüm yedekleme verileri kalıcı olarak silinip silinmediğini anlamına gelir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


Bir kasa tarafından zorla silmek için:

1. Azure aboneliğinizde oturum açın `Connect-AzAccount` komutunu ve izleyin ekrandaki yönergeleri izleyin.

   ```powershell
    Connect-AzAccount
   ```
2. İlk kez kullandığınız Azure Backup, Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider).

   ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

3. Yönetici ayrıcalıklarıyla bir PowerShell penceresi açın.
4. Kullanım `Set-ExecutionPolicy Unrestricted` herhangi bir kısıtlama kaldırmak için.
5. Azure Resource Manager istemci paketi chocolately.org indirmek için aşağıdaki komutu çalıştırın.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

6. Azure Resource Manager API istemcisi yüklemek için aşağıdaki komutu kullanın.

   `choco.exe install armclient`

7. Azure portalında, abonelik kimliği ve kaynak grubu adı silmek istediğiniz kasayı toplayın.
8. PowerShell'de, abonelik kimliği, kaynak grubu adı ve kasa adını kullanarak aşağıdaki komutu çalıştırın. Komutu çalıştırdığınızda, kasayı ve tüm bağımlılıklarını siler.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
9. Kasa boş değil, "Bu kasa içinde mevcut kaynaklar bulunduğundan kasa silinemiyor" hatasını alıyorsunuz. İçerilen bir kasa içinde kaldırmak için aşağıdakileri yapın:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

10. Azure portalında, kasa silinir doğrulayın.


## <a name="remove-vault-items-and-delete-the-vault"></a>Kasa öğeleri kaldırıp kasayı silin

Bu yordam, yedekleme verileri ve altyapı sunucularını kaldırmak için bazı örnekler sağlar. Her şeyi bir kasasından kaldırıldıktan sonra silebilirsiniz.

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

5. İsteğe bağlı olarak verileri neden silmekte olduğunuz bir neden belirtin ve yorum ekleyin.
6. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
7. İş tamamlandıktan sonra hizmeti bir ileti gönderir: **yedekleme işlemi durduruldu ve yedekleme verileri silindi**.
8. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>Yedekleme Altyapısı sunucuları kaldırın

1. Kasa Panosu menüsünden tıklayın **Yedekleme Altyapısı**.
2. Tıklayın **yedekleme yönetim sunucuları** sunucuları görüntülemek için. 

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

2. Öğeye sağ tıklayın > **Sil**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. . İçinde **yedeklemeyi Durdur** > **bir seçenek belirleyin**seçin **yedekleme verilerini Sil**.
4. Öğesinin adını yazın ve tıklayın **yedeklemeyi Durdur**. 
   - Bu öğeyi silmek istediğiniz doğrular.
   - **Yedeklemeyi Durdur** doğruladıktan sonra düğmesini etkinleştirir.
   - Korumak ve verileri silme, kasayı silmek mümkün olmayacaktır.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. İsteğe bağlı olarak verileri neden silmekte olduğunuz bir neden belirtin ve yorum ekleyin.
6. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
7. İş tamamlandıktan sonra hizmeti bir ileti gönderir: **yedekleme işlemi durduruldu ve yedekleme verileri silindi**.
8. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.


### <a name="remove-azure-backup-agent-recovery-points"></a>Azure Yedekleme aracısı kurtarma noktalarını kaldırmak

1. Kasa Panosu menüsünden tıklayın **Yedekleme Altyapısı**.
2. Tıklayın **yedekleme yönetim sunucuları** altyapı sunucularını görüntüleme.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. İçinde **korumalı sunucuların** Azure Backup Aracısı'nı tıklatın.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Azure Backup Aracısı kullanılarak korunan sunucular listesinde sunucuya tıklayın.

    ![belirli bir korumalı sunucuyu seçin](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Seçili sunucu panosunda **Sil**.

    ![Seçili sunucuyu silin](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Üzerinde **Sil** menüsünden öğesinin adını yazın ve tıklayın **Sil**.
   - Bu öğeyi silmek istediğiniz doğrular.
   - **Yedeklemeyi Durdur** doğruladıktan sonra düğmesini etkinleştirir.
   - Korumak ve verileri silme, kasayı silmek mümkün olmayacaktır.

     ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. İsteğe bağlı olarak verileri neden silmekte olduğunuz bir neden belirtin ve yorum ekleyin.
8. Silme işlemi tamamlanmış olduğunu doğrulamak için Azure iletileri denetleyin ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/messages.png).
7. İş tamamlandıktan sonra hizmeti bir ileti gönderir: **yedekleme işlemi durduruldu ve yedekleme verileri silindi**.
8. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.






### <a name="delete-the-vault-after-removing-dependencies"></a>Bağımlılıklar kaldırdıktan sonra kasayı silin

1. Tüm bağımlılıkları kaldırıldığında, kaydırma **Essentials** kasa menüsünden bölmesinde.
2. Tüm olmayan doğrulayın **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeler kasaya görünmeye varsa bunları kaldırın.

2. Kasa panosunda Kasası'nda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**. Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Ne yedekleme işlemi durdurur ancak verileri tut?

Yedekleme işlemi durdurur, ancak yanlışlıkla veri korumak, önceki bölümlerde açıklandığı gibi yedekleme verilerini silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasaları.
