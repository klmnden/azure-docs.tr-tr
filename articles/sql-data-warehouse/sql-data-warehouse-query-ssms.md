---
title: "Azure SQL veri ambarı - SSMS bağlanma | Microsoft Docs"
description: "SQL Server Management Studio (SSMS) bağlanmak ve Azure SQL Data Warehouse sorgulamak için kullanın."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 207fb9fd861c66039fbde89681aed3df3a2f4021
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile SQL Data Warehouse'a bağlanma
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

SQL Server Management Studio (SSMS) bağlanmak ve Azure SQL Data Warehouse sorgulamak için kullanın. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Var olan bir SQL veri ambarı. Bir tane oluşturmak için bkz. [SQL Veri Ambarı oluşturma][Create a SQL Data Warehouse].
* SQL Server Management Studio (yüklü SSMS). [SSMS yükleme] [ Install SSMS] ücretsiz, zaten yoksa.
* Tam SQL server adı. Bunu bulmak için bkz. [SQL Veri Ambarı'na bağlanma][Connect to SQL Data Warehouse].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. SQL Data Warehouse'unuza bağlanma
1. SSMS açın.
2. Nesne Gezgini'ni açın. Bunu yapmak için seçin **dosya** > **bağlanmak Nesne Gezgini**.
   
    ![SQL Server Nesne Gezgini][1]
3. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma][2]
   
   * **Sunucu adı** Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik Doğrulaması**. **SQL Server Kimlik Doğrulaması**'nı veya **Active Directory Tümleşik Kimlik Doğrulaması**'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan**'a tıklayın.
4. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma][3]

## <a name="2-run-a-sample-query"></a>2. Örnek sorgu çalıştırma
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu**'yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu][4]
3. Şu TSQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Sorguyu çalıştırın. Bunu yapmak için tıklatın `Execute` veya şu kısayolu kullanın: `F5`.
   
    ![Sorgu çalıştırma][5]
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları][6]

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yapabildiğinize göre [PowerBI ile verileri görselleştirmeyi][visualizing the data with PowerBI] deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [SQL Veri Ambarı’nda kimlik doğrulama][Authenticate to SQL Data Warehouse].

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
