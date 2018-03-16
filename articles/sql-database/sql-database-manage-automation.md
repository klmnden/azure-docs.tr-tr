---
title: "Azure otomasyonu kullanarak Azure SQL veritabanlarını yönetme | Microsoft Docs"
description: "Nasıl Azure Otomasyon hizmetine ölçekte Azure SQL veritabanını yönetmek için kullanılabilir hakkında bilgi edinin."
services: sql-database, automation
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 02/03/2017
ms.author: carlrab
ms.openlocfilehash: 0174b2b1dd5942e17ea60c2dce624c87fd1289c8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure otomasyonu kullanarak Azure SQL veritabanlarını yönetme
Bu kılavuz Azure Otomasyon hizmetine ve nasıl Azure SQL veritabanlarınızın yönetimini basitleştirmek için kullanılabilmesi için tanıtılacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) işlem Otomasyonu aracılığıyla bulut yönetimi basitleştirmek için bir Azure hizmetidir. Azure otomasyonu kullanarak, uzun süre çalışan, el ile hatasız ve sık tekrarlanan görevleri güvenilirlik, verimliliği ve kuruluşunuz için değer süre artırmak için otomatik olarak yapılabilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçekler bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Tam olarak gerekli görevleri ortaya böylece Azure Otomasyonu'nda işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla artırılabilir devre dışı.

İşletimsel ek yükü azaltmak ve boşaltmak BT / iş ekler çalışmaları odaklanmaya DevOps personel değer Azure Automation'ın otomatik olarak çalıştırılmak üzere bulut yönetim görevlerinizi taşıyarak.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Azure Otomasyonu Azure SQL veritabanlarını yönetme nasıl yardımcı olabilir?
Azure SQL veritabanı yönetilebilir Azure Otomasyonu'nda kullanarak [Azure SQL Database PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) kullanılabilir olan [Azure PowerShell Araçları](/powershell/azure/overview). Tüm SQL DB yönetim görevlerinizi hizmet içinde gerçekleştirebilmeniz azure Otomasyonu kutudan çıktığında, bu Azure SQL Database PowerShell cmdlet'leri vardır. Ayrıca Azure Automation bu cmdlet'leri Azure hizmeti ve 3. taraf sistemi arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri ile eşleştirin.

Azure Otomasyonu, ayrıca PowerShell kullanarak SQL komutları vererek SQL sunucuları ile doğrudan iletişim kurmak için özelliğine sahiptir.

[Azure Otomasyonu runbook Galerisi](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) Azure SQL veritabanları, diğer Azure Hizmetleri ve 3. taraf sistemi yönetimi otomatikleştirme başlamak için ürün ekibi ve topluluk runbook'lar çeşitli içerir. Galeri runbook'ları şunları içerir:

* [Bir SQL Server veritabanında SQL sorguları çalıştırma](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [(Yukarı veya aşağı) dikey olarak ölçeklendirmek bir zamanlamaya göre bir Azure SQL veritabanı](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Veritabanı boyut üst sınırına yaklaşıyor varsa bir SQL tablosunu kesme](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Yüksek oranda parçalanmış varsa bir Azure SQL veritabanı tablolarında dizin](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu ve nasıl Azure SQL veritabanlarını yönetmek için kullanılmadan öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Azure Otomasyonu genel bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
* [Azure Otomasyonu öğrenme Haritası](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Otomasyonu: SQL aracınızı bulutta](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

