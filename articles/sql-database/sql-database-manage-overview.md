---
title: "Genel Bakış: SQL Veritabanı için yönetim araçları | Microsoft Belgeleri"
description: "Azure SQL Veritabanı yönetim araçlarını ve seçeneklerini karşılaştırır"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 73cc9d1d02ed915466f0eac2d9a42fe8cf9e2fb9
ms.openlocfilehash: 32d0a1fd8671bbd9d450711dd686ca49c2178c16


---
# <a name="overview-management-tools-for-sql-database"></a>Genel Bakış: SQL Database için yönetim araçları
Bu konu başlığında, Azure SQL veritabanlarını yönetmek için kullanabileceğiniz araçlar ve seçenekler incelenmektedir. Bu sayede işiniz, işletmeniz ve kendiniz için doğru aracı seçebilirsiniz. Doğru aracı seçmek yönettiğiniz veritabanı sayısına, göreve ve bir görevin gerçekleştirilme sıklığına bağlıdır.

## <a name="azure-portal"></a>Azure portalına
[Azure portalı](https://portal.azure.com) veritabanlarını ve mantıksal sunucuları oluşturma, güncelleştirme ve silmenin yanı sıra veritabanı etkinliğini izlemek için kullanabileceğiniz bir web tabanlı uygulamadır. Azure'a yeni başlıyorsanız, yalnızca birkaç veritabanını yönetiyorsanız veya hızlıca bir şeyler yapmanız gerekiyorsa bu araç idealdir.

Portalı kullanma hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak SQL Veritabanlarını yönetme](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio ve Visual Studio'daki SQL Server Veri Araçları
SQL Server Management Studio (SSMS) ve SQL Server Veri Araçları (SSDT), bilgisayarınızda çalışan ve buluttaki veritabanınızı yönetmek ve geliştirmek için kullanabileceğiniz istemci araçlarıdır. Visual Studio veya diğer tümleşik geliştirme ortamlarını (IDE) tanıyan bir uygulama geliştiricisiyseniz [Visual Studio'da SSDT kullanmayı deneyin](https://msdn.microsoft.com/library/mt204009.aspx). Çoğu veritabanı yöneticisi tarafından bilinen SSMS, Azure SQL veritabanlarıyla kullanılabilir. [SSMS'nin en son sürümünü indirin](https://msdn.microsoft.com/library/mt238290) ve Azure SQL Veritabanıyla çalışırken her zaman en son sürümü kullanın. Azure SQL Veritabanlarınızı SSMS ile yönetme hakkında daha fazla bilgi için bkz. [SQL Veritabanlarını SSMS ile yönetme](sql-database-manage-azure-ssms.md).

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı özelliklere sahip olmak için her zaman [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) ve [SQL Server Veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx)'nın en son sürümünü kullanın.
>  

## <a name="powershell"></a>PowerShell
PowerShell'i veritabanlarını ve elastik havuzları yönetmek ve Azure kaynak dağıtımlarını otomatik hale getirmek için kullanabilirsiniz. Microsoft, çok sayıda veritabanını yönetmek ve bir üretim ortamında dağıtımın yanı sıra kaynak değişikliklerini otomatik hale getirmek için bu aracın kullanılmasını önerir.

Daha fazla bilgi için bkz. [SQL Veritabanını PowerShell ile yönetme](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Elastik Veritabanı araçları
Elastik veritabanı araçlarını aşağıdakine benzer eylemleri gerçekleştirmek için kullanabilirsiniz 

* [Elastik iş](sql-database-elastic-jobs-overview.md) kullanan veritabanı kümesinde T-SQL betiği yürütmek
* [Ayırma-birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md) ile çok kiracılı model veritabanlarını tek kiracılı modele taşıma
* [Elastik ölçeklendirme istemci kitaplığını](sql-database-elastic-database-client-library.md) kullanarak tek kiracılı model veya çok kiracılı model içindeki veritabanlarını yönetme.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Otomasyonu](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)




<!--HONumber=Dec16_HO3-->


