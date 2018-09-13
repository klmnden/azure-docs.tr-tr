---
title: Azure Otomasyonu'nu kullanarak Azure SQL veritabanlarını yönetme | Microsoft Docs
description: Nasıl Azure Otomasyonu uygun ölçekte Azure SQL veritabanlarını yönetmek için kullanılabilir hakkında bilgi edinin.
services: sql-database, automation
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 43476cfcae2035c3b8e94b4a5e264a0c8ff424e0
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715457"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure Automation'ı kullanarak Azure SQL database'leri yönetme
Bu kılavuzda Azure Otomasyonu ve nasıl, Azure SQL veritabanlarınızı yönetimini basitleştirmek için kullanılabileceğini başlatacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) bulut yönetimini işlem Otomasyonu ile basitleştirmeyi için bir Azure hizmetidir. Azure otomasyonu kullanarak, güvenilirlik, verimlilik ve kuruluşunuz için değer süresini artırmak için uzun süre çalışan, el ile hata yapmaya açık ve sık sık tekrarlanan görevleri otomatikleştirilebilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe, gereksinimlerinizi karşılayacak şekilde ölçeklendirilen bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Görevleri tam olarak gerekli olduğunda gerçekleşir. böylece Azure Automation'da işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla çalıştırmasının.

Alt işlemsel yük ve boş BT / iş kazandıran işe odaklanmak için DevOps personeli, Azure Otomasyonu tarafından otomatik olarak çalıştırılmak için bulut Yönetimi görevlerinizi taşıyarak değer.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Azure Otomasyonu Azure SQL veritabanlarına yönetmenize nasıl yardımcı olabilir?
Azure SQL veritabanı yönetilen Azure Automation'da kullanarak [Azure SQL Database PowerShell cmdlet'leri](https://docs.microsoft.com/en-us/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#sql) kullanılabilir olan [Azure PowerShell Araçları](/powershell/azure/overview). Azure Otomasyonu bu Azure SQL Database PowerShell cmdlet'leri kullanılabilir yepyeni sahiptir; böylece tüm hizmet içinde SQL DB yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca, Azure automation'da bu cmdlet'ler Azure Hizmetleri ve 3. taraf sistemleri arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri eşleşebileceğini denetleyebilmesi.

Azure Otomasyonu, PowerShell kullanarak SQL komutları vererek SQL sunucuları ile doğrudan iletişim kurabilir de vardır.

[Azure Otomasyonu runbook Galerisi](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) Azure SQL veritabanları, diğer Azure Hizmetleri ve 3. taraf sistemleri yönetimini otomatik hale getirmeye başlamanın ürün ekibi ve topluluk runbook'lar çeşitli içerir. Galerideki runbook'ları şunları içerir:

* [SQL Server veritabanına karşı SQL sorguları çalıştırma](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [(Yukarı veya aşağı) dikey olarak ölçeklendirme bir zamanlamaya göre bir Azure SQL veritabanı](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Veritabanını en büyük boyutuna yaklaştığında, bir SQL tablosunu Kes](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Yüksek oranda parçalanmış, Azure SQL veritabanı tablolarında dizin](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu ve nasıl Azure SQL veritabanlarını yönetmek için kullanılmadan ilişkin temel bilgileri öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Otomasyonu'na genel bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
* [Azure Otomasyonu öğrenme Haritası](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Otomasyonu: Bulutta SQL Aracısı](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

