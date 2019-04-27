---
title: Azure Site Recovery hizmetini kullanarak Azure Iaas Vm'leri için başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri bir Azure bölgesinden diğerine taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: dc49b33fd3e6d582b31af5fe0507884e60205757
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60791563"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Azure ile birlikte müşteri Tabanınızla birlikte büyür ve yükselen taleplerine ayak uydurmanıza yeni bölgeler için destek ekler. Yeni özellikler hizmetlerinde aylık eklenir. Sanal makinelerinizi (VM) farklı bir bölgeye veya kullanılabilirliğini artırmak için kullanılabilirlik alanları içine taşımak isteyebilirsiniz.

Bu öğreticide, sanal makinelerinizin taşımak istediğiniz farklı senaryolar açıklanmaktadır. Ayrıca, daha yüksek kullanılabilirlik elde etmek için hedef bölgede mimarisi yapılandırma açıklanır. 

Bu öğreticide, hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> 
> * Sanal makineleri taşımak için nedenler
> * Tipik mimariler
> * Bir hedef bölgeye olduğu gibi sanal makinelerin taşınmasında
> * Sanal makinelerin kullanılabilirliğini artırmak için

## <a name="reasons-to-move-azure-vms"></a>Azure sanal makineleri taşımak için nedenler

Aşağıdaki nedenlerle Vm'leri taşıyabilirsiniz:

- Zaten bir bölgede dağıtılmış ve uygulama veya hizmetinizin son kullanıcılara yakın olan yeni bir bölge desteği eklendi. Bu senaryoda, gecikme süresini azaltmak için yeni bölge olarak sanal makinelerinizin taşımak istersiniz. Abonelikleri birleştirmek istiyorsanız veya taşımak ihtiyaç duyduğunuz idare ya da kuruluş kurallar varsa aynı yaklaşımı kullanın.
- Sanal makinenizin Tek Örnekli sanal makine veya bir kullanılabilirlik kümesinin bir parçası olarak dağıtılmıştır. Kullanılabilirlik SLA'larını artırmak istiyorsanız, sanal makinelerinizin bir kullanılabilirlik bölgesine taşıyabilirsiniz.

## <a name="steps-to-move-azure-vms"></a>Azure sanal makineleri taşımak için adımlar

Sanal makinelerin taşınmasında aşağıdaki adımları içerir:

1. Önkoşulları doğrulayın.
2. Kaynak Vm'leri hazırlayın.
3. Hedef bölge hazırlayın.
4. Hedef bölge için verileri kopyalayın. Hedef bölge için kaynak sanal makineden veri kopyalamak için Azure Site Recovery çoğaltma teknolojisini kullanın.
5. Test yapılandırması. Çoğaltma işlemi tamamlandıktan sonra üretim dışı bir ağ için bir yük devretme testi gerçekleştirerek yapılandırmayı test edin.
6. Taşıma gerçekleştirin.
7. Kaynak bölgedeki kaynakları atın.

> [!NOTE]
> Bu adımlar hakkında ayrıntılar aşağıdaki bölümlerde verilmiştir.
> [!IMPORTANT]
> Şu anda, Azure Site Recovery Vm'leri bir bölgeden diğerine taşınmasını destekler ancak bir bölge içinde taşıma desteklemiyor.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Çok katmanlı dağıtımı için tipik mimariler

Bu bölümde, azure'da çok katmanlı bir uygulama için en yaygın dağıtım mimarisi açıklanmaktadır. Örnek bir genel IP ile üç katmanlı bir uygulamadır. (Web, uygulama ve veritabanı) katmanlarının her iki VM vardır ve diğer katmanlar için bir Azure load balancer ile bağlı. Veritabanı katmanı, SQL Server Always On yüksek kullanılabilirlik için sanal makineler arasında çoğaltma vardır.

* **Tek Örnekli VM'ler çeşitli katmanlarda dağıtılan**: Bir katmandaki her bir VM Tek Örnekli VM olarak yapılandırılır ve diğer katmanlar için yük Dengeleyiciler tarafından bağlanmış. Bu benimsemek en basit yapılandırmadır.

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/regular-deployment.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her bir katmandaki Vm'leri**: Bir katmandaki her bir sanal makine bir kullanılabilirlik kümesine yapılandırılır. [Kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bir kümede birden fazla yalıtılmış donanım düğümde, Azure'da dağıttığınız VM'lerin dağıtılmasını sağlar. Bu, Azure'da bir donanım veya yazılım hatası oluşursa, yalnızca sanal makinelerinizin bir alt etkilenir ve genel çözümünüzün kullanılabilir ve çalışır durumda kalması sağlanır.

     ![Kullanılabilirlik kümeleri arasında VM dağıtımı](media/move-vm-overview/avset.png)

* **Kullanılabilirlik alanında dağıtılan her bir katmandaki Vm'leri**: Bir katmandaki her bir VM üzerinde yapılandırılmış [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview). Bir Azure bölgesi içinde kullanılabilirlik alanı, hata etki alanı ve bir güncelleme etki alanı birleşimidir. Örneğin, bir Azure bölgesinde üç bölgelerindeki üç veya daha fazla VM oluşturursanız, sanal makinelerinizin etkili bir şekilde üç hata etki alanları ve üç güncelleştirme etki alanları arasında dağıtılır. Azure platform güncelleştirme etki alanlarında, farklı bölgelerdeki sanal makineleri aynı anda güncelleştirilmez emin olmak için bu dağıtım tanır.

     ![Kullanılabilirlik alanı dağıtımı](media/move-vm-overview/zone.png)

## <a name="move-vms-as-is-to-a-target-region"></a>Bir hedef bölgeye olduğu gibi sanal makineleri taşıma

Temel [mimarileri](#typical-architectures-for-a-multi-tier-deployment) daha önce belirtildiği gibi İşte için hedef bölgede olduğundan taşıma gerçekleştirdikten sonra dağıtımları gibi görünecektir.

* **Çeşitli katmanlarda dağıtılan tek örnekli VM'ler**

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/single-zone.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her katmanındaki VM'ler**

     ![Çapraz bölge kullanılabilirlik kümeleri](media/move-vm-overview/crossregionaset.png)

* **Kullanılabilirlik alanında dağıtılan her katmanındaki VM'ler**

     ![Kullanılabilirlik alanları genelinde VM dağıtımı](media/move-vm-overview/azonecross.png)

## <a name="move-vms-to-increase-availability"></a>Sanal makinelerin kullanılabilirliğini artırmak için Taşı

* **Çeşitli katmanlarda dağıtılan tek örnekli VM'ler**

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/single-zone.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her bir katmandaki Vm'leri**: Sanal makinelerinizin bir kullanılabilirlik, Azure Site RECOVERY'yi kullanarak, sanal makine için çoğaltmayı etkinleştirdiğinizde ayrı kullanılabilirlik kümesindeki yapılandırabilirsiniz. Taşıma işlemini tamamladıktan sonra % 99,9 oranında kullanılabilirlik SLA'sı olacaktır.

     ![Kullanılabilirlik kümeleri ve kullanılabilirlik bölgeleri arasında VM dağıtımı](media/move-vm-overview/aset-azone.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> 
> * [Azure Vm'lerini başka bir bölgeye Taşı](azure-to-azure-tutorial-migrate.md)
> 
> * [Kullanılabilirlik alanına Azure sanal makineleri taşıma](move-azure-vms-avset-azone.md)

