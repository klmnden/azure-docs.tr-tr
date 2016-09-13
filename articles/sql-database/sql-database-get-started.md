<properties
    pageTitle="SQL Database öğreticisi: SQL veritabanı oluşturma | Microsoft Azure"
    description="SQL Veritabanı mantıksal sunucusu, sunucu güvenlik duvarı kuralı, SQL veritabanı ve örnek veriler oluşturma hakkında bilgi edinin. Ayrıca, istemci araçlarına bağlanma, kullanıcıları yapılandırma ve bir veritabanı güvenlik duvarı kuralı oluşturma hakkında bilgi alın."
    keywords="sql veritabanı öğreticisi, sql veritabanı oluşturma"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="carlrab"/>


# SQL Veritabanı öğreticisi: Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma

**Tek veritabanı**

> [AZURE.SELECTOR]
- [Azure portalı](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

- SQL veritabanlarını barındırmak için bir Azure SQL Veritabanı mantıksal sunucusu oluşturun.
- Veri olmadan, örnek verilerle veya SQL veritabanı yedeklemesine ait verilerle bir SQL veritabanı oluşturun.
- Tek bir IP adresi veya bir IP adresi aralığı için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma.

Aynı görevleri [C#](sql-database-get-started-csharp.md) veya [PowerShell](sql-database-get-started-powershell.md) kullanarak gerçekleştirebilirsiniz.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Sonraki adımlar
Bu SQL Veritabanı öğreticisini tamamladığınıza ve bazı örnek verilerle bir veritabanı oluşturduğunuza göre sık kullandığınız araçları kullanarak araştırmaya hazırsınız.

- Transact-SQL ve SQL Server Management Studio'yu (SSMS) daha önce kullandıysanız [SSMS ile bir SQL veritabanına bağlanma ve veritabanını sorgulama](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.

- Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.

- Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.

- Şirket içi SQL Server veritabanlarınızı Azure'a taşımak istiyorsanız daha fazla bilgi edinmek için bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).

- BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP kullanarak bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).


## Ek kaynaklar

[SQL Database nedir?](sql-database-technical-overview.md)



<!--HONumber=sep16_HO1-->


