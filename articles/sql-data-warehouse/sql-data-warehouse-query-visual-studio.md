---
title: Azure SQL Data Warehouse'u sorgulama (Visual Studio) | Microsoft Docs
description: Visual Studio ile SQL Data Warehouse'u sorgulayın.
services: sql-data-warehouse
documentationcenter: NA
author: sonyam
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/16/2016
ms.author: sonyama;barbkess

---
# SQL Data Warehouse'u sorgulama (Visual Studio)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> 
> 

Azure SQL Data Warehouse’u yalnızca birkaç dakika içinde sorgulamak için Visual Studio kullanın. Bu yöntem Visual Studio’daki SQL Server Veri Araçları (SSDT) uzantısını kullanır. 

## Ön koşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Var olan bir SQL veri ambarı. Bir tane oluşturmak için bkz. [SQL Data Warehouse oluşturma][SQL Data Warehouse oluşturma].
* Visual Studio için SSDT. Visual Studio varsa büyük olasılıkla buna da sahip olursunuz. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio’yu ve SSDT’yi yükleme][Visual Studio’yu ve SSDT’yi yükleme].
* Tam SQL server adı. Bunu bulmak için bkz. [SQL Data Warehouse'a bağlanma][SQL Data Warehouse'a bağlanma].

## 1. SQL Data Warehouse'unuza bağlanma
1. Visual Studio 2013 veya Visual Studio 2015'i açın.
2. SQL Server Nesne Gezgini'ni açın. Bunu gerçekleştirmek için **Görünüm** > **SQL Server Nesne Gezgini**'ni seçin.
   
    ![SQL Server Nesne Gezgini][1]
3. **SQL Server ekle** simgesine tıklayın.
   
    ![SQL Server ekleme][2]
4. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma][3]
   
   * **Sunucu adı** Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik Doğrulaması**. **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma][4]

## 2. Örnek sorgu çalıştırma
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu][5]
3. Şu TSQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu gerçekleştirmek için yeşil ok simgesine tıklayın veya şu kısayolu kullanın: `CTRL`+`SHIFT`+`E`.
   
    ![Sorgu çalıştırma][6]
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları][7]

## Sonraki adımlar
Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile verileri görselleştirmeyi][PowerBI ile verileri görselleştirmeyi] deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL Data Warehouse’da kimlik doğrulama][SQL Data Warehouse’da kimlik doğrulama].

<!--Arcticles-->
[SQL Data Warehouse'a bağlanma]: sql-data-warehouse-connect-overview.md
[SQL Data Warehouse oluşturma]: sql-data-warehouse-get-started-provision.md
[Visual Studio’yu ve SSDT’yi yükleme]: sql-data-warehouse-install-visual-studio.md
[SQL Data Warehouse’da kimlik doğrulama]: sql-data-warehouse-authentication.md
[PowerBI ile verileri görselleştirmeyi]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portalına]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png



<!--HONumber=Aug16_HO1-->


