---
title: "Azure portalı: Azure SQL veritabanını yedekleme ve geri yükleme | Microsoft Docs"
description: "Bu öğretici otomatik yedeklerden belirli bir noktaya geri yükleme gerçekleştirme, otomatik yedekleri Azure Kurtarma Hizmetleri kasasında saklama ve Azure Kurtarma Hizmetleri kasasından geri yükleme gerçekleştirme konusunda bilgi vermektedir"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 797d91b21fcd71672890c6c77bc81eadd73b47ff
ms.lasthandoff: 03/10/2017


---
# <a name="tutorial-back-up-and-restore-an-azure-sql-database-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak Azure SQL Veritabanını yedekleme ve geri yükleme
Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

- Bir veritabanının var olan yedeklerini görüntüleme
- Bir veritabanını daha önceki bir noktaya geri yükleme
- Bir veritabanı yedekleme dosyasını Azure Kurtarma Hizmetleri kasasında uzun süreli saklamak üzere yapılandırma
- Azure Kurtarma Hizmetleri kasasındaki bir veritabanını geri yükleme

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 30 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).

> [!TIP]
> Aynı görevleri, [PowerShell](sql-database-get-started-backup-recovery-powershell.md) aracılığıyla kullanmaya başlama öğreticilerinde de gerçekleştirebilirsiniz.
>

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure hesabı**. Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](https://azure.microsoft.com/free/) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure oluşturma izinleri**. Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* **SQL Server Management Studio**. SQL Server Management Studio’nun en son sürümünü [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndirin) sayfasından indirip yükleyebilirsiniz. SSMS için sürekli yeni özellikler yayınlandığından Azure SQL Veritabanı’na bağlanırken her zaman en son sürümü kullanın.

* **Taban sunucu ve veritabanları** Bu öğreticide kullanılan bir sunucu ve iki veritabanını yükleyip yapılandırmak için **Azure’a Dağıt** düğmesine tıklayın. Düğmeye tıklandığında **Şablondan dağıt** dikey penceresi açılır; yeni bir kaynak grubu oluşturun ve oluşturulacak yeni sunucu için **Yönetici Oturum Açma Parolası** belirtin:

   [![indir](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fsqldbtutorial.blob.core.windows.net%2Ftemplates%2Fsqldbgetstarted.json)


> [!NOTE]
> Bu öğretici şu konu başlıklarının içeriğini öğrenmenize yardımcı olacaktır: [SQL Veritabanı yedekleri](sql-database-automated-backups.md), [Uzun süreli yedek saklama](sql-database-long-term-retention.md) ve [Otomatik veritabanı yedeklerini kullanarak bir Azure SQL veritabanını kurtarma](sql-database-recovery-using-backups.md).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Azure hesabınızı kullanarak Azure portalında oturum açma
[Var olan aboneliğinizi](https://account.windowsazure.com/Home/Index) kullanarak Azure portala bağlanmak için aşağıdaki adımları uygulayın.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)

<a name="create-logical-server-bk"></a>

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Bir veritabanının hizmet tarafından oluşturulan yedeklerinden en eski geri yükleme noktasını görüntüleme

Öğreticinin bu bölümünde veritabanınızın [hizmet tarafından oluşturulan otomatik yedeklerinde](sql-database-automated-backups.md) en eski geri yükleme noktası hakkında bilgileri görüntüleyeceksiniz. 

1. Veritabanınızın **SQL veritabanı** dikey penceresini açın, **sqldbtutorialdb**.

   ![yeni örnek veritabanı dikey penceresi](./media/sql-database-get-started/new-sample-db-blade.png)

2. Araç çubuğunda **Geri yükle**'ye tıklayın.

   ![geri yükleme araç çubuğu](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. Geri yükleme dikey penceresinde en eski geri yükleme noktasını inceleyin.

   ![en eski geri yükleme noktası](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme

Öğreticinin bu bölümünde veri tabanını belirli bir noktadan yeni bir veritabanına geri yükleyeceksiniz.

1. Veritabanının **Geri yükle** dikey penceresinde eski bir noktaya geri yükleyeceğiniz yeni veritabanı için varsayılan adı inceleyin (ad, var olan veritabanının adına zaman damgası eklenerek oluşturulur). Bu ada bir sonraki adımlarda belirttiğiniz zaman eklenir.

   ![geri yüklenen veritabanının adı](./media/sql-database-get-started-backup-recovery/restored-database-name.png)

2. **Geri yükleme noktası (UTC)** giriş kutusundaki **takvim** simgesine tıklayın.

   ![geri yükleme noktası](./media/sql-database-get-started-backup-recovery/restore-point.png)

2. Takvimde saklama döneminde olan bir tarih seçin

   ![geri yükleme noktası tarihi](./media/sql-database-get-started-backup-recovery/restore-point-date.png)

3. **Geri yükleme noktası (UTC)** giriş kutusunda otomatik veritabanı yedeklerinden veritabanına geri yüklemek istediğiniz tarih için bir saat belirtin.

   ![geri yükleme noktası saati](./media/sql-database-get-started-backup-recovery/restore-point-time.png)

   >[!NOTE]
   >Veritabanı adı, seçtiğiniz tarihi ve saati gösterecek şekilde değiştirilir. Ayrıca belirli bir noktaya geri yüklediğiniz hedef sunucuyu değiştiremeyeceğinize de dikkat edin. Farklı bir sunucuya geri yüklemek için [Coğrafi Geri Yükleme](sql-database-disaster-recovery.md#recover-using-geo-restore) özelliğini kullanın. Son olarak, [elastik havuza](sql-database-elastic-jobs-overview.md) veya farklı bir fiyatlandırma katmanına istediğiniz zaman geri dönebilirsiniz. 
   >

4. Veritabanınızı, yeni veritabanında daha önceki bir zamana geri yüklemek için **Tamam**'a tıklayın.

5. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress.png)

6. Geri yükleme işi tamamlandığında yeni geri yüklenen veritabanını görüntülemek için **SQL veritabanları** dikey penceresini açın.

   ![geri yüklenen veritabanı](./media/sql-database-get-started-backup-recovery/restored-database.png)

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Otomatik yedekleri Azure Kurtarma Hizmetleri kasasında uzun süreli saklamak üzere yapılandırma 

Öğreticinin bu bölümünde [Azure Kurtarma Hizmetleri kasasını yapılandırarak otomatik yedekleri](sql-database-long-term-retention.md) hizmet katmanınızın saklama süresinden daha uzun bir süre saklamayı öğreneceksiniz. 


> [!TIP]
> Uzun süreli yedek saklamadaki yedekleri silmek için bkz. [PowerShell’i kullanarak uzun süreli yedek saklamayı yönetme](sql-database-manage-long-term-backup-retention-powershell.md).
>

1. Sunucunuzun **SQL Server** dikey penceresini açın, **sqldbtutorialserver**.

   ![sql sunucusu dikey penceresi](./media/sql-database-get-started/sql-server-blade.png)

2. **Uzun süreli yedek saklama**'ya tıklayın.

   ![uzun süreli yedek saklama bağlantısı](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. **sqldbtutorial - Uzun süreli yedek saklama** dikey penceresinde önizleme koşullarını gözden geçirin ve kabul edin (önceden yapmadıysanız veya bu özellik önizleme sürümünden çıkmadıysa).

   ![önizleme koşullarını kabul edin](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. sqldbtutorialdb veritabanı için uzun süreli yedek saklamayı yapılandırmak üzere tablodan veritabanını seçip araç çubuğunda **Yapılandır**'a tıklayın.

   ![uzun süreli yedek saklama için veritabanı seçme](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. **Yapılandırma** dikey penceresinde **Kurtarma hizmetleri kasası** bölümünde **Gerekli ayarları yapılandır**'a tıklayın.

   ![kasayı yapılandırma bağlantısı](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. **Kurtarma hizmetleri kasası** dikey penceresinde var olan kasalardan birini seçin (varsa). Aboneliğinize ait kurtarma hizmetleri kasası yoksa akıştan çıkın ve kurtarma hizmetleri kasası oluşturun.

   ![yeni kasa oluşturma bağlantısı](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. **Kurtarma Hizmetleri kasaları** dikey penceresinde **Ekle**'ye tıklayın.

   ![yeni kasa ekleme bağlantısı](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. **Kurtarma Hizmetleri kasası** dikey penceresinde yeni Kurtarma Hizmetleri kasası için geçerli bir ad girin.

   ![yeni kasa adı](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Aboneliğinizi ve kaynak grubunu seçtikten sonra kasa için bir konum belirleyin. İşiniz bittiğinde **Oluştur**'a tıklayın.

   ![yeni kasa oluşturma](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.
   >

10. Yeni kasayı oluşturduktan sonra gerekli adımları gerçekleştirerek **Kurtarma hizmetleri kasası** dikey penceresine dönün.

11. **Kurtarma hizmetleri kasası** dikey penceresinde kasaya ve ardından **Seç**'e tıklayın.

   ![var olan kasayı seçme](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. **Yapılandır** dikey penceresinde yeni saklama ilkesi için geçerli bir ad belirleyin, varsayılan saklama ilkesini gerekli şekilde değiştirin ve ardından **Tamam**'a tıklayın.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. **sqldbtutorial - Uzun süreli yedek saklama** dikey penceresinde **Kaydet**'e ve ardından **Tamam**'a tıklayarak uzun süreli yedek saklama ilkesini seçilen tüm veritabanlarına uygulayın.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Yapılandırdığınız Azure Kurtarma Hizmetleri kasasında bu yeni ilkeyi kullanan uzun süreli yedek saklamayı etkinleştirmek için **Kaydet**'e tıklayın.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

15. Uzun süreli yedek saklama etkinleştirildikten sonra **sqldbtutorialvault** dikey penceresini açın (**Tüm kaynaklar**'a gidin ve aboneliğinizin kaynak listesinden seçin).

   ![kurtarma hizmetleri kasasını görüntüleme](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault.png)

> [!IMPORTANT]
> Yapılandırma yapıldıktan sonra yedekler sonraki yedi gün içinde kasada görünür. Bu öğreticiye devam etmek için yedeklerin kasada görünmesini bekleyin.
>

## <a name="view-backups-in-long-term-retention"></a>Uzun süreli saklama kapsamındaki yedekleri görüntüleme

Öğreticinin bu bölümünde [uzun süreli saklama](sql-database-long-term-retention.md) kapsamındaki veritabanı yedekleriniz hakkındaki bilgileri görüntüleyeceksiniz. 

1. Kasadaki veritabanı yedekleriniz tarafından kullanılan depolama alanı miktarını görüntülemek için **sqldbtutorialvault** dikey penceresini açın (**Tüm kaynaklar**'a gidin ve aboneliğinizin kaynak listesinden seçin).

   ![kurtarma hizmetleri kasasını ve yedekleri görüntüleme](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Veritabanınızın **SQL veritabanı** dikey penceresini açın, **sqldbtutorialdb**.

   ![yeni örnek veritabanı dikey penceresi](./media/sql-database-get-started/new-sample-db-blade.png)

3. Araç çubuğunda **Geri yükle**'ye tıklayın.

   ![geri yükleme araç çubuğu](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Geri yükleme dikey penceresinde **Uzun süreli**'ye tıklayın.

5. Azure kasası yedeklemelerinin altında **Bir yedekleme seçin**'e tıklayarak uzun süreli saklama kapsamındaki kullanılabilir veritabanı yedeklerini görüntüleyebilirsiniz.

   ![kasadaki yedekler](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Bir veritabanını uzun süreli yedek saklama kapsamındaki bir yedekten geri yükleme

Öğreticinin bu bölümünde veritabanını Azure Kurtarma Hizmetleri kasasındaki bir yedekten yeni bir veritabanına geri yükleyeceksiniz.

1. **Azure kasası yedekleri** dikey penceresinde geri yüklenecek yedeğe ve ardından **Seç**'e tıklayın.

   ![kasadaki bir yedeği seçme](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. **Veritabanı adı** metin kutusunda geri yüklenen veritabanı için bir ad girin.

   ![yeni veritabanı adı](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Veritabanınızı kasadaki yedekten yeni veritabanına geri yüklemek için **Tamam**'a tıklayın.

4. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![kasadan geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Geri yükleme işi tamamlandığında yeni geri yüklenen veritabanını görüntülemek için **SQL veritabanları** dikey penceresini açın.

   ![kasadan geri yüklenen veritabanı](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
- Yedekleri geri yükleme hakkında bilgi edinmek için bkz. [yedekten geri yükleme](sql-database-recovery-using-backups.md)

