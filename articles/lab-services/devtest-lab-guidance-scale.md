---
title: Azure DevTest Labs altyapınızın ölçeğini
description: Bu makalede, Azure DevTest Labs altyapınızı ölçeklendirmeye yönelik rehberlik sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 25a088686c739c53feadd6354baf75f3147bdc33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561498"
---
# <a name="scale-up-your-azure-devtest-labs-infrastructure"></a>Azure DevTest Labs altyapınızın ölçeğini
DevTest Labs Kurumsal ölçekte uygulamadan önce birkaç temel karar noktaları vardır. Bu karar noktaları yüksek bir düzeyde anlamak, tasarım kararlarını gelecekte bir kuruluşla yardımcı olur. Ancak, bu noktaları geri bir kavram kanıtı başlatılmasını bir kuruluş tutmak zorunda değil. Ölçek büyütme ilk planlama için ilk üç alanlar şunlardır:

- Ağ ve güvenlik
- Abonelik topolojisi
- Rolleri ve sorumlulukları

## <a name="networking-and-security"></a>Ağ ve güvenlik
Tüm kuruluşlar için temel ağ ve güvenlik var. Bir Kurumsal Çapta dağıtımını çok daha ayrıntılı analiz gerekirken, azaltılmış birkaç kavram kanıtı başarıyla gerçekleştirmek için gereksinimleri vardır. Odak birkaç önemli alanlar şunlardır:

