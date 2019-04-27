---
title: Azure Otomasyonu'nu kullanarak Azure SQL veritabanlarını yönetme | Microsoft Docs
description: Nasıl Azure Otomasyonu uygun ölçekte Azure SQL veritabanlarını yönetmek için kullanılabilir hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: e488e742fc49102f7c58d132a66bca2347ad575c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60702104"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure Automation'ı kullanarak Azure SQL database'leri yönetme

Bu kılavuzda Azure Otomasyonu ve nasıl, Azure SQL veritabanlarınızı yönetimini basitleştirmek için kullanılabileceğini başlatacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?

[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) bulut yönetimini işlem Otomasyonu ile basitleştirmeyi için bir Azure hizmetidir. Azure otomasyonu kullanarak, güvenilirlik, verimlilik ve kuruluşunuz için değer süresini artırmak için uzun süre çalışan, el ile hata yapmaya açık ve sık sık tekrarlanan görevleri otomatikleştirilebilir.

Azure Otomasyonu, yüksek güvenilirlik ve yüksek kullanılabilirlik ile bir iş akışı yürütme altyapısı sağlar ve, kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçeklendirir. Görevleri tam olarak gerekli olduğunda gerçekleşir. böylece Azure Automation'da işlemleri el ile üçüncü taraf sistemleri tarafından veya zamanlanan aralıklarla çalıştırmasının.

Alt işlemsel yük ve boş BT / iş kazandıran işe odaklanmak için DevOps personeli, Azure Otomasyonu tarafından otomatik olarak çalıştırılmak için bulut Yönetimi görevlerinizi taşıyarak değer.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Azure Otomasyonu Azure SQL veritabanlarına yönetmenize nasıl yardımcı olabilir?

Azure SQL veritabanı yönetilen Azure Automation'da kullanarak [Azure SQL Database PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/servicemanagement/azure/#sql) kullanılabilir olan [Azure PowerShell Araçları](/powershell/azure/overview). Azure Otomasyonu bu Azure SQL Database PowerShell cmdlet'leri kullanılabilir yepyeni sahiptir; böylece tüm hizmet içinde SQL DB yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca, cmdlet'leri ve üçüncü taraf sistemleri Azure hizmetleri arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri ile Azure Otomasyonu'nda bu cmdlet'ler eşleşebileceğini denetleyebilmesi.

Azure Otomasyonu, PowerShell kullanarak SQL komutları vererek SQL sunucuları ile doğrudan iletişim kurabilir de vardır.

[Azure Otomasyonu runbook Galerisi](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/) Azure SQL veritabanları, diğer Azure Hizmetleri ve üçüncü taraf sistemlerde yönetimini otomatik hale getirmeye başlamanın ürün ekibi ve topluluk runbook'lar çeşitli içerir. Galerideki runbook'ları şunları içerir:

- [SQL Server veritabanına karşı SQL sorguları çalıştırma](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [(Yukarı veya aşağı) dikey olarak ölçeklendirme bir zamanlamaya göre bir Azure SQL veritabanı](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [Veritabanını en büyük boyutuna yaklaştığında, bir SQL tablosunu Kes](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [Yüksek oranda parçalanmış, Azure SQL veritabanı tablolarında dizin](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu ve nasıl Azure SQL veritabanlarını yönetmek için kullanılmadan ilişkin temel bilgileri öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

- [Azure Otomasyonu'na genel bakış](../automation/automation-intro.md)
- [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
- [Azure Otomasyonu: Bulutta SQL aracınızı](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 