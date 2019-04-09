---
title: Azure Lab Services Hakkında | Microsoft Docs
description: Lab Services’in geliştiriciler, test uzmanları, eğitimciler, öğrenciler ve diğerleri tarafından kullanılabilen sanal makinelerle laboratuvar oluşturma, yönetme ve güvenli hale getirmeyi nasıl kolaylaştırabildiğini öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: a4ca5cba924a3269f279469f26e68acdb0ad0659
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59257629"
---
# <a name="an-introduction-to-azure-lab-services"></a>Azure Lab Services’a giriş
Azure Lab Services, takımınız için bulutta hızlıca bir ortam ayarlamanızı sağlar (örneğin: geliştirme ortamı, test ortamı, sınıf laboratuvarı ortamı). Laboratuvar sahibi laboratuvarı oluşturur, Windows veya Linux sanal makineleri sağlar, gerekli yazılım ve araçları yükler ve laboratuvar kullanıcıları için kullanılabilir hale getirir. Laboratuvar kullanıcıları, laboratuvardaki sanal makinelere (VM) bağlanır ve bunları günlük işleri, kısa süreli projeleri ya da sınıf egzersizleri yapmak için kullanır. Kullanıcılar laboratuvardaki kaynakları kullanmaya başladıktan sonra, laboratuvar yöneticisi birden fazla laboratuvardaki maliyet ve kullanımı analiz edebilir ve kuruluşunuzun veya takımınızın maliyetlerini en iyi duruma getirmeye yönelik kapsayıcı ilkeler ayarlayabilir.

> [!IMPORTANT]
> **Azure DevTest Labs**, yeni laboratuvar türleri (Azure Lab Services) ile genişletiliyor!
>  
> Azure Lab Services sınıf laboratuvarlarını gibi yönetilen Laboratuvar türlerini oluşturmanıza olanak sağlar. Hizmet işleme hataları Vm'leri dönen ve altyapısını ölçeklendirme ek olarak, yönetilen Laboratuvar türü için tüm altyapı yönetimini işler. Şimdilik [DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) ve [Azure Lab Services](https://azure.microsoft.com/services/lab-services/) farklı hizmetler Azure Portalı'nda olmaya devam edecektir. 

## <a name="key-capabilities"></a>Temel işlevler

Azure Lab Services aşağıdaki temel özellikleri destekler:

- **Hızlı ve esnek bir laboratuvar kurulumu**. Laboratuvar sahipleri Azure Lab Services’i kullanarak gereksinimlerine uygun bir laboratuvarı hızlıca ayarlayabilir. Hizmet ilgileniriz yönetilen Laboratuvar türleri için tüm Azure altyapı iş ya da kendi kendine yönetmek ve Laboratuvar sahibinin abonelik altyapısında özelleştirmek Laboratuvar sahibini etkinleştirmek için seçenek sunar. Hizmet, sizin yerinize yönettiği laboratuvarlar için yerleşik ölçeklendirme ve esneklik özelliği sağlar.
- **Laboratuvar kullanıcıları için basitleştirilmiş deneyim**. Bir sınıf laboratuvarına gibi bir yönetilen Laboratuvar türündeki Laboratuvar kullanıcılar kayıt kodunu içeren bir laboratuvar için kaydolun ve Laboratuvar kaynaklarını kullanmak üzere Laboratuvar dilediğiniz zaman erişin. DevTest Labs hizmetinde oluşturulan bir laboratuvarda laboratuvar sahibi, laboratuvar kullanıcılarına sanal makine oluşturma ve sanal makinelere erişme, veri disklerini yönetme ve yeniden kullanma ve yeniden kullanılabilir gizli diziler ayarlama izinleri verebilir.  
- **Maliyet iyileştirme ve analizi**. Laboratuvar sahibi, sanal makineleri otomatik olarak kapatmak ve başlatmak için laboratuvar zamanlamaları ayarlayabilir. Laboratuvar sahibi, laboratuvarın sanal makinelerine kullanıcılar tarafından erişilebildiğinde zaman dilimlerini belirlemek üzere bir zamanlama ayarlayabilir, maliyeti iyileştirmek için kullanıcı ya da laboratuvar başına kullanım ilkeleri belirleyebilir ve bir laboratuvardaki kullanım ve etkinlik eğilimlerini analiz edebilir. Sınıf laboratuvarlarını gibi yönetilen Laboratuvar türleri için şu anda küçük bir alt kümesini maliyet iyileştirmesi ve çözümleme seçenekleri kullanılabilir.
- **Yerleşik güvenlik**. Laboratuvar sahibi, özel bir sanal ağ ve bir laboratuvar alt ağı oluşturabilir ve paylaşılan bir genel IP adresini etkinleştirebilir. Laboratuvar kullanıcıları, ExpressRoute veya siteden siteye VPN ile yapılandırılan sanal ağı kullanarak kaynaklara güvenle erişebilir. (şu anda yalnızca DevTest Labs ile kullanılabilir)
- **İş akışlarınız ve araçlarınızla tümleştirme**. Azure Lab Services, laboratuvarları kuruluşunuzun web sitesi ve yönetim sistemleri ile tümleştirme olanağı sağlar. Ortamları sürekli tümleştirme/sürekli dağıtım (CI/CD) araçlarınızın içinden otomatik olarak sağlayabilirsiniz. (şu anda yalnızca DevTest Labs ile kullanılabilir)

