<properties
    pageTitle="SQL Database öğreticisi: Güvenliğe Başlarken"
    description="Bir veritabanına erişmek ve veritabanını yönetmek üzere kullanıcı hesabı oluşturmayı öğrenin"
    keywords=""
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
    ms.date="07/19/2016"
    ms.author="carlrab"/>

# SQL Database öğreticisi: Azure portalını kullanarak bir veritabanına erişmek ve veritabanını yönetmek üzere SQL veritabanı kullanıcı hesabı oluşturma


> [AZURE.SELECTOR]
- [Başlangıç öğreticisi](sql-database-get-started-security.md)
- [Erişim verme](sql-database-manage-logins.md)

Bu öğreticide, şu işlemleri gerçekleştirmek üzere Azure portalını nasıl kullanacağınızı öğreneceksiniz:

- Sunucu düzeyinde asıl oturum açma işlemini kullanarak SQL Database'de oturum açma
- SQL Database kullanıcı hesabı oluşturma
- Kullanıcı veritabanında bir SQL Database kullanıcı hesabına dbo izinleri atama
- Sunucu düzeyinde sorumlu olmayan bir kullanıcı hesabıyla SQL veritabanına bağlanma 

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Sonraki adımlar
Artık SQL Database öğreticisini tamamladığınıza, bir kullanıcı hesabı oluşturduğunuza ve kullanıcı hesabına dbo izinlerini atadığınıza göre [SQL Database güvenliği](sql-database-manage-logins.md) hakkında daha fazla bilgi edinmeye hazırsınız.





<!--HONumber=Aug16_HO1-->


