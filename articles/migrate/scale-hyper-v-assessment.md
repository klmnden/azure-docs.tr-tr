---
title: Çok sayıda Hyper-V Vm'lerini Azure geçişi ile Azure'a geçiş için değerlendirme | Microsoft Docs
description: Çok sayıda Hyper-V Vm'lerini Azure geçişi hizmetini kullanarak Azure'a geçiş için değerlendirme açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 95704f2694892b349d0967fca2160dabd990b472
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811550"
---
# <a name="assess-large-numbers-of-hyper-v-vms-for-migration-to-azure"></a>Çok sayıda Hyper-V Vm'lerini Azure'a geçiş için değerlendirme

Bu makalede, şirket içi Hyper-V Vm'leri Azure geçişi Server değerlendirmesi aracını kullanarak azure'a geçiş için çok sayıda (> 1000) değerlendirmek açıklar.

[Azure geçişi](migrate-services-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 


Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Uygun ölçekte değerlendirme planlayın.
> * Azure izinlerini yapılandırma ve değerlendirme için Hyper-V'leri hazırlama.
> * Bir Azure geçişi projesi oluşturun ve bir değerlendirme oluşturun.
> * Değerlendirme, geçiş için planlarken gözden geçirin.


> [!NOTE]
> Bir-birkaç sanal makinelerinin uygun ölçekte değerlendirme önce değerlendirmek için izleyin kavram denemek istiyorsanız bizim [öğretici serisi](tutorial-prepare-hyper-v.md)

## <a name="plan-for-assessment"></a>Değerlendirme planlama

Hyper-V Vm'lerini çok sayıda değerlendirmesi için planlama yaparken bir dikkat etmeniz gereken birkaç şey vardır:

- **Azure geçişi projeleri planlamak**: Azure geçişi projeleri dağıtma konusunda bilgi bulun. Örneğin, farklı coğrafi bölgelerde veri merkezlerinizdeki olan veya farklı bir coğrafi bölge bulma, değerlendirme veya geçiş ile ilgili meta verileri depolamak gerekirse, birden çok proje gerekebilir.
- **Cihazları planlama**: Azure geçişi, bir Hyper-V VM dağıtılan bir şirket içi Azure geçişi gereç, sürekli olarak değerlendirme ve geçiş için Vm'leri bulmak için kullanır. Gereç VM'ler, diskler ve ağ bağdaştırıcıları ekleme gibi ortam değişiklikleri izler. Ayrıca bunlar hakkındaki meta verileri ve performans verilerini Azure'a gönderir. Dağıtmak için kaç cihazları bulmanız gerekir.


## <a name="planning-limits"></a>Sınırları Planlama
 
Planlama için bu tabloda özetlenen sınırlarını kullanın.

**Planlama** | **Limitler**
--- | --- 
**Azure geçiş projeleri** | Bir projede en fazla 10.000 Vm'leri değerlendirin.
**Azure geçişi Gereci** | Gereçlerden biri en fazla 5000 Vm'leri bulabilir.<br/> En fazla 300 Hyper-V konaklarını bir gereç bağlanabilirsiniz.<br/> Bir gereç yalnızca tek bir Azure geçişi proje ile ilişkili olabilir.<br/><br/> 
**Azure geçişi değerlendirmesi** | Tek değerlendirmede en fazla 10.000 Vm'leri değerlendirebilirsiniz.



## <a name="other-planning-considerations"></a>Diğer planlama konuları

- Bulma Gereci başlatmak için her Hyper-V ana bilgisayar'ı seçmeniz gerekir. 
- Çok kiracılı bir ortam kullanıyorsanız, şu anda yalnızca belirli bir kiracıya ait VM'lerin bulamaz. 

## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlama

Azure ve Hyper-V server değerlendirmesi için hazırlayın. 

1. Doğrulama [Hyper-V desteği gereksinimleri ve sınırlamaları](migrate-support-matrix-hyper-v.md).
2. Azure geçişi ile etkileşim kurmak Azure hesabınız için izinleri ayarla
3. Hyper-V konakları ve sanal makineleri hazırlama

Bölümündeki yönergeleri [Bu öğreticide](tutorial-prepare-hyper-v.md) bu ayarları yapılandırmak için.

## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak, aşağıdakileri yapın:

1. Bir Azure geçişi projeleri oluşturun.
2. Azure geçişi Server değerlendirmesi aracı, projeler ekleyin.

[Daha fazla bilgi edinin](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Oluşturma ve değerlendirme gözden geçirin

1. Hyper-V Vm'leri için Değerlendirmeler oluşturun.
1. Geçiş planlaması için hazırlık değerlendirmeleri gözden geçirin.

[Daha fazla bilgi edinin](tutorial-assess-hyper-v.md) oluşturma ve değerlendirmeleri gözden geçirme hakkında.
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * Azure geçişi değerlendirmeleri Hyper-V Vm'leri için ölçeklendirmek planlanan
> * Azure ve Hyper-V değerlendirmesi için hazır
> * Bir Azure geçişi projesi oluşturulur ve değerlendirmeler çalıştı
> * Geçiş için hazırlık değerlendirmeleri gözden geçirdik.

Şimdi [öğrenin nasıl](concepts-assessment-calculation.md) değerlendirmeleri hesaplanır ve nasıl [değerlendirmeleri değiştirme](how-to-modify-assessment.md)
