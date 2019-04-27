---
title: Azure DevTest Labs altyapı İdaresi
description: Bu makalede, Azure DevTest Labs altyapısının İdaresi için yönergeler sağlar.
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
ms.openlocfilehash: c5514a43602106cf045b575d289e02b591468359
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561660"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---resources"></a>Azure DevTest Labs altyapı - kaynak İdaresi
Bu makalede, hizalama ve kaynakları, kuruluşunuzdaki Yönetim için DevTest Labs yöneliktir. 

## <a name="align-within-an-azure-subscription"></a>Bir Azure aboneliğinde Hizala 

### <a name="question"></a>Soru
Bir Azure aboneliğinde DevTest Labs kaynakları nasıl hizalama?

### <a name="answer"></a>Yanıt
Genel uygulama geliştirme için Azure'u kullanmak bir kuruluş başlamadan önce BT planlayıcıları ilk özelliği genel Portföyüne hizmetlerin bir parçası olarak konusunda gözden geçirmelidir. Gözden geçirme için alanları aşağıdaki sorunlar ele almalıdır:

- Uygulama geliştirme yaşam döngüsüyle ilişkili maliyet ölçmek nasıl?
- Kuruluş, önerilen hizmet şirket güvenlik ilkesiyle teklifini nasıl hizalama? 
- Segmentlere ayırma geliştirme ve üretim ortamlarını ayırmak için gerekli mi? 
- Hangi denetimlerin, uzun vadeli kolaylığı yönetimi, kararlılık ve büyüme için sunulan?

**İlk yöntem önerilen** kuruluşun Azure sınıflandırma üretim ve geliştirme abonelikler arasında bölümler Burada özetlenen incelemektir. Aşağıdaki diyagramda, önerilen Sınıflandırma, geliştirme/test ve üretim ortamları için bir mantıksal ayrım sağlar. Bu yaklaşımda, bir kuruluş, ayrı ayrı her ortamla ilişkilendirilmiş maliyetleri izlemek için fatura kodları ortaya çıkarabilir. Daha fazla bilgi için [öngörücü abonelik İdaresi](/azure/architecture/cloud-adoption/appendix/azure-scaffold). Ayrıca, kullanabileceğiniz [Azure etiketleri](../azure-resource-manager/resource-group-using-tags.md) izleme ve faturalandırma için kaynakları düzenlemek için.

