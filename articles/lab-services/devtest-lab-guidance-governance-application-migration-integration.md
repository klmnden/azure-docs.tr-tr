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
ms.openlocfilehash: 75ce5d6a88b5398bd010cc363b4241bc90068f55
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60193008"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---application-migration-and-integration"></a>Azure DevTest Labs altyapı - uygulama geçiş ve tümleştirme İdaresi
Geliştirme/test Laboratuvar ortamınız oluşturulduktan sonra aşağıdaki sorular hakkında düşünmek gerekir:

- Nasıl ortamı içinde proje ekibinizin yazılımınız?
- Nasıl gerekli Kurumsal ilkelerle izleyin ve değer uygulamanıza eklemek için çevikliği korumak emin olabilirim?

## <a name="azure-marketplace-images-vs-custom-images"></a>Azure Market görüntüleri özel görüntüler karşılaştırması

### <a name="question"></a>Soru
Azure Market görüntüsü kendi özel kuruluş görüntü veya ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Azure Market, belirli kaygıları veya kuruluş gereksinimlerine sahip olmadığınız sürece varsayılan olarak kullanılmalıdır. Ortak örnek verilebilir;

- Bir uygulamayı temel görüntüsü bir parçası olarak dahil olacak şekilde gerektiren karmaşık yazılım kurulumu.
- Yükleme ve Kurulum bir uygulamanın Azure Market görüntüsü üzerinde eklenecek işlem süresi verimli bir kullanım olmayan saatler sürebilir.
- Geliştiriciler ve test ediciler bir sanal makineyi hızlı bir şekilde erişmesi ve yeni bir sanal makine hazırlık süresini en aza indirmek istiyorsunuz.
- Uyumluluk veya tüm makineler için yapılması gerekenleri yasal koşullar (örneğin, güvenlik ilkeleri).

Özel görüntüleri kullanarak hafifçe kabul edilmemelidir. Şimdi bu temel alınan temel görüntüleri için VHD dosyaları yönetmek sahip oldukları fazladan karmaşıklık tanıtır. Düzenli olarak bu temel görüntüleri yazılım güncelleştirmeleri ile düzeltmeniz gerekir. Bu güncelleştirmeler, yeni işletim sistemi (OS) güncelleştirmeler ve güncelleştirmeleri ya da yazılım paketi kendisi için gerekli yapılandırma değişikliklerini içerir.

## <a name="formula-vs-custom-image"></a>Özel görüntü veya formül

### <a name="question"></a>Soru
Özel görüntü veya formül ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Genellikle, bu senaryoda faktör, ekonomik ve yeniden kullanabilirsiniz.

Burada birçok kullanıcıları/labs temel görüntünün üstüne yazılım çok sayıda görüntü gerektiren bir senaryo varsa, özel bir görüntü oluşturarak maliyetini azaltabilir. Başka bir deyişle, görüntü bir kez oluşturulur. Bu sanal makine ve Kurulum meydana geldiğinde çalışan sanal makinenin nedeniyle sonucunda oluşan maliyet hazırlık süresini azaltır.

Ancak, ek bir faktörü unutmayın yazılım değişiklikleri sıklığıdır. Çalıştırırsanız günlük oluşturur ve kullanıcılarınızın sanal makinelerde gereken yazılım bir formül yerine özel bir görüntü kullanarak geçirmeniz gerekir.

## <a name="use-custom-organizational-images"></a>Kuruluş özel görüntülerinizi kullanın

### <a name="question"></a>Soru
DevTest Labs ortamına özel kuruluş görüntülerim getirmek için bir kolayca yinelenebilir işlemini nasıl ayarlayabilirim?

### <a name="answer"></a>Yanıt
Bkz: [bu video görüntü Fabrika desenini](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/). Bu senaryo Gelişmiş bir senaryodur ve sağlanan örnek betikler yalnızca betiklerdir. Herhangi bir değişikliğe ihtiyaç olup, yönetmek ve ortamınızda kullanılan komut dosyaları korumak gerekir.

Azure işlem hatlarında özel resim bir işlem hattı oluşturmak için DevTest Labs'i kullanarak:

