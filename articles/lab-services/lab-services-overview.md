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
ms.date: 04/19/2018
ms.author: spelluru
ms.openlocfilehash: b42183369b9ad88c77a05a91fdba8fe0efca2a8c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="an-introduction-to-azure-lab-services-formerly-azure-devtest-labs"></a>Azure Lab Services’e giriş (önceki adıyla Azure DevTest Labs)
Azure Lab Services, takımınız için bulutta hızlıca bir ortam ayarlamanızı sağlar (örneğin: geliştirme ortamı, test ortamı, sınıf laboratuvarı ortamı). Laboratuvar sahibi laboratuvarı oluşturur, Windows veya Linux sanal makineleri sağlar, gerekli yazılım ve araçları yükler ve laboratuvar kullanıcıları için kullanılabilir hale getirir. Laboratuvar kullanıcıları, laboratuvardaki sanal makinelere (VM) bağlanır ve bunları günlük işleri, kısa süreli projeleri ya da sınıf egzersizleri yapmak için kullanır. Kullanıcılar laboratuvardaki kaynakları kullanmaya başladıktan sonra, laboratuvar yöneticisi birden fazla laboratuvardaki maliyet ve kullanımı analiz edebilir ve kuruluşunuzun veya takımınızın maliyetlerini en iyi duruma getirmeye yönelik kapsayıcı ilkeler ayarlayabilir.

> [!IMPORTANT]
> Azure DevTest Labs, yeni laboratuvar türleri (Azure Lab Services) ile genişletiliyor! 
> 
> Azure Lab Services, sınıf laboratuvarları gibi yönetilen laboratuvarlar oluşturmanıza olanak tanır. Hizmet, yönetilen bir laboratuvar için, VM’leri tasarlamaktan hataları işlemeye ve altyapıyı ölçeklendirmeye varan tüm altyapı yönetimi konularını ele alır. Yönetilen laboratuvarlar şu an için önizleme aşamasındadır. Önizleme aşaması sona erdikten sonra, yeni laboratuvar türleri ve mevcut DevTest Labs, tüm laboratuvar türlerinin gelişmeye devam edeceği Azure Lab Services adı altında birleşecektir. 

## <a name="key-capabilities"></a>Temel işlevler
Azure Lab Services aşağıdaki temel özellikleri destekler: 

- **Hızlı ve esnek bir laboratuvar kurulumu**. Laboratuvar sahipleri Azure Lab Services’i kullanarak gereksinimlerine uygun bir laboratuvarı hızlıca ayarlayabilir. Hizmet, laboratuvar sahibinin aboneliği kapsamında yönetilen laboratuvarlara yönelik tüm Azure altyapı işlerini halletme veya laboratuvar yöneticilerinin altyapıyı kendi kendine yönetmesini ya da özelleştirmesini sağlama seçeneği sunar. Hizmet, sizin yerinize yönettiği laboratuvarlar için yerleşik ölçeklendirme ve esneklik özelliği sağlar. 
- **Laboratuvar kullanıcıları için basitleştirilmiş deneyim**. Sınıf laboratuvarı gibi yönetilen bir laboratuvarda laboratuvar kullanıcıları, bir kayıt kodu ile laboratuvara kaydolabilir ve laboratuvarın kaynaklarını kullanmak üzere diledikleri zaman laboratuvara erişebilir. DevTest Labs hizmetinde oluşturulan özel bir laboratuvarda laboratuvar sahibi, laboratuvar kullanıcılarına sanal makine oluşturma ve sanal makinelere erişme, veri disklerini yönetme ve yeniden kullanma ve yeniden kullanılabilir gizli diziler oluşturma izinleri verebilir.  
- **Maliyet iyileştirme ve analizi**. Laboratuvar sahibi, sanal makineleri otomatik olarak kapatmak ve başlatmak için laboratuvar zamanlamaları ayarlayabilir. Laboratuvar sahibi, laboratuvarın sanal makinelerine kullanıcılar tarafından erişilebildiğinde zaman dilimlerini belirlemek üzere bir zamanlama ayarlayabilir, maliyeti iyileştirmek için kullanıcı ya da laboratuvar başına kullanım ilkeleri belirleyebilir ve bir laboratuvardaki kullanım ve etkinlik eğilimlerini analiz edebilir. Sınıf laboratuvarları gibi yönetilen laboratuvarlar için şu anda daha küçük bir maliyet iyileştirme ve analiz seçenekleri alt kümesi mevcuttur. 
- **Yerleşik güvenlik**. Laboratuvar sahibi, özel bir sanal ağ ve bir laboratuvar alt ağı oluşturabilir ve paylaşılan bir genel IP adresini etkinleştirebilir. Laboratuvar kullanıcıları, ExpressRoute veya siteden siteye VPN ile yapılandırılan sanal ağı kullanarak kaynaklara güvenle erişebilir. (şu anda yalnızca DevTest Labs ile kullanılabilir)
- **İş akışlarınız ve araçlarınızla tümleştirme**. Azure Lab Services, laboratuvarları kuruluşunuzun web sitesi ve yönetim sistemleri ile tümleştirme olanağı sağlar. Ortamları sürekli tümleştirme/sürekli dağıtım (CI/CD) araçlarınızın içinden otomatik olarak sağlayabilirsiniz. (şu anda yalnızca DevTest Labs ile kullanılabilir)

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

## <a name="user-profiles"></a>Kullanıcı profilleri
Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır. 

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi
Genellikle, bir kuruluşun bulut kaynaklarının, aynı zamanda Azure aboneliğinin sahibi olan BT yöneticisi, laboratuvar hesap sahibi olarak hareket eder ve aşağıdaki görevleri yerine getirir:   

- Kuruluşunuz için bir laboratuvar hesabı ayarlar.
- Tüm laboratuvarlardaki ilkeleri yönetir ve yapılandırır.
- Kuruluştaki kişilere laboratuvar hesabı altında laboratuvar oluşturma izinleri verir.

### <a name="lab-creator"></a>Laboratuvar oluşturucu 
Genellikle, geliştirme sorumlusu/yöneticisi, öğretmen, hackathon konağı, çevrimiçi eğitmen gibi kullanıcılar laboratuvar hesabı altında laboratuvarlar oluşturur. Laboratuvar oluşturucusu aşağıdaki görevleri yerine getirir: 

- Laboratuvar oluşturur.
- Laboratuvarda sanal makineler oluşturur. 
- Sanal makinelere uygun yazılımları yükler.
- Laboratuvara kimlerin erişebileceğini belirtir.
- Laboratuvar kullanıcılarına laboratuvar bağlantısı sağlar.

### <a name="lab-user"></a>Laboratuvar kullanıcısı
Laboratuvar kullanıcısı aşağıdaki görevleri yerine getirir:

- Laboratuvar kullanıcısının laboratuvara kaydolmak için laboratuvar oluşturucusundan aldığı kayıt bağlantısını kullanır. 
- Laboratuvardaki bir sanal makineye bağlanır ve geliştirme, test veya sınıf çalışmalarını gerçekleştirmek için kullanır. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)
- [Özel bir laboratuvarı ayarlama](tutorial-create-custom-lab.md)
