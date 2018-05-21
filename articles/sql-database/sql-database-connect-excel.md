---
title: Excel'i SQL Veritabanı'na bağlama | Microsoft Belgeleri
description: Microsoft Excel'i bulut üzerinde Azure SQL veritabanına nasıl bağlayacağınızı öğrenin. Raporlama ve veri araştırması için Excel'e veri aktarın.
services: sql-database
keywords: excel’i sql’e bağlama, verileri excel’e aktarma
author: joseidz
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 03/10/2017
ms.author: craigg
ms.openlocfilehash: 6f2894d65240580346b99d203f8289652d8e6618
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Excel'i bir Azure SQL veritabanına bağlama ve rapor oluşturma

SQL Database'i bulutta Excel bağlanmak ve veri içeri aktarın ve tablolar ve grafikler veritabanındaki değerlere göre oluşturun. Bu öğreticide, Excel ile bir veritabanı tablosu arasındaki bağlantıyı ayarlayacak, Excel'e ilişkin verilerin ve bağlantı bilgilerinin depolandığı dosyayı kaydedecek ve ardından veritabanı değerlerini kullanarak bir özet grafik oluşturacaksınız.

Başlayabilmek için Azure'da bir SQL veritabanınızın olması gerekir. Henüz bir veritabanınız yoksa birkaç dakika içinde çalışır durumda olan, örnek veriler içeren bir veritabanı edinmek üzere [İlk SQL veritabanınızı oluşturma](sql-database-get-started-portal.md) adlı makaleye göz atın. Bu makalede, ilgili makaleden Excel'e örnek veriler aktaracaksınız ancak kendi verilerinizle benzer adımları izleyebilirsiniz.