**İkinci önerilen uygulama** Azure Enterprise portal geliştirme ve test aboneliğinde etkinleştirmektir. Azure Kurumsal aboneliğinin genellikle kullanılabilir olmayan bir istemci işletim sistemlerini çalıştıran bir kuruluşun sağlar. Ardından, burada yalnızca işlem için ödeme yaparsınız ve lisans hakkında endişelenmeyin değil Kurumsal Yazılım kullanın. Microsoft SQL Server gibi Iaas galeri görüntüleri dahil olmak üzere belirlenmiş hizmetler faturalandırması yalnızca tüketimini temel alarak, sağlar. Azure geliştirme ve test aboneliği hakkında daha fazla bilgi bulunabilir [burada](https://azure.microsoft.com/offers/ms-azr-0148p/) Kurumsal Sözleşme (EA) müşterileri için ve [burada](https://azure.microsoft.com/offers/ms-azr-0023p/) Kullandıkça Öde müşterileri için.

![Abonelikler ile kaynak hizalama](./media/devtest-lab-guidance-governance/resource-alignment-with-subscriptions.png)

Bu model, bir kuruluş Azure DevTest Labs uygun ölçekte dağıtma esnekliği sağlar. Bir kuruluş, paralel olarak çalışan 100 ila 1000 arasında sanal makineler ile çeşitli Departmanlar için labs yüzlerce destekleyebilir. Bu, aynı ilkeler yapılandırma yönetimi ve güvenlik denetimlerinin paylaşabileceğiniz bir merkezi Kurumsal Laboratuvar çözümü kavramı yükseltir.

Bu model, ayrıca kuruluş Azure abonelikleriyle ilişkili kaynak sınırlarının harcadıklarını değil sağlar. Abonelik ve hizmet sınırlamaları hakkında daha fazla ayrıntı için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md). Sağlama işlemi DevTest Labs, çok sayıda kaynak grupları kullanabilir. Azure DevTest abonelik destek isteği aracılığıyla artırılacak sınırları isteyebilir. Geliştirme abonelik kullanım arttıkça üretim abonelik içindeki kaynaklara etkilenmez. DevTest Labs ölçeklendirme hakkında daha fazla bilgi için bkz. [DevTest Labs'de kotalar ve sınırlar ölçeklendirme](devtest-lab-scale-lab.md).

Hazır olunması gereken bir ortak abonelik düzeyi ağ IP aralığı atamalarını hem üretim hem de geliştirme abonelikleri desteklemek için nasıl ayrılacağını sınırdır. Bu atamaları büyüme (şirket içi bağlantı ya da Azure'nın uygulama için varsayılan olarak ayarlanıyor yerine, ağ yığınını yönetmek için Kurumsal gerektiren başka bir ağ topolojisi varsayılarak) zaman içinde hesap. Atanan ve çok büyük alt ağlar ile ayrılmış büyük bir IP adres öneki olan birkaç sanal ağlara sahip olmasını yerine küçük bir alt ağlar ile birden çok sanal ağ olması önerilir. Örneğin, 10 abonelikleriyle, 10 sanal ağlar (her abonelik için bir tane) tanımlayabilirsiniz. Aynı alt ağda aboneliğe ait sanal ağ yalıtımı gerektirmeyen tüm labs paylaşabilirsiniz.

## <a name="maintain-naming-conventions"></a>Adlandırma kuralları

### <a name="question"></a>Soru
DevTest Labs ortamımın nasıl bir adlandırma kuralı korunsun mu?

### <a name="answer"></a>Yanıt
Azure işlemleri için geçerli Kurumsal adlandırma kuralları'nı genişletin ve DevTest Labs'i ortam genelinde tutarlı hale getirmek isteyebilirsiniz.

DevTest Labs dağıtırken, belirli bir başlangıç ilkeleri sahip olmasını öneririz. Merkezi bir betiği ve JSON şablonları tutarlılığı zorlamak için bu ilkeleri dağıtırsınız. Abonelik düzeyinde mi uygulanacağına Azure ilkeleri aracılığıyla adlandırma ilkeleri uygulanabilir. Azure İlkesi JSON örneği için bkz. [Azure ilkesi örnekleri](../governance/policy/samples/index.md).

## <a name="number-of-users-per-lab-and-labs-per-organization"></a>Laboratuvar ve kuruluş labs'te başına kullanıcı sayısı

### <a name="question"></a>Soru 
Laboratuvar ve bir kuruluş genelindeki gerekli labs genel sayısı başına kullanıcı sayısının oranı nasıl belirlerim?

### <a name="answer"></a>Yanıt
İş birimleri ve aynı geliştirme proje ile ilişkili olan geliştirme gruplar aynı lab ile ilişkili olduğunu öneririz. Her iki grubuna uygulanacak ilkeler, görüntüleri ve kapatma ilkeleri aynı türde sağlar. 

Coğrafi sınırlar dikkate almanız gerekebilir. Örneğin, Kuzey Doğu'daki geliştiriciler, Doğu ABD 2 sağlanan bir laboratuvar Amerika Birleşik Devletleri (ABD) kullanabilir. Ve bir kaynak Güney Orta ABD kullanmak için Texas, Dallas, Denver Colorado geliştiriciler yönlendirilebilir. Dış bir üçüncü taraf ile ortak çabaya ise iç geliştiriciler tarafından kullanılmayan bir laboratuvar atanabilir. 

Azure DevOps projeleri içinde belirli bir proje için bir laboratuvar de kullanabilirsiniz. Ardından, güvenlik kaynakları her iki kümesine erişim sağlayan belirtilen Azure Active Directory grup olarak uygulanır. Laboratuvara atanan sanal ağ, kullanıcılar birleştirmek için başka bir sınır olabilir.

## <a name="deletion-of-resources"></a>Kaynakları silme

### <a name="question"></a>Soru
Nasıl biz bir laboratuvar içindeki kaynakların silinmesini engelleyebilir miyim?

### <a name="answer"></a>Yanıt
Böylece yalnızca yetkili kullanıcıların kaynakları silmeniz veya Laboratuvar ilkeleri değiştirme Laboratuvar düzeyinde uygun izinleri ayarlamanızı öneririz. Geliştiriciler, içinde yerleştirilmelidir **DevTest Labs kullanıcılar** grubu. Baş geliştirici ya da altyapı sağlama olmalıdır **DevTest Labs sahibi**. Yalnızca iki Laboratuvar sahibi olmasını öneririz. Bu ilke bozulmalarını önlemek için kod deposu genişletir. Laboratuvar kullandığı kaynakları kullanma haklarına sahip ancak Laboratuvar ilkeleri güncelleştirilemiyor. Rolleri ve yerleşik her grup Laboratuvar sahip hakları listeleyen şu makaleye bakın: [Azure DevTest Labs'de sahibini ve kullanıcıları ekleme](devtest-lab-add-devtest-user.md).

## <a name="move-lab-to-another-resource-group"></a>Laboratuvar başka bir kaynak grubuna taşıma 

### <a name="question"></a>Soru
Bir laboratuvar başka bir kaynak grubuna taşımak için destekleniyor mu?

### <a name="answer"></a>Yanıt
Evet. Laboratuvarınız için giriş sayfasından kaynak grubu sayfasına gidin. Ardından, **taşıma** araç ve farklı kaynak grubuna taşımak istediğiniz Laboratuvar seçin. Bir laboratuvar oluşturduğunuzda, otomatik olarak bir kaynak grubu oluşturulur. Ancak, Laboratuvar Kurumsal adlandırma kurallarına farklı kaynak grubuna taşımak isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [maliyet ve sahiplik yönetme](devtest-lab-guidance-governance-cost-ownership.md).
