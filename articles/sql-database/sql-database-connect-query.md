---
title: Azure SQL Veritabanı Bağlanma ve Sorgulama hızlı başlangıçları | Microsoft Docs
description: Azure SQL veritabanına nasıl bağlanacağınızı ve bu veritabanını nasıl sorgulayacağınızı gösteren Azure SQL Veritabanı hızlı başlangıçları.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 613b4cf2b08269259a4608a6960b815777cd0ae9
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608043"
---
# <a name="quickstarts-azure-sql-database-connect-and-query"></a>Hızlı Başlangıçlar: Azure SQL veritabanına bağlanma ve sorgulama

Aşağıdaki tabloda, Azure SQL veritabanına nasıl bağlanılacağını ve Azure SQL veritabanının nasıl sorgulanacağını gösteren Azure örneklerinin bağlantıları yer almaktadır. Ayrıca Taşıma Düzeyi Güvenliği için bazı öneriler sunar.

## <a name="quickstarts"></a>Hızlı Başlangıçlar

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

## <a name="next-steps"></a>Sonraki adımlar

Bağlanabilirlik mimarisi bilgileri için bkz. [Azure SQL Veritabanı Bağlanabilirlik Mimarisi](sql-database-connectivity-architecture.md)