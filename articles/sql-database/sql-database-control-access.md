---
title: "Azure SQL Veritabanına erişim verme | Microsoft Docs"
description: "Microsoft Azure SQL Veritabanına erişim verme hakkında bilgi edinin."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/18/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 4f75c60a0dffec0d51d1f5ba448bd76235ac38bf
ms.openlocfilehash: 7b0a1ac97641419234e128cc30b643f9e655ec6b

---
# <a name="azure-sql-database-access-control"></a>Azure SQL Veritabanı erişim denetimi
SQL Veritabanı güvenliği sağlamak için erişimi IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları, kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama sistemleri ve kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle denetler. 

> [!IMPORTANT]
> SQL Veritabanı güvenlik özelliklerine genel bakış için bkz. [SQL güvenliğine genel bakış](sql-database-security-overview.md). SQL Server kimlik doğrulamasını kullanan bir öğretici için bkz. [SQL Veritabanı Öğreticisi: SQL Server kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-sql-authentication-get-started.md). Azure Active Directory kimlik doğrulamasını kullanan bir öğretici için bkz. [SQL Veritabanı Öğreticisi: AAD kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-aad-authentication-get-started.md).

## <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları
Microsoft Azure SQL Veritabanı, Azure ile diğer İnternet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sunar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir. Daha fazla bilgi için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](sql-database-firewall-configure.md)

Azure SQL Veritabanı hizmeti yalnızca 1433 numaralı TCP bağlantı noktasından kullanılabilir. Bilgisayarınızdan bir SQL Veritabanına erişmek için istemci bilgisayarınızdaki güvenlik duvarının 1433 numaralı TCP bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun. Başka uygulamalar için gerekli değilse 1433 numaralı bağlantı noktasından gelen TCP bağlantılarını engelleyin. 

Bağlantı işleminin bir parçası olarak, Azure sanal makinelerinden gelen bağlantılar her bir çalışan rolü için farklı olan bir IP adresine ve bağlantı noktasına yönlendirilir. Bağlantı noktası numarası 11000 ile 11999 arasındadır. TCP bağlantı noktaları hakkında daha fazla bilgi için bkz. [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Kimlik Doğrulaması

SQL Veritabanı iki kimlik doğrulaması türünü destekler:

* **SQL Kimlik Doğrulaması**: Kullanıcı adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak veritabanı sahibi veya "dbo" olarak sunucudaki tüm veritabanları için kimlik doğrulamasından geçebilirsiniz. 
* **Azure Active Directory Kimlik Doğrulaması**: Azure Active Directory tarafından yönetilen kimlikleri kullanır, yönetilen ve tümleşik etki alanları ile kullanılabilir. [Mümkün olduğunda](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory kimlik doğrulamasını (tümleşik güvenlik) kullanın. Azure Active Directory Kimlik Doğrulamasını kullanmak istiyorsanız, Azure AD kullanıcılarını ve gruplarını yönetme izni olan "Azure AD yöneticisi" adlı başka bir sunucu yöneticisi daha oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir. Azure Active Directory kimlik doğrulamasını etkinleştirme amacıyla Azure AD yöneticisi oluşturma adımları için bkz. [Azure Active Directory Kimlik Doğrulaması kullanarak SQL Veritabanına Bağlanma](sql-database-aad-authentication.md).

Veritabanı Altyapısı, 30 dakikadan fazla boşta kalan bağlantıları kapatır. Bağlantının kullanılabilmesi için yeniden oturum açılması gerekir. SQL Veritabanına yapılan sürekli etkin bağlantılar için en az 10 saatte bir yeniden yetkilendirme (veritabanı altyapısı tarafından gerçekleştirilir) gerekir. Veritabanı altyapısı, kullanılan özgün parolayı kullanarak yeniden yetkilendirme gerçekleştirmeyi dener ve kullanıcı müdahalesi gerekli değildir. SQL Veritabanında bir parola sıfırlandığında performans nedeniyle bağlantı, bağlantı havuzu nedeniyle sıfırlansa dahi yeniden kimlik doğrulaması gerektirmez. Bu, şirket içi SQL Server'ın davranışından farklıdır. Bağlantının ilk yetkilendirme adımından sonra parolanın değiştirilmesi halinde bağlantının sonlandırılması ve yeni parolayla yeni bir bağlantı kurulması gerekir. KILL DATABASE CONNECTION iznine sahip bir kullanıcı SQL Veritabanı bağlantılarını [KILL](https://msdn.microsoft.com/library/ms173730.aspx) komutunu kullanarak sonlandırabilir.

Kullanıcı hesapları ana veritabanında oluşturulabilir ve sunucudaki tüm veritabanlarında izin verilebilir veya bunlar veritabanı içinde oluşturulabilir (bağımsız kullanıcılar olarak adlandırılır). Oturum açma bilgisi oluşturma ve yönetme hakkında bilgi için bkz. [Oturum açma bilgilerini yönetme](sql-database-manage-logins.md). Taşınabilirlik ve ölçeklenebilirlik özelliklerini geliştirmek için ölçeklenebilirliği geliştiren kapsayıcılı veritabanı kullanıcılarını kullanın. Bağımsız kullanıcılar hakkında daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) ve [Bağımsız Veritabanları](https://technet.microsoft.com/library/ff929071.aspx).

En iyi uygulama olarak, uygulamanızın kimlik doğrulaması için farklı bir hesap kullanması gerekir. Bu şekilde uygulamaya verilen izinleri sınırlayabilir ve uygulama kodunuzun SQL ekleme saldırısına açık olması durumunda kötü niyetli etkinlik risklerini azaltabilirsiniz. Önerilen yaklaşım, uygulamanızın doğrudan veritabanında kimlik doğrulamasından geçmesi için [bağımsız veritabanı kullanıcısı](https://msdn.microsoft.com/library/ff929188) oluşturmaktır. 

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir kullanıcının bir Azure SQL Veritabanında gerçekleştirebileceklerini tanımlar ve bu durum, kullanıcı hesabınızın veritabanı [rolü üyelikleri](https://msdn.microsoft.com/library/ms189121) ve [nesne düzeyi izinleri](https://msdn.microsoft.com/library/ms191291.aspx) ile denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın. Daha fazla bilgi için bkz. [Oturum açma bilgilerini yönetme](sql-database-manage-logins.md).

Genelde ana veritabanına yalnızca yöneticilerin erişmesi gerekir. Tüm kullanıcı veritabanlarına rutin erişim, her bir veritabanında oluşturulan bağımsız yönetici olmayan veritabanı kullanıcılarıyla sağlanmalıdır. Bağımsız veritabanı kullanıcılarını tercih ettiğinizde ana veritabanı için oturum açma bilgisi oluşturmanız gerekmez. Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx).

Bu özellikler ayrıca izinleri sınırlamak veya artırmak için de kullanılabilir.

* [Kimliğe Bürünme](https://msdn.microsoft.com/library/vstudio/bb669087) ve [modül imzalama](https://msdn.microsoft.com/library/bb669102) ile izinler geçici olarak ve güvenli bir şekilde artırılabilir.
* [Satır Düzeyinde Güvenlik](https://msdn.microsoft.com/library/dn765131) ile bir kullanıcının erişebileceği satırlar sınırlandırılabilir.
* [Veri Maskeleme](sql-database-dynamic-data-masking-get-started.md) ile hassas verilerin kapsamı sınırlandırılabilir.
* [Saklı yordamlar](https://msdn.microsoft.com/library/ms190782) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı güvenlik özelliklerine genel bakış için bkz. [SQL güvenliğine genel bakış](sql-database-security-overview.md).
- Güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı Güvenlik Duvarı](sql-database-firewall-configure.md).
- Kullanıcılar ve oturum açma bilgileri hakkında bilgi almak için bkz. [Oturum açma bilgilerini yönetme](sql-database-manage-logins.md). 
- SQL Veritabanı koruma özellikleri hakkında ayrıntılı bilgi için bkz. [Veri koruma ve güvenlik](sql-database-protect-data.md).
- Öngörülebilir izleme hakkında daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlama](sql-database-auditing-get-started.md) ve [SQL Veritabanı Tehdit Algılamayı kullanmaya başlama](sql-database-threat-detection-get-started.md).
- SQL Server kimlik doğrulamasını kullanan bir öğretici için bkz. [SQL Veritabanı Öğreticisi: SQL Server kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-sql-authentication-get-started.md).
- Azure Active Directory kimlik doğrulamasını kullanan bir öğretici için bkz. [SQL Veritabanı Öğreticisi: Azure AD kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-aad-authentication-get-started.md).



<!--HONumber=Feb17_HO1-->


