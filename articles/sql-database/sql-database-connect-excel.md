---
title: "Excel'i SQL Veritabanı'na bağlama | Microsoft Belgeleri"
description: "Microsoft Excel'i bulut üzerinde Azure SQL veritabanına nasıl bağlayacağınızı öğrenin. Raporlama ve veri araştırması için Excel'e veri aktarın."
services: sql-database
keywords: "excel’i sql’e bağlama, verileri excel’e aktarma"
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 34ff5c479cfcf1e861a82205eef93dfee01cb4a2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Excel'i bir Azure SQL veritabanına bağlama ve rapor oluşturma

SQL Database'i bulutta Excel bağlanmak ve veri içeri aktarın ve tablolar ve grafikler veritabanındaki değerlere göre oluşturun. Bu öğreticide, Excel ile bir veritabanı tablosu arasındaki bağlantıyı ayarlayacak, Excel'e ilişkin verilerin ve bağlantı bilgilerinin depolandığı dosyayı kaydedecek ve ardından veritabanı değerlerini kullanarak bir özet grafik oluşturacaksınız.

Başlayabilmek için Azure'da bir SQL veritabanınızın olması gerekir. Henüz bir veritabanınız yoksa birkaç dakika içinde çalışır durumda olan, örnek veriler içeren bir veritabanı edinmek üzere [İlk SQL veritabanınızı oluşturma](sql-database-get-started-portal.md) adlı makaleye göz atın. Bu makalede, ilgili makaleden Excel'e örnek veriler aktaracaksınız ancak kendi verilerinizle benzer adımları izleyebilirsiniz.

Ayrıca, bir Excel kopyanızın olması gerekir. Bu makalede [Microsoft Excel 2016](https://products.office.com/) kullanılmıştır.

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Excel'i bir SQL veritabanına bağlama ve ODC dosyası oluşturma
1. Excel'i SQL veritabanına bağlamak için Excel'i açarak yeni bir çalışma kitabı oluşturun veya var olan bir Excel çalışma kitabını açın.
2. Sayfanın üstündeki menü çubuğunda **Veri** seçeneğine, **Diğer Kaynaklardan** seçeneğine ve ardından **SQL Server'dan** seçeneğine tıklayın.
   
   ![Veri kaynağı seçin: Excel'i SQL veritabanına bağlayın.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Veri Bağlantı Sihirbazı açılır.
3. **Veritabanı Sunucusuna Bağlan** iletişim kutusunda, bağlanmak istediğiniz SQL Database **Sunucu adını** şu biçimde girin: <*sunucuadı*>**.database.windows.net**. Örneğin, **adworkserver.database.windows.net**.
4. **Oturum açma kimlik bilgileri** bölümünde **Aşağıdaki Kullanıcı Adını ve Parolayı kullan**'a tıklayıp SQL Database sunucusunu oluşturduğunuz sırada belirlediğiniz **Kullanıcı Adı** ve **Parolayı** girerek **İleri**'ye tıklayın.
   
   ![Sunucu adını ve oturum açma kimlik bilgilerini girme](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > Ağ ortamınıza bağlı olarak, SQL Database sunucusunun istemci IP adresinizden gelen trafiğe izin vermemesi halinde bağlanamayabilirsiniz veya mevcut bağlantınız kesilebilir. [Azure portalına](https://portal.azure.com/) gidip SQL sunucuları seçeneğine tıklayın, sunucunuza tıklayın ve ardından ayarlar altında bulunan güvenlik duvarı seçeneğine tıklayıp istemci IP adresinizi ekleyin. Ayrıntılı bilgi için bkz. [Güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).
   > 
   > 
5. **Veritabanı ve Tablo Seç** iletişim kutusunda, kullanmak istediğiniz veritabanını listeden seçin, ardından kullanmak istediğiniz tablolara veya görünümlere tıklayın (biz **vGetAllCategories** seçeneğini belirledik) ve ardından **İleri**'ye tıklayın.
   
    ![Bir veritabanı ve tablo seçin.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    **Veri Bağlantısı Dosyasını Kaydet ve Tamamla** iletişim kutusu açılır. Burada, Excel'in kullandığı Office veritabanı bağlantısı (*.odc) dosyasına ilişkin bilgiler sağlarsınız. Varsayılanları bırakabilir veya kendi seçimlerinizi özelleştirebilirsiniz.
6. Varsayılanları bırakabilirsiniz ancak mutlaka bir **Dosya Adı** belirtin. **Açıklama**, **Kolay Ad** ve **Arama Anahtar Sözcükleri** neye bağlanıldığının hatırlanması ve bağlantının bulunması konusunda size ve diğer kullanıcılara yardımcı olur. ODC dosyasında depolanan bağlantı bilgilerinin her bağlandığınızda güncelleştirilmesini istiyorsanız **Verileri yenilemek için her zaman bu dosyayı kullan**'a tıkladıktan sonra **Son**'a tıklayın.
   
    ![ODC dosyasını kaydetme](./media/sql-database-connect-excel/save-odc-file.png)
   
    **VeriIeri içeri aktar** iletişim kutusu görünür.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Excel'e veri aktarma ve özet grafik oluşturma
Artık bağlantıyı kurup verileri ve bağlantı bilgilerini kullanarak dosyayı oluşturduğunuza göre verileri içeri aktarmaya hazırsınız.

1. **Verileri İçeri Aktar** iletişim kutusunda, verilerinizi çalışma sayfasında nasıl sunmak istediğinize ilişkin seçeneğe tıklayın ve ardından **TAMAM**'a tıklayın. Biz **PivotChart** seçeneğini belirledik. Ayrıca, **Yeni çalışma sayfası** oluşturmayı veya **Bu verileri Veri Modeline ekle** seçeneğini belirlemeyi de tercih edebilirsiniz. Veri Modelleri hakkında daha fazla bilgi için bkz. [Excel'de veri modeli oluşturma](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Daha önceki adımda oluşturduğunuz ODC dosyası hakkındaki bilgilere ulaşmak ve verileri yenileme seçeneklerini belirlemek için Click **Özellikler**'e tıklayın.
   
    ![Excel'de veri biçimini belirleme](./media/sql-database-connect-excel/import-data.png)
   
    Artık çalışma sayfasında boş bir özet grafiği ve grafik var.
2. **PivotTable Alanları** altında, görüntülemek istediğiniz alanların onay kutularını işaretleyin.
   
    ![Veritabanı raporunu yapılandırın.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Diğer Excel çalışma kitaplarını ve çalışma sayfalarını veritabanına bağlamak istiyorsanız **Veri**'ye tıklayın, **Bağlantılar**'a tıklayın ve ardından **Ekle**'ye tıklayıp oluşturduğunuz bağlantıyı listeden seçerek **Aç**'a tıklayın.
> ![Başka bir çalışma kitabından bağlantı açma](./media/sql-database-connect-excel/open-from-another-workbook.png)
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Gelişmiş sorgulama ve analiz için [SQL Server Management Studio ile SQL Database'e bağlanma](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* [Esnek havuzların](sql-database-elastic-pool.md) avantajları hakkında bilgi edinin.
* [Arka uçta SQL Database'e bağlanan bir web uygulaması oluşturma](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md) hakkında bilgi edinin.

