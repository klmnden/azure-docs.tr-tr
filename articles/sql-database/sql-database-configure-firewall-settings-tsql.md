---
title: "T-SQL: Azure SQL Veritabanı güvenlik duvarı kuralları | Microsoft Docs"
description: "Transact-SQL aracılığıyla Azure SQL veritabanlarına erişen IP adresleri için sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kurallarını nasıl yapılandıracağınızı öğrenin."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: 71e692a1-5e2f-4a18-a6d6-527b849cf68e
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 2f24c1c43300a3c5035fbb4e90c3d13f73c1bc74
ms.openlocfilehash: ef51f63984d342422ee5f2a93acfee6357df57ca


---
# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>T-SQL kullanarak Azure SQL Veritabanı’nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Veritabanı, sunucularınıza ve veritabanlarınıza yönelik bağlantılara izin vermek için güvenlik duvarı kurallarını kullanır. Azure SQL Veritabanı sunucunuzdaki asıl veritabanına veya bir kullanıcı veritabanına yönelik seçmeli erişim izni sağlamak için sunucu düzeyinde veya veritabanı düzeyinde güvenlik duvarı ayarları tanımlayabilirsiniz.

> [!IMPORTANT]
> Azure’daki uygulamaların veritabanı sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Güvenlik duvarı kuralları ve Azure’dan bağlantıları etkinleştirme hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı Güvenlik Duvarı](sql-database-firewall-configure.md). Azure bulut sınırları içerisinde bağlantı kuruyorsanız bazı ek TCP bağlantı noktalarını açmanız gerekebilir. Daha fazla bilgi edinmek için [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433’ten sonraki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md) konusunun **SQL Veritabanının V12 Sürümü: Dış ve iç karşılaştırması** bölümüne bakın
> 
> 

## <a name="server-level-firewall-rules"></a>Sunucu düzeyinde güvenlik duvarı kuralları
Yalnızca sunucu düzeyinde sorumlu oturumu veya Azure Active Directory yöneticisi tarafından T-SQL kullanılarak sunucu düzeyinde bir güvenlik duvarı kuralı oluşturulabilir.

1. SQL Server Management Studio’yu kullanarak bir sorgu penceresi başlatın ve sanal asıl veritabanına bağlanın.
2. Sorgu penceresinin içinden sunucu düzeyinde güvenlik duvarı kuralları seçilebilir, oluşturulabilir, güncelleştirilebilir veya silinebilir.
3. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak veya güncelleştirmek için `sp_set_firewall_rule` saklı yordamını kullanın. Aşağıdaki örnekte, Contoso sunucusunda bir IP adresi aralığı etkinleştirilir.<br/>İşleme hangi kuralların zaten mevcut olduğuna bakarak başlayın.
   
        SELECT * FROM sys.firewall_rules ORDER BY name;
   
    Daha sonra bir güvenlik duvarı kuralı ekleyin.
   
        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
   
    Sunucu düzeyindeki bir güvenlik duvarı kuralını silmek için sp_delete_firewall_rule saklı yordamını yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural silinmektedir.
   
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
   
   Bu saklı yordamlar hakkında daha fazla bilgi için bkz. [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) ve [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Veritabanı düzeyinde güvenlik duvarı kuralları
Yalnızca veritabanı üzerinde **CONTROL** iznine sahip olan bir veritabanı kullanıcısı (veritabanı sahibi gibi) veritabanı düzeyinde güvenlik duvarı kuralı oluşturabilir.

1. IP adresiniz için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturduktan sonra Klasik portal veya SQL Server Management Studio aracılığıyla bir sorgu penceresi başlatın.
2. Veritabanı düzeyinde bir güvenlik duvarı kuralı oluşturmak istediğiniz veritabanına bağlanın.
   
    Veritabanı düzeyinde yeni bir güvenlik duvarı kuralı oluşturmak veya mevcut bir kuralı güncelleştirmek için `sp_set_database_firewall_rule` saklı yordamını yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı yeni bir güvenlik duvarı kuralı oluşturulmaktadır.
   
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
   
    Veritabanı düzeyindeki mevcut bir kuralı silmek için `sp_delete_database_firewall_rule` saklı yordamını yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural silinmektedir.
   `
   
        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Bu saklı yordamlar hakkında daha fazla bilgi için bkz. [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) ve [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

> [!NOTE]
> Veritabanı düzeyinde güvenlik duvarlarının kullanımını gösteren bir öğretici için bkz. [SQL Veritabanı Öğreticisi: SQL Server kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-sql-authentication-get-started.md).
>


## <a name="next-steps"></a>Sonraki adımlar
Başka yöntemler kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturmayla ilgili nasıl yapılır makaleleri için bkz: 

* [Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings.md)
* [PowerShell kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-powershell.md)
* [REST API kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-rest.md)

Veritabanı oluşturmaya yönelik bir öğretici için bkz. [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).
Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Veritabanlarına nasıl gidileceğini anlamak için bkz. [Veritabanı erişimi ve oturum güvenliğini yönetme](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Ek kaynaklar
* [Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)
* [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589)




<!--HONumber=Jan17_HO3-->