> [!NOTE]
> Şu anda Azure Lab Services yalnızca Azure Market görüntülerinden oluşturulan sanal makineleri destekler. Özel görüntüler kullanmak veya laboratuvar ortamında başka PaaS kaynakları oluşturmak istiyorsanız, DevTest Labs kullanın. Daha fazla bilgi için bkz. [DevTest Labs'de özel görüntü oluşturma](devtest-lab-create-custom-image-from-vm-using-portal.md) ve [Resource Manager şablonlarını kullanarak laboratuvar ortamları oluşturma](devtest-lab-create-environment-from-arm.md).

## <a name="scenarios"></a>Senaryolar

Azure Lab Services’in desteklediği senaryolardan bazıları şunlardır:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Bulutta sınıfınız için yeniden boyutlandırılabilen bir bilgisayar laboratuvarı oluşturma  

- Yönetilen bir sınıf laboratuvarı oluşturun. Hizmete tam olarak ihtiyacınız olanları söylediğinizde laboratuvarın altyapısını sizin için oluşturup yönetir, böylece siz de laboratuvarın ayrıntılarına değil, ders vermeye odaklanabilirsiniz.
- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, sınıf çalışmaları için VM’leri kullanabilecekleri sınırlı sayıda saat verin.  
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.
- İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin.

### <a name="use-devtest-labs-for-development-environments"></a>Geliştirme ortamları için DevTest Labs kullanma

Azure DevTest Labs, çok sayıda önemli senaryoyu uygulamak için kullanılabilir ancak başlıca senaryolardan biri, geliştiriciler için geliştirme makinelerini barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Geliştiricilerin isteğe bağlı olarak geliştirme makinelerini hızlıca sağlaması.
- Yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamları sağlama
- Geliştiriciler, geliştirme makinelerini gerekli olan her durumda özelleştirebilir.
- Yöneticiler, geliştiricilerin geliştirme için gerektiğinde daha fazla VM alamamasını ve kullanımda olmadığında VM’lerin kapanmasını sağlayarak maliyetleri kontrol edebilir.

Daha fazla bilgi için bkz. [Geliştirme için DevTest Labs kullanma](devtest-lab-developer-lab.md).

### <a name="use-devtest-labs-for-test-environments"></a>Test ortamları için DevTest Labs kullanma

Azure DevTest Labs’i çok sayıda önemli senaryoyu uygulamak için kullanabilirsiniz ancak başlıca senaryolardan biri, test uzmanları için makineleri barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Test uzmanları, yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayabilir ve bu sayede uygulamalarının en son sürümünü test edebilir.
- Test uzmanları birden fazla test aracısı sağlayarak yük testlerinin ölçeğini artırabilirler.
- Yöneticiler, test uzmanlarının test için gerektiğinde daha fazla VM alamamasını ve kullanımda olmadığında VM’lerin kapanmasını sağlayarak maliyetleri kontrol edebilir.

Daha fazla bilgi için bkz. [Test için DevTest Labs kullanma](devtest-lab-test-env.md).

## <a name="types-of-labs"></a>Labs türleri
İki tür labs oluşturabilirsiniz: **yönetilen Laboratuvar türlerini** Azure Lab Services ile ve **labs** Azure Lab Services ile. Yalnızca, bir laboratuar ortamında gerekir ve ayarlama ve Laboratuvar için gerekli altyapıyı yönetmek service gerisini halleder girmek isterseniz birini **yönetilen Laboratuvar türlerini**. Şu anda **sınıf laboratuvarı** Azure Lab Services ile oluşturabilirsiniz yalnızca yönetilen Laboratuvar türüdür. Kendi altyapınızı yönetmek istiyorsanız, kullanarak Laboratuvar oluşturma **Azure DevTest Labs**.

Aşağıdaki bölümlerde bu laboratuvarlar hakkında daha ayrıntılı bilgi verilmektedir. 

## <a name="managed-lab-types"></a>Yönetilen laboratuvar türleri
Azure Lab Services, altyapısı Azure tarafından yönetilen laboratuvarlar oluşturmanızı sağlar. Bu makalede, bunları yönetilen Laboratuvar türleri olarak ifade eder. Laboratuvar türlerini teklif farklı türleri için belirli gereksinimlerinize uyan Laboratuvar yönetilen. Şu anda yönetilen desteklenen Laboratuvar türü okunur **sınıf laboratuvarı**. 

Yönetilen Laboratuvar türlerini, minimal kurulumu ile hemen çalışmaya başlamanız için etkinleştirin. Hizmet, VM’leri tasarlamaktan hataları işlemeye ve altyapıyı ölçeklendirmeye varan tüm laboratuvar altyapısı yönetimi konularını ele alır. Bir yönetilen Laboratuvar türü gibi bir sınıf laboratuvarı oluşturmak için ilk olarak, kuruluşunuz için bir laboratuvar hesabı oluşturmanız gerekir. Laboratuvar hesabı, kuruluştaki tüm laboratuvarların yönetildiği merkezi hesap olarak görev yapar. 

Oluşturma ve Azure kaynakları bu yönetilen Laboratuvar türlerini kullandığınızda, hizmet oluşturur ve dahili Microsoft aboneliği kaynakları yönetir. Bunlar sizin Azure aboneliğinizde oluşturulmaz. Hizmet bu kaynakların dahili Microsoft aboneliklerindeki kullanımını takip eder. Bu kullanım, laboratuvar hesabını içeren Azure aboneliğinize faturalanır.   

Bazıları **kullanım örnekleri için yönetilen Laboratuvar türlerini**: 

- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, ev ödevi veya kişisel projeleri için VM’leri kullanabilecekleri sınırlı sayıda saat verin.
- İşlem yoğunluklu veya grafik yoğunluklu araştırmalar gerçekleştirmek üzere yüksek performanslı işlem VM’leri içeren bir havuz oluşturun. Gerektiğinde VM’leri çalıştırın ve işiniz bittikten sonra makineleri temizleyin. 
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.  
- Bir hackathon barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 


## <a name="devtest-labs"></a>DevTest Labs
Tüm altyapıyı ve yapılandırmayı kendi başınıza, kendi aboneliğiniz içinde yönetmek istediğiniz senaryolar olabilir. Bunu yapmak için, Azure portalda Azure DevTest Labs ile bir laboratuvar oluşturabilirsiniz. Bu laboratuvarlar için bir laboratuvar hesabı oluşturmanız gerekmez. Bu Laboratuvar, laboratuvarı hesabındaki (yönetilen Laboratuvar türleri için var olan) gösterilmez.  

**DevTest Labs kullanmaya ilişkin kullanım örneklerinden** bazıları aşağıda verilmiştir: 

- Bir hackathon veya bir konferansta uygulamalı oturum barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 
- Uygulamanızla yapılandırılan bir VM havuzu oluşturun ve takımınızın yoğun hata ayıklama için bir sanal makineyi kolayca kullanmasını sağlayın.  
- Geliştiricilere, ihtiyaç duydukları tüm araçlarla yapılandırılmış sanal makineler sağlayın. Maliyeti en aza indirmek için otomatik başlatma ve kapatma zamanlayın. 
- Dağıtımınızın bir parçası olarak tekrarlanan şekilde bir test makineleri laboratuvarı oluşturun. En son bitleri test edin ve işiniz bittiğinde test makinelerini temizleyin. 
- Farklı şekilde yapılandırılmış çeşitli sanal makineler ve ölçeklendirme ile performans testi için birden fazla test aracısı ayarlayın. 
- Ürününüzün en son sürümü ile yapılandırılmış bir laboratuvar kullanarak müşterilerinize eğitim oturumları sunun. Her müşteriye laboratuvarda kullanmak üzere sınırlı sayıda saat verin. 


## <a name="managed-lab-types-vs-devtest-labs"></a>Yönetilen laboratuvar türleri ve DevTest Labs
Aşağıdaki tabloda Azure Lab Services tarafından desteklenen iki laboratuvar türü karşılaştırılmaktadır: 

| Özellikler | Yönetilen laboratuvar türleri | DevTest Labs |
| -------- | ----------------- | ---------- |
| Laboratuvarda Azure altyapısı yönetimi. |  Hizmet tarafından otomatik olarak yönetilir | Kendi başınıza yönetirsiniz  |
| Altyapı sorunlarında yerleşik esneklik | Hizmet tarafından otomatik olarak gerçekleştirilir | Kendi başınıza yönetirsiniz  |
| Abonelik yönetimi | Hizmet, hizmeti destekleyen Microsoft abonelikleri içinde kaynak ayırmayı gerçekleştirir. Ölçeklendirme hizmet tarafından otomatik olarak gerçekleştirilir. | Kendi Azure aboneliğinizde kendi başınıza yönetirsiniz. Hiçbir otomatik ölçeklendirme abonelikler. |
| Laboratuvar içinde Azure Resource Manager dağıtımı | Kullanılamaz | Kullanılabilir |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın: 

- [Sınıf Laboratuvarları Hakkında](./classroom-labs/classroom-labs-overview.md)
- [DevTest Laboratuvarları Hakkında](devtest-lab-overview.md)
