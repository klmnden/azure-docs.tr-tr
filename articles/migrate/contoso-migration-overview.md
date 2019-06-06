---
title: Contoso geçiş serisi | Microsoft Docs
description: Geçiş stratejisi ve senaryoları, şirket içi veri merkezinizi Azure'a geçirme için Contoso tarafından kullanılan genel bir bakış sağlar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: raynew
ms.openlocfilehash: 048772edadca36a63870a2965c703ca7e6ec8c63
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729898"
---
# <a name="contoso-migration-series"></a>Contoso geçiş serisi


Bir dizi nasıl geçirir Contoso adlı kurgusal kuruluş altyapısına şirket gösteren makale sahibiz [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulut. 

Serinin bilgiler ve altyapısının bir geçişi ayarlayın ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Bunlar ilerledikçe senaryoları, karmaşık hale gelmesi. Contoso şirketi geçiş misyonunu nasıl tamamlandıktan makaleleri gösterir ancak boyunca işaretçiler için genel okuma ve belirli yönergeler sağlanır.

## <a name="migration-articles"></a>Geçiş makaleleri

Serinin makalelerde, aşağıdaki tabloda özetlenmiştir.  

- Her geçiş senaryosu, geçiş stratejisi belirleme biraz farklı iş hedeflerini tarafından yönetilir.
- Her dağıtım senaryosu için geçiş tamamlandıktan sonra geçiş ve öneri temizleme ve sonraki adımları gerçekleştirmek için iş sürücüleri ve hedefler, önerilen bir mimari, adımları hakkında bilgi veriyoruz.

**Makale** | **Ayrıntılar** 
--- | --- 
[1. makale: Genel bakış](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. 
[2. makale: Azure altyapısı dağıtma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. 
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-assessment)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir.
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview).
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso, kendi SmartHotel360 uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'lerine geçirir. 
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) |Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. 
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso Azure vm'lerine, Site Recovery hizmetini kullanarak kendi Linux osTicket uygulamasının lift-and-shift ile taşıma geçiş tamamlanır.
[Makale 8: MySQL için Azure sanal makineler ve Azure veritabanı üzerinde bir Linux uygulaması barındırma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso, Linux osTicket uygulaması, Site Recovery kullanarak Azure Vm'lerine geçirir. Bu uygulama veritabanı için Azure veritabanı için MySQL MySQL Workbench kullanarak geçirir. 
[Makale 9: Bir Azure web uygulaması ve Azure SQL veritabanı içinde bir uygulamayı yeniden düzenleme](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso, SmartHotel360 uygulama için bir Azure web uygulaması geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir.     
[Makale 10: MySQL için bir Azure web uygulaması ve Azure veritabanı'nda bir Linux uygulama yeniden düzenleyin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. 
[11. makale: Azure DevOps Hizmetleri Team Foundation Server'da yeniden düzenleyin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir.
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. 
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur.  
[Makale 14: Azure'a geçiş ölçeklendirin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. 


    

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/) buluta geçiş. 