- [Giriş: Azure DevTest labs'deki bir görüntü fabrikası ayarlayarak sanal makineye dakikalar içinde hazır olun](https://blogs.msdn.microsoft.com/devtestlab/2016/09/14/introduction-get-vms-ready-in-minutes-by-setting-up-image-factory-in-azure-devtest-labs/)
- [Fabrika – bölüm 2 görüntü! Sanal makineler oluşturmak için Azure işlem hatları ve Fabrika Laboratuvar Kurulumu](https://blogs.msdn.microsoft.com/devtestlab/2017/10/25/image-factory-part-2-setup-vsts-to-create-vms-based-on-devtest-labs/)
- [Görüntü Factory – bölüm 3: Özel görüntüleri kaydetmek ve dağıtmak için birden çok Laboratuvarları](https://blogs.msdn.microsoft.com/devtestlab/2018/01/10/image-factory-part-3-save-custom-images-and-distribute-to-multiple-labs/)
- [Video: Azure DevTest Labs ile özel görüntü üretici](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)

## <a name="patterns-to-set-up-network-configuration"></a>Ağ Yapılandırması'kurmak için desenler

### <a name="question"></a>Soru
Nasıl sağlamak, geliştirme ve test sanal makineleri genel internet ulaşamıyor? Ağ Yapılandırması'kurmak için herhangi bir önerilen Düzen var mı?

### <a name="answer"></a>Yanıt
Evet. Dikkate alınması gereken – gelen ve giden trafiği iki unsur vardır.

**Gelen trafik** – internet ulaşılamıyor sonra sanal makinenin genel IP adresi yok. Kullanıcı bir genel IP adresi oluşturabilir, bir abonelik düzeyi ilkesi ayarlanmış olduğundan emin olun. yaygın bir yaklaşımdır.

**Giden trafik** – sanal makineleri doğrudan genel internet'e giderek önlemek ve kurumsal bir güvenlik duvarı aracılığıyla trafiği zorlamak istiyorsanız sonra şirket içi trafiği expressroute üzerinden yönlendirebilir veya VPN kullanarak zorlamalı yönlendirme.

> [!NOTE]
> Proxy ayarları olmadan trafiği engelleyen bir proxy sunucunuz varsa, özel durumlar için laboratuvar yapıt depolama hesabı eklemek unutmayın.

Ağ güvenlik grupları, sanal makine ya da alt ağlar için de kullanabilirsiniz. Bu adım, ek bir izin ver / trafiği engellemek için koruma katmanı ekler.

## <a name="new-vs-existing-virtual-network"></a>Yeni var olan sanal ağı

### <a name="question"></a>Soru
Yeni bir sanal ağ ve var olan bir sanal ağ kullanarak DevTest Labs ortamım için ne zaman oluşturmalıyım?

### <a name="answer"></a>Yanıt
Ardından Vm'lerinizi mevcut altyapısıyla etkileşim kurmak gerekiyorsa, DevTest Labs ortamınız içinde mevcut bir sanal ağ kullanmayı düşünmeniz gerekir. Ayrıca, ExpressRoute kullanıyorsanız, sanal ağlar en aza indirmek isteyebilirsiniz / alt ağlar Aboneliklerde kullanılmak üzere atanan, IP adres alanı parçalara böylece. Ayrıca, sanal ağ eşleme düzeni burada (merkez-uç model) kullanmayı da düşünmelisiniz. Bir Azure ağı yukarı gelen özelliğinde bölgeler arasında eşleme olmasına rağmen bu yaklaşım, belirli bir bölge içinde abonelikler arasında vnet/alt ağ iletişimi sağlar.

Aksi takdirde, DevTest Labs her ortamın kendi sanal ağı olabilir. Ancak, bulunduğuna dikkat edin [sınırları](../azure-subscription-service-limits.md) abonelik başına sanal ağ sayısı. Bu sınır 100 olarak oluşturulabilir olsa varsayılan süreyi 50 ' dir.

## <a name="shared-public-or-private-ip"></a>Paylaşılan, genel veya özel IP

### <a name="question"></a>Soru
Özel IP ve genel IP ile paylaşılan bir IP ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Bir siteden siteye VPN veya Express Route kullandığınız makinelerinizi genel internet üzerinden erişilebilir, iç ağ aracılığıyla ve erişilemez olacak şekilde özel IP'ler kullanmayı düşünün.

> [!NOTE]
> Laboratuvar sahibi yanlışlıkla, VM'ler için genel IP adresleri hiç oluşturmasını sağlamak için bu alt ağ ilkesi değiştirebilirsiniz. Abonelik sahibi, genel IP'ler oluşturulmasını önleyen bir abonelik İlkesi oluşturmanız gerekir.

Paylaşılan genel IP'ler kullanırken bir laboratuvarındaki sanal makinelerde bir genel IP adresi paylaşın. Bu yaklaşım, belirli bir aboneliği için genel IP adresleri barındırabileceğiniz ihlal önlemek gerektiğinde yararlı olabilir.

## <a name="limits-of-number-of-virtual-machines-per-user-or-lab"></a>Kullanıcı veya Laboratuvar başına sanal makine sayısı sınırlamaları

### <a name="question"></a>Soru
Kullanıcı başına veya Laboratuvar başına ayarlamalısınız kural kaç tane sanal makineyi bakımından var mı?

### <a name="answer"></a>Yanıt
Kullanıcı başına veya Laboratuvar başına sanal makine sayısını düşünürken, üç temel sorun vardır:

- **Genel maliyet** , takım Laboratuvardaki kaynaklardaki ayırabilirsiniz. Birçok makineleri dönmesi kolay bir işlemdir. Maliyetleri denetlemek için kullanıcı başına ve/veya Laboratuvar başına sanal makinelerin sayısını sınırlamak için bir mekanizma olan
- Bir laboratuvar sanal makinelerin toplam sayısı tarafından etkilenen [abonelik düzeyi kotaları](../azure-subscription-service-limits.md) kullanılabilir. Üst sınırları 800 kaynak grupları, abonelik başına biridir. (Paylaşılan genel IP'ler kullanılır sürece) DevTest Labs her VM için şu anda yeni bir kaynak grubu oluşturur. Bir abonelikte 10 labs varsa, laboratuvarlar yaklaşık 79 sanal makineler (800 üst sınırı – 10 kaynak grupları 10 laboratuvarlar için) her Laboratuvardaki uygun olabilecek Laboratuvar başına 79 sanal makine =.
- Laboratuvar şirket Express Route üzerinden (örneğin) bağlı olduğunda **IP adresi alanları tanımlanan** VNet/alt ağ için. Vm'leri Laboratuvardaki oluşturulması başarısız olmayan emin olmak için (hata: IP adresi alınamıyor), IP adres alanı kullanılabilir hizalı Laboratuvar başına en fazla VM Laboratuvar sahipleri belirtebilirsiniz.

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma

### <a name="question"></a>Soru
DevTest Labs Ortamımın Resource Manager şablonlarını nasıl kullanabilirim?

### <a name="answer"></a>Yanıt
Bölümünde anlatılan adımları kullanarak DevTest Labs ortamına Resource Manager şablonlarınızı dağıtmak [ortamları özellik DevTest labs'deki](devtest-lab-test-env.md) makalesi. Aslında, Resource Manager şablonlarınızı bir Git deposuna (Azure depoları veya GitHub) denetleyin ve ekleme bir [şablonlarınızın özel depoya](devtest-lab-test-env.md) Laboratuvar için.

Bu senaryo, konak geliştirme makineler için DevTest Labs'i kullanarak faydalı olmayabilir ancak üretim temsilcisi bir hazırlık ortamı oluşturuyorsanız yararlı olabilir.

Ayrıca, kaç kullanıcı seçeneği veya Laboratuvar başına sanal makineler yalnızca Laboratuvar ve tüm ortamları (Resource Manager şablonlarını) tarafından yerel olarak oluşturulan sanal makinelerde sayısını sınırlayan hatalarının ayıklanabileceğini belirtmekte yarar.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [DevTest Labs'de ortamları kullanma](devtest-lab-test-env.md).