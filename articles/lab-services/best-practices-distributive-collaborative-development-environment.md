---
title: Azure DevTest Labs kaynakların işbirliğine dayalı geliştirme dağıtılmış | Microsoft Docs
description: DevTest Labs kaynaklar geliştirmek bir dağıtılmış ve işbirliğine dayalı geliştirme ortamını ayarlama için en iyi uygulamalar sağlanır.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: d8892b2d00008c9d67f8bc28d1abb7d562dfd95c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079892"
---
# <a name="best-practices-for-distributed-and-collaborative-development-of-azure-devtest-labs-resources"></a>Azure DevTest Labs kaynakların dağıtılmış ve işbirliğine dayalı geliştirme için en iyi uygulamalar
Farklı ekipler veya geliştirme ve temel bir kod bakımının kişilere dağıtılmış işbirliğine dayalı geliştirme sağlar. Başarılı olması için geliştirme süreci oluşturması, paylaşması ve bilgi tümleştirme yeteneğini bağlıdır. Bu anahtar geliştirme ilkesini Azure DevTest Labs içinde kullanılabilir. Bir kuruluş içindeki farklı labs arasında yaygın olarak dağıtılan kaynakların bir laboratuvar içindeki birkaç türü vardır. Farklı türdeki kaynakların iki alanlarına odaklanan:

- (Laboratuvar tabanlı) Laboratuvar içinde dahili olarak depolanan kaynakları
- Depolanan kaynakları [laboratuvara bağlı dış depolardaki](devtest-lab-add-artifact-repo.md) (kod deposu tabanlı). 

Bu belge, özelleştirme ve kalite tüm düzeylerde sağlarken birden çok ekipte işbirliği ve dağıtım izin bazı en iyi uygulamaları açıklar.

## <a name="lab-based-resources"></a>Laboratuvar tabanlı kaynaklar

### <a name="custom-virtual-machine-images"></a>Özel sanal makine görüntüleri
Gecelik temelinde labs dağıtılan özel görüntüleri ortak bir kaynak olabilir. Ayrıntılı bilgi için bkz. [görüntü Fabrika](image-factory-create.md).    

### <a name="formulas"></a>Formüller
[Formülleri](devtest-lab-manage-formulas.md) Laboratuvar özgüdür ve dağıtım mekanizması yoksa. Laboratuvar üyeleri formüllerin tüm geliştirme yapın. 

## <a name="code-repository-based-resources"></a>Kod deposu tabanlı kaynaklar
Kod depoları, yapıtlar ve ortamlar göre iki farklı özellikler mevcuttur. Bu makalede, özellikler ve kullanılabilir yapıtlar ve ortamlar kuruluş düzeyinde veya takım düzeyinde özelleştirme yeteneği izin vermek için depoları ve iş akışı en etkili bir şekilde ayarlamak nasıl üzerinden gider.  Bu iş akışı, standardına göre [kaynak kodu denetim dallanma stratejisi](/devops/repos/tfvc/branching-strategies-with-tfvc?view=azure-devops). 

### <a name="key-concepts"></a>Önemli kavramlar
Kaynak bilgileri yapıtlar için meta verileri, betikleri içerir. Kaynak bilgileri ortamlar için PowerShell betikleri, DSC betiklerini, Zip dosyaları vb. gibi diğer destekleyici dosyaları ile meta verileri ve Resource Manager şablonlarını içerir.  

### <a name="repository-structure"></a>Depo yapısı  
Kaynak kodu denetimi (SCC) için yaygın yapılandırma Labs'de (Resource Manager şablonları, meta veriler ve betikler) için kullanılan kod dosyaları depolamak için çok katmanlı yapısı oluşturmaktır. Özellikle, farklı iş düzeyleri tarafından yönetilen kaynakları depolamak için farklı bir depo oluşturun:   

- Şirket genelinde kaynakları.
- İş Birim/bölüm-geniş kaynakları
- Takım özgü kaynaklar.

Bu düzeylerinin her biri farklı bir depoya burada master dalıyla üretim kalitesinde olması gereken bağlayın. [Dalları](/devops/repos/git/git-branching-guidance?view=azure-devops) belirli kaynaklarla (yapıtları veya şablonlar) geliştirilmesi için her depoda olacaktır. Kuruluşun laboratuvarlara aynı anda birden çok deposu ve birden fazla dalları kolayca bağlayabilirsiniz gibi bu yapı iyi DevTest Labs ile hizalar. Depo adı, kullanıcı arabirimi (UI) adları aynı olduğunda, Karışıklığı önlemek için açıklama ve yayımcı dahil edilir.
     
Aşağıdaki diyagramda iki depoları gösterilmektedir: BT bölümü tarafından tutulan bir şirket deposu ve ar -ge bölme işlemi tarafından tutulan bir bölme deposu.

![Bir örnek dağıtarak ve işbirliğine dayalı geliştirme ortamı](./media/best-practices-distributive-collaborative-dev-env/distributive-collaborative-dev-env.png)
   
Bir laboratuvara bağlı birden fazla depoya sahip olmak için büyük esneklik sağlar, ancak tutarken yüksek kaliteli ana dala geliştirme için bu katmanlı bir yapı sağlar.

## <a name="next-steps"></a>Sonraki adımlar    
Aşağıdaki makalelere bakın:

- Bir depoyu kullanarak bir laboratuvara ekleme [Azure portalında](devtest-lab-add-artifact-repo.md) veya aracılığıyla [Azure kaynak yönetimi şablonu](add-artifact-repository.md)
- [DevTest Labs yapıtları](devtest-lab-artifact-author.md)
- [DevTest Labs ortamları](devtest-lab-create-environment-from-arm.md).
