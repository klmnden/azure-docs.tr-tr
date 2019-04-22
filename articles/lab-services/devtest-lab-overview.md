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
ms.openlocfilehash: b7cd6bb1fd0377ca1440d9c667453df922aacbd4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698665"
---
# <a name="about-azure-devtest-labs"></a>Azure DevTest Labs hakkında
Azure DevTest Labs geliştiriciler takımlar verimli bir şekilde sanal makineler (VM'ler) ve PaaS kaynaklarına onay beklemeden otomatik yönetmek etkinleştirir.

DevTest Labs, önceden yapılandırılmış tabanları veya Azure Resource Manager şablonları oluşan labs oluşturur. Bu, tüm gerekli araçlara ve ortamlar oluşturmak için kullanabileceğiniz yazılım vardır. Ortamlar, saatler veya günler yerine birkaç dakika içinde oluşturabilirsiniz.

DevTest Labs kullanarak aşağıdaki görevleri gerçekleştirerek uygulamalarınızı en son sürümlerini test edebilirsiniz:

- Yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayın.
- Kolayca, dağıtım işlem hattı talep üzerine ortamları sağlamak için DevTest Labs ile tümleştirin.
- Birden çok test aracısına sağlayarak sınama yük ölçeğini ve eğitim ve tanıtım için önceden hazırlanan ortamlar oluşturun.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]

## <a name="capabilities"></a>Özellikler
DevTest Labs sanal makineleri ile çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Vm'leri hızla Beşten az basit adımları izleyerek oluşturun.
- Yapılandırılmış, onaylanan ve Ekip Lideri ya Orta düzenlemeyle VM tabanlarını düzenlenmiş bir listesini arasından BT.
- Tüm yazılım ve araçlar zaten yüklü olan önceden oluşturulmuş özel görüntülerden VM'ler oluşturun. 
- Vm'leri oluştururken, yüklü yazılımların en yeni derlemeleri ile birlikte aslında özel görüntüleri formüllerden VM'ler oluşturun. 
- Sağlanan sonra Vm'lerine dağıtılmış uzantıları yapıtları yükleme.
- Otomatik kapatmayı ayarlamayı ve zamanlamaları vm'lerde otomatik olarak başlat.
- Önceden oluşturulmuş bir sanal makine oluşturma sürecine gitmeden talep.

DevTest Labs ile PaaS ortamlar çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Üçten basit adımları izleyerek PaaS ortamları hızlıca oluşturma için Kaynak Yöneticisi'ni kullanın.
- Yapılandırılmış ve Ekip Lideri ya Orta düzenlemeyle Resource Manager şablonlarını düzenlenmiş bir listesini arasından seçim BT.
- Bir laboratuvar bağlamında Azure keşfetmek için Resource Manager şablonu kullanarak boş bir kaynak grubu (sanal) çalıştırın.

DevTest Labs da sağlayan merkezi BT wastes denetlemek, kaynakları maliyetlerini en iyi duruma getirmek ve bütçelerini içinde aşağıdaki görevleri gerçekleştirerek kalın için:  

- Otomatik kapatma ve otomatik başlatma zamanlamalar Vm'lerde ayarlanıyor.
- İlke kullanıcılar oluşturabileceğiniz VM sayısını ayarlama.
- Sanal makinelerin boyutlarını ve kullanıcıların arasından seçim galeri görüntüleri ilkeleri ayarlama.
- Laboratuvarları izleme, maliyetleri ve ayarı hedefler.
- Böylece, gerekli eylemleri yüksek tahmini maliyetleri Laboratuvarları için bildirim alarak.

DevTest Labs, oluşturma, yapılandırma ve bulut ortamları yönetme aşağıdaki avantajları sağlar.

## <a name="cost-control-and-governance"></a>Maliyet denetim ve idare
DevTest Labs, maliyetlerin aşağıdaki görevleri gerçekleştirmenize izin vererek kolaylaştırır:

- [Üzerinde Laboratuvar ilkelerini ayarlama](devtest-lab-get-started-with-lab-policies.md), kullanıcı başına veya Laboratuvar başına sanal makinelerin sayısı gibi. 
- Oluşturma [otomatik olarak kapatmak için ilkeleri](devtest-lab-set-lab-policy.md) ve Vm'leri başlatın.
- Vm'leri ve PaaS kaynakları maliyetlerini izlemek içinde kalmak için labs içinde hazırladık yukarı [bütçenizi](devtest-lab-configure-cost-management.md).
- Bunları dışında kaynakları aşamasında olmayan şekilde laboratuvarlarınızı bağlamı içinde kalır.

## <a name="quickly-get-to-ready-to-test"></a>Hızlı bir şekilde hazır test Al
DevTest Labs, ekibinizin uygulama geliştirip test etmek için ihtiyaç duyduğu her şeyi ile donatılmış önceden hazırlanan ortamlar oluşturmanızı sağlar. Yalnızca [bu ortamları talep etmek](devtest-lab-add-claimable-vm.md) uygulamanızın son iyi derlemesinin yüklü olduğu ve başlangıç çalışır. Veya daha hızlı, daha yalın bir ortam oluşturmak için kapsayıcıları kullanabilirsiniz.

## <a name="create-once-use-everywhere"></a>Bir kez oluşturun, her yerde kullanın
Yakalama ve paylaşma PaaS [ortam şablonlarını](devtest-lab-create-environment-from-arm.md) ve [yapıtları](add-artifact-repository.md) ekip veya kuruluş içinde — tüm kaynak denetiminde — kolayca Geliştirici oluşturma ve test ortamları.

## <a name="worry-free-self-service"></a>Sorunsuz Self Servis
DevTest Labs, geliştiricilere ve test edicilere hızla ve kolayca sağlayan [Iaas sanal makineleri oluşturma](devtest-lab-add-vm.md) ve [PaaS kaynaklarına](devtest-lab-create-environment-from-arm.md) bir önceden yapılandırılmış bir kaynak kümesini kullanarak.

## <a name="use-iaas-and-paas-resources"></a>Iaas ve PaaS kaynakları kullan 
Geliştiriciler ayrıca Azure Service Fabric kümeleri gibi PaaS kaynakları Web Apps özelliği, Azure App Service ve SharePoint grupları Resource Manager şablonları kullanarak yavaşlatabilir. PaaS üzerinde Labs'de kullanmaya başlamak için şablonları kullanın. [genel ortam depo](devtest-lab-configure-use-public-environments.md) veya [Laboratuvar kendi Git deposuna bağlanın](devtest-lab-create-environment-from-arm.md#configure-your-own-template-repositories). Bu kaynaklardaki bütçeniz dahilinde, maliyetleri de izleyebilirsiniz.

## <a name="integrate-with-your-existing-toolchain"></a>Mevcut araç zincirinizle tümleştirin
Eklentiler veya API için geliştirme/test ortamlarını sağlama doğrudan tercih ettiğiniz önceden yapılan kullanım [sürekli tümleştirme (CI) aracı](devtest-lab-integrate-ci-cd-vsts.md), tümleşik geliştirme ortamı (IDE) veya otomatik yayın ardışık düzeni. Ayrıca, kapsamlı komut satırı aracını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- DevTest Labs hakkında daha fazla bilgi için bkz: [DevTest Labs kavramları](devtest-lab-concepts.md).
- Adım adım yönergeler içeren bir kılavuz için bkz. [Öğreticisi: Azure DevTest Labs'i kullanarak bir laboratuvarı ayarlama ayarlamak](tutorial-create-custom-lab.md).