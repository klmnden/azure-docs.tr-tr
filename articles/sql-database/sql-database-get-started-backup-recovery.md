---
title: "Veri koruma ve kurtarma amacıyla Azure SQL veritabanlarını yedeklemeye ve geri yükleme işlevlerini kullanmaya başlama | Microsoft Belgeleri"
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
ms.sourcegitcommit: 7f26cd0f6c5f9c7a2fe692bfcdc6ef60d1b2200f
ms.openlocfilehash: d4ea089ed4b5d29c261b25e95f4d304611f9a857


---
<!------------------
This topic is annotated with TEMPLATE guidelines for TUTORIAL TOPICS.


Metadata guidelines

title
    60 characters or less. Tells users clearly what they will do (deploy an ASP.NET web app to App Service). Not the same as H1. It's 60 characters or fewer including all characters between the quotes and the Microsoft Docs site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015."
------------------>

<!----------------

TEMPLATE GUIDELINES for tutorial topics

The tutorial topic shows users how to solve a problem using a product or service. It includes the prerequisites and steps users need to be successful.  

It is a "solve a problem" topic, not a "learn concepts" topic.

DO include this:
    • What users will do
    • What they will create or accomplish by the end of the tutorial
    • Time estimate
    • Optional but useful: Include a diagram or video. Diagrams help users see the big picture of what they are doing. A video of the steps can be used by customers as an alternative to following the steps in the topic.
    • Prerequisites: Technical expertise and software requirements
    • End-to-end steps. At the end, include next steps to deeper or related tutorials so users can learn more about the service

DON'T include this:
    • Conceptual info about the service. This info is in overview topics that you can link to in the prerequisites section if necessary

------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What will I do in this topic?" Write the H1 heading in conversational language and use search keywords as much as possible. Since this is a "solve a problem" topic, make sure the title indicates that. Use a strong, specific verb like "Deploy."  
        
    Heading must use an industry standard term. If your feature is a proprietary name like "elastic pools", use a synonym. For example: "Learn about elastic pools for multi-tenant databases." In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery"></a>Veri Koruma ve Kurtarma için Yedekleme ve Geri Yükleme İşlevlerini Kullanmaya Başlama

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top keywords that you are using throughout the article.The introduction should be brief and to the point of what users will do and what they will accomplish. 

    In this example:
     

Sentence #1 Explains what the user will do. This is also the metadata description. 
    This tutorial shows how to deploy an ASP.NET web application to a web app in Azure App Service by using Visual Studio 2015. 

Sentence #2 Explains what users will learn and the benefit.  
    When you’re finished, you’ll have a simple web application up and running in the cloud.

-------------------->


Bu kullanmaya başlama öğreticisinde, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

- Bir veritabanının var olan yedeklerini görüntüleme
- Bir veritabanını daha önceki bir noktaya geri yükleme
- Bir veritabanı yedekleme dosyasını Azure Kurtarma Hizmetleri kasasında uzun süreli saklamak üzere yapılandırma
- Azure Kurtarma Hizmetleri kasasındaki bir veritabanını geri yükleme

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 30 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).


## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* [Azure portalı ve SQL Server Management Studio aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started.md) öğreticisini veya bu öğreticinin [PowerShell sürümünü](sql-database-get-started-powershell.md) tamamladınız. Tamamlamadıysanız, bu öğretici önkoşulunu tamamlayın veya devam etmeden önce bu öğreticinin [PowerShell sürümünün](sql-database-get-started-powershell.md) sonundaki PowerShell betiğini çalıştırın.


> [!TIP]
> Aynı görevleri, [PowerShell](sql-database-get-started-backup-recovery-powershell.md) aracılığıyla kullanmaya başlama öğreticilerinde de gerçekleştirebilirsiniz.


## <a name="sign-in-by-using-your-existing-account"></a>Var olan hesabınızı kullanarak oturum açın
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
> Yedekleri silmek için bkz. [Uzun süreli saklama yedeklerini silme](sql-database-long-term-retention-delete.md).


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


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
- Yedekleri geri yükleme hakkında bilgi edinmek için bkz. [yedekten geri yükleme](sql-database-recovery-using-backups.md)



<!--HONumber=Dec16_HO4-->


