---
title: Azure SQL veri ambarı için kimlik doğrulaması | Microsoft Docs
description: Azure SQL Data Warehouse için Azure Active Directory (AAD) veya SQL Server kimlik doğrulaması kullanarak kimlik doğrulaması öğrenin.
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 173bc797cb6436decddb68aaf1599ea7a6dd597e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="authenticate-to-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için kimlik doğrulaması
Azure SQL Data Warehouse için Azure Active Directory (AAD) veya SQL Server kimlik doğrulaması kullanarak kimlik doğrulaması öğrenin.

SQL veri ambarı'na bağlanmak için kimlik doğrulama amacıyla güvenlik kimlik bilgilerini geçmesi gerekir. Bağlantı kurulduktan sonra belirli bağlantı ayarları, sorgu oturumu bir parçası olarak yapılandırılır.  

Güvenlik ve veri ambarınız için bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL kimlik doğrulaması
SQL veri ambarı'na bağlanmak için aşağıdaki bilgileri sağlamanız gerekir:

* Tam servername
* SQL kimlik doğrulaması belirtin
* Kullanıcı adı
* Parola
* Varsayılan veritabanı (isteğe bağlı)

Varsayılan olarak, bağlantınızı bağlandığı *ana* veritabanı ile değil, kullanıcı veritabanı. Kullanıcı veritabanına bağlanmak için ikisinden birini seçebilirsiniz:

* Varsayılan veritabanı sunucunuz SQL Server Nesne Gezgini SSDT, SSMS, veya uygulama bağlantı dizenizi kaydedilirken belirtin. Örneğin, bir ODBC bağlantı için InitialCatalog parametresini ekleyin.
* Kullanıcı veritabanı oturumu içinde SSDT oluşturmadan önce vurgulayın.

> [!NOTE]
> Transact-SQL deyimini **kullanım Veritabanım;** bir bağlantı için veritabanını değiştirmek için desteklenmiyor. SSDT ile SQL Data Warehouse'a bağlanma yönergeler için bkz [sorgu Visual Studio ile] [ Query with Visual Studio] makalesi.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) kimlik doğrulaması
[Azure Active Directory] [ What is Azure Active Directory] kimlik doğrulama mekanizmasıdır Microsoft Azure SQL Data Warehouse için Azure Active Directory (Azure AD) kullanarak kimlikleri bağlayan bir. Azure Active Directory kimlik doğrulaması ile veritabanı kullanıcıları kimlikleri ve diğer Microsoft Hizmetleri tek bir merkezi konumda merkezi olarak yönetebilir. Merkezi kimlik yönetimi, SQL Data Warehouse kullanıcıları yönetmek için tek bir yer sağlar ve izin Yönetimi basitleştirir. 

### <a name="benefits"></a>Avantajlar
Azure Active Directory yararlar şunlardır:

* SQL Server kimlik doğrulaması için bir alternatif sunar.
* Yardımcı veritabanı sunucuları arasında kullanıcı kimlikleri artışı durdurun.
* Tek bir yerde parola döndürme sağlar
* Dış (AAD) gruplarını kullanarak veritabanı izinleri yönetin.
* Depolama parolalar, tümleşik Windows kimlik doğrulaması ve Azure Active Directory tarafından desteklenen kimlik doğrulama başka biçimlerde etkinleştirerek ortadan kaldırır.
* Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları bulunan kullanır.
* SQL veri ambarı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* Active Directory Evrensel kimlik doğrulaması aracılığıyla çok faktörlü kimlik doğrulaması için SQL Server Management Studio destekler. Çok faktörlü kimlik doğrulaması açıklaması için bkz: [SSMS desteklemek için SQL Database ve SQL Data Warehouse ile Azure AD MFA](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory hala yeni ve bazı sınırlamalar vardır. Azure Active Directory ortamınız için uygun olmanın olduğundan emin olmak için bkz: [Azure AD özelliklerini ve sınırlamalarını][Azure AD features and limitations], özellikle ek hususlar.
> 
> 

### <a name="configuration-steps"></a>Yapılandırma adımları
Azure Active Directory kimlik doğrulamasını yapılandırmak için aşağıdaki adımları izleyin.

1. Oluşturma ve Azure Active Directory doldurma
2. İsteğe bağlı: İlişkilendir ya da şu anda Azure aboneliğinizle ilişkili olan active directory değiştirme
3. Azure SQL Data Warehouse için Azure Active Directory yönetici oluşturun.
4. İstemci bilgisayarları yapılandırın
5. Azure AD kimlikleri eşlenen veritabanınızdaki kapsanan veritabanı kullanıcıları oluşturun
6. Azure AD kimlik kullanarak, veri ambarına bağlama

Şu anda Azure Active Directory Kullanıcıları SSDT nesne Gezgini'nde gösterilmez. Geçici bir çözüm olarak, kullanıcılar görüntülemek [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>Ayrıntılarını bulun
* Adımları yapılandırmak ve Azure Active Directory kimlik doğrulaması kullanmak için Azure SQL Database ve Azure SQL Data Warehouse için neredeyse aynıdır. Konusunda ayrıntılı adımları [SQL Database'e veya SQL veri ambarı tarafından kullanarak Azure Active Directory kimlik doğrulaması için bağlanma](../sql-database/sql-database-aad-authentication.md).
* Özel veritabanı rolleri oluşturun ve kullanıcıları rollere ekleyin. Ardından rollere ayrıntılı izinleri verin. Daha fazla bilgi için bkz: [veritabanı altyapısı izinleri ile çalışmaya başlama](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio ve diğer uygulamalarla veri ambarınız sorgulama başlatmak için bkz: [sorgu Visual Studio ile][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
