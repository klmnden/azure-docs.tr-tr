---
title: VMware Vm'lerini çok sayıda Azure geçişi ile Azure'a geçiş için değerlendirme | Microsoft Docs
description: VMware Vm'lerini çok sayıda Azure geçişi hizmetini kullanarak Azure'a geçiş için değerlendirme açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 6102a1c59be3627b95dc1e0cb1d1d712d5832d2f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811342"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Çok sayıda VMware Vm'lerini Azure'a geçiş için değerlendirme


Bu makalede, şirket içi VMware vm'lerinin Azure geçişi Server değerlendirmesi aracını kullanarak azure'a geçiş için çok sayıda (35.000 1000) değerlendirmek nasıl açıklar.

[Azure geçişi](migrate-services-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Uygun ölçekte değerlendirme planlayın.
> * Azure izinlerini yapılandırma ve VMware için değerlendirme hazırlayın.
> * Bir Azure geçişi projesi oluşturun ve bir değerlendirme oluşturun.
> * Değerlendirme, geçiş için planlarken gözden geçirin.


> [!NOTE]
> Bir-birkaç sanal makinelerinin uygun ölçekte değerlendirme önce değerlendirmek için izleyin kavram denemek istiyorsanız bizim [öğretici serisi](tutorial-prepare-vmware.md)

## <a name="plan-for-assessment"></a>Değerlendirme planlama

VMware Vm'lerini çok sayıda değerlendirmesi için planlama yaparken bir dikkat etmeniz gereken birkaç şey vardır:

- **Azure geçişi projeleri planlamak**: Azure geçişi projeleri dağıtma konusunda bilgi bulun. Örneğin, farklı coğrafi bölgelerde veri merkezlerinizdeki olan veya farklı bir coğrafi bölge bulma, değerlendirme veya geçiş ile ilgili meta verileri depolamak gerekirse, birden çok proje gerekebilir. 
- **Cihazları planlama**: Azure geçişi, bir VMware VM dağıtılmış bir şirket içi Azure geçişi gereç, sürekli Vm'leri bulmak için kullanır. Gereç VM'ler, diskler ve ağ bağdaştırıcıları ekleme gibi ortam değişiklikleri izler. Ayrıca bunlar hakkındaki meta verileri ve performans verilerini Azure'a gönderir. Kaç cihazları dağıtmak için ihtiyacınız şekil gerekir.
- **Bulma için hesapları planlama**: Azure geçişi Gereci değerlendirme ve geçiş için Vm'leri bulmak için vCenter Server'a erişimi olan bir hesabı kullanır. 10. 000'den fazla VM bulduğunuza, birden çok hesaplarını ayarlama.


## <a name="planning-limits"></a>Sınırları Planlama
 
Planlama için bu tabloda özetlenen sınırlarını kullanın.

**Planlama** | **Limitler**
--- | --- 
**Azure geçiş projeleri** | Bir projedeki kadar 35.000 Vm'leri değerlendirin.
**Azure geçişi Gereci** | Bir gereç yalnızca tek bir vCenter Server'a bağlanabilirsiniz.<br/><br/> Bir gereç yalnızca tek bir Azure geçişi proje ile ilişkili olabilir.<br/> Bir vCenter sunucusu en fazla 10.000 Vm'lerde gereçlerden biri bulabilir.
**Azure geçişi değerlendirmesi** | Tek değerlendirmede en fazla 35.000 Vm'leri değerlendirebilirsiniz.

Bu sınırları göz önünde, bazı örnek dağıtımlar aşağıda verilmiştir:


**vCenter sunucusu** | **Sunucu Vm'lerinde** | **Öneri** | **Eylem**
---|---|---
Bir | < 10,000 | Bir Azure geçişi projesi.<br/> Bir gereç.<br/> Bulma için bir vCenter hesabı. | Gerecini ayarlamak, vCenter sunucusu bir hesapla bağlanın.
Bir | > 10,000 | Bir Azure geçişi projesi.<br/> Birden çok cihazları.<br/> Birden fazla vCenter hesabı. | Her 10.000 VM'ler için gereç ayarlayın.<br/><br/> VCenter hesaplarını ayarlama ve 10. 000'den küçük VM'ler için bir hesap için erişimi sınırlandırmak üzere envanteri bölün.<br/> Her Gereci vCenter sunucusu bir hesapla bağlanın.<br/> Farklı gereçlerle bulunan makinelerde bağımlılıkları analiz edebilirsiniz.
Birden çok | < 10,000 |  Bir Azure geçişi projesi.<br/> Birden çok cihazları.<br/> Bulma için bir vCenter hesabı. | Cihazları ayarlayabilir, vCenter sunucusu bir hesapla bağlanın.<br/> Farklı gereçlerle bulunan makinelerde bağımlılıkları analiz edebilirsiniz.
Birden çok | > 10,000 | Bir Azure geçişi projesi.<br/> Birden çok cihazları.<br/> Birden fazla vCenter hesabı. | VCenter sunucusunu bulma < 10.000 Vm'leri ayarlamışsa gereçlerden biri her bir vCenter Server için.<br/><br/> VCenter sunucusunu bulma > 10.000 Vm'leri ayarlarsanız bir gereç her 10.000 VM'ler için.<br/> VCenter hesaplarını ayarlama ve 10. 000'den küçük VM'ler için bir hesap için erişimi sınırlandırmak üzere envanteri bölün.<br/> Her Gereci vCenter sunucusu bir hesapla bağlanın.<br/> Farklı gereçlerle bulunan makinelerde bağımlılıkları analiz edebilirsiniz.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Çok müşterili bir ortamda bulma planlama

Çok kiracılı bir ortam için planlıyorsanız, vCenter sunucusunu bulma kapsamını belirleyebilirsiniz.

- VCenter Server datacenter, küme veya klasörü kümeleri, konak veya konak veya tek tek sanal makineleri klasörü Gereci bulma kapsamı ayarlayabilirsiniz.
- Ortamınızı kiracılar arasında paylaşılır ve her kiracıya ayrı olarak keşfetmek istemiyorsanız, bulma için kullandığı Gereci vCenter hesabı için erişim kapsamını belirleyebilirsiniz. 
    - Kiracıların konaklar paylaşıyorsanız, kimlik bilgilerini belirli kiracıya ait sanal makineler için yalnızca okuma erişimi ile oluşturun. 
    - Azure geçişi Gereci bulma için bu kimlik bilgilerini kullanın.
    - VCenter hesabı vCenter VM klasör düzeyinde verilen erişimi varsa, azure geçişi değerlendirmesi Vm'leri bulamaz. Konakların ve kümelerin klasörleri desteklenir. 

## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlama

Azure ile VMware server değerlendirmesi için hazırlayın. 

1. Doğrulama [VMware desteği gereksinimlerini ve sınırlamalarını](migrate-support-matrix-vmware.md).
2. Azure geçişi ile etkileşim kurmak Azure hesabınız için izinleri ayarlayın.
3. VMware değerlendirmesi için hazırlayın.


Bölümündeki yönergeleri [Bu öğreticide](tutorial-prepare-vmware.md) bu ayarları yapılandırmak için.


## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak, aşağıdakileri yapın:

1. Bir Azure geçişi projeleri oluşturun.
2. Azure geçişi Server değerlendirmesi aracı, projeler ekleyin.

[Daha fazla bilgi edinin](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Oluşturma ve değerlendirme gözden geçirin

1. VMware Vm'leri için Değerlendirmeler oluşturun.
1. Geçiş planlaması için hazırlık değerlendirmeleri gözden geçirin.


Bölümündeki yönergeleri [Bu öğreticide](tutorial-assess-vmware.md) bu ayarları yapılandırmak için.
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * VMware Vm'leri için Azure geçişi değerlendirmeleri ölçeklendirmek planlanan
> * Hazırlanan Azure ve VMware için değerlendirme
> * Bir Azure geçişi projesi oluşturulur ve değerlendirmeler çalıştı
> * Geçiş için hazırlık değerlendirmeleri gözden geçirdik.

Şimdi [öğrenin nasıl](concepts-assessment-calculation.md) değerlendirmeleri hesaplanır ve nasıl [değerlendirmeleri değiştirme](how-to-modify-assessment.md).
