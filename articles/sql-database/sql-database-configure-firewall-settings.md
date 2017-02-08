---
title: "Azure portalı: Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma | Microsoft Docs"
description: "Azure portalı aracılığıyla Azure SQL veritabanlarına erişen IP adresleri için sunucu düzeyinde güvenlik duvarı kurallarını nasıl yapılandıracağınızı öğrenin."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 11/28/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: e5834558d761784239813afc6bbb3e77cebcf1fa
ms.openlocfilehash: fcdd0b855d64eb4b04ef1ea6d7752e9c664557a6


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları oluşturma ve yönetme
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 

Sunucu düzeyinde güvenlik duvarı kuralları, yöneticilerin belirtilen bir IP adresinden ya da IP adresi aralığından bir SQL Veritabanı sunucusuna erişmesine imkan tanır. Aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız sunucu düzeyinde güvenlik duvarı kurallarını kullanıcılar için de kullanabilirsiniz. Microsoft, güvenliği artırmak ve veritabanınızı daha taşınabilir hale getirmek açısından, mümkün olan durumlarda veritabanı düzeyinde güvenlik duvarı kurallarının kullanılmasını önerir. SQL Veritabanı güvenlik duvarlarına genel bir bakış için bkz. [SQL Veritabanı güvenlik duvarı kurallarına genel ](sql-database-firewall-configure.md).

> [!Note]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kurallarını yönetmek için gereken adımları yineleyin.

* Kullanmakta olduğunuz bilgisayarı eklemek için İstemci IP’si ekle’ye tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin.
* Mevcut bir kuralı silmek için kuralın üstüne gelip satırın sonunda X işareti görünene kadar bekleyin. Kuralı kaldırmak için X işaretine tıklayın.

Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu düzeyinde güvenlik duvarları kullanarak sunucu sağlama ve sunucuya bağlanma öğreticisi için bkz. [Öğretici: Azure portal ve SQL Server Management Studio kullanarak Azure SQL veritabanı sağlama ve erişim](sql-database-get-started.md).
- SQL Server kimlik doğrulaması ve veritabanı düzeyinde güvenlik duvarları ile ilgili öğretici için bkz. [SQL kimlik doğrulaması ve yetkilendirme](sql-database-control-access-sql-authentication-get-started.md)
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Veritabanlarına bağlanabilen ek kullanıcıların nasıl oluşturulduğunu öğrenmek için bkz. [SQL Veritabanı Kimlik Doğrulaması ve Yetkilendirme: Erişim Verme](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Ek kaynaklar
* [Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)   
* [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Feb17_HO1-->


