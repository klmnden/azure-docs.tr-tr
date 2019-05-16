---
title: Azure SQL Veritabanı Bağlanma ve Sorgulama hızlı başlangıçları | Microsoft Docs
description: Azure SQL veritabanına nasıl bağlanacağınızı ve bu veritabanını nasıl sorgulayacağınızı gösteren Azure SQL Veritabanı hızlı başlangıçları.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: a8513344c35c14ebf06f3693da618ed20047d07b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65752692"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Hızlı Başlangıçlar: Azure SQL veritabanı bağlanma ve sorgulama

Aşağıdaki tabloda, Azure SQL veritabanına nasıl bağlanılacağını ve Azure SQL veritabanının nasıl sorgulanacağını gösteren Azure örneklerinin bağlantıları yer almaktadır. Ayrıca Taşıma Düzeyi Güvenliği için bazı öneriler sunar.

## <a name="quickstarts"></a>Hızlı girişler

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|Bu hızlı başlangıçta SSMS kullanarak bir Azure SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır.|
|[Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Bu hızlı başlangıçta Azure Data Studio'yu kullanarak bir Azure SQL veritabanına bağlanma, sonra Transact-SQL (T-SQL) deyimlerini kullanarak Azure Data Studio öğreticilerinde kullanılan TutorialDB'yi oluşturma gösterilmektedir.|
|[Azure portal](sql-database-connect-query-portal.md)|Bu hızlı başlangıçta Sorgu düzenleyicisini kullanarak bir SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|Bu hızlı başlangıçta Visual Studio Code’u kullanarak bir Azure SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır.|
|[Visual Studio ile .NET](sql-database-connect-query-dotnet-visual-studio.md)|Bu hızlı başlangıçta, .NET framework kullanarak Visual Studio ile Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturma işleminin nasıl yapılacağı açıklanır.|
|[.NET core](sql-database-connect-query-dotnet-core.md)|Bu hızlı başlangıçta, Windows/Linux/macOS’ta .NET Core kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturma işleminin nasıl yapılacağı açıklanır.|
|[Go](sql-database-connect-query-go.md)|Bu hızlı başlangıçta Go kullanarak Azure SQL veritabanına nasıl bağlanacağınız gösterilmiştir. Verileri sorgulamak ve değiştirmek için Transact-SQL bildirimleri de gösterilir.|
|[Java](sql-database-connect-query-java.md)|Bu hızlı başlangıçta Java kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir.|
|[Node.js](sql-database-connect-query-nodejs.md)|Bu hızlı başlangıçta, Node.js kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işleminin nasıl yapılacağı açıklanır.|
|[PHP](sql-database-connect-query-php.md)|Bu hızlı başlangıçta, PHP kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işleminin nasıl yapılacağı açıklanır.|
|[Python](sql-database-connect-query-python.md)|Bu hızlı başlangıçta Python kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir. |
|[Ruby](sql-database-connect-query-ruby.md)|Bu hızlı başlangıçta, Ruby kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işleminin nasıl yapılacağı açıklanır.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>SQL Veritabanı bağlanabilirliği için TLS konuları
Taşıma Katmanı Güvenliği (TLS), Microsoft’un Azure SQL Veritabanına bağlanmak için tedarik ettiği veya desteklediği tüm sürücüler tarafından kullanılır. Özel yapılandırma gerekli değildir. SQL Server veya Azure SQL Veritabanına yönelik tüm bağlantılar için tüm uygulamaların aşağıdaki yapılandırmalar veya eşdeğerleriyle ayarlanmasını öneririz:

 - **Encrypt = On**
 - **TrustServerCertificate = Off**

Bazı sistemler bu yapılandırma anahtar sözcükleri için farklı ancak eşdeğer anahtar sözcükler kullanmaktadır. Bu yapılandırmalar istemci sürücüsünün sunucudan alınan TLS sertifikası kimliğini doğrulamasını sağlamaktadır.

Ayrıca Ödeme Kartı Endüstrisi - Veri Güvenliği Standardı’na (PCI-DSS) uymanız gerekiyorsa istemcide TLS 1.1 ve 1.0’ı devre dışı bırakmanızı öneririz.

Microsoft olmayan sürücüler varsayılan olarak TLS’yi kullanmayabilir. Bu, Azure SQL Veritabanına bağlanırken bir faktör olabilir. Ekli sürücüleri olan uygulamalar bu bağlantı ayarlarını denetlemenize izin vermeyebilir. Hassas verilerle etkileşimde bulunan sistemlerde kullanmadan önce bu tarz sürücülerin ve uygulamaların güvenliğini incelemenizi öneririz.

## <a name="libraries"></a>Kitaplıklar

Azure SQL veritabanı'na bağlanmak için çeşitli kitaplıkları ve çerçeveleri kullanabilirsiniz. Kullanıma sunduğumuz [alma Eğitmenleri](https://aka.ms/sqldev) C#, Java, Node.js, PHP ve Python gibi programlama ile hızlı bir şekilde kullanmaya başlamak için. MacOS üzerinde SQL Server Linux veya Windows veya Docker kullanarak ardından bir uygulama oluşturun.

Aşağıdaki tabloda bağlantı kitaplıkları listeler veya *sürücüleri* istemci uygulamalarını değişik bağlanmak ve şirket içinde çalışan SQL Server kullanmak için dilleri içinde veya bulutta kullanabilirsiniz. Linux, Windows veya Docker kullanın ve Azure SQL veritabanı ve Azure SQL veri ambarı bağlanmak için bunları kullanın. 

| Dil | Platform | Ek kaynaklar | Karşıdan Yükle | başlarken |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [SQL Server için Microsoft ADO.NET](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [İndir](https://www.microsoft.com/net/download/) | [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [SQL Server için Microsoft JDBC sürücüsü](https://msdn.microsoft.com/library/mt484311.aspx) | [İndir](https://go.microsoft.com/fwlink/?linkid=852460) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [SQL Server için PHP SQL sürücüsü](https://docs.microsoft.com/sql/connect/php/microsoft-php-driver-for-sql-server) | [İndir](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server) | [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/)
| Node.js | Windows, Linux, macOS | [SQL Server için node.js sürücüsü](https://msdn.microsoft.com/library/mt652093.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt652094.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [SQL Python sürücüsü](https://msdn.microsoft.com/library/mt652092.aspx) | Seçenekler'i yükleyin: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [SQL Server için Ruby sürücüsü](https://msdn.microsoft.com/library/mt691981.aspx) | [Yükleme](https://msdn.microsoft.com/library/mt711041.aspx) | [Kullanmaya başlama](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [SQL Server için Microsoft ODBC sürücüsü](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [İndir](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

Aşağıdaki tabloda, bulutta nesne ilişkisel eşleme (ORM) çerçeveleri ve istemci uygulamaları veya şirket içinde çalışan SQL Server ile kullanabileceğiniz web çerçeveleri örneklerini listeler. Linux, Windows veya Docker çerçeveleri kullanın ve SQL veritabanı ve SQL veri ambarına bağlanmak için bunları kullanın. 

| Dil | Platform | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[ORM hazırda bekleme](https://hibernate.org/orm)|
| PHP | Windows, Linux, macOS | [Laravel (Eloquent)](https://laravel.com/docs/eloquent)<br>[Doctrine](https://www.doctrine-project.org/projects/orm.html) |
| Node.js | Windows, Linux, macOS | [ORM sequelize](https://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |
||||

## <a name="next-steps"></a>Sonraki adımlar

- Bağlanabilirlik mimarisi bilgileri için bkz. [Azure SQL Veritabanı Bağlanabilirlik Mimarisi](sql-database-connectivity-architecture.md)
- Bulma [SQL Server sürücüleri](https://msdn.microsoft.com/library/mt654049.aspx) istemci uygulamalarından bağlanmak için kullanılabilir
- SQL veritabanı'na bağlanma:
  - [.NET (C#) kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-dotnet.md) 
  - [PHP kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-php.md) 
  - [Node.js kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-nodejs.md) 
  - [Java kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-java.md) 
  - [Python kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-python.md)
  - [Ruby kullanarak SQL Veritabanı’na bağlanma](sql-database-connect-query-ruby.md)
- Yeniden deneme mantığı kod örnekleri:
  - [Dayanıklı ADO.NET ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
  - [Dayanıklı PHP ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-php-p42h]

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php
