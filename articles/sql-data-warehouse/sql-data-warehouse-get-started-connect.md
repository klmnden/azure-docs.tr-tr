<properties
   pageTitle="Visual Studio ile SQL Data Warehouse'a bağlanma | Microsoft Azure"
   description="SQL Data Warehouse'a bağlanıp birkaç sorgu çalıştırarak başlayın."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/13/2016"
   ms.author="sonyama;barbkess"/>

# Visual Studio ile SQL Data Warehouse'a bağlanma

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)

Adım adım ilerleyeceğimiz bu bölümde Visual Studio'da bulunan Microsoft SQL Server Veri Araçları (SSDT) uzantısını kullanarak yalnızca birkaç dakika içinde bir Azure SQL Data Warehouse'a nasıl bağlanacağınız gösterilmektedir. Bağlantı kurduktan sonra basit bir sorgu çalıştıracaksınız.

## Önkoşullar

+ SQL Data Warehouse'daki AdventureWorksDW örnek verileri. Oluşturmak için bkz. [SQL Data Warehouse oluşturma][].
+ Visual Studio için SQL Server Veri Araçları Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio'yu ve SSDT'yi yükleme][].

## 1. Adım: Tam Azure SQL sunucu adını bulma

SQL Data Warehouse veritabanınız bir Azure SQL Server ile ilişkilidir. Veritabanınıza bağlanmak için sunucunun tam adı (**sunucuadı**.database.windows.net*) gerekir.

Tam sunucu adını bulmak için:

1. [Azure portalına][] gidin.
2. **SQL veritabanları** seçeneğine tıklayın ve ardından bağlanmak istediğiniz veritabanına tıklayın. Bu örnekte AdventureWorksDW örnek veritabanı kullanılmıştır.
3. Tam sunucu adını bulun.

    ![Tam sunucu adı][1]

## 2. Adım: SQL Data Warehouse'unuza bağlanma

1. Visual Studio 2013 veya Visual Studio 2015'i açın.
2. SQL Server Nesne Gezgini'ni açın. Bunu gerçekleştirmek için **Görünüm** > **SQL Server Nesne Gezgini**'ni seçin.

    ![SQL Server Nesne Gezgini][2]

3. **SQL Server ekle** simgesine tıklayın.

    ![SQL Server ekleme][3]

4. Sunucuya Bağlan penceresindeki alanları doldurun.

    ![Sunucuya bağlanma][4]

    - **Sunucu adı** Önceden tanımlanmış olan **sunucu adını** girin.
    - **Kimlik Doğrulaması**. **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
    - **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
    - **Bağlan**'a tıklayın.

5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.

    ![AdventureWorksDW'yi araştırma][5]

## 3. Adım: Örnek sorgu çalıştırma

Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.

2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.

    ![Yeni sorgu][6]

3. Şu TSQL sorgusunu sorgu penceresine kopyalayın:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Sorguyu çalıştırın. Bunu gerçekleştirmek için yeşil ok simgesine tıklayın veya şu kısayolu kullanın: `CTRL`+`SHIFT`+`E`.

    ![Sorgu çalıştırma][7]

5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.

    ![Sorgu sonuçları][8]

## Sonraki adımlar

Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile verileri görselleştirmeyi][] deneyin.

Ortamınızı Windows kimlik doğrulaması için yapılandırmak üzere bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Database'e veya SQL Data Warehouse'a Bağlanma][].

<!--Arcticles-->
[SQL Data Warehouse oluşturma]: sql-data-warehouse-get-started-provision.md
[Visual Studio'yu ve SSDT'yi yükleme]: sql-data-warehouse-install-visual-studio.md
[Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Database'e veya SQL Data Warehouse'a Bağlanma]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[PowerBI ile verileri görselleştirmeyi]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portalına]: https://portal.azure.com

<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/add-server.png
[4]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[5]: ./media/sql-data-warehouse-get-started-connect/explore-sample.png
[6]: ./media/sql-data-warehouse-get-started-connect/new-query2.png
[7]: ./media/sql-data-warehouse-get-started-connect/run-query.png
[8]: ./media/sql-data-warehouse-get-started-connect/query-results.png



<!----HONumber=Jun16_HO2-->


