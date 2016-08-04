<properties
    pageTitle="SQL Database öğreticisi: SQL veritabanı oluşturma | Microsoft Azure"
    description="SQL Database mantıksal sunucusu, sunucu güvenlik duvarı kuralı, SQL veritabanı ve örnek veri ayarlamanın ve istemci araçlarına bağlanmanın yanı sıra kullanıcıları, veritabanını ve güvenlik duvarı kuralını yapılandırmayı öğrenin."
    keywords="sql database tutorial, create a sql database"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/14/2016"
    ms.author="carlrab"/>

# SQL Database öğreticisi: Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma

**Tek veritabanı**

> [AZURE.SELECTOR]
- [Azure portalı](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Bu öğreticide Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

- SQL veritabanlarını barındırmak için SQL Database mantıksal sunucusu oluşturma
- Veri olmadan, örnek verilerle veya SQL veritabanı yedeklemesine ait verilerle bir SQL veritabanı oluşturma.
- Tek bir IP adresi veya bir IP adresi aralığı için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma.

Aynı görevleri [C#](sql-database-get-started-csharp.md) veya [PowerShell](sql-database-get-started-powershell.md)'i kullanarak gerçekleştirmek için bu bağlantıları kullanın.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Sonraki adımlar
Bu SQL Database öğreticisini tamamladığınıza ve bazı örnek verilerle bir veritabanı oluşturduğunuza göre sık kullandığınız araçları kullanarak araştırmaya hazırsınız.

- Transact-SQL ve SQL Server Management Studio'yu daha önce kullandıysanız [SSMS ile bir SQL veritabanına bağlanma ve veritabanını sorgulama](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.

- Excel kullanmayı biliyorsanız [Excel ile SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.

- Kodlamaya başlamak için hazırsanız [SQL Database ve SQL Server için Bağlantı Kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.

- Şirket içi SQL Server veritabanlarınızı Azure'a taşımak istiyorsanız daha fazla bilgi edinmek için bkz. [Bir veritabanını Azure SQL Database'e aktarma](sql-database-cloud-migrate.md).


## Daha Fazla Bilgi Edinin

[SQL Database nedir?](sql-database-technical-overview.md)




<!----HONumber=Jun16_HO2-->


