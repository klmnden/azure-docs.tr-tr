---
title: "PowerShell: Azure SQL Veritabanı güvenlik duvarı kurallarını yapılandırma | Microsoft Docs"
description: "PowerShell aracılığıyla Azure SQL veritabanlarına erişen IP adresleri için sunucu düzeyinde güvenlik duvarı kurallarını nasıl yapılandıracağınızı öğrenin."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 30dcea72-61c1-48b6-8e1d-b1db2eb61567
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: d80bd1fbb5cdb0492e521a4d600f657fac0e3325


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>PowerShell kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Azure SQL Veritabanı, sunucularınıza ve veritabanlarınıza yönelik bağlantılara izin vermek için güvenlik duvarı kurallarını kullanır. SQL Veritabanı sunucunuzdaki asıl veritabanına veya bir kullanıcı veritabanına yönelik seçmeli erişim izni sağlamak için sunucu düzeyinde veya veritabanı düzeyinde güvenlik duvarı ayarları tanımlayabilirsiniz.

> [!IMPORTANT]
> Azure’daki uygulamaların veritabanı sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Güvenlik duvarı kuralları ve Azure’dan bağlantıları etkinleştirme hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı Güvenlik Duvarı](sql-database-firewall-configure.md). Azure bulut sınırları içerisinde bağlantı kuruyorsanız bazı ek TCP bağlantı noktalarını açmanız gerekebilir. Daha fazla bilgi edinmek için [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433’ten sonraki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md) konusunun "SQL Veritabanının V12 Sürümü: Dış ve iç karşılaştırması" bölümüne bakın.
> 
> 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Sunucu güvenlik duvarı kuralları oluşturma
Azure PowerShell kullanılarak sunucu düzeyinde güvenlik duvarı kuralları oluşturulabilir, güncelleştirilebilir ve silinebilir.

Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturmak için [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) cmdlet’ini yürütün. Aşağıdaki örnekte, Contoso sunucusunda bir IP adresi aralığı etkinleştirilir.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'        

Sunucu düzeyindeki mevcut bir güvenlik duvarı kuralını değiştirmek için [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx) cmdlet’ini yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural için kabul edilebilir IP adresleri aralığı değiştirilmektedir.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -StartIPAddress 192.168.1.4 -EndIPAddress 192.168.1.10 -FirewallRuleName 'ContosoFirewallRule' -ServerName 'Contoso'

Sunucu düzeyindeki mevcut bir güvenlik duvarı kuralını silmek için [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx) cmdlet’ini yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural silinmektedir.

    Remove-AzureRmSqlServerFirewallRule -FirewallRuleName 'ContosoFirewallRule' -ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>PowerShell kullanarak güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını PowerShell kullanarak da yönetebilirsiniz. Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603586\(v=azure.300\).aspx)

## <a name="next-steps"></a>Sonraki adımlar
Transact-SQL’i kullanarak sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma hakkında bilgi edinmek için bkz. [T-SQL kullanarak Azure SQL Veritabanı’nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-tsql.md).

Başka yöntemler kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturma hakkında bilgi edinmek için bkz:

* [Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings.md)
* [REST API kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-rest.md)

Veritabanı oluşturmaya yönelik bir öğretici için bkz. [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).
Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Veritabanlarına nasıl gidileceğini anlamak için bkz. [Veritabanı erişimi ve oturum güvenliğini yönetme](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Ek kaynaklar
* [Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)
* [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!--HONumber=Jan17_HO1-->


