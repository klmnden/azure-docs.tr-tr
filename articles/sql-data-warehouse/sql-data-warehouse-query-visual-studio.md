---
title: "Azure SQL Veri Ambarı&quot;na Bağlanma - VSTS | Microsoft Belgeleri"
description: "Visual Studio ile SQL Data Warehouse&quot;u sorgulayın."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 77474214c6fafe7f591030d30f6a46c66fbc5c09
ms.openlocfilehash: 71a56d0e99308d3f7f514283792a2155a05a7172


---
# <a name="connect-to-sql-data-warehouse-with-visual-studio-and-ssdt"></a>Visual Studio ve SSDT ile SQL Veri Ambarı'na bağlanma
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Azure SQL Data Warehouse’u yalnızca birkaç dakika içinde sorgulamak için Visual Studio kullanın. Bu yöntem Visual Studio’daki SQL Server Veri Araçları (SSDT) uzantısını kullanır. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Var olan bir SQL veri ambarı. SQL veri ambarı oluşturmak için bkz. [SQL Veri Ambarı oluşturma][SQL Veri Ambarı oluşturma].
* Visual Studio için SSDT. Visual Studio varsa büyük olasılıkla buna da sahip olursunuz. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio ve SSDT’yi yükleme][Visual Studio ve SSDT’yi yükleme].
* Tam SQL server adı. Adı bulmak için bkz. [SQL Veri Ambarı’na bağlanma][SQL Veri Ambarı’na bağlanma].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. SQL Data Warehouse'unuza bağlanma
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

## <a name="2-run-a-sample-query"></a>2. Örnek sorgu çalıştırma
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

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile verileri görselleştirmeyi][PowerBI ile verileri görselleştirmeyi] deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL Veri Ambarı’nda kimlik doğrulama][SQL Veri Ambarı’nda kimlik doğrulama].

<!--Arcticles-->
[SQL Veri Ambarı’na bağlanma]: sql-data-warehouse-connect-overview.md
[SQL Veri Ambarı oluşturma]: sql-data-warehouse-get-started-provision.md
[Visual Studio ve SSDT’yi yükleme]: sql-data-warehouse-install-visual-studio.md
[SQL Veri Ambarı’nda kimlik doğrulama]: sql-data-warehouse-authentication.md
[PowerBI ile verileri görselleştirmeyi]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png



<!--HONumber=Nov16_HO3-->


