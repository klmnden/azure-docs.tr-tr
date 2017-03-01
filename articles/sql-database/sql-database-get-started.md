---
title: "Hızlı başlangıç: İlk Azure SQL Veritabanınız | Microsoft Belgeleri"
description: "Azure portalında SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturmayı öğrenin. Ayrıca SQL Server Management Studio’yu Azure SQL Veritabanı ile kullanma hakkında bilgi alın."
keywords: "sql veritabanı öğreticisi, sql veritabanı oluşturma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: single databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 166a9d7032bb75188a790bea1724aefd194dcefa
ms.openlocfilehash: 36afd5c8bccb080ae3aaf1b4975d317b9087a3b3


---
# <a name="create-connect-to-and-query-your-first-azure-sql-databases-in-the-azure-portal-and-using-ssms"></a>Azure portalında ve SSMS’yi kullanarak ilk Azure SQL veritabanlarınızı oluşturun, bunlara bağlanın ve sorgu gerçekleştirin

Bu öğreticide, Azure portalında ve SSMS’yi kullanarak Azure SQL veritabanları oluşturma, bunlara bağlanma ve sorgu gerçekleştirmeyi öğreneceksiniz. Bu öğreticiyi tamamladıktan sonra:

* Mantıksal bir sunucu, sunucu düzeyinde bir güvenlik duvarı ve iki veritabanı içeren bir kaynak grubu oluşturmuş olacaksınız.
* Azure portalında ve SQL Server Management Studio’yu kullanarak sunucu ve veritabanı özelliklerini nasıl görüntüleyeceğinizi öğreneceksiniz.
* Azure portalında ve SQL Server Management Studio’yu kullanarak bir veritabanını nasıl sorgulayacağınızı öğreneceksiniz.

**Tahmini süre**: Bu öğretici yaklaşık 30 dakika sürer (önkoşulları karşıladığınız varsayılarak).