- **Azure aboneliği** – DevTest Labs, dağıtılacak kaynaklar oluşturmak için uygun haklara sahip bir Azure aboneliğine erişiminiz olması gerekir. Bir Kurumsal Anlaşma ve Kullandıkça Öde Azure abonelikleri erişim elde etmek için çeşitli yollarla vardır. Bir Azure aboneliğine erişim elde etmeye daha fazla bilgi için bkz: [Azure için Kurumsal Lisans](https://azure.microsoft.com/pricing/enterprise-agreement/).
- **Şirket kaynaklarına erişimi** – bazı kuruluşların kaynaklarını DevTest labs'deki sahip şirket içi kaynaklara erişim gerektirir. Azure, şirket içi ortamınızdan güvenli bir bağlantı gereklidir. Bu nedenle, ayarladığınız önemli olduğu yukarı/başlama önce bir VPN veya Express Route bağlantı yapılandırın. Daha fazla bilgi için [sanal ağlarına genel bakış](../virtual-network/virtual-networks-overview.md).
- **Ek güvenlik gereksinimleri** – diğer güvenlik gereksinimleriyle makine ilkeler, genel IP adresleri, internet'e bağlanmak için erişim gibi bir kavram kanıtı uygulamadan önce gözden geçirilmesi gereken senaryolardır. 

## <a name="subscription-topology"></a>Abonelik topolojisi
Abonelik bir önemli tasarım husus DevTest Labs kuruluşa dağıtırken topolojidir. Ancak, bu kavram kanıtını tamamladıktan sonra kadar tüm kararları pekiştirilmesine için gerekli değildir. Bir kurumsal uygulama için gerekli olan abonelik sayısı değerlendirme sırasında iki uç nokta vardır: 

- Tüm kuruluş için bir abonelik
- Abonelik kullanıcı başına

Ardından, her yaklaşımın olumlu vurgulayın.

### <a name="one-subscription"></a>Bir abonelik
Genellikle bir abonelik yaklaşım büyük bir kuruluşta yönetilebilir değil. Ancak, abonelik sayısını sınırlama aşağıdaki avantajları sağlar:

- **Tahmin** Kurumsal maliyetlerini.  Tüm kaynakları tek bir havuzda olduğundan ödemeyle tek bir abonelikte çok daha kolay olur. Bu yaklaşım, verilen herhangi bir zamanda bir fatura dönemi içinde maliyet denetimi kullanmak için ölçer, daha basit üzerinde kararı sağlar.
- **Yönetilebilirlik** birçok farklı abonelikler arasında güncelleştirmeler yapma aksine, bir Abonelikteki tüm güncelleştirmeleri yalnızca gerekli olacağından VM'ler, yapıtlar, formüller, ağ yapılandırması, izinler, ilkeleri vb. daha kolay olur.
- **Ağ** çaba büyük ölçüde kuruluşlar şirket içi bağlantı, bir gereksinim olduğu için tek bir abonelikte basitleştirilmiştir. Sanal ağları bağlama (merkez-uç model) abonelikler arasında ek yapılandırma, yönetim, IP adresi alanları, vb. gerektiren ek aboneliklerle gerekli değildir.
- **Ekip işbirliği** daha kolay olduğunda herkesin aynı abonelikte çalışma-Örneğin, bir iş arkadaşınız bir VM'ye yeniden atama, paylaşma vb. takım kaynaklarını daha kolay olur.

### <a name="subscription-per-user"></a>Abonelik kullanıcı başına
Kullanıcı başına ayrı bir abonelik alternatif spektrumun için eşit fırsat sağlar. Sahip birden fazla aboneliğin avantajları şunlardır:

- **Azure kotaları ölçeklendirme** benimseme engelleyebildiğinden gidip değil. Azure aboneliği başına 200 depolama hesabı sağlar. Örneğin, bu makalenin yazıldığı tarih itibarıyla. Çoğu Azure Hizmetleri için işletimsel kotası (çoğu özelleştirilebilir, bazıları olamaz). Bu abonelik kullanıcı başına modelde çoğu kotalar ulaştığını olasılığı düşüktür. Geçerli Azure ölçekleme kotaları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- **Yansıtma** grupları veya bireysel geliştiriciler kuruluşların kendi geçerli modelini kullanarak maliyetleri için hesap izin vermek çok daha kolay hale gelir.
- **Sahipliği & izinleri** DevTest Labs ortamları basittir. Geliştiricilere abonelik düzeyinde erişim sağlar ve bunlar %100 ağ yapılandırması, Laboratuvar ilkeleri ve VM yönetimi dahil olmak üzere her şey için sorumlu.

Kuruluşta spektrumun uç yeterli kısıtlamaları olabilir. Bu nedenle, bu uç ortasında denk gelen bir yolla abonelikler gerekebilir. En iyi uygulama, bir kuruluşun hedef abonelikler en az sayıda olası göz önünde toplam abonelik sayısını artırmak zorlayıcı işlevleri bulundurarak olarak kullanılacak olmalıdır. Yinelemek için abonelik topolojisi DevTest Labs bir kurumsal dağıtım için kritik öneme sahiptir, ancak bir kavram kanıtı gecikmeden. Ek ayrıntılar vardır [idare](devtest-lab-guidance-governance-policy-compliance.md) ilişkin abonelik ve Laboratuvar ayrıntı düzeyi kuruluştaki karar verin.

## <a name="roles-and-responsibilities"></a>Rolleri ve sorumlulukları
Bir kavram kanıtı DevTest Labs ile tanımlanan sorumlulukları – abonelik sahibi, DevTest Labs sahibi, DevTest Labs kullanıcısı ve isteğe bağlı olarak katkıda bulunan üç birincil rol yok.

- **Abonelik sahibi** – abonelik sahibi Azure kullanıcı atama, ilkelerini yönetme, oluşturma ve ağ topolojisini yönetme de dahil olmak üzere aboneliği isteyen kota artırma işlemleri, vb. yönetme izni vardır. Daha fazla bilgi için [bu makaleye](../role-based-access-control/rbac-and-directory-admin-roles.md) bakın.
- **DevTest Labs sahibi** – DevTest Labs sahibi Laboratuvar tam yönetim erişimi yoktur. Bu kişi maliyet ayarları, genel Laboratuvar ayarları ve diğer VM/yapıt tabanlı görevleri yönetme ekleme/kullanıcıları kaldırmak için sorumlu değildir. Laboratuvar sahibi ayrıca DevTest Labs kullanıcısı tüm haklarına sahiptir.
- **DevTest Labs kullanıcısı** – DevTest Labs kullanıcısı oluşturun ve laboratuvarındaki sanal makinelerde kullanma. Bu kişiler, oluşturdukları Vm'lerde bazı en az bir yönetici yetkileri sahip (Başlat/Durdur/delete/yapılandırma Vm'lerini). Kullanıcılar, diğer kullanıcıların sanal makineleri yönetemez.

## <a name="next-steps"></a>Sonraki adımlar
Bu serideki sonraki makaleye bakın: [Azure DevTest Labs uygulamasını düzenleyin](devtest-lab-guidance-orchestrate-implementation.md)