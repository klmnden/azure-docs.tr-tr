---
title: Excel Azure SQL veritabanı'nda tek bir veritabanına bağlanma | Microsoft Docs
description: Microsoft Excel Azure SQL veritabanı'nda tek bir veritabanına bağlanmayı öğreneceksiniz. Raporlama ve veri araştırması için Excel'e veri aktarın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: joseidz
ms.author: craigg
ms.reviewer: ''
manager: craigg
ms.date: 02/12/2019
ms.openlocfilehash: a6e0adc6b4abbb58504b6f56c8def72440ad370d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061407"
---
# <a name="connect-excel-to-a-single-database-in-azure-sql-database-and-create-a-report"></a>Excel Azure SQL veritabanı'nda tek bir veritabanına bağlanma ve rapor oluşturma

Azure SQL veritabanı'nda tek bir veritabanı Excel bağlanmak ve verileri içeri aktarın ve tablolar ve grafikler veritabanı değerlere göre oluşturun. Bu öğreticide, Excel ile bir veritabanı tablosu arasındaki bağlantıyı ayarlayacak, Excel'e ilişkin verilerin ve bağlantı bilgilerinin depolandığı dosyayı kaydedecek ve ardından veritabanı değerlerini kullanarak bir özet grafik oluşturacaksınız.

Başlamadan önce tek bir veritabanı gerekir. Yoksa, bkz. [tek veritabanı oluşturma](sql-database-single-database-get-started.md) ve [sunucu düzeyinde IP Güvenlik Duvarı oluşturma](sql-database-server-level-firewall-rule.md) örnek verilerle tek bir veritabanı birkaç dakika içinde çalışmaya başlamanızı sağlayacak.

Bu makalede, örnek verileri ilgili makaleden Excel'e içeri aktaracaksınız ancak kendi verilerinizle benzer adımları izleyebilirsiniz.

