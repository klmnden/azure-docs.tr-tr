---
title: Azure DevTest Labs hakkında | Microsoft Docs
description: Nasıl DevTest Labs oluşturmak, yönetmek ve Azure sanal makinelerini izlemek kolaylaştırmak bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 599b4325e0eb9aa7f6ccca2626c6d4b14f38149d
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339338"
---
# <a name="about-azure-devtest-labs"></a>Azure DevTest Labs hakkında
Azure DevTest Labs sanal makineleri verimli bir şekilde Self Servis ve/veya geliştirme, test, eğitim ve tanıtım vb. için ihtiyaç duydukları Araçlar sabit bir onayları için beklemenize gerek kalmadan ihtiyaç duydukları PaaS kaynaklarına takımdaki geliştiricilerin sağlayan bir hizmettir. 

Laboratuvar zaten önceden yapılandırılmış tabanları ya da Resource Manager şablonları ile tüm gerekli araçlara ve geliştiricilerin ortamlar oluşturmak için kullanabileceği yazılım oluşur. Geliştiriciler, saatler veya günler yerine birkaç dakika içinde kendi ortamlarını oluşturabilir. 

DevTest Labs'i kullanarak, aşağıdaki görevleri gerçekleştirerek uygulamanızın en son sürümünü test edebilirsiniz:

- Hızlı bir şekilde yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını sağlama
- Dağıtım işlem hattınızı üzerine ortamları sağlamak için DevTest Labs ile kolayca tümleştirin
- Birden çok test aracısına sağlayarak sınama yük ölçeğini ve eğitim ve tanıtım için önceden hazırlanan ortamlar oluşturun.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Özellikler
DevTest Labs, sanal makineler ile çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Bir sanal makineyi hızlıca beş basit adımları izleyerek oluşturun.
- Yapılandırılmış, onaylanan ve Ekip Lideri ya Orta düzenlemeyle VM tabanlarını düzenlenmiş bir listesini arasından BT.
- Tüm yazılım ve araçları görüntüde yüklü olan önceden oluşturulmuş özel görüntülerden makineleri oluşturun. 
- Aslında özel görüntüler ve bir sanal makine oluşturma sırasında yüklenen yazılıma en son derlemesini formüllerden makineleri oluşturun.
- VM hazırlandıktan sonra dağıtılmış uzantıları yapıtları yükleme.
- Otomatik kapatma ' ayarlayın ve başlangıç zamanlamaları kapatma günün sonunda olması ve ardından yukarı makinelerde otomatik ve sonraki sabah çalıştırılan.
- Yalnızca önceden oluşturulmuş bir sanal makine, makine oluşturma sürecinde inmek zorunda kalmadan talep edin. 

DevTest Labs ile PaaS ortamlar çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- PaaS Azure Resource Manager ortamları hızlıca Üçten az basit adımları izleyerek oluşturun.
- Yapılandırılmış, onaylanan ve Ekip Lideri ya Orta düzenlemeyle Resource Manager şablonlarını düzenlenmiş bir listesini arasından BT.
- Laboratuvar bağlamında kalsanız yine de Azure'nın tamamını keşfetmek için bir Resource Manager şablonu kullanarak boş bir kaynak grubu (sanal) çalıştırın.
- DevTest Labs oluşturma, yapılandırma ve bulut geliştirme ve test ortamları yönetme aşağıdaki avantajları sağlar.

Bir Self-Servis modeli dışında bir takım geliştiriciler için Hizmet Merkezi sağlar wastes denetlemek, kaynakları maliyetlerini en iyi duruma getirmek ve bütçelerini içinde aşağıdaki görevleri gerçekleştirerek için BT: 

- Otomatik kapatma ayarı ve otomatik zamanlamaları sanal makineleri başlatın.
- İlkeleri olan sanal kullanıcı sayısı ayarı oluşturabilirsiniz.
- Sanal makine boyutları ve galeri görüntüleri kullanıcılar ilkelerini ayarlamaya arasından seçim yapabilirsiniz.
- Laboratuvar maliyetlerini ve ayarı hedefleri izleme.
- Gerekli eylemleri yararlanabilmeniz için laboratuvar yüksek tahmini maliyetleri bildirim alarak. 

