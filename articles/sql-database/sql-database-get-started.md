---
title: "SQL Veritabanı öğreticisi: Sunucu, sunucu düzeyinde güvenlik duvarı kuralı, örnek veritabanı, veritabanı düzeyinde güvenlik duvarı oluşturma ve SQL Server Management Studio ile bağlanma | Microsoft Docs"
description: "SQL Veritabanı mantıksal sunucusu, sunucu güvenlik duvarı kuralı, SQL veritabanı ve örnek veriler oluşturma hakkında bilgi edinin. Ayrıca, istemci araçlarına bağlanma, kullanıcıları yapılandırma ve bir veritabanı güvenlik duvarı kuralı oluşturma hakkında bilgi alın."
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
ms.date: 11/23/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: c2252fc81f97019391ca2ba957f8402c4e97a9c2
ms.openlocfilehash: f9b17c1cc77918fb1989b94b5bb359a697ceea7c


---
# <a name="get-started-with-azure-sql-database-servers-databases-and-firewall-rules-by-using-the-azure-portal-and-sql-server-management-studio"></a>Azure portalı ve SQL Server Management Studio aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama

Bu kullanmaya başlama öğreticisinde, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

* Yeni bir Azure kaynak grubu oluşturma
* Azure SQL mantıksal sunucusu oluşturma
* Azure SQL mantıksal sunucusu özelliklerini görüntüleme
* Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
* Adventure Works LT örnek veritabanını tek veritabanı olarak oluşturma
* Azure’da Adventure Works LT örnek veritabanı özelliklerini görüntüleme

Bu öğreticide, SQL Server Management Studio’nun en son sürümünü kullanarak aynı zamanda şu işlemleri yapmayı da öğreneceksiniz:

* Mantıksal sunucuya ve asıl veritabanına bağlanma
* Asıl veritabanını sorgulama
* Örnek veritabanına bağlanma
* Örnek veritabanını sorgulama

Bu öğreticiyi tamamladığınızda; Azure kaynak grubu içinde çalışan ve mantıksal sunucuya bağlı bir örnek veritabanınız ve bir de boş veritabanınız olur. Ayrıca, sunucu düzeyindeki sorumlunun, sunucuda belirtilen IP adresinden (veya IP adresi aralığından) oturum açabilmesini sağlamak için yapılandırılmış bir sunucu düzeyinde güvenlik duvarı kuralına da sahip olacaksınız. 

**Tahmini süre**: Bu öğretici yaklaşık 30 dakika sürer (önkoşulları karşıladığınız varsayılarak).

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

