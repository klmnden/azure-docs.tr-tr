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
ms.openlocfilehash: e07149865d2dda52e33003964c2852a8aaccf76f
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493204"
---
# <a name="about-azure-devtest-labs"></a>Azure DevTest Labs hakkında
Azure DevTest Labs geliştiriciler takımlar verimli bir şekilde sanal makineler (VM'ler) ve PaaS kaynaklarına onay beklemeden otomatik yönetmek etkinleştirir.

DevTest Labs, önceden yapılandırılmış tabanları veya Azure Resource Manager şablonları oluşan labs oluşturur. Bu, tüm gerekli araçlara ve ortamlar oluşturmak için kullanabileceğiniz yazılım vardır. Ortamlar, saatler veya günler yerine birkaç dakika içinde oluşturabilirsiniz.

DevTest Labs kullanarak aşağıdaki görevleri gerçekleştirerek uygulamalarınızı en son sürümlerini test edebilirsiniz:

- Yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayın.
- Kolayca, dağıtım işlem hattı talep üzerine ortamları sağlamak için DevTest Labs ile tümleştirin.
- Birden çok test aracısına sağlayarak sınama yük ölçeğini ve eğitim ve tanıtım için önceden hazırlanan ortamlar oluşturun.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Özellikler
DevTest Labs sanal makineleri ile çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Vm'leri hızla Beşten az basit adımları izleyerek oluşturun.
- Yapılandırılmış, onaylanan ve Ekip Lideri ya Orta düzenlemeyle VM tabanlarını düzenlenmiş bir listesini arasından BT.
- Tüm yazılım ve araçlar zaten yüklü olan önceden oluşturulmuş özel görüntülerden VM'ler oluşturun. 
- Sanal makineleri oluşturulduğunda yüklü yazılım en yeni derlemeleri ile birleştirilmiş özel görüntülerden VM'ler oluşturun.
- Sağlanan sonra Vm'lerine dağıtılmış uzantıları yapıtları yükleme.
- Otomatik kapatmayı ayarlamayı ve zamanlamaları vm'lerde otomatik olarak başlat.
- Önceden oluşturulmuş bir sanal makine oluşturma sürecine gitmeden talep.

DevTest Labs ile PaaS ortamlar çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Üçten basit adımları izleyerek PaaS ortamları hızlıca oluşturma için Kaynak Yöneticisi'ni kullanın.
- Düzenlenmiş bir listesini, yapılandırılır ve Ekip Lideri ya Orta düzenlemeyle Resource Manager şablonları arasından seçim BT.
- Bir laboratuvar bağlamında Azure keşfetmek için Resource Manager şablonu kullanarak boş bir kaynak grubu (sanal) çalıştırın.

DevTest Labs da sağlayan merkezi BT wastes denetlemek, kaynakları maliyetlerini en iyi duruma getirmek ve bütçelerini içinde aşağıdaki görevleri gerçekleştirerek kalın için: 

- Otomatik kapatma ve otomatik başlatma zamanlamalar Vm'lerde ayarlanıyor.
- İlke kullanıcılar oluşturabileceğiniz VM sayısını ayarlama.
- Sanal makinelerin boyutlarını ve kullanıcıların arasından seçim galeri görüntüleri ilkeleri ayarlama.
- Laboratuvarları izleme, maliyetleri ve ayarı hedefler.
- Böylece, gerekli eylemleri yüksek tahmini maliyetleri Laboratuvarları için bildirim alarak.

DevTest Labs, oluşturma, yapılandırma ve bulut ortamları yönetme aşağıdaki avantajları sağlar.

## <a name="control-costs-and-governance"></a>Maliyetleri ve idare
DevTest Labs, maliyetlerin aşağıdaki görevleri gerçekleştirmenize izin vererek kolaylaştırır:

- Kullanıcı başına veya Laboratuvar başına sanal makinelerin sayısı gibi Laboratuvar ilkeleri ayarlayın. 
- Otomatik olarak kapatılmasını ve başlatmak için ilkeler oluşturun.
- Vm'leri ve PaaS kaynakları maliyetlerini izlemek bütçeniz dahilinde için labs içinde hazırladık ayarlama.
- Bunları dışında kaynakları aşamasında olmayan şekilde laboratuvarlarınızı bağlamı içinde kalır.

## <a name="quickly-get-to-ready-to-test"></a>Hızlı bir şekilde hazır test Al
DevTest Labs, ekibinizin uygulama geliştirip test etmek için ihtiyaç duyduğu her şeyi ile donatılmış önceden hazırlanan ortamlar oluşturmanızı sağlar. Yalnızca uygulamanızın son iyi derlemesinin yüklü olduğu yeri bu ortamları talep etmek ve çalışmaya başlayın. Veya daha hızlı, daha yalın bir ortam oluşturmak için kapsayıcıları kullanabilirsiniz.

## <a name="create-once-use-everywhere"></a>Bir kez oluşturun, her yerde kullanın
Yakalamak ve PaaS ortam şablonlarını ve yapıtları ekibiniz veya kuruluşunuz içinde paylaşmak — tüm kaynak denetiminde — kolayca Geliştirici oluşturma ve test ortamları.

## <a name="save-time-on-setup"></a>Kurulum zamandan tasarruf edin  
Bir önceden yapılandırılmış bir kaynak kümesini kullanarak Iaas Vm'leri ve PaaS kaynaklarına kolayca oluşturabilirsiniz.

## <a name="use-iaas-and-paas-resources"></a>Iaas ve PaaS kaynakları kullan 
Geliştiriciler ayrıca Azure Service Fabric kümeleri gibi PaaS kaynakları Web Apps özelliği, Azure App Service ve SharePoint grupları Resource Manager şablonları kullanarak yavaşlatabilir. PaaS üzerinde Labs'de kullanmaya başlamak için genel ortam depodan şablonları kullanabilir veya Laboratuvar kendi Git deposuna bağlanın. Bu kaynaklardaki bütçeniz dahilinde, maliyetleri de izleyebilirsiniz.

## <a name="integrate-with-your-existing-toolchain"></a>Mevcut araç zincirinizle tümleştirin
Önceden yapılmış eklentileri veya API geliştirme/test ortamları sağlayın doğrudan, tercih ettiğiniz sürekli tümleştirme (CI) aracı, tümleşik geliştirme ortamı (IDE) veya otomatik yayın ardışık düzeni. Ayrıca, kapsamlı komut satırı aracını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- DevTest Labs hakkında daha fazla bilgi için bkz: [DevTest Labs kavramları](devtest-lab-concepts.md).
- Adım adım yönergeler içeren bir kılavuz için bkz. [Öğreticisi: Azure DevTest Labs'i kullanarak bir laboratuvarı ayarlama ayarlamak](tutorial-create-custom-lab.md).