> [!TIP]
> [PowerShell](sql-database-get-started-powershell.md) veya [C#](sql-database-get-started-csharp.md) kullanarak bir Azure SQL veritabanı oluşturma, buna bağlanma ve sorgu gerçekleştirmeyi de öğrenebilirsiniz.
>

> [!NOTE]
> Bu öğretici şu konu başlıklarının içeriğini öğrenmenize yardımcı olacaktır: [SQL Veritabanı sunucusuna genel bakış](sql-database-server-overview.md), [SQL veritabanına genel bakış](sql-database-overview.md) ve [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](sql-database-firewall-configure.md). SQL Veritabanı hizmetine genel bir bakış için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
>  

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure hesabı**. [Ücretsiz bir Azure hesabı açabilir](https://azure.microsoft.com/free/) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure oluşturma izinleri**. Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* **SQL Server Management Studio**. SQL Server Management Studio’nun en son sürümünü [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndirin) sayfasından indirip yükleyebilirsiniz. SSMS için sürekli yeni özellikler yayınlandığından Azure SQL Veritabanı’na bağlanırken her zaman en son sürümü kullanın.

### <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu yordamdaki adımlarda [Azure hesabınızı ](https://account.windowsazure.com/Home/Index)kullanarak Azure portalına nasıl bağlanabileceğiniz açıklanır.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Yeni mantıksal SQL sunucusu oluşturma

Bu yordamdaki adımlar, Azure portalı ile tercih ettiğiniz bölgede nasıl yeni bir mantıksal sunucu oluşturabileceğinizi gösterir. Mantıksal sunucu, SQL veritabanınızı içinde oluşturduğunuz ve kullanıcıların Azure SQL Veritabanı güvenlik duvarını aşarak bağlanmasına izin vermek için güvenlik duvarı kuralları oluşturduğunuz nesnedir. 

1. **Yeni**’ye tıklayıp **sql sunucusu** yazın ve sonra **ENTER**’a basın.

    ![mantıksal sql sunucusu](./media/sql-database-get-started/logical-sql-server.png)
2. **SQL sunucusu (mantıksal sunucu)** seçeneğine tıklayın.
   
    ![oluştur-mantıksal sql sunucusu](./media/sql-database-get-started/create-logical-sql-server.png)
3. Yeni SQL Sunucusu (yalnızca mantıksal sunucu) dikey penceresini açmak için **Oluştur**’a tıklayın.

    ![yeni-mantıksal sql sunucusu](./media/sql-database-get-started/new-logical-sql-server.png)
3. **Sunucu adı** metin kutusunda, yeni mantıksal sunucu için geçerli bir ad belirtin. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![yeni sunucu adı](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Yeni sunucunuzun tam adı, genel olarak benzersiz olmalı ve **<sunucunuzun_adi>.database.windows.net** biçiminde olmalıdır. Bu tam sunucu adını öğreticinin ilerleyen bölümlerinde sunucunuza ve veritabanlarınıza bağlanmak için kullanacaksınız.
    >
    
4. **Sunucu yöneticisi oturumu** metin kutusunda, bu sunucuda SQL kimlik doğrulaması oturum açma işlemi için kullanılacak olan kullanıcı adını belirtin. Bu oturum açma işlemi, asıl sunucu oturum açma işlemi olarak ifade edilir. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![SQL yöneticisi oturum açma adı](./media/sql-database-get-started/sql-admin-login.png)
5. **Parola** ve **Parolayı onaylayın** metin kutularında, sunucu sorumlusu oturum açma hesabı için parola belirtin. Yeşil onay işareti, geçerli bir parola sağladığınızı gösterir.
    
    ![SQL yönetici parolası](./media/sql-database-get-started/sql-admin-password.png)
6. **Abonelik** açılan kutusundan içinde nesne oluşturma izniniz olan bir abonelik seçin.

    ![aboneliği](./media/sql-database-get-started/subscription.png)
7. **Kaynak grubu** metin kutusunun altından **Yeni oluştur**’u seçip yeni kaynak grubu için geçerli bir ad sağlayın. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.

    ![yeni kaynak grubu](./media/sql-database-get-started/new-resource-group.png)

8. **Konum** metin kutusunda, mantıksal sunucunuzu oluşturacağınız bir veri merkezi seçin.
    
    ![sunucu konumu](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > **Azure hizmetlerinin sunucuya erişmesine izin ver** onay kutusu bu dikey pencerede değiştirilemez. Bu ayarı, sunucu güvenlik duvarı dikey penceresinde değiştirebilirsiniz. Daha fazla bilgi için bkz. [Güvenliğe giriş](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. **Panoya sabitle** onay kutusunu seçin.

10. Bu betiği Azure’a dağıtarak mantıksal sunucunuzu oluşturmak için **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-database-get-started/create.png)

11. Sunucunuz oluşturulduktan sonra sunucunuzun varsayılan olarak görüntülenen özelliklerini gözden geçirin. 

    ![sql sunucusu dikey penceresi](./media/sql-database-get-started/sql-server-blade.png)
12. Mantıksal SQL sunucunuzun ek özelliklerini görüntülemek için **Özellikler**’e tıklayın.

    ![sql sunucusu özellikleri](./media/sql-database-get-started/sql-server-properties.png)
13. Bu öğreticinin devamında kullanmak üzere, tam sunucu adını panonuza kopyalayın.

    ![sql sunucusu tam adı](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

Bu yordamdaki adımlar, Azure portalında nasıl sunucu düzeyinde bir güvenlik duvarı kuralı oluşturabileceğinizi gösterir. Azure SQL Veritabanı güvenlik duvarı, varsayılan olarak mantıksal sunucunuza ve ona ait veritabanlarına yönelik dış bağlantıları önler. Sunucunuza bağlanabilmek amacıyla bir sonraki yordamda bağlanmak için kullanacağınız bilgisayarın IP adresine yönelik bir güvenlik duvarı oluşturmanız gerekir. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](sql-database-firewall-configure.md).

1. SQL sunucusu dikey penceresinde **Güvenlik Duvarı**’na tıklayarak sunucunuzun Güvenlik Duvarı dikey penceresini açın. İstemci bilgisayarınızın IP adresinin görüntülendiğine dikkat edin.

    ![sql sunucusu güvenlik duvarı](./media/sql-database-get-started/sql-server-firewall.png)

2. Araç çubuğundan **İstemci IP’si ekle**’ye tıklayarak geçerli IP adresiniz için bir güvenlik duvarı kuralı oluşturun.

    ![istemci IP’si ekle](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Tek bir IP adresi veya bir IP adresi aralığının tamamı için bir güvenlik duvarı kuralı oluşturabilirsiniz. Güvenlik duvarını açmak SQL yönetici ve kullanıcılarının; sunucuda, geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanında oturum açmasını sağlar.
    >

4. Sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğundan **Kaydet**’e tıklayın ve sonra **Tamam**’a tıklayarak Başarılı iletişim kutusunu kapatın.

    ![başarılı](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>SSMS ile sunucuya bağlanma

Bu yordamdaki adımlarda SQL Server Management Studio’yu kullanarak SQL mantıksal sunucunuza nasıl bağlanabileceğiniz açıklanır. SSMS, DBA'nın SQL sunucularını ve veritabanlarını yönetmek için kullandığı birincil araçtır.

1. SQL Server Management Studio’yu açın (SSMS’yi açmak için Windows arama kutusuna **Microsoft SQL Server Management Studio** yazıp **Enter** tuşuna basın).

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. **Sunucuya Bağlan** iletişim kutusuna bir önceki adımda belirlediğiniz tam sunucu adınızı girin ve sonra sunucunuzu sağladığınız sırada belirttiğiniz kullanıcı adı ve parolayı sağlayın.

    ![sunucuya bağlan](./media/sql-database-get-started/connect-to-server.png)
4. **Bağlan**’a tıklayarak bağlantıyı başlatın ve SMSS’de Nesne Gezgini’ni açın.

    ![sunucuya bağlanıldı](./media/sql-database-get-started/connected-to-server.png)
5. Asıl veritabanındaki nesneleri görüntülemek için Nesne Gezgini’nde; **Veritabanları**, **Sistem Veritabanları** ve **asıl** seçeneklerini genişletin.

    ![asıl veritabanı](./media/sql-database-get-started/master-database.png)
6. **Asıl** seçeneğine sağ tıkladıktan sonra **Yeni Sorgu**’ya tıklayın.

    ![asıl veritabanını sorgula](./media/sql-database-get-started/query-master-database.png)

8. Sorgu penceresine aşağıdaki sorguyu yazın:

   ```select * from sys.objects```

9.  Araç çubuğunda, asıl veritabanındaki tüm sistem nesnelerinin listesini döndürmek için **Yürüt**’e tıklayın.

    ![asıl veritabanı sistem nesnelerini sorgula](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > SQL güvenliğini kullanmaya başlamak için bkz. [SQL kimlik doğrulamasını kullanmaya başlama](sql-database-control-access-sql-authentication-get-started.md)
    >

## <a name="create-a-database-with-sample-data"></a>Örnek verilerle veritabanı oluşturma

Bu yordamdaki adımlarda, Azure portalındaki daha önce oluşturduğunuz mantıksal sunucuyla ilişkili örnek verileri kullanarak nasıl veritabanı oluşturabileceğiniz açıklanır. 

1. Azure portalında, varsayılan dikey penceredeki **SQL veritabanı**’na tıklayın.

    ![sql veritabanları](./media/sql-database-get-started/new-sql-database.png)
2. SQL veritabanları dikey penceresindeki **Ekle**’ye tıklayın. 

    ![sql veritabanı ekle](./media/sql-database-get-started/add-sql-database.png)

    ![sql veritabanı dikey penceresi](./media/sql-database-get-started/sql-database-blade.png)
3. **Veritabanı adı** metin kutusunda geçerli bir veritabanı adı sağlayın.

    ![sql veritabanı adı](./media/sql-database-get-started/sql-database-name.png)
4. **Kaynak seç** bölümünden **Örnek (AdventureWorksLT)** seçeneğini belirleyin.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
5. **Sunucu** bölümünden sunucunuzun seçili olduğunu doğrulayın. Ayrıca, veritabanlarının sunuculara tek veritabanı olarak eklenebileceği gibi (varsayılan) elastik havuzlara da eklenebileceğini unutmayın. Elastik havuzlar hakkında daha fazla bilgi için bkz. [Elastik havuzlar](sql-database-elastic-pool.md).

6. **Fiyatlandırma katmanı** bölümünden fiyatlandırma katmanını **Temel** olarak değiştirip **Seç**’e tıklayın. İsterseniz daha sonra fiyatlandırma katmanını artırabilirsiniz, ancak öğrenme amacıyla en düşük maliyetli katmanı kullanmanızı öneririz.

    ![fiyatlandırma katmanı](./media/sql-database-get-started/pricing-tier.png)
7. **Panoya sabitle** onay kutusunu işaretleyin ve ardından **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-database-get-started/create.png)

8. Veritabanınız oluşturulduktan sonra Azure portalında veritabanınızın özelliklerini görüntüleyin. Sonraki öğreticiler bu dikey penceredeki seçenekleri anlamanıza yardımcı olur. 

    ![yeni örnek veritabanı dikey penceresi](./media/sql-database-get-started/new-sample-db-blade.png)

## <a name="query-the-database-in-the-azure-portal"></a>Azure portalında veritabanını sorgulama

Bu yordamdaki adımlar, veritabanını doğrudan Azure portalında nasıl sorgulayabileceğiniz açıklanır. 

1. SQL veritabanları dikey penceresinde, araç çubuğundaki **Ekle**’ye tıklayın.

    ![araçlar](./media/sql-database-get-started/tools.png)
2. Araçlar dikey penceresinde **Sorgu düzenleyicisi (önizleme)** öğesine tıklayın.

    ![sorgu düzenleyicisi](./media/sql-database-get-started/query-editor.png)
3. Sorgu düzenleyicisinin bir önizleme özelliği olduğunu onaylamak için onay kutusunu işaretleyip **Tamam**’a tıklayın.
4. **Sorgu düzenleyicisi** dikey penceresinde **Oturum Aç**’a tıklayın.

    ![sorgu düzenleyicisi dikey penceresi](./media/sql-database-get-started/query-editor-blade.png)
5. Yetkilendirme türünü ve Oturum Açmayı gözden geçirip bu oturuma ait parolayı belirtin. 

    ![sorgu düzenleyicisi oturum açma](./media/sql-database-get-started/query-editor-login.png)
6. Oturum açmayı denemek için **Tamam**’a tıklayın.
7. Kimlik doğrulaması gerçekleştirdikten sonra sorgu penceresine aşağıdaki sorguyu yazıp **Çalıştır**’a tıklayın.

   ```select * from sys.objects```

    ![sorgu düzenleyici sorgusu](./media/sql-database-get-started/query-editor-query.png)

8. **Sonuçlar** bölmesindeki sorgu sonuçlarını gözden geçirin.

    ![sorgu düzenleyicisi sonuçları](./media/sql-database-get-started/query-editor-results.png)

## <a name="query-the-database-with-ssms"></a>SSMS ile veritabanını sorgulama

Bu yordamdaki adımlarda, SQL Server Management Studio’yu kullanarak veritabanına bağlanma ve sonra veritabanındaki nesneleri görüntülemek üzere örnek verileri sorgulama işlemlerini nasıl gerçekleştirebileceğiniz açıklanır.

1. SQL Server Management Studio’ya geçiş yapın ve örnek veritabanını görüntülemek için, Nesne Gezgini’nde **Veritabanları**’na tıkladıktan sonra araç çubuğundaki **Yenile**’ye tıklayın.

    ![ssms ile yeni örnek veritabanı](./media/sql-database-get-started/new-sample-db-ssms.png)
2. Nesne Gezgini’nde, nesnelerini görüntülemek için yeni veritabanınızı genişletin.

    ![ssms ile yeni örnek veritabanı nesneleri](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Örnek veritabanınıza sağ tıkladıktan sonra **Yeni Sorgu**’ya tıklayın.

    ![ssms ile yeni örnek veritabanı sorgusu](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. Sorgu penceresine aşağıdaki sorguyu yazın:

   ```select * from sys.objects```
   
9.  Araç çubuğunda, örnek veritabanındaki tüm sistem nesnelerinin listesini döndürmek için **Yürüt**’e tıklayın.

    ![ssms ile yeni örnek veritabanında sistem nesneleri sorgusu](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-blank-database-with-ssms"></a>SSMS ile boş veritabanı oluşturma

Bu yordamdaki adımlarda SQL Server Management Studio’yu kullanarak nasıl yeni bir veritabanı oluşturabileceğiniz açıklanır.

1. Nesne Gezgini’nde **Veritabanları**’na sağ tıklayın ve sonra **Yeni veritabanı**’na tıklayın.

    ![ssms ile yeni bir boş veritabanı](./media/sql-database-get-started/new-blank-database-ssms.png)

2. **Yeni Veritabanı** iletişim kutusundaki Veritabanı adı metin kutusunda bir veritabanı adı belirtin. 

    ![ssms ile yeni boş veritabanının adı](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. Yeni Veritabanı iletişim kutusunda, **Seçenekler**’e tıkladıktan sonra, Sürüm’ü **Temel** olarak değiştirin.

    ![ssms ile yeni boş veritabanı seçenekleri](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Bu iletişim kutusunda Azure SQL Veritabanı için değiştirebileceğiniz diğer seçenekleri gözden geçirin. Bu seçenekler hakkında daha fazla bilgi için bkz. [Veritabanı Oluşturma](https://msdn.microsoft.com/library/dn268335.aspx).
    >

4. Boş veritabanını oluşturmak için **Tamam**’a tıklayın.
5. Tamamlandığında, yeni oluşturulan boş veritabanını görüntülemek için Nesne Gezgini’ndeki Veritabanı düğümünü yenileyin. 

    ![nesne gezgini içinde yeni boş veritabanı](./media/sql-database-get-started/new-blank-database-object-explorer.png)

## <a name="troubleshoot-connectivity"></a>Bağlantı sorunlarını giderme

Azure SQL Veritabanına bağlantı başarısız olduğunda hata iletileri alırsınız. Bağlantı sorunlarının nedeni SQL Azure veritabanının yeniden yapılandırılması, güvenlik duvarı ayarları, bağlantının zaman aşımına uğraması veya yanlış oturum bilgilerinin sağlanması olabilir. Bir bağlantı sorun gidericisi aracı için bkz. [Microsoft Azure SQL Veritabanı bağlantı sorunlarını giderme](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database).

## <a name="delete-a-single-database-in-the-azure-portal"></a>Azure portalında tek veritabanını silme

Bu yordamdaki adımlarda, tek veritabanını Azure portalı aracılığıyla nasıl silebileceğiniz açıklanır.

1. Azure portalındaki SQL veritabanları dikey penceresinde silmek istediğiniz veritabanına tıklayın. 
2.  kendi SQL veritabanınız için **Sil**’e tıklayın.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Bu veritabanını kalıcı olarak silme isteğinizi onaylamak için **Evet**’e tıklayın.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Veritabanınız için sağlanan bekletme süresi boyunca veritabanınızı hizmet tarafından başlatılan otomatik yedeklemelerden geri yükleyebilirsiniz (sunucunun kendisini silmediğiniz sürece). Temel sürüm veritabanlarını yedi gün içinde geri yükleyebilirsiniz. Diğer tüm sürümlerde veritabanı 35 gün içerisinde geri yüklenebilir. Sunucunun kendisini silerseniz, sunucuyu ve silinen veritabanlarının hiçbirini kurtaramazsınız. Veritabanı yedeklemeleri hakkında daha fazla bilgi için [SQL Veritabanı yedeklemeleri](sql-database-automated-backups.md), yedeklemelerden veritabanını geri yükleme hakkında bilgi için [Veritabanı kurtarma](sql-database-recovery-using-backups.md) bölümüne bakın. Silinmiş bir veritabanını geri yüklemeye ilişkin nasıl yapılır makalesi için bkz. [Silinmiş bir Azure SQL veritabanını geri yükleme - Azure portal](sql-database-restore-deleted-database-portal.md).
>


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyi tamamladığınıza göre; bu öğreticide öğrendiklerinizi geliştirecek ek öğreticilerden birini keşfetmek isteyebilirsiniz. 

- SQL Server kimlik doğrulama öğreticisini kullanmaya başlamak için bkz. [SQL kimlik doğrulaması ve yetkilendirme](sql-database-control-access-sql-authentication-get-started.md)
- Azure Active Directory kimlik doğrulama öğreticisini kullanmaya başlamak için bkz. [AAD kimlik doğrulaması ve yetkilendirme](sql-database-control-access-aad-authentication-get-started.md)
* Azure portalındaki örnek veritabanını sorgulamak isterseniz bkz. [Genel önizleme: SQL veritabanları için etkileşimli sorgu deneyimi](https://azure.microsoft.com/updates/azure-sql-database-public-preview-t-sql-editor/)
* Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.
* Şirket içi SQL Server veritabanlarınızı Azure’a taşımak istiyorsanız, bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).
* BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP ile bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).
* Tabloları ve diğer nesneleri oluşturmaya başlamak istiyorsanız, [Tablo oluşturma](https://msdn.microsoft.com/library/ms365315.aspx) içindeki “Tablo oluşturmak için” konusuna bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- Teknik bir genel bakış için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Fiyatlandırma bilgileri için bkz. [Azure SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO3-->