Ayrıca, bir Excel kopyanızın olması gerekir. Bu makalede [Microsoft Excel 2016](https://products.office.com/) kullanılmıştır.

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Excel'i SQL veritabanı ve yük veri bağlama

1. Excel'i SQL veritabanına bağlamak için Excel'i açarak yeni bir çalışma kitabı oluşturun veya var olan bir Excel çalışma kitabını açın.
2. Sayfanın üstündeki menü çubuğunda seçin **veri** sekmesinde **Veri Al**, Azure'ı seçin ve ardından **Azure SQL veritabanı'ndan**. 

   ![Veri kaynağı seçin: Excel, SQL veritabanı'na bağlanın.](./media/sql-database-connect-excel/excel_data_source.png)

   Veri Bağlantı Sihirbazı açılır.
3. **Veritabanı Sunucusuna Bağlan** iletişim kutusunda, bağlanmak istediğiniz SQL Database **Sunucu adını** şu biçimde girin: <*sunucuadı*> **.database.windows.net**. Örneğin, **msftestserver.database.windows.net**. İsteğe bağlı olarak, veritabanınızın adını girin. Seçin **Tamam** kimlik bilgilerini penceresini açın. 

   ![Veritabanı sunucusu iletişim kutusuna bağlanın](media/sql-database-connect-excel/server-name.png)

4. İçinde **SQL Server veritabanı** iletişim kutusunda **veritabanı** sol tarafında ve ardından girin, **kullanıcı adı** ve **parola** için Bağlanmak istediğiniz SQL veritabanı sunucusu. Seçin **Connect** açmak için **Gezgin**. 

   ![Sunucu adını ve oturum açma kimlik bilgilerini girme](./media/sql-database-connect-excel/connect-to-server.png)

   > [!TIP]
   > Ağ ortamınıza bağlı olarak, SQL Veritabanı sunucusunun istemci IP adresinizden gelen trafiğe izin vermemesi halinde bağlanamayabilirsiniz veya mevcut bağlantınız kesilebilir. [Azure portalına](https://portal.azure.com/) gidip SQL sunucuları seçeneğine tıklayın, sunucunuza tıklayın ve ardından ayarlar altında bulunan güvenlik duvarı seçeneğine tıklayıp istemci IP adresinizi ekleyin. Ayrıntılı bilgi için bkz. [Güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).

5. İçinde **Gezgin**seçin listeden çalışmak istediğiniz veritabanını seçin tabloları veya görünümleri ile çalışmak istediğiniz (seçtik **vGetAllCategories**) ve ardından **yük**Excel elektronik tablosuna veritabanınızdan verileri taşımak için.

    ![Bir veritabanı ve tablo seçin.](./media/sql-database-connect-excel/select-database-and-table.png)

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Excel'e veri aktarma ve özet grafik oluşturma

Bağlantı kurduktan sonra veri yükleme ile birkaç farklı seçeneğiniz vardır. Örneğin, aşağıdaki adımları SQL veritabanı'nda bulunan verileri temel alan bir Özet Grafik oluşturun. 

1. Önceki bölümde, ancak seçmek yerine bu kez, adımları **yük**seçin **yük** gelen **yük** açılır.
2. Ardından, bu verileri çalışma kitabınızı görüntüleme istediğiniz şekli seçin. Biz **PivotChart** seçeneğini belirledik. Ayrıca, **Yeni çalışma sayfası** oluşturmayı veya **Bu verileri Veri Modeline ekle** seçeneğini belirlemeyi de tercih edebilirsiniz. Veri Modelleri hakkında daha fazla bilgi için bkz. [Excel'de veri modeli oluşturma](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 

    ![Excel'de veri biçimini belirleme](./media/sql-database-connect-excel/import-data.png)

    Artık çalışma sayfasında boş bir özet grafiği ve grafik var.
3. **PivotTable Alanları** altında, görüntülemek istediğiniz alanların onay kutularını işaretleyin.

    ![Veritabanı raporunu yapılandırın.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Diğer Excel çalışma kitaplarını ve çalışma sayfalarını veritabanına bağlanmak istiyorsanız seçin **veri** sekmesine tıklayın ve **son kaynaklar** başlatmak için **son kaynaklar** iletişim kutusu. Buradan, listeden oluşturduğunuz bağlantıyı seçin ve ardından **açık**.
> ![Son kaynakları iletişim kutusu](media/sql-database-connect-excel/recent-connections.png)

## <a name="create-a-permanent-connection-using-odc-file"></a>.Odc dosyası kullanarak kalıcı bir bağlantı oluşturma

Bağlantı ayrıntıları kalıcı olarak kaydetmek için bir .odc dosyası oluşturabilir ve bu bağlantısı içinde seçilebilir bir seçenek **varolan bağlantılar** iletişim kutusu. 

1. Sayfanın üstündeki menü çubuğunda seçin **veri** sekmesine tıklayın ve ardından **varolan bağlantılar** başlatmak için **varolan bağlantılar** iletişim kutusu. 
   1. Seçin **daha fazlası için Gözat** açmak için **veri kaynağı Seç** iletişim kutusu.   
   2. Seçin **+NewSqlServerConnection.odc** dosya ve ardından **açın** açmak için **Veri Bağlantı Sihirbazı'nı**.

      ![Yeni bağlantı iletişim kutusu](media/sql-database-connect-excel/new-connection.png)

2. İçinde **Veri Bağlantı Sihirbazı**, sunucu adınız ve SQL veritabanı kimlik bilgilerinizi yazın. **İleri**’yi seçin. 
   1. Açılır listeden verilerinizi içeren veritabanını seçin. 
   2. Tablo veya Görünüm ilgilendiğiniz seçin. VGetAllCategories seçtik.
   3. **İleri**’yi seçin. 

      ![Veri Bağlantı Sihirbazı](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Dosyanızın konumunu seçin **dosya adı**ve **kolay ad** Veri Bağlantı Sihirbazı'nın sonraki ekranda. Bu olası istenmeyen erişim için verilerinizi getirebilir ancak parolayı dosyasına kaydetmek seçebilirsiniz. Seçin **son** ne zaman hazır. 

    ![Veri bağlantısı Kaydet](media/sql-database-connect-excel/save-data-connection.png)

4. Nasıl verilerinizi içeri aktarmak istediğinizi seçin. Bir PivotTable yapmak seçtik. Bağlantı özelliklerini seçerek değiştirebilirsiniz **özellikleri**. Seçin **Tamam** ne zaman hazır. Ardından parolayı dosyasıyla kaydetmek belirlemediyseniz, kimlik bilgilerinizi girmeniz istenir. 

    ![Verileri İçeri Aktarma](media/sql-database-connect-excel/import-data2.png)

5. Yeni bağlantınızı genişleterek kaydedildiğini doğrulayın **veri** sekme ve seçerek **varolan bağlantılar**. 

    ![Mevcut bağlantı](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* Gelişmiş sorgulama ve analiz için [SQL Server Management Studio ile SQL Database'e bağlanma](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* [Esnek havuzların](sql-database-elastic-pool.md) avantajları hakkında bilgi edinin.
* [Arka uçta SQL Database'e bağlanan bir web uygulaması oluşturma](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md) hakkında bilgi edinin.