> [!TIP]
> Aynı görevleri, [C#](sql-database-get-started-csharp.md) veya [PowerShell](sql-database-get-started-powershell.md) aracılığıyla kullanmaya başlama öğreticilerinde de gerçekleştirebilirsiniz.
>

### <a name="sign-in-by-using-your-existing-account"></a>Var olan hesabınızı kullanarak oturum açın
[Var olan aboneliğinizi](https://account.windowsazure.com/Home/Index) kullanarak Azure portala bağlanmak için aşağıdaki adımları uygulayın.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Azure portalında yeni bir mantıksal SQL sunucusu oluşturma

1. **Yeni**’ye tıklayıp **sql sunucusu** yazın ve sonra **ENTER**’a basın.

    ![mantıksal sql sunucusu](./media/sql-database-get-started/logical-sql-server.png)
2. **SQL sunucusu (mantıksal sunucu)** seçeneğine tıklayın.
   
    ![oluştur-mantıksal sql sunucusu](./media/sql-database-get-started/create-logical-sql-server.png)
3. Yeni SQL Sunucusu (mantıksal sunucu) dikey penceresini açmak için **Oluştur**’a tıklayın.

    ![yeni-mantıksal sql sunucusu](./media/sql-database-get-started/new-logical-sql-server.png)
3. Sunucu adı metin kutusunda, yeni mantıksal sunucu için geçerli bir ad belirtin. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![yeni sunucu adı](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Yeni sunucunuzun tam adı <sunucu_adınız>.database.windows.net olacaktır.
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
    > **Azure hizmetlerinin sunucuya erişmesine izin ver** onay kutusu bu dikey pencerede değiştirilemez. Bu ayarı, sunucu güvenlik duvarı dikey penceresinde değiştirebilirsiniz. Daha fazla bilgi için bkz. [Güvenliğe giriş](sql-database-get-started-security.md).
    >
    
9. **Oluştur**’a tıklayın.

    ![oluştur düğmesi](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-sql-server-properties-in-the-azure-portal"></a>Azure portalında mantıksal SQL sunucusu özelliklerini görüntüleme

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

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. SQL sunucusu dikey penceresinde, Ayarlar’ın altında; SQL sunucusunun Güvenlik Duvarı dikey penceresini açmak için **Güvenlik Duvarı**’na tıklayın.

    ![sql sunucusu güvenlik duvarı](./media/sql-database-get-started/sql-server-firewall.png)

2. Görüntülenen istemci IP adresini gözden geçirin ve bunun sizin IP adresiniz olduğunu, tercih ettiğiniz bir tarayıcıyı kullanarak (“IP adresim nedir” sorusuyla) İnternet üzerinden doğrulayın. IP adresleri bazen çeşitli nedenlerle eşleşmez.

    ![IP adresiniz](./media/sql-database-get-started/your-ip-address.png)

3. IP adreslerinin eşleştiğini varsayarak, araç çubuğundaki **İstemci IP’si ekle**’ye tıklayın.

    ![istemci IP’si ekle](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Sunucudaki SQL Veritabanı güvenlik duvarını, tek bir IP adresi veya bir adresler aralığının tamamı üzerinde açabilirsiniz. Güvenlik duvarını açmak SQL yönetici ve kullanıcılarının; sunucuda, geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanında oturum açmasını sağlar.
    >

4. Sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için, araç çubuğunda **Kaydet**’e ve sonra **Tamam**’a tıklayın.

    ![istemci IP’si ekle](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>SQL sunucusuna, SQL Server Management Studio (SSMS) kullanarak bağlanma

1. Eğer henüz yapmadıysanız, [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndir) sayfasından SSMS’in en son sürümünü indirin ve yükleyin. SSMS'nin son sürümü, güncel kalabilmek için indirebileceğiniz yeni bir sürüm olduğunda size bir istem gönderir.

2. Yükledikten sonra, Windows arama kutusuna **Microsoft SQL Server Management Studio** yazın ve SSMS’yi açmak için **Enter**’a basın:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. Sunucuya Bağlan iletişim kutusunda; SQL Server Kimlik Doğrulaması’nı kullanarak, SQL sunucunuza bağlanmak için gereken bilgileri girin.

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
    > SQL güvenliğini keşfetmek için bkz. [SQL güvenliğine giriş](sql-database-get-started-security.md)
    >

## <a name="create-new-database-in-the-azure-portal-using-adventure-works-lt-sample"></a>Adventure Works LT örneğini kullanarak Azure portalında yeni veritabanı oluşturma

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

## <a name="view-database-properties-in-the-azure-portal"></a>Azure portalında veritabanı özelliklerini görüntüleme

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

## <a name="connect-and-query-sample-database-using-sql-server-management-studio"></a>SQL Server Management Studio kullanarak örnek veritabanına bağlanma ve veritabanını sorgulama

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

## <a name="create-a-new-blank-database-using-sql-server-management-studio"></a>SQL Server Management Studio kullanarak yeni bir boş veritabanı oluşturma

1. Nesne Gezgini’nde **Veritabanları**’na sağ tıklayın ve sonra **Yeni veritabanı**’na tıklayın.

    ![ssms ile yeni bir boş veritabanı](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > Ayrıca, SSMS’nin sizin için Transact-SQL kullanarak bir veritabanı oluşturma betiği oluşturmasını sağlayabilirsiniz.
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

> [!TIP]
> Öğrenirken, kullanmadığınız veritabanlarını silerek maliyet tasarrufu yapabilirsiniz. Temel sürüm veritabanlarını yedi gün içinde geri yükleyebilirsiniz. Ancak sunucuyu silmeyin. Bunu yapmanız durumunda, sunucuyu ve silinen veritabanlarının hiçbirini kurtaramazsınız.
>


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyi tamamladığınıza göre; bu öğreticide öğrendiklerinizi geliştirecek ek öğreticilerden birini keşfetmek isteyebilirsiniz. 

* Azure SQL Veritabanı güvenliğini keşfetmeye başlamak isterseniz bkz. [Güvenliğe giriş](sql-database-get-started-security.md).
* Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.
* Şirket içi SQL Server veritabanlarınızı Azure’a taşımak istiyorsanız, bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).
* BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP kullanarak bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).
* Tabloları ve diğer nesneleri oluşturmaya başlamak istiyorsanız, [Tablo oluşturma](https://msdn.microsoft.com/library/ms365315.aspx) içindeki “Tablo oluşturmak için” konusuna bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- Teknik bir genel bakış için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Fiyatlandırma bilgileri için bkz. [Azure SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Dec16_HO3-->


