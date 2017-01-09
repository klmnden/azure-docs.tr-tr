---
title: "SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma | Microsoft Belgeleri"
description: "Güvenlik duvarının Azure SQL Server’a erişen IP adresleri için nasıl yapılandırılacağını öğrenin."
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
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: a87bb18aeacbc980fc6859c7c83a102dce0263a8


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

- Bir başlangıç öğreticisi için bkz. [SQL Veritabanı öğreticisi: Sunucu, sunucu düzeyinde güvenlik duvarı kuralı, örnek veritabanı, veritabanı düzeyinde güvenlik duvarı oluşturma ve SQL Server’a bağlanma](sql-database-get-started.md).
- Güvenliğe giriş konulu bir öğretici için bkz. [Güvenliğe giriş](sql-database-get-started-security.md)
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Veritabanlarına bağlanabilen ek kullanıcıların nasıl oluşturulduğunu öğrenmek için bkz. [SQL Veritabanı Kimlik Doğrulaması ve Yetkilendirme: Erişim Verme](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Ek kaynaklar
* [Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)   
* [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Dec16_HO4-->


