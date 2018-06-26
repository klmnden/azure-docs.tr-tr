---
title: Azure kurtarma Hizmetleri kasasını Sil '
description: Bu makalede, bir kurtarma Hizmetleri kasasını Sil açıklanmaktadır. Makale bir kasa silmeyi deneyin, ancak olamaz sorun giderme adımlarını içerir.
services: service-name
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 6/21/2018
ms.author: markgal
ms.openlocfilehash: d8169eba6790e49a85d69434663faabe7430942e
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937609"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede, bir kurtarma Hizmetleri Kasası'nı tüm öğeleri kaldırın ve ardından silin açıklanmaktadır. Bir sunucu kayıtlıysa ve yedekleme verilerini tutan bir kurtarma Hizmetleri kasası silinemiyor. Bir kasa silmeyi deneyin ancak kullanamazsanız, kasaya yedekleme verileri almak için hala yapılandırılır.

Kasayı silme, başlıklı bölüme bakın öğrenmek için [Azure portalından bir kasa silme](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Kurtarma Hizmetleri kasasında herhangi bir veri koruyun ve kasa silmek istediğiniz istemiyorsanız, bölümüne bakın [force tarafından kasayı silin](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Kasaya nedir emin değilseniz ve kasayı silebilirsiniz emin olmak ihtiyacınız varsa bölümüne bakın [Kaldır kasası bağımlılıkları ve delete kasa](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Azure portalından bir kasa silme

Açık kurtarma Hizmetleri kasası zaten varsa, ikinci adıma atlayın.

1. Azure Portalı'nı açın ve Panodan silmek istediğiniz kasaya açın.

   Hub menüsünde panoya sabitlenmiş kurtarma Hizmetleri kasası yoksa tıklatın **tüm hizmetleri** ve kaynak listesinde yazın **kurtarma Hizmetleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Aboneliğinizde kasalarının listesi görüntülemek için **kurtarma Hizmetleri kasaları**.

   ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Kurtarma Hizmetleri kasalarının listesi görüntülenir. 

   ![Kasa listesinden seçin](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

2. Listeden silmek istediğiniz kasayı seçin. Kasa seçtiğinizde kasa panosu açılır.

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

3. Kasa panosunda bir kasa silmek için tıklatın **silmek**. Kasayı silmek istediğinizi doğrulamanız istenir.

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Varsa **kasa silme hatası** görünür, bağımlılıkları kasadan kaldırabilirsiniz veya kasası force tarafından silmek için PowerShell kullanabilirsiniz. Aşağıdaki bölümlerde bu görevlerin nasıl gerçekleştirileceğini açıklamaktadır.

    ![Kasa silme hatası](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Force tarafından kurtarma Hizmetleri kasasını Sil

Kurtarma Hizmetleri kasası force tarafından silmek için PowerShell kullanın. Zorla yollarla kurtarma Hizmetleri kasasını ve ilişkili tüm yedekleme verileri kalıcı olarak silinir. 

> [!Warning]
> Kurtarma Hizmetleri kasası silmek için PowerShell kullanılırken kasadaki tüm yedekleme verileri kalıcı olarak silmek istediğinizden emin olun.
>

Kurtarma Hizmetleri kasası silmek için:

1. Azure hesabınızda oturum açın.

   Oturum açtığınızda, Azure aboneliğinizle `Connect-AzureRmAccount` komut ve izleyin ekrandaki yönergeleri.

   ```powershell
    Connect-AzureRmAccount
   ```
   Azure Backup'ı ilk kullanımınızda [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) cmdlet'iyle Azure Kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz gerekir.

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

2. Yönetici ayrıcalıklarıyla bir PowerShell penceresi açın.

3. Kullanım `Set-ExecutionPolicy Unrestricted` kısıtlamalar kaldırmak için.

4. Chocolately.org Azure Resource Manager istemci paketini karşıdan yüklemek için aşağıdaki komutu çalıştırın.

    `iex ((New-Object System.Net.WebClient) DownloadString('https://chocolatey.org/install.ps1))`

5. Azure Resource Manager API İstemci yüklemek için aşağıdaki komutu kullanın.

   `choco.exe install armclient`

6. Azure portalında abonelik kimliği ve ilişkili kaynak grubu adı, silmek istediğiniz kurtarma Hizmetleri kasası toplayın.

7. PowerShell'de, abonelik kimliği, kaynak grubu adı ve kurtarma Hizmetleri kasa adı kullanarak şu komutu çalıştırın. Komutu çalıştırdığınızda, kasaya ve tüm bağımlılıkları siler.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
8. Aboneliğiniz Azure portalında oturum açın ve kasa silinmiş doğrulayın.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Kasa bağımlılıkları kaldırın ve kasayı silin

Kasa bağımlılıkları el ile kaldırmak için her bir öğe veya sunucusunda ve kurtarma Hizmetleri kasası arasında yapılandırması silin. Aşağıdaki yordam boyunca ilerledikçe kullanmak **yedekleme öğeleri** menü (görüntü bakın) için:

* Azure depolama (Azure dosyaları) yedeklemeleri
* Azure VM yedekleri SQL Server
* Azure sanal makineleri yedekleme
* Microsoft Azure kurtarma Hizmetleri Aracısı yedekleri

Kullanım **Yedekleme Altyapısı** menü (görüntü bakın) için:

* Azure yedekleme sunucusu yedeklemeleri
* System Center DPM yedeklemelerini

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. Kasa Panosu menüsünden korunan öğeler bölümüne gidin ve'ı tıklatın **yedekleme öğeleri**. Bu menüde durdurun ve Azure dosya sunucuları, SQL sunucuları Azure VM ve Azure sanal makineleri silin. Bu örnekte, biz yedekleme verilerini bir Azure dosya sunucusundan kaldıracağız.

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/selected-backup-items.png)

2. Bu türdeki tüm öğeleri görüntülemek için bir yedekleme türü seçin.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. Listedeki tüm öğelerin, öğeyi sağ tıklatın ve bağlam menüsünden seçin **Dur yedekleme**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png) 

    Durdur yedekleme menüsü açılır.

4. Üzerinde **durdurmak yedekleme** menüsünde gelen **bir seçenek belirleyin** menüsünde, select **yedekleme verilerini Sil**öğesinin adını yazın ve'ı tıklatın **Dur yedekleme**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adı yazın. **Durdurmak yedekleme** öğeyi doğrulayın sonra düğmesini etkinleştirir. Veri korursanız, kasayı silmek mümkün olmayacaktır.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsterseniz, verileri neden silmekte olduğunuz bir gerekçe sağlamasını ve yorumlar ekleyin. İş tamamlandı doğrulamak için Azure iletilerini denetleyin ![yedekleme verilerini silmek](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmeti bir ileti gönderir: *yedekleme işlemi durduruldu ve yedekleme verileri silindi*.

5. Listeden bir öğe üzerinde sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **yenileme** kasadaki öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede hiç öğe olduğunda kaydırın **Essentials** kurtarma Hizmetleri kasası menü bölmesinde. Döndürmemelidir olması herhangi **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan** listelenir. Öğeleri kasaya görüntülenmeye devam ederse, üç adıma geri dönün ve farklı öğe türü listesini seçin.  

6. Kasa araç çubuğunda başka öğe yok olduğunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="removing-azure-backup-server-or-dpm"></a>Azure Backup sunucusu veya DPM kaldırma

1. Kasa Panosu menüsünden Yönet bölümüne gidin ve'ı tıklatın **Yedekleme Altyapısı**. 

2. Alt menüde tıklatın **yedekleme yönetim sunucuları** Azure yedekleme sunucuları ve System Center DPM sunucusu görüntülemek için. durdurun ve Azure dosya sunucuları, SQL sunucuları Azure VM ve Azure sanal makineleri silin. 

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Sil ve alt menüsünden seçin istediğiniz öğeyi sağ **silmek**.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Durdur yedekleme menüsü açılır.

4. Üzerinde **durdurmak yedekleme** menüsünde gelen **bir seçenek belirleyin** menüsünde, select **yedekleme verilerini Sil**öğesinin adını yazın ve'ı tıklatın **Dur yedekleme**.

    Silmek istediğiniz doğrulamak için adını yazın. **Durdurmak yedekleme** öğeyi doğrulayın sonra düğmesini etkinleştirir. Veri tutuyorsanız kasası silinemiyor.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    İsteğe bağlı olarak, bir nedeni neden, verileri siliniyor ve açıklamaları ekleme sağlayabilir. İş tamamlandığını doğrulamak için Azure iletilerini denetleyin ![yedekleme verilerini silmek](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmeti bir ileti gönderir: yedekleme işlemi durduruldu ve yedekleme verileri silindi.

5. Listeden bir öğe üzerinde sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **yenileme** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede hiç öğe olduğunda kaydırın **Essentials** kurtarma Hizmetleri kasası menü bölmesinde. Döndürmemelidir olması herhangi **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan** listelenir. Öğeleri kasaya görüntülenmeye devam ederse, üç adıma geri dönün ve farklı öğe türü listesini seçin.  
6. Olduğunda daha fazla öğe kasadaki kasa panosunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.


## <a name="removing-azure-backup-agent-recovery-points"></a>Azure Yedekleme aracısı kurtarma noktalarını kaldırma

1. Kasa Panosu menüsünden Yönet bölümüne gidin ve'ı tıklatın **Yedekleme Altyapısı**.

2. Alt menüye tıklayın **korunan sunucuları** listesini görüntülemek için sunucu türleri, Azure Backup aracısını dahil olmak üzere korumalı.

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. İçinde **korunan sunucuları** listesinde, Azure yedekleme Aracısı'nı tıklatın.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    Azure Backup aracısını kullanan korumalı sunucuları listesi açılır.

    ![belirli korumalı sunucuyu seçin](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

4. Sunucular listesinde, kendi menüsünü açmak için tıklatın.

    ![Seçilen sunucunun panosunu görüntülemek](./media/backup-azure-delete-vault/selected-protected-server.png)

5. Seçilen sunucunun Pano menüsünde **silmek**.

    ![Seçili sunucu silme](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Üzerinde **silmek** menüsünde öğesinin adını yazın ve tıklatın **silmek**.

    Bunu silmek istediğinizde doğrulamak için öğesinin adı yazın. **Silmek** öğeyi doğrulayın sonra düğmesini etkinleştirir.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    İsteğe bağlı olarak, bir nedeni neden, verileri siliniyor ve açıklamaları ekleme sağlayabilir. İş tamamlandığını doğrulamak için Azure iletilerini denetleyin ![yedekleme verilerini silmek](./media/backup-azure-delete-vault/messages.png). <br/>
    İş tamamlandıktan sonra hizmeti bir ileti gönderir: yedekleme işlemi durduruldu ve yedekleme verileri silindi.

7. Listeden bir öğe üzerinde sildikten sonra **yedekleme öğeleri** menüsünde tıklatın **yenileme** kasadaki kalan öğeleri görmek için.

      ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

      Listede hiç öğe olduğunda kaydırın **Essentials** kurtarma Hizmetleri kasası menü bölmesinde. Döndürmemelidir olması herhangi **yedekleme öğeleri**, **yönetim sunucularını yedekleme**, veya **öğeleri çoğaltılan** listelenir. Öğeleri kasaya görüntülenmeye devam ederse, üç adıma geri dönün ve farklı öğe türü listesini seçin.  
8. Olduğunda daha fazla öğe kasadaki kasa panosunda tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

9. Kasayı silmek istediğiniz doğrulamak için tıklatın **Evet**.

    Kasa silinir ve portal döndürür **yeni** servis menüsü.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Ne yedekleme işlemini durdurun ancak verileri korur?

Yedekleme işlemi ancak yanlışlıkla durdurursanız *korumak* verileri kasa silmeden önce yedekleme verilerini silmeniz gerekir. Yedekleme verilerini silmek için:

1. Üzerinde **yedekleme öğeleri** menüsünde öğeyi sağ tıklatın ve bağlam menüsünde **yedekleme verileri Sil**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Yedekleme verilerini Sil** menüsü açılır.
2. Üzerinde **yedekleme verilerini Sil** menüsünde öğesinin adını yazın ve tıklatın **silmek**.

    ![Yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Veri sildikten sonra adım 4 c dönün ve işlemine devam edin.
