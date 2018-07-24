---
title: Azure'a Contoso geçişine genel bakış | Microsoft Docs
description: Geçiş stratejisi ve senaryoları, şirket içi veri merkezinizi Azure'a geçirme için Contoso tarafından kullanılan genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: be9302d47766ddc7d75496e28e9b18a8e870c878
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216249"
---
# <a name="contoso-migration-overview"></a>Contoso geçiş: genel bakış


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
**Yeniden oluşturma** | Azure bulut teknolojilerini kullanarak sıfırdan bir uygulama yeniden tarafından adım bir şeyler daha fazla sürer yeniden oluşturun.<br/><br/> Örneğin, Azure işlevleri, Azure yapay ZEKA, Azure SQL veritabanı yönetilen örneği ve Azure Cosmos DB gibi yerel bulut teknolojileriyle yeşil alan uygulamalar da oluşturabilirsiniz. | Hızlı geliştirme istediğiniz ve işlevsellik ve kullanım ömrü mevcut uygulamaları sınırlı.<br/><br/> (Azure tarafından sağlanan DevOps uygulamaları dahil) işinizde yeniliği hızlandırmak hazır olduğunuzda, yerel bulut teknolojilerini kullanarak yeni uygulamalar oluşturmak ve yapay ZEKA, blok zinciri ve IOT geliştirmeleri avantajlarından yararlanın.

## <a name="migration-articles"></a>Geçiş makaleleri

Serinin makalelerde, aşağıdaki tabloda özetlenmiştir.  

- Her geçiş senaryosu, geçiş stratejisi belirleme biraz farklı iş hedeflerini tarafından yönetilir.
- Her dağıtım senaryosu için geçiş tamamlandıktan sonra geçiş ve öneri temizleme ve sonraki adımları gerçekleştirmek için iş sürücüleri ve hedefler, önerilen bir mimari, adımları hakkında bilgi veriyoruz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
Makale 1: genel bakış | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Bu makalede.
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmeti ve veritabanı geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı. | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel şirket içi uygulama için nasıl çalıştığını gösterir. Contoso uygulaması ön uç sanal makine geçirir Azure Site Recovery ve bir SQL Azure veritabanı geçiş hizmeti kullanarak yönetilen örneğe, uygulama veritabanını kullanarak. | Kullanılabilir
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme SmartHotel uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak Azure sanal makinelerine gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve Always On kullanılabilirlik grubu SQL Server üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso uygulaması Vm'leri ve Azure veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Azure Site Recovery kullanarak SmartHotel uygulama nasıl geçirdiğini gösterir. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Azure Site Recovery kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması Azure Site Recovery kullanarak Azure Vm'leri için nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleştirilmiş bir Azure Web uygulamasına nasıl geçirdiğini gösterir. Bunlar, Azure MySQL Server örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir






### <a name="demo-apps"></a>Tanıtım uygulamaları

İki tanıtım uygulamaları - SmartHotel ve osTicket makaleleri kullanın.

- **SmartHotel360**: Bu uygulama, Microsoft Azure ile çalışırken kullanabileceğiniz bir test uygulaması olarak geliştirilmiştir. Açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360). Bu bir SQL Server veritabanına bağlı bir ASP.NET uygulamasıdır. Şu anda Windows Server 2008 R2 ve SQL Server 2008 R2 çalıştıran iki VMware Vm'lerinde uygulamasıdır. Uygulama Vm'leri barındırılan şirket içinde ve vCenter Server tarafından yönetilir.
- **osTicket**: bir açık kaynak hizmet Masası gişe Linux üzerinde çalışan uygulama. Buradan indirebileceğiniz [GitHub](https://github.com/osTicket/osTicket). Geçerli uygulama iki VMware Ubuntu 16.04 LTS çalıştıran, Apache 2, PHP 7.0 ve MySQL 5.7 kullanarak Vm'leri olur
    

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](contoso-migration-infrastructure.md) Contoso ayarlar bir şirket içi ve geçişe hazırlamak için Azure altyapı.