DevTest Labs, oluşturma, yapılandırma ve bulut ortamları yönetme aşağıdaki avantajları sağlar:

## <a name="cost-control-and-governance"></a>Maliyet denetim ve idare
DevTest Labs, maliyetlerin aşağıdaki görevleri gerçekleştirmenize izin vererek kolaylaştırır:

- Kullanıcı ve Laboratuvar başına sanal makine sayısı başına - sanal makine (VM) sayısı gibi Laboratuvar ilkeleri ayarlayın. 
- Otomatik olarak kapatılmasını ve başlatmak için ilkeler oluşturun.
- Sanal makinelere ve PaaS kaynaklarına önceden tanımlanmış bütçenizin kalmanız Laboratuvarınızı içinde küme çalışmaya başladıktan maliyetleri izlemenize olanak sağlar. 
- Böylece, temel alınan kaynak grubu veya abonelik dışında herhangi bir kaynağa olunan'kurmak bitirme Laboratuvar bağlamı içinde kalmak geliştiricilerin yardımcı olur.

## <a name="quickly-get-to-ready-to-test"></a>Hızlı bir şekilde hazır test Al
DevTest Labs, geliştirme ve test etmek için takımınızın ihtiyaç duyduğu her şeyi ile önceden hazırlanan ortamlar oluşturmanızı sağlar. Yalnızca uygulamanızın son iyi derlemesinin yüklü olduğu yeri ortamları ve hemen çalışmaya başlamak talep. Dilerseniz de kapsayıcılar bile daha hızlı ve daha akıllı ortam oluşturmak için kullanın.

## <a name="create-once-use-everywhere"></a>Bir kez oluşturun, her yerde kullanın
Yakalama ve PaaS ortam şablonlarını ve yapıtları ekibiniz veya kuruluşunuz - tüm kaynak denetiminde - Geliştirici oluşturma ve test ortamlarını kolayca paylaşın.

## <a name="worry-free-self-service"></a>Sorunsuz Self Servis
DevTest Labs, geliştiricilere ve test edicilere hızlı bir şekilde sağlar ve sanal makineler (Iaas) ve PaaS kaynakları bir kendileri için önceden yapılandırılmış bir kaynak kümesini kullanarak yalnızca birkaç tıklamayla kolayca oluşturur.

## <a name="use-iaas-and-paas-resources"></a>Iaas ve PaaS kaynakları kullan 
DevTest Labs, geliştiricilerin Web Apps, Service Fabric kümeleri, SharePoint grupları gibi PaaS kaynaklarına çalıştırın ve benzeri de sağlar. Laboratuvar içinde bir Resource Manager şablonu kullanarak. Genel ortam depomuzda Resource Manager şablonlarını kullanabilir veya Laboratuvar PaaS üzerinde Labs'de kullanmaya başlamak için kendi Git deposuna bağlanın. Ayrıca, maliyetleri, önceden tanımlanmış bir bütçe içinde kalmak için bu kaynakları izleyebilirsiniz. 

## <a name="integrate-with-your-existing-toolchain"></a>Mevcut araç zincirinizle tümleştirin
Önceden yapılmış eklentileri veya API'mizi geliştirme ve Test ortamlarınızı doğrudan, tercih ettiğiniz sürekli tümleştirme (CI) aracı, tümleşik geliştirme ortamı (IDE) veya otomatik yayın ardışık düzeni sağlamak için kullanın. Ayrıca, kapsamlı komut satırı aracımızı da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- DevTest Labs hakkında daha fazla bilgi için bkz: [DevTest Labs kavramları](devtest-lab-concepts.md)
- Adım adım yönergeler içeren bir kılavuz için bkz. [Öğreticisi: Azure DevTest Labs'i kullanarak bir laboratuvarı ayarlama ayarlayın](tutorial-create-custom-lab.md)


