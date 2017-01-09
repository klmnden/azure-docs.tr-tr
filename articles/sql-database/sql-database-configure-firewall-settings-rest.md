---
title: "REST API kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma | Microsoft Belgeleri"
description: "Güvenlik duvarının Azure SQL veritabanlarına erişen IP adresleri için nasıl yapılandırılacağını öğrenin."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: cc0faa49daaafe19c71d2c765b8e865be04f81e2


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>REST API kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma
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

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>REST API aracılığıyla sunucu düzeyinde güvenlik duvarı kurallarını yönetme
1. REST API aracılığıyla güvenlik duvarı kurallarını yönetme kimliği doğrulanmalıdır. Bilgi edinmek için bkz. [Azure Resource Manager API ile yetkilendirme için geliştirici kılavuzu](../azure-resource-manager/resource-manager-api-authentication.md).
2. REST API kullanılarak sunucu düzeyinde kurallar oluşturulabilir, güncelleştirilebilir ve silinebilir
   
    Sunucu düzeyinde bir kural oluşturmak veya güncelleştirmek için aşağıdakini kullanarak PUT yöntemini uygulayın:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    İstek Gövdesi
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Sunucu düzeyindeki mevcut bir kuralı silmek için aşağıdakini kullanarak DELETE yöntemini uygulayın:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>REST API kullanarak güvenlik duvarı kurallarını yönetme
* [Güvenlik Duvarı Kuralı Oluşturma veya Güncelleştirme](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Güvenlik Duvarı Kuralı Silme](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Güvenlik Duvarı Kuralı Alma](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Tüm Güvenlik Duvarı Kurallarını Listeleme](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Transact-SQL’i kullanarak sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturmayla ilgili bir nasıl yapılır makalesi için bkz. [T-SQL kullanarak Azure SQL Veritabanı’nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-tsql.md). 

Başka yöntemler kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturmayla ilgili nasıl yapılır makaleleri için bkz: 

* [Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings.md)
* [PowerShell kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-powershell.md)

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