Ayrıca, bir Excel kopyanızın olması gerekir. Bu makalede [Microsoft Excel 2016](https://products.office.com/) kullanılmıştır.

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Excel'i bir SQL veritabanı ve yük veri bağlama
1. Excel'i SQL veritabanına bağlamak için Excel'i açarak yeni bir çalışma kitabı oluşturun veya var olan bir Excel çalışma kitabını açın.
2. Sayfanın üstündeki menü çubuğunda seçin **veri** sekmesine **Veri Al**Azure seçin ve ardından **Azure SQL veritabanından**. 
   
   ![Veri kaynağı seçin: Excel'i SQL veritabanına bağlayın.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Veri Bağlantı Sihirbazı açılır.
3. **Veritabanı Sunucusuna Bağlan** iletişim kutusunda, bağlanmak istediğiniz SQL Database **Sunucu adını** şu biçimde girin: <*sunucuadı*>**.database.windows.net**. Örneğin, **msftestserver.database.windows.net**. İsteğe bağlı olarak, veritabanınızın adını girin. Seçin **Tamam** kimlik bilgileri penceresini açın. 

   ![Sunucu name.png](media/sql-database-connect-excel/server-name.png)

1. İçinde **SQL Server veritabanı** iletişim kutusunda **veritabanı** sol yan ve ardından girin, **kullanıcı adı** ve **parola** için SQL veritabanı sunucusuna bağlanmak istediğiniz. Seçin **Bağlan** açmak için **Gezgini**. 

  ![Sunucu adını ve oturum açma kimlik bilgilerini girme](./media/sql-database-connect-excel/connect-to-server.png)
   
  > [!TIP]
  > Ağ ortamınıza bağlı olarak, SQL Database sunucusunun istemci IP adresinizden gelen trafiğe izin vermemesi halinde bağlanamayabilirsiniz veya mevcut bağlantınız kesilebilir. [Azure portalına](https://portal.azure.com/) gidip SQL sunucuları seçeneğine tıklayın, sunucunuza tıklayın ve ardından ayarlar altında bulunan güvenlik duvarı seçeneğine tıklayıp istemci IP adresinizi ekleyin. Ayrıntılı bilgi için bkz. [Güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).
   
   
5. İçinde **Gezgini**seçin tabloları veya görünümleri ile çalışmak istediğiniz listeden çalışmak istediğiniz veritabanını seçin (seçtik **vGetAllCategories**) ve ardından **yük**için SQL Azure veritabanınızdaki verileri taşımak için excel elektronik tablo.
   
    ![Bir veritabanı ve tablo seçin.](./media/sql-database-connect-excel/select-database-and-table.png)
   

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Excel'e veri aktarma ve özet grafik oluşturma
Bağlantı belirlediğinize göre veri yükleme ile birkaç farklı seçeneğiniz vardır. Örneğin, aşağıdaki adımlar, SQL veritabanında bulunan verileri temel alan bir Özet Grafik oluşturur. 

1. Önceki bölümde ancak seçmek yerine bu kez, adımları **yük**seçin **yük** gelen **yük** açılır.
2. Ardından, nasıl çalışma kitabınızı bu verileri görüntülemek istediğinizi seçin. Biz **PivotChart** seçeneğini belirledik. Ayrıca, **Yeni çalışma sayfası** oluşturmayı veya **Bu verileri Veri Modeline ekle** seçeneğini belirlemeyi de tercih edebilirsiniz. Veri Modelleri hakkında daha fazla bilgi için bkz. [Excel'de veri modeli oluşturma](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 
   
    ![Excel'de veri biçimini belirleme](./media/sql-database-connect-excel/import-data.png)
   
    Artık çalışma sayfasında boş bir özet grafiği ve grafik var.
2. **PivotTable Alanları** altında, görüntülemek istediğiniz alanların onay kutularını işaretleyin.
   
    ![Veritabanı raporunu yapılandırın.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Diğer Excel çalışma kitaplarını ve çalışma sayfalarını veritabanına bağlamak istiyorsanız seçin **veri** sekmesini tıklatın ve seçin **son kaynakları** başlatmak için **son kaynakları** iletişim kutusu. Buradan listeden oluşturduğunuz bağlantıyı seçin ve ardından **açık**.
> ![Yeni bağlantıları](media/sql-database-connect-excel/recent-connections.png)
 
## <a name="create-a-permanent-connection-using-odc-file"></a>.Odc dosyasını kullanarak kalıcı bir bağlantı oluşturma
Bağlantı ayrıntıları kalıcı olarak kaydetmek için bir .odc dosyası oluşturabilir ve bu bağlantısı seçilebilir seçeneği içinde **varolan bağlantılar** iletişim kutusu. 

1. Sayfanın üstündeki menü çubuğunda seçin **veri** sekmesini tıklatın ve ardından **varolan bağlantılar** başlatmak için **varolan bağlantılar** iletişim kutusu. 
    1. Seçin **daha fazlası için Gözat** açmak için **veri kaynağı Seç** iletişim kutusu.   
    2. Seçin **+NewSqlServerConnection.odc** dosya ve ardından **açmak** açmak için **Veri Bağlantı Sihirbazı**.

    ![Yeni Bağlantı](media/sql-database-connect-excel/new-connection.png)

2. İçinde **Veri Bağlantı Sihirbazı'nı**, sunucunuzun adını ve SQL veritabanı kimlik bilgileri türü. **İleri**’yi seçin. 
    1. Aşağı açılan verilerinizden içeren bir veritabanı seçin. 
    2. Tablo veya Görünüm ilgilendiğiniz seçin. VGetAllCategories seçtik.
    3. **İleri**’yi seçin. 

    ![Veri Bağlantı Sihirbazı](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Dosya konumunu seçin **dosya adı**ve **kolay ad** Veri Bağlantı Sihirbazı'nın sonraki ekranında. Bu olası istenmeyen erişimi verilerinize getirebilir olsa parolayı dosyaya kaydet seçebilirsiniz. Seçin **son** ne zaman hazır. 

    ![Veri bağlantısı Kaydet](media/sql-database-connect-excel/save-data-connection.png)

4. Nasıl verilerinizi almak istediğinizi seçin. Bir PivotTable yapmak seçtik. Tarafından seçin bağlantı özelliklerini de değiştirebilirsiniz **özellikleri**. Seçin **Tamam** zaman hazır. Ardından parolayı dosyayla kaydetmek seçmedi, kimlik bilgilerinizi girmeniz istenir. 

    ![Veri alma](media/sql-database-connect-excel/import-data2.png)

5. Yeni bağlantınızı genişleterek kaydedildi doğrulayın **veri** sekmesini tıklatın ve seçme **varolan bağlantılar**. 

    ![Varolan bir bağlantıyı](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Sonraki adımlar
* Gelişmiş sorgulama ve analiz için [SQL Server Management Studio ile SQL Database'e bağlanma](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* [Esnek havuzların](sql-database-elastic-pool.md) avantajları hakkında bilgi edinin.
* [Arka uçta SQL Database'e bağlanan bir web uygulaması oluşturma](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md) hakkında bilgi edinin.

