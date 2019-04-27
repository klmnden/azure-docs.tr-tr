---
title: Azure'a Contoso geçişine genel bakış | Microsoft Docs
description: Geçiş stratejisi ve senaryoları, şirket içi veri merkezinizi Azure'a geçirme için Contoso tarafından kullanılan genel bir bakış sağlar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 8c1b5cc8a9f1c1246bd1973539e3bd00b340a657
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679043"
---
# <a name="contoso-migration-overview"></a>Contoso geçişi: Genel Bakış


Bu makalede nasıl geçirir Contoso adlı kurgusal kuruluş altyapısına şirket gösterilmektedir [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulut. 

Bu belge, Contoso adlı kurgusal şirketin Azure'a nasıl geçirdiğini gösteren makaleler serisinin ilk değil. Serinin bilgiler ve altyapısının bir geçişi ayarlayın ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz. Contoso şirketi geçiş misyonunu nasıl tamamlandıktan makaleleri gösterir ancak boyunca işaretçiler için genel okuma ve belirli yönergeler sağlanır.

## <a name="introduction"></a>Giriş

Azure, kapsamlı bir bulut hizmetleri kümesine erişim sağlar. Geliştiriciler ve BT uzmanları, bu hizmetler oluşturmanızı, dağıtmanızı ve araçları ve çerçeveleri, veri merkezlerinden oluşan global bir ağda aracılığıyla bir aralıkta uygulamaları yönetmek için kullanabilirsiniz. İşiniz dijital değişimle ilişkili zorluklarla karşılaştığında Azure, kaynakları ve işlemleri en iyi duruma nasıl getireceğinizi, müşteriler ve çalışanlar ile nasıl etkileşim kuracağınızı ve ürünlerinizi nasıl dönüştüreceğinizi belirlemenize yardımcı olur.

Ancak Azure, bulutun hız ve esneklik, düşük maliyetler, performans ve güvenilirlik açısından sağladığı tüm avantajlarla bile çoğu kuruluşun bir süre daha şirket içi veri merkezlerini çalıştırması gerekeceğinin bilincindedir. Bulutu benimsemenin önündeki engellere karşı Azure, şirket içi veri merkezleriniz ile Azure genel bulutu arasında köprü işlevi görecek bir hibrit bulut stratejisi sunar. Örneğin, Azure Backup'ı korumak için şirket içi kaynaklara gibi Azure bulut kaynaklarını kullanma veya şirket içi iş yüklerini Öngörüler edinmek için Azure Analytics'i kullanma. 

Hibrit bulut stratejisinin bir parçası olarak, Azure, şirket içi uygulamalar ve iş yüklerini buluta geçirme için büyüyen çözümleri sunar. Basit adımları uygulayarak, şirket içi kaynaklarınızın Azure bulutunda nasıl çalışacağını belirlemek için bunları kapsamlı bir şekilde değerlendirebilirsiniz. Daha sonra, elde ettiğiniz kapsamlı değerlendirme ile kaynakları Azure’a güvenle geçirebilirsiniz. Kaynaklar Azure’da hazır ve çalışır duruma geldiğinde erişimi, esnekliği, güvenliği ve güvenilirliği sürdürüp geliştirmek için bu kaynakları iyi duruma getirebilirsiniz.

## <a name="migration-strategies"></a>Geçiş stratejileri

Buluta geçiş stratejileri dört kategoriden ayrılır: barındırma, yeniden düzenleme, yeniden oluşturma veya yeniden oluşturun. Benimseme stratejisi, iş sürücüleri ve geçiş amaçları bağlıdır. Birden çok stratejilerini benimseyen. Örneğin, (lift-and-shift ile taşıma) basit uygulamalar ya da işiniz için kritik olmayan uygulamaların yeniden barındırılmasına yönelik seçin ancak daha da karmaşık ve iş açısından kritik olan yeniden oluşturma. Stratejiler göz atalım.


**Stratejisi** | **Tanım** | **Ne zaman kullanılır?** 
--- | --- | --- 
**Yeniden barındırma** | Genellikle için bir "lift-and-shift" geçiş adlandırılır. Bu seçeneği, kod değişikliği gerektirmez ve şimdi, mevcut uygulamalarınızı Azure'a hızlıca geçirin. Her uygulama, riskini ve maliyetini ilişkili bir kod değişikliği olmadan bulutun avantajlarından avantajlarından yararlanmanın tam olarak geçirilir. | Uygulamaları hızla buluta taşımak gerektiğinde.<br/><br/> Bir uygulamayı değiştirmeden taşımak istediğinizde.<br/><br/> Ne zaman uygulamalarınızı desteklemesi için bunlar yararlanabilir, böylece [Azure Iaas](https://azure.microsoft.com/overview/what-is-iaas/) ölçeklenebilirlik geçişten sonra.<br/><br/> Ne zaman uygulamaları işletmeniz için önemli olan, ancak uygulama özellikleri Acil değişiklikler gerekmez.
**Yeniden düzenleme** | Böylece bağlanabilecekleri genellikle "yeniden paketleme olarak" adlandırılan, yeniden düzenleme uygulamalar, küçük değişiklikler gerektirir [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/)ve bulut teklifleri kullanın.<br/><br/> Örneğin, Azure App Service veya Azure Kubernetes Service (AKS) için mevcut uygulamaları geçirebilirsiniz.<br/><br/> Alternatif olarak, PostgreSQL ve Azure Cosmos DB için Azure SQL veritabanı yönetilen örneği, MySQL için Azure veritabanı, Azure veritabanı gibi seçenekler halinde ilişkisel ve ilişkisel olmayan veritabanlarının yeniden. | Uygulamanızı Azure'da çalışması için kolayca paketlenebilir durumunda.<br/><br/> Uygulamak istiyorsanız yenilikçi DevOps uygulamalarını Azure tarafından sağlanan veya iş yükleri için bir kapsayıcı stratejisi kullanarak Devops'a hakkında düşünmek.<br/><br/> Yeniden düzenleme, varolan kod tabanı ve mevcut geliştirme becerilerinizi taşınabilirliği hakkında düşünmeniz gerekir.
**Yeniden oluşturma** | Geçiş için işleve bölmenize değiştirme ve işlevini ve uygulama mimarisi bulut ölçeklenebilirlik sağlamak için en iyi duruma getirmek için kod tabanının genişletme odaklanır.<br/><br/> Örneğin, bir grubun bir kolayca ölçeklendirin ve birlikte çalışan mikro hizmetler tek parça bir uygulamayı aşağı bozar.<br/><br/> Veya Azure SQL veritabanı yönetilen örneği, MySQL için Azure veritabanı, PostgreSQL ve Azure Cosmos DB için Azure veritabanı gibi tam olarak yönetilen bir DBaaS çözümleri için ilişkisel ve ilişkisel olmayan veritabanları yeniden oluşturma. | Uygulamalarınıza yeni özellikler eklemek veya bir bulut platformunda etkili bir şekilde çalışması için önemli düzeltmeler gerektiğinde.<br/><br/> Uygulama ilişkin mevcut yatırım kullanmak istediğinizde, ölçeklenebilirlik gereksinimlerini karşılamak, yenilikçi Azure DevOps uygulamalarını uygulamak ve sanal makinelerin kullanımını en aza indirin.
**Yeniden oluşturma** | Azure bulut teknolojilerini kullanarak sıfırdan bir uygulama yeniden tarafından adım bir şeyler daha fazla sürer yeniden oluşturun.<br/><br/> Örneğin, yeşil alan uygulamalarla oluşturabilirsiniz [bulutta yerel](https://azure.com/cloudnative) teknolojiler, Azure işlevleri, Azure yapay ZEKA, Azure SQL veritabanı yönetilen örneği ve Azure Cosmos DB ister. | Hızlı geliştirme istediğiniz ve işlevsellik ve kullanım ömrü mevcut uygulamaları sınırlı.<br/><br/> (Azure tarafından sağlanan DevOps uygulamaları dahil) işinizde yeniliği hızlandırmak hazır olduğunuzda, yerel bulut teknolojilerini kullanarak yeni uygulamalar oluşturmak ve yapay ZEKA, blok zinciri ve IOT geliştirmeleri avantajlarından yararlanın.

## <a name="migration-articles"></a>Geçiş makaleleri

Serinin makalelerde, aşağıdaki tabloda özetlenmiştir.  

- Her geçiş senaryosu, geçiş stratejisi belirleme biraz farklı iş hedeflerini tarafından yönetilir.
- Her dağıtım senaryosu için geçiş tamamlandıktan sonra geçiş ve öneri temizleme ve sonraki adımları gerçekleştirmek için iş sürücüleri ve hedefler, önerilen bir mimari, adımları hakkında bilgi veriyoruz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
1. makale: Genel Bakış | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Bu makalede
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, kendi SmartHotel360 uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'lerine geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir [makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) 
[Makale 8: MySQL için Azure sanal makineler ve Azure veritabanı üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Linux osTicket uygulaması, Site Recovery kullanarak Azure Vm'lerine geçirir. Bu uygulama veritabanı için Azure veritabanı için MySQL MySQL Workbench kullanarak geçirir. | Kullanılabilir
[Makale 9: Bir Azure web uygulaması ve Azure SQL veritabanı içinde bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso, SmartHotel360 uygulama için bir Azure web uygulaması geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir. | Kullanılabilir    
[Makale 10: MySQL için bir Azure web uygulaması ve Azure veritabanı'nda bir Linux uygulama yeniden düzenleyin](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir
[11. makale: Azure DevOps Hizmetleri Team Foundation Server'da yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir    
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir 
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir

Contoso tüm altyapı öğeleri ayarlar bu makaledeki tüm geçiş senaryolarını tamamlamak gerekir. 







### <a name="demo-apps"></a>Tanıtım uygulamaları

İki tanıtım uygulamaları - SmartHotel360 ve osTicket makaleleri kullanın.

- **SmartHotel360**: Bu uygulama, Azure ile çalışırken kullanabileceğiniz bir test uygulaması olarak Microsoft tarafından geliştirilmiştir. Açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360). Bu bir SQL Server veritabanına bağlı bir ASP.NET uygulamasıdır. Şu anda Windows Server 2008 R2 ve SQL Server 2008 R2 çalıştıran iki VMware Vm'lerinde uygulamasıdır. Uygulama Vm'leri barındırılan şirket içinde ve vCenter Server tarafından yönetilir.
- **osTicket**: Bir açık kaynak hizmet Masası gişe Linux üzerinde çalışan uygulama. Buradan indirebileceğiniz [GitHub](https://github.com/osTicket/osTicket). Şu anda iki VMware Ubuntu 16.04 LTS çalıştıran, Apache 2, PHP 7.0 ve MySQL 5.7 kullanarak Vm'leri uygulamasıdır
    

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](contoso-migration-infrastructure.md) Contoso ayarlar bir şirket içi ve geçişe hazırlamak için Azure altyapı.



