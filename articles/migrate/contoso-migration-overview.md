---
title: Azure Contoso geçişine genel bakış | Microsoft Docs
description: Contoso tarafından kendi şirket içi veri merkezini Azure'a geçirmek için kullanılan senaryoları ve geçiş stratejisi genel bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: ec0308bb2e39c3801305748f19783e70d58b0b7e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232590"
---
# <a name="contoso-migration-overview"></a>Contoso geçiş: genel bakış


Bu ilk bir nasıl Contoso adlı kurgusal kuruluş olan geçiş kendi şirket içi altyapı gösteren bir dizi makaledir [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulut. 

Makale serisi anlamak ve Azure geçirmeyi ve karma bir ortamda çalışmak için farklı seçenekleri denemek için yardımcı örnek dağıtımlar sağlar. Contoso şirketi geçiş misyonu nasıl tamamlandıktan makaleleri gösterir ancak boyunca işaretçileri genel okumak ve özel yönergeler sağlanır.

## <a name="introduction"></a>Giriş

Azure bulut Hizmetleri kapsamlı bir kümesini erişim sağlar. Geliştiriciler ve BT uzmanı, bu hizmetleri oluşturmak, dağıtmak ve araçları ve veri merkezleri genel bir ağ üzerinden çerçeveler bir aralıkta uygulamaları yönetmek için kullanabilirsiniz. İşiniz dijital değişimle ilişkili zorluklarla karşılaştığında Azure, kaynakları ve işlemleri en iyi duruma nasıl getireceğinizi, müşteriler ve çalışanlar ile nasıl etkileşim kuracağınızı ve ürünlerinizi nasıl dönüştüreceğinizi belirlemenize yardımcı olur.

Ancak Azure, bulutun hız ve esneklik, düşük maliyetler, performans ve güvenilirlik açısından sağladığı tüm avantajlarla bile çoğu kuruluşun bir süre daha şirket içi veri merkezlerini çalıştırması gerekeceğinin bilincindedir. Bulutu benimsemenin önündeki engellere karşı Azure, şirket içi veri merkezleriniz ile Azure genel bulutu arasında köprü işlevi görecek bir hibrit bulut stratejisi sunar. Örneğin, şirket içi kaynakları korumak için yedekleme gibi Azure bulut kaynaklarını kullanma veya şirket içi iş yüklerine ilişkin içgörü elde etmek için Azure Analytics’i kullanma. 

Karma bulut stratejisinin bir parçası, Azure geçirme şirket içi uygulamalar ve iş yüklerinin buluta büyüyen çözümleri sağlar. Basit adımları uygulayarak, şirket içi kaynaklarınızın Azure bulutunda nasıl çalışacağını belirlemek için bunları kapsamlı bir şekilde değerlendirebilirsiniz. Daha sonra, elde ettiğiniz kapsamlı değerlendirme ile kaynakları Azure’a güvenle geçirebilirsiniz. Kaynaklar Azure’da hazır ve çalışır duruma geldiğinde erişimi, esnekliği, güvenliği ve güvenilirliği sürdürüp geliştirmek için bu kaynakları iyi duruma getirebilirsiniz.

## <a name="migration-strategies"></a>Geçiş stratejileri

Buluta geçiş stratejileri dört büyük kategoriye ayrılır: rehost, yeniden düzenleyin, rearchitect veya yeniden oluşturun. Benimsemeyi stratejisi, iş sürücüleri ve geçiş amaçları bağlıdır. Birden çok strateji benimsemeyi. Örneğin, iş için kritik olmayan uygulamaları (yükseltme ve shift) basit uygulamaları veya yeniden barındırma tercih ancak daha da karmaşık ve iş açısından kritik olanlar rearchitect. Stratejileri bakalım.


**Stratejisi** | **Tanım** | **Ne zaman kullanılacağı** 
--- | --- | --- 
**Yeniden barındırma** | Genellikle için "yükseltme ve shift" geçiş olarak adlandırılır. Bu seçenek kod değişikliklerini gerektirmez ve şimdi, mevcut uygulamalarınızı Azure için hızlı bir şekilde geçirilir. Her uygulama, kod değişiklikleri ile ilgili maliyet ve riski olmadan bulut avantajlarından yararlanmasını olarak geçirilir. | Ne zaman uygulamaları hızlı bir şekilde buluta taşımanız gerekir.<br/><br/> Bir uygulamayı değiştirmeden taşımak istediğinizde.<br/><br/> Ne zaman uygulamalarınızı tasarlanmış böylece bunlar yararlanabilirsiniz [Azure Iaas](https://azure.microsoft.com/overview/what-is-iaas/) ölçeklenebilirlik geçişten sonra.<br/><br/> Ne zaman uygulamaları işletmeniz için önemli olan, ancak uygulama yeteneklerinin Acil değişiklikler gerekmez.
**Yeniden düzenleme** | Böylece bağlanabilecekleri genellikle "yeniden paketleme olarak" adlandırılan, yeniden düzenleme uygulamalar, küçük değişiklikler gerektirir [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/)ve bulut teklifleri kullanın.<br/><br/> Örneğin, var olan uygulamaları Azure uygulama hizmeti ya da Azure Kubernetes hizmet (AKS) geçişi yapılamadı.<br/><br/> Veya, PostgreSQL ve Azure Cosmos DB yönetilen Azure SQL veritabanı örneği, Azure veritabanı için MySQL, Azure veritabanı gibi seçeneklerini içine ilişkisel ve ilişkisel olmayan veritabanlarını yeniden. | Uygulamanızı Azure'da çalışması için kolayca paketlenebilir durumunda.<br/><br/> Uygulamak isterseniz yenilikçi DevOps yöntemler Azure tarafından sağlanan veya DevOps iş yükleri için bir kapsayıcı stratejisini kullanma hakkında düşündüğünüzü.<br/><br/> Yeniden düzenleme için var olan kod temeli ve kullanılabilir geliştirme becerileri taşınabilirlik hakkında düşünmeniz gerekir.
**Rearchitect** | Geçiş için bütçeden değiştirme ve uygulama işlevsellik ve bulut ölçeklenebilirliği uygulama mimarisi iyileştirmek için temel kod genişletildiğinde odaklanır.<br/><br/> Örneğin, bir grup birlikte çalışır ve kolay ölçeklendirmenizi mikro tek yapılı bir uygulamasına aşağı bozar.<br/><br/> Veya, ilişkisel rearchitect ve ilişkisel olmayan veritabanlarına tam olarak yönetilen Azure SQL veritabanı örneği, Azure veritabanı için MySQL, PostgreSQL ve Azure Cosmos DB Azure veritabanı gibi DBaaS çözümleri yönetilir. | Uygulamalarınızı yeni özellikleri içerecek şekilde ya da bir bulut platformunda etkin şekilde çalışması için önemli düzeltmeler gerektiğinde.<br/><br/> Uygulama Yatırımlar kullanmak istediğinizde, ölçeklendirme gereksinimlerini karşılıyor, yenilikçi Azure DevOps yöntemler uygulamak ve sanal makinelerin kullanımını en aza.
**Yeniden oluşturma** | Azure bulut teknolojilerini kullanarak sıfırdan bir uygulama yeniden oluşturma adım bir şey daha fazla sürer yeniden oluşturun.<br/><br/> Örneğin, sunucusuz, Azure AI, yönetilen Azure SQL veritabanı örneği ve Azure Cosmos DB gibi bulut yerel teknolojileriyle yeşil alan uygulamalar oluşturabilir. | Ne zaman hızlı geliştirme istediğiniz ve mevcut uygulamalar işlevselliği ve kullanım ömrü sınırlı.<br/><br/> (Azure tarafından sağlanan DevOps uygulamalar dahil) iş yenilik hızlandırmak hazır olduğunuzda, bulut yerel teknolojilerini kullanarak yeni uygulamalar oluşturmak ve AI, blockchain ve IOT geliştirmeleri avantajlarından yararlanın.

## <a name="migration-articles"></a>Geçiş makaleleri

Contoso geçiş serisi oluşturan makaleleri aşağıdaki tabloda özetlenmiştir.  

- Karmaşıklık artırılması senaryolar verilmiştir ve biz onlara zamanla ekliyoruz.
- Her geçiş senaryosu geçiş stratejisi belirlemek biraz farklı iş hedeflerini tarafından yönetilir.
- Her dağıtım senaryosu için geçiş tamamlandıktan sonra geçiş öneri temizleme ve sonraki adımlar için gerçekleştirmek için iş sürücüleri ve hedefler, önerilen mimarisi, adımları hakkında bilgi sağlar.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
Makale 1: Genel Bakış (Bu makalede) | Contoso'nun geçiş stratejisi, makale serisi ve kullanılan örnek uygulamaları genel bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-overview.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md) | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Rehost Azure VM'ler ve SQL yönetilen örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM web uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği kullanarak bir uygulama veritabanına [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) hizmet. | Kullanılabilir
[Makale 5: Azure VM'ler için yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso SmartHotel uygulama Site Recovery hizmetini kullanarak sanal makineleri geçirmek nasıl gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. VM uygulama ve veritabanı geçiş hizmeti uygulama veritabanı bir SQL Server AlwaysOn Kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
[Makale 7: Azure VM'ler için Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Contoso VM'ler uygulama için Azure Site RECOVERY'yi kullanarak nasıl geçirir gösterir. | Planlandı
[Makale 8: Linux uygulama Azure VM'ler ve Azure MySQL sunucusu için yeniden barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso uygulaması Site RECOVERY'yi kullanarak VM nasıl geçirir gösterir ve MySQL çalışma ekranı Azure MySQL Server örneğine geçirmek için kullanır. | Planlandı



### <a name="demo-apps"></a>Tanıtım uygulamalarını

İki tanıtım uygulamalarını - SmartHotel ve osTicket makaleleri kullanın.

- **SmartHotel360**: Bu uygulamayı Azure ile çalışırken kullanabileceğiniz bir test uygulaması olarak Microsoft tarafından geliştirilmiştir. Açık kaynak olarak sağlanır ve buradan indirebilirsiniz [GitHub](https://github.com/Microsoft/SmartHotel360). Bir SQL Server veritabanına bağlanan bir ASP.NET uygulaması değil. Şu anda Windows Server 2008 R2 ve SQL Server 2008 R2 çalıştıran iki VMware VM'ler üzerinde uygulamasıdır. Uygulama VM'ler barındırılan şirket içi ve vCenter Server tarafından yönetilir.
- **osTicket**: Linux üzerinde çalışan bir uygulamanın raporlama bir açık kaynak hizmet Masası. Buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket). Geçerli uygulama Apache 2, PHP 7.0 ve MySQL 5.7 kullanarak Ubuntu 16.04LTS, çalışan iki VMware VM'ler üzerinde olur
    

## <a name="next-steps"></a>Sonraki adımlar

[Bilgi nasıl](contoso-migration-infrastructure.md) Contoso, kendi şirket içi ve Azure altyapı geçiş için hazırlanırken ayarlar.



