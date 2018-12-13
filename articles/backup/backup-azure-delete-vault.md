---
title: Azure kurtarma Hizmetleri kasasında Sil '
description: Bu makalede, bir kurtarma Hizmetleri kasasını silme açıklanmaktadır. Bu makale, bir kasayı silmeyi deneyin ancak olamaz, sorun giderme adımları içerir.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 7/6/2018
ms.author: raynew
ms.openlocfilehash: d7617ce96181a0708dfa4731c07d581e332bdff4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873113"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede, tüm öğeleri bir kurtarma Hizmetleri kasasından kaldırın ve ardından silin açıklanmaktadır. Bir sunucuya kaydedilir ve yedekleme verilerini tutar bir kurtarma Hizmetleri kasası silinemiyor. Bir kasayı silmeyi deneyin ancak kullanamazsanız, yedekleme verilerini almak için kasa yine de yapılandırılır.

Bölümüne bakın, bir kasayı silme hakkında bilgi edinmek için [Azure portalından bir kasayı silme](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Kurtarma Hizmetleri Kasası'nda hiçbir veriyi saklamak ve kasayı silmek isteyip istemediğiniz, bölümüne bakın. [zorla tarafından kasayı silin](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Kasada nedir emin değilseniz ve kasayı silebilirsiniz emin olmanız gerekir, bölümüne bakın. [Kaldır kasa bağımlılıkları ve delete kasası](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Azure portalından bir kasayı silme

Açık kurtarma Hizmetleri kasası zaten varsa ikinci bir adıma atlayın.

1. Azure portalını açın ve silmek istediğiniz kasayı panoyu açın.

   Hub menüsünde, panoya sabitlenmiş kurtarma Hizmetleri kasası yoksa tıklayın **tüm hizmetleri** ve kaynak listesinde yazın **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Aboneliğinizde kasalarının listesi görüntülemek için tıklayın **kurtarma Hizmetleri kasaları**.

   ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Kurtarma Hizmetleri kasalarının listesi görüntülenir. 

   ![listeden kasayı seçin](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

1. Listeden silmek istediğiniz kasayı seçin. Kasayı seçtiğinizde, kasa panosu açılır.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Kasa panosunda bir kasayı silme için tıklatın **Sil**. Kasayı silmek istediğinizi doğrulamanız istenir.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Varsa **kasa silme hatası** görünürse, bağımlılıkları kasadan kaldırabilirsiniz veya force tarafından kasayı silmek için PowerShell kullanabilirsiniz. Aşağıdaki bölümlerde, bu görevlerin nasıl gerçekleştirileceğini açıklamaktadır.

    ![Kasa silme hatası](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Force tarafından kurtarma Hizmetleri kasasını silme

Bir kurtarma Hizmetleri kasası tarafından zorla silmek için PowerShell kullanabilirsiniz. Zorla yöntemlerle kurtarma Hizmetleri kasasını ve ilişkili tüm yedekleme verileri kalıcı olarak silinir. 

> [!Warning]
> Bir kurtarma Hizmetleri kasasını silmek için PowerShell kullanırken kasasındaki tüm yedekleme verileri kalıcı olarak silmek istediğinizden emin olun.
>

Bir kurtarma Hizmetleri kasasını silmek için:

1. Azure hesabınızda oturum açın.

   `Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
    Connect-AzureRmAccount
   ```
   Azure Backup'ı ilk kullanımınızda [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) cmdlet'iyle Azure Kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir.

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

1. Yönetici ayrıcalıklarıyla bir PowerShell penceresi açın.

1. Kullanım `Set-ExecutionPolicy Unrestricted` herhangi bir kısıtlama kaldırmak için.

1. Azure Resource Manager istemci paketi chocolately.org indirmek için aşağıdaki komutu çalıştırın.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

1. Azure Resource Manager API istemcisi yüklemek için aşağıdaki komutu kullanın.

   `choco.exe install armclient`

1. Azure portalında, abonelik kimliği ve ilişkili kaynak grubu adı silmek istediğiniz kurtarma Hizmetleri kasası için toplayın.

1. PowerShell'de, abonelik kimliği, kaynak grubu adı ve kurtarma Hizmetleri kasa adı kullanarak şu komutu çalıştırın. Komutu çalıştırdığınızda, kasayı ve tüm bağımlılıklarını siler.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
   Silebilmek için önce kasa boş olmalıdır. Aksi takdirde, "Bu kasa içinde mevcut kaynaklar bulunduğundan kasa silinemiyor" Alıntısı yapılan bir hata alırsınız. Aşağıdaki komutu bir kasa içinde bir kapsayıcı kaldırılacağı gösterilmektedir:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```
   
1. Aboneliğiniz Azure portalında oturum açın ve kasa silinir doğrulayın.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Kasa bağımlılıkları kaldırın ve kasasını silme

Kasa bağımlılıkları el ile kaldırmak için her bir öğe veya sunucu arasında yapılandırmasını silme ve kurtarma Hizmetleri kasası. Aşağıdaki yordam boyunca ilerledikçe kullanın **yedekleme öğeleri** menü (resme bakın) için:

* Azure depolama (Azure dosyaları) yedekleri
* SQL Server Azure VM yedeklemeleri
* Azure sanal makineleri yedekleme
* Microsoft Azure kurtarma Hizmetleri Aracısı yedekleme

Kullanım **Yedekleme Altyapısı** menü (resme bakın) için:

* Azure Backup sunucusu yedekleme
* System Center DPM yedeklemelerini

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. Kasa Panosu menüsünden korumalı öğeler bölümüne inin ve tıklayın **yedekleme öğeleri**. Bu menüde, durdurun ve Azure dosya sunucuları, Azure VM ve Azure sanal makineler'de SQL Server'ı silin. Bu örnekte, bir Azure dosya sunucusundan yedekleme verilerini kaldıracağız.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/selected-backup-items.png)

1. Bu türdeki tüm öğeleri görüntülemek için bir yedekleme türünü seçin.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

1. Tüm öğeleri listesinde bir öğeye sağ tıklayın ve bağlam menüsünden seçin **yedeklemeyi Durdur**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png) 

    Yedeklemeyi Durdur menüsünü açar.

1. Üzerinde **yedeklemeyi Durdur** menüsünde, gelen **bir seçenek belirleyin** menüsünde **yedekleme verilerini Sil**öğesinin adını yazın ve tıklayın **yedeklemeyi Durdur**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adını yazın. **Yedeklemeyi Durdur** öğesi doğruladıktan sonra düğmesini etkinleştirir. Veri tutuyorsanız, kasayı silmek mümkün olmayacaktır.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsterseniz, neden veri silmekte olduğunuz bir neden belirtin ve yorumlar ekleyin. İş tamamlandı doğrulamak için Azure iletileri denetleyin ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmetin bir ileti gönderir: *yedekleme işlemi durduruldu ve yedekleme verileri silindi*.

1. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede bir öğe olduğunda, kaydırma **Essentials** bölmesinde kurtarma Hizmetleri kasası menüsünde. Var olmamalıdır tüm **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeleri hala kasaya görünüyorsa, üç adımına dönmek ve farklı öğe türü listesini seçin.  

1. Kasa araç çubuğunda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**.

    Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.

## <a name="removing-azure-backup-server-or-dpm"></a>Azure Backup sunucusu veya DPM kaldırılıyor

1. Kasa Panosu menüsünden Yönet bölümüne inin ve tıklayın **Yedekleme Altyapısı**. 

1. Alt menüde tıklayın **yedekleme yönetim sunucuları** Azure Backup sunucusu ve System Center DPM sunucusu görüntülemek için. Durdur ve Azure dosya sunucuları, Azure VM ve Azure sanal makineler'de SQL Server'ı silin. 

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

1. Silin ve alt menüden istediğiniz öğeye sağ **Sil**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Yedeklemeyi Durdur menüsünü açar.

1. Üzerinde **yedeklemeyi Durdur** menüsünde, gelen **bir seçenek belirleyin** menüsünde **yedekleme verilerini Sil**öğesinin adını yazın ve tıklayın **yedeklemeyi Durdur**.

    Silmek istediğiniz doğrulamak için adını yazın. **Yedeklemeyi Durdur** öğesi doğruladıktan sonra düğmesini etkinleştirir. Verileri Tut kasayı silemezsiniz.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsteğe bağlı olarak, bir neden neden, verileri silmek ve açıklamalar ekleme sağlayabilirsiniz. İşin tamamlandığını doğrulamak için Azure iletileri denetleyin ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmetin bir ileti gönderir: yedekleme işlemi durduruldu ve yedekleme verileri silindi.

1. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede bir öğe olduğunda, kaydırma **Essentials** bölmesinde kurtarma Hizmetleri kasası menüsünde. Var olmamalıdır tüm **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeleri hala kasaya görünüyorsa, üç adımına dönmek ve farklı öğe türü listesini seçin.  
1. Kasa panosunda Kasası'nda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**.

    Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.


## <a name="removing-azure-backup-agent-recovery-points"></a>Azure Backup Aracısı kurtarma noktalarını kaldırma

1. Kasa Panosu menüsünden Yönet bölümüne inin ve tıklayın **Yedekleme Altyapısı**.

1. Alt menüde **korumalı sunucuların** listesini görüntülemek için Azure Backup Aracısı dahil, sunucu türleri korumalı.

    ![panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

1. İçinde **korumalı sunucuların** Azure Backup Aracısı'nı tıklatın.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    Azure Backup Aracısı kullanılarak korunan sunucuların listesi açılır.

    ![belirli bir korumalı sunucuyu seçin](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

1. Sunucular listesinde, bir alt menüsünü açmak için tıklayın.

    ![Seçilen sunucunun panosunu görüntüleyin](./media/backup-azure-delete-vault/selected-protected-server.png)

1. Seçili sunucu Panosu menüsünde **Sil**.

    ![Seçili sunucuyu silin](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

1. Üzerinde **Sil** menüsünden öğesinin adını yazın ve tıklayın **Sil**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adını yazın. **Sil** öğesi doğruladıktan sonra düğmesini etkinleştirir.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    İsteğe bağlı olarak, bir neden neden, verileri silmek ve açıklamalar ekleme sağlayabilirsiniz. İşin tamamlandığını doğrulamak için Azure iletileri denetleyin ![yedekleme verilerini Sil](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmetin bir ileti gönderir: yedekleme işlemi durduruldu ve yedekleme verileri silindi.

1. Üzerinde listesindeki bir öğeyi sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **Yenile** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede bir öğe olduğunda, kaydırma **Essentials** bölmesinde kurtarma Hizmetleri kasası menüsünde. Var olmamalıdır tüm **yedekleme öğeleri**, **yedekleme yönetim sunucuları**, veya **çoğaltılan öğeler** listelenir. Öğeleri hala kasaya görünüyorsa, üç adımına dönmek ve farklı öğe türü listesini seçin.  
1. Kasa panosunda Kasası'nda daha fazla öğe olduğunda tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Kasayı silmek istediğinizi doğrulamak için tıklayın **Evet**.

    Kasa silinir ve portal döndürür **yeni** hizmet menüsünü.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Ne yedekleme işlemi durdurur ancak verileri tut?

Yedekleme işlemi ancak yanlışlıkla iptal ederseniz *korumak* veri kasayı silmeden önce yedekleme verilerini silmeniz gerekir. Yedekleme verilerini silmek için:

1. Üzerinde **yedekleme öğeleri** menüsünde, öğeye sağ tıklayın ve bağlam menüsünde **yedekleme verilerini Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Yedekleme verilerini Sil** menüsü açılır.
1. Üzerinde **yedekleme verilerini Sil** menüsünden öğesinin adını yazın ve tıklayın **Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Verileri sildikten sonra adım 4 c dönün ve işleme devam.
