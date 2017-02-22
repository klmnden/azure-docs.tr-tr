---
title: "Hızlı başlangıç: İlk Azure SQL Veritabanınız | Microsoft Belgeleri"
description: "Azure portalı ile SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturmayı öğrenin. Ayrıca SQL Server Management Studio’yu Azure SQL Veritabanı ile kullanma hakkında bilgi alın."
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
ms.date: 02/04/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 6453cca9f876e6c363fbed463263c0f9684a3e70
ms.openlocfilehash: b838974de06ecbc751254064e2310df51c450086


---
# <a name="quick-start-tutorial-your-first-azure-sql-database"></a>Hızlı başlangıç öğreticisi: İlk Azure SQL veritabanınız

Bu hızlı başlangıç öğreticisinde şunlar hakkında bilgi alacaksınız:

* [Yeni mantıksal sunucu oluşturma](sql-database-get-started.md#create-a-new-logical-sql-server) 
* [Mantıksal sunucu özelliklerini görüntüleme](sql-database-get-started.md#view-the-logical-server-properties) 
* [Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma](sql-database-get-started.md#create-a-server-level-firewall-rule) 
* [SSMS ile sunucuya bağlanma](sql-database-get-started.md#connect-to-the-server-with-ssms) 
* [Örnek verilerle veritabanı oluşturma](sql-database-get-started.md#create-a-database-with-sample-data) 
* [Veritabanı özelliklerini görüntüleme](sql-database-get-started.md#view-the-database-properties) 
* [Azure portalında veritabanını sorgulama](sql-database-get-started.md#query-the-database-in-the-azure-portal) 
* [SSMS ile veritabanına bağlanma ve sorgulama](sql-database-get-started.md#connect-and-query-the-database-with-ssms) 
* [SSMS ile boş veritabanı oluşturma](sql-database-get-started.md#create-a-blank-database-with-ssms) 
* [Bağlantı sorunlarını giderme](sql-database-get-started.md#troubleshoot-connectivity) 
* [Veritabanı silme](sql-database-get-started.md#delete-a-single-database) 


Bu hızlı başlangıç öğreticisinde, Azure kaynak grubu içinde çalışan ve mantıksal sunucuya bağlı bir örnek veritabanı ve bir boş veritabanı oluşturursunuz. Ayrıca, sunucu düzeyindeki sorumlunun, sunucuda belirtilen IP adresinden oturum açabilmesini sağlamak için yapılandırılmış iki sunucu düzeyinde güvenlik duvarı kuralı da oluşturursunuz. Son olarak, Azure portalında bir veritabanını sorgulama ve SQL Server Management Studio kullanarak bağlanıp sorgulama işlemleri hakkında bilgi alırsınız. 

**Tahmini süre**: Bu öğretici yaklaşık 30 dakika sürer (önkoşulları karşıladığınız varsayılarak).

> [!TIP]
> Aynı görevleri [C#](sql-database-get-started-csharp.md) veya [PowerShell](sql-database-get-started-powershell.md) ile gerçekleştirebilirsiniz.
>

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Bu hızlı başlangıç öğreticisi şu konu başlıklarının içeriğini öğrenmenize yardımcı olacaktır: [SQL Veritabanı sunucusuna genel bakış](sql-database-server-overview.md), [SQL veritabanına genel bakış](sql-database-overview.md) ve [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-with-your-azure-account"></a>Azure hesabınızla Azure portalında oturum açma
[Azure hesabınızla](https://account.windowsazure.com/Home/Index) Azure portalına bağlanmak için aşağıdaki adımları uygulayın.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Yeni mantıksal SQL sunucusu oluşturma

Azure portalı ile tercih ettiğiniz bölgede yeni bir mantıksal sunucu oluşturmak için bu yordamdaki adımları izleyin.

1. **Yeni**’ye tıklayıp **sql sunucusu** yazın ve sonra **ENTER**’a basın.

    ![mantıksal sql sunucusu](./media/sql-database-get-started/logical-sql-server.png)
2. **SQL sunucusu (mantıksal sunucu)** seçeneğine tıklayın.
   
    ![oluştur-mantıksal sql sunucusu](./media/sql-database-get-started/create-logical-sql-server.png)
3. Yeni SQL Sunucusu (mantıksal sunucu) dikey penceresini açmak için **Oluştur**’a tıklayın.

    ![yeni-mantıksal sql sunucusu](./media/sql-database-get-started/new-logical-sql-server.png)
3. Sunucu adı metin kutusunda, yeni mantıksal sunucu için geçerli bir ad belirtin. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![yeni sunucu adı](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Yeni sunucunuzun tam adı <sunucunuzun_adi>.database.windows.net biçiminde olacaktır.
    >
    
4. Sunucu yöneticisi oturum açma adı metin kutusunda, bu sunucuda SQL kimlik doğrulaması oturum açma işlemi için kullanılacak olan kullanıcı adını belirtin. Bu oturum açma işlemi, asıl sunucu oturum açma işlemi olarak bilinir. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![SQL yöneticisi oturum açma adı](./media/sql-database-get-started/sql-admin-login.png)
5. **Parola** ve **Parolayı onaylayın** metin kutularında, sunucu sorumlusu oturum açma hesabı için parola belirtin. Yeşil onay işareti, geçerli bir parola sağladığınızı gösterir.
    
    ![SQL yönetici parolası](./media/sql-database-get-started/sql-admin-password.png)
6. İçinde nesne oluşturma izniniz olan bir abonelik seçin.

    ![aboneliği](./media/sql-database-get-started/subscription.png)
7. Kaynak grubu metin kutusunda **Yeni oluştur**’u seçin ve sonra kaynak grubu metin kutusunda yeni kaynak grubu için geçerli bir isim belirtin (daha önce oluşturduğunuz bir kaynak grubu varsa, bu kaynak grubunu da kullanabilirsiniz). Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.

    ![yeni kaynak grubu](./media/sql-database-get-started/new-resource-group.png)

8. **Konum** metin kutusunda, konumunuza uygun bir veri merkezi (örneğin, "Avustralya Doğu") seçin.
    
    ![sunucu konumu](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > **Azure hizmetlerinin sunucuya erişmesine izin ver** onay kutusu bu dikey pencerede değiştirilemez. Bu ayarı, sunucu güvenlik duvarı dikey penceresinde değiştirebilirsiniz. Daha fazla bilgi için bkz. [Güvenliğe giriş](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-server-properties"></a>Mantıksal sunucu özelliklerini görüntüleme

Azure portalı ile sunucu özelliklerini görüntülemek için bu yordamdaki adımları izleyin. Sonraki bir yordamda bu sunucuya bağlanmak için tam sunucu adı gerekli olacaktır. 

1. Azure portalında **Diğer hizmetler**’e tıklayın.

    ![diğer hizmetler](./media/sql-database-get-started/more-services.png)
2. Filtre metin kutusunda, **SQL** yazın ve Azure içinde SQL sunucularını sık kullanılan olarak belirtmek için, SQL sunucuları seçeneğinin yanındaki yıldıza tıklayın. 

    ![sık kullanılan olarak ayarla](./media/sql-database-get-started/favorite.png)
3. Varsayılan dikey pencerede, Azure aboneliğinizde SQL sunucularının listesini açmak için **SQL sunucuları**’na tıklayın. 

    ![yeni sql sunucusu](./media/sql-database-get-started/new-sql-server.png)

4. Azure portalında özelliklerini görüntülemek için yeni SQL sunucunuza tıklayın. Sonraki öğreticiler bu dikey penceredeki seçenekleri anlamanıza yardımcı olur.

    ![sql sunucusu dikey penceresi](./media/sql-database-get-started/sql-server-blade.png)
5. Mantıksal SQL sunucusunun çeşitli özelliklerini görüntülemek için Ayarlar’ın altındaki **Özellikler**’e tıklayın.

    ![sql sunucusu özellikleri](./media/sql-database-get-started/sql-server-properties.png)
6. Bu öğreticinin devamında kullanmak üzere, tam sunucu adını panonuza kopyalayın.

    ![sql sunucusu tam adı](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

Sonraki yordamda SQL Server Management Studio ile sunucunuza bağlanmak için Azure portalı ile yeni bir sunucu düzeyinde güvenlik duvarı kuralı oluşturmak üzere bu yordamdaki adımları izleyin.

1. SQL sunucusu dikey penceresinde, Ayarlar’ın altında; SQL sunucusunun Güvenlik Duvarı dikey penceresini açmak için **Güvenlik Duvarı**’na tıklayın.

    ![sql sunucusu güvenlik duvarı](./media/sql-database-get-started/sql-server-firewall.png)

2. Araç çubuğundaki **İstemci IP’si ekle** öğesine tıklayın.

    ![istemci IP’si ekle](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Sunucudaki SQL Veritabanı güvenlik duvarını, tek bir IP adresi veya bir adresler aralığının tamamı üzerinde açabilirsiniz. Güvenlik duvarını açmak SQL yönetici ve kullanıcılarının; sunucuda, geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanında oturum açmasını sağlar.
    >

4. Sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için, araç çubuğunda **Kaydet**’e ve sonra **Tamam**’a tıklayın.

    ![istemci IP’si ekle](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>SSMS ile sunucuya bağlanma

SQL Server Management Studio ile SQL mantıksal sunucusuna bağlanmak için bu yordamdaki adımları izleyin.

1. Eğer henüz yapmadıysanız, [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndir) sayfasından SSMS’in en son sürümünü indirin ve yükleyin. SSMS'nin son sürümü, güncel kalabilmek için indirebileceğiniz yeni bir sürüm olduğunda size bir istem gönderir.

2. Yükledikten sonra, Windows arama kutusuna **Microsoft SQL Server Management Studio** yazın ve SSMS’yi açmak için **Enter**’a basın:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. Sunucuya Bağlan iletişim kutusunda; SQL Server Kimlik Doğrulaması ile SQL sunucunuza bağlanmak için gereken bilgileri girin.

    ![sunucuya bağlan](./media/sql-database-get-started/connect-to-server.png)
4. **Bağlan**'a tıklayın.

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
    > SQL güvenliğini keşfetmek için bkz. [SQL güvenliğine giriş](sql-database-control-access-sql-authentication-get-started.md)
    >

## <a name="create-a-database-with-sample-data"></a>Örnek verilerle veritabanı oluşturma

Azure portal ile örnek veriler içeren bir veritabanı oluşturmak için bu yordamdaki adımları izleyin. Bu veritabanını, daha önce oluşturduğunuz mantıksal sunucuya bağlı olarak oluşturursunuz. Sunucuyu oluşturduğunuz bölgede temel hizmet katmanı mevcut değilse, sunucunuzu silin ve başka bir bölgede yeniden oluşturun. Silme adımları için bu öğreticideki son yordama bakın.

1. Azure portalında, varsayılan dikey penceredeki **SQL veritabanı**’na tıklayın.

    ![sql veritabanları](./media/sql-database-get-started/new-sql-database.png)
2. SQL veritabanları dikey penceresindeki **Ekle**’ye tıklayın.

    ![sql veritabanı ekle](./media/sql-database-get-started/add-sql-database.png)
3. SQL Veritabanı dikey penceresinde sizin için tamamlanan bilgileri gözden geçirin.

    ![sql veritabanı dikey penceresi](./media/sql-database-get-started/sql-database-blade.png)
4. Geçerli bir etki alanı adı belirtin.

    ![sql veritabanı adı](./media/sql-database-get-started/sql-database-name.png)
5. Kaynak seçin’in altında **Örnek**’e tıkladıktan sonra Örnek seçin’in altında **AdventureWorksLT [V12]**’ye tıklayın.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
6. Sunucu altında, sunucu yöneticisi oturum açma kullanıcı adını ve parolasını belirtin.

    ![sunucu kimlik bilgileri](./media/sql-database-get-started/server-credentials.png)

    > [!NOTE]
    > Veritabanları, sunuculara tek veritabanı olarak eklenebileceği gibi (varsayılan) elastik havuzlara da eklenebilir. Elastik havuzlar hakkında daha fazla bilgi için bkz. [Elastik havuzlar](sql-database-elastic-pool.md).
    >

7. Fiyatlandırma katmanı altında, fiyatlandırma katmanını **Temel** olarak değiştirin (isterseniz daha sonra fiyatlandırma katmanını artırabilirsiniz ancak öğrenme amacıyla en düşük maliyetli katmanı kullanmanızı öneririz).

    ![fiyatlandırma katmanı](./media/sql-database-get-started/pricing-tier.png)
8. **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-database-get-started/create.png)

## <a name="view-the-database-properties"></a>Veritabanı özelliklerini görüntüleme

Azure portalı ile veritabanını sorgulamak için bu yordamdaki adımları izleyin.

1. SQL veritabanı dikey penceresinde, özelliklerini Azure portalında görüntülemek için yeni veritabanınıza tıklayın. Sonraki öğreticiler bu dikey penceredeki seçenekleri anlamanıza yardımcı olur. 

    ![yeni örnek veritabanı dikey penceresi](./media/sql-database-get-started/new-sample-db-blade.png)
2. Veritabanınız hakkındaki ek bilgileri görüntülemek için **Özellikler**’e tıklayın.

    ![yeni örnek veritabanı özellikleri](./media/sql-database-get-started/new-sample-db-properties.png)

3. **Veritabanı bağlantı dizelerini göster**’e tıklayın.

    ![yeni örnek veritabanı bağlantı dizeleri](./media/sql-database-get-started/new-sample-db-connection-strings.png)
4. **Genel Bakış**’a tıkladıktan sonra, Temel Bileşenler bölmesinde sunucu adınıza tıklayın.
    
    ![yeni örnek veritabanı temel bileşenler bölmesi](./media/sql-database-get-started/new-sample-db-essentials-pane.png)
5. Sunucunuzun Temel Bileşenler bölmesinde, yeni eklenen veritabanınızı görebilirsiniz.

    ![sunucu temel bileşenleri bölmesindeki yeni örnek veritabanı](./media/sql-database-get-started/new-sample-db-server-essentials-pane.png)

## <a name="query-the-database-in-the-azure-portal"></a>Azure portalında veritabanını sorgulama

Azure portalındaki sorgu düzenleyicisi ile veritabanını sorgulamak için bu yordamdaki adımları izleyin. Sorgu, veritabanındaki nesneleri gösterir.

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
7. İstemcinizin IP adresine yönelik güvenlik duvarı kuralı mevcut olmadığından istemcinizin izni olmadığını belirten bir oturum açma hatası alırsanız, istemcinizin hata penceresindeki IP adresini kopyalayın ve bu veritabanının SQL sunucusu dikey penceresinde sunucu düzeyinde güvenlik duvarı kuralı oluşturun.

    ![sorgu düzenleyicisi hatası](./media/sql-database-get-started/query-editor-error.png)
8. Veritabanınızda oturum açmak için önceki 6 adımı yineleyin.
9. Kimlik doğrulaması yaptıktan sonra sorgu penceresine aşağıdaki sorguyu yazın:

   ```select * from sys.objects```

    ![sorgu düzenleyici sorgusu](./media/sql-database-get-started/query-editor-query.png)
10.  **Çalıştır**’a tıklayın.
11. **Sonuçlar** bölmesindeki sorgu sonuçlarını gözden geçirin.

    ![sorgu düzenleyicisi sonuçları](./media/sql-database-get-started/query-editor-results.png)

## <a name="connect-and-query-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma ve sorgulama

SQL Server Management Studio ile veritabanına bağlanmak ve sonra veritabanındaki nesneleri görüntülemek üzere örnek verileri sorgulamak için bu yordamdaki adımları izleyin.

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

SQL Server Management Studio ile mantıksal sunucu üzerinde yeni bir veritabanı oluşturmak için bu yordamdaki adımları izleyin.

1. Nesne Gezgini’nde **Veritabanları**’na sağ tıklayın ve sonra **Yeni veritabanı**’na tıklayın.

    ![ssms ile yeni bir boş veritabanı](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > Ayrıca, SSMS’nin sizin için Transact-SQL ile bir veritabanı oluşturma betiği oluşturmasını sağlayabilirsiniz.
    >

2. Yeni Veritabanı iletişim kutusunda, Veritabanı adı metin kutusunda bir veritabanı adı belirtin. 

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

> [!IMPORTANT]
> Bağlantı sorunlarınız varsa bkz. [Bağlantı sorunları](sql-database-troubleshoot-common-connection-issues.md).
> 

## <a name="delete-a-single-database"></a>Tek veritabanını silme

Azure portalı aracılığıyla tek veritabanını silmek için bu yordamdaki adımları izleyin.

1. Azure portalında SQL veritabanınıza ait dikey pencerede **Sil**’e tıklayın.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Bu veritabanını kalıcı olarak silme isteğinizi onaylamak için **Evet**’e tıklayın.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Veritabanınız için bekletme süresi boyunca veritabanınızı hizmet tarafından başlatılan otomatik yedeklemelerden geri yükleyebilirsiniz. Temel sürüm veritabanlarını yedi gün içinde geri yükleyebilirsiniz. Ancak sunucuyu silmeyin. Bunu yapmanız durumunda, sunucuyu ve silinen veritabanlarının hiçbirini kurtaramazsınız. Veritabanı yedeklemeleri hakkında daha fazla bilgi için [SQL Veritabanı yedeklemeleri](sql-database-automated-backups.md), yedeklemelerden veritabanını geri yükleme hakkında bilgi için [Veritabanı kurtarma](sql-database-recovery-using-backups.md) bölümüne bakın. Silinmiş bir veritabanını geri yüklemeye ilişkin nasıl yapılır makalesi için bkz. [Silinmiş bir Azure SQL veritabanını geri yükleme - Azure portal](sql-database-restore-deleted-database-portal.md).
>


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyi tamamladığınıza göre; bu öğreticide öğrendiklerinizi geliştirecek ek öğreticilerden birini keşfetmek isteyebilirsiniz. 

- SQL Server kimlik doğrulama öğreticisini kullanmaya başlamak için bkz. [SQL kimlik doğrulaması ve yetkilendirme](sql-database-control-access-sql-authentication-get-started.md)
- Azure Active Directory kimlik doğrulama öğreticisini kullanmaya başlamak için bkz. [AAD kimlik doğrulaması ve yetkilendirme](sql-database-control-access-aad-authentication-get-started.md)
* Azure portalındaki örnek veritabanını sorgulamak isterseniz bkz. [Genel önizleme: SQL veritabanları için etkileşimli sorgu deneyimi](https://azure.microsoft.com/en-us/updates/azure-sql-database-public-preview-t-sql-editor/)
* Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.
* Şirket içi SQL Server veritabanlarınızı Azure’a taşımak istiyorsanız, bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).
* BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP ile bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).
* Tabloları ve diğer nesneleri oluşturmaya başlamak istiyorsanız, [Tablo oluşturma](https://msdn.microsoft.com/library/ms365315.aspx) içindeki “Tablo oluşturmak için” konusuna bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- Teknik bir genel bakış için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Fiyatlandırma bilgileri için bkz. [Azure SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO2-->


