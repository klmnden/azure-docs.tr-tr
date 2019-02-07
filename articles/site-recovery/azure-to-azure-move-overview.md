---
title: Azure Iaas Vm'leri Azure Site Recovery hizmetini kullanarak başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri bir Azure bölgesinden diğerine taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5d844692b6199d93fa835da1021c9753311e17de
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823432"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Azure, kapsamlı bir şekilde yanı sıra müşteri temel büyüyor ve artan talepleri olan yeni bölgeler için destek ekleme. Hizmetler arasında aylık olarak eklenin yeni özellikleri de vardır. Bu nedenle, ne zaman farklı bir bölgeye veya kullanılabilirliğini artırmak için kullanılabilirlik alanları, Vm'lerinizin taşıyın istiyorsunuz zamanlar vardır.

Bu belge çeşitli senaryolar üzerinden burada Vm'lerinizi ve kılavuz mimari yüksek kullanılabilirlik elde etmek için hedef yapılandırılmalıdır nasıl taşımak istediğiniz size yol gösterir. 
> [!div class="checklist"]
> * [Neden Azure Vm'leri taşıyabilir](#why-would-you-move-azure-vms)
> * [Azure sanal makineleri taşıma](#how-to-move-azure-vms)
> * [Tipik mimariler](#typical-architectures-for-a-multi-tier-deployment)
> * [Bir hedef bölgeye olduğu gibi sanal makineleri taşıma](#move-azure-vms-to-another-region)
> * [Sanal makinelerin kullanılabilirliğini artırmak için Taşı](#move-vms-to-increase-availability)


## <a name="why-would-you-move-azure-vms"></a>Neden Azure Vm'leri taşıyabilir

Müşteriler, aşağıdaki nedenlerden dolayı Vm'leri Taşı:-

- Zaten dağıtılmış tek bir bölge ve yeni bir bölgeye destek, uygulamanızın veya hizmetinizin, son kullanıcılara yakın olan eklendi sonra istiyorsunuz **Vm'leriniz, yeni bölge için olduğu gibi taşımak** gecikme süresini azaltmak için. Aynı yaklaşımı alınmış abonelikleri birleştirmek istediğiniz veya idare vardır / Kuruluş kuralları taşımanız gerekir. 
- VM'nizi tek bir örnek olarak VM dağıtıldı veya kullanılabilirlik bir parçası olarak ayarlandığında, yapabilecekleriniz SLAsm kullanılabilirliğini artırmak istiyorsanız **Vm'lerinizi Taşı bir kullanılabilirlik kümesi**. 

## <a name="how-to-move-azure-vms"></a>Azure sanal makineleri taşıma
Sanal makinelerin taşınmasında aşağıdaki adımları içerir:

1. Önkoşulları doğrulama 
2. Kaynak Vm'leri hazırlama 
3. Hedef bölge hazırlama 
4. Hedef bölge - kullanımı Azure Site Recovery çoğaltma teknolojisini hedef bölgeye kaynak sanal makineden veri kopyalamak için veri kopyalama
5. Test yapılandırması: Bir çoğaltma tamamlandığında, test yapılandırması için bir üretim dışı ağ üzerinden yük devretme testi uygulayarak.
6. Taşımayı gerçekleştirmek 
7. Kaynak bölgedeki kaynakları AT 


> [!IMPORTANT]
> Şu anda Azure Site Recovery Vm'lerden bölgesinde diğerine taşınmasını destekler ve bir bölge içinde taşıma desteklemiyor. 

> [!NOTE]
> Bu adımlarla ilgili ayrıntılı yönergeler verilmiştir belgelerinde her senaryo için aşağıda belirtildiği gibi

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Çok katmanlı dağıtımı için tipik mimariler
Bölümde en yaygın dağıtım mimarileri müşteriler aracılığıyla Yürüyüşü, azure'da çok katmanlı bir uygulama için benimseyin. Biz burada sürüp bir üç katmanlı uygulamanın bir genel IP ile örnektir. Her katman – Web, uygulama ve veritabanı 2 VM'ler sahiptir ve diğer Katmanlar bir yük dengeleyiciye bağlı. Veritabanı katmanı için yüksek kullanılabilirlik (HA) VM'ler arasında SQL Always ON çoğaltma vardır.

1.  **Çeşitli katmanlarda dağıtılan örnek Vm'leri tek**-bir katmandaki her VM, yük Dengeleyiciler diğer katmanlara bağlanmış tek bir örnek VM olarak yapılandırılır. Müşteriler benimseyin en basit yapılandırmadır.

       ![tek Vm'leri](media/move-vm-overview/regular-deployment.PNG)

2. **Kullanılabilirlik kümesi dağıtılan her bir katmandaki Vm'leri** -bir katmandaki her sanal makine bir kullanılabilirlik yapılandırılmış ayarlayın. [Kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) bir kümede birden fazla yalıtılmış donanım düğümde, Azure'da dağıttığınız VM'lerin dağıtılmasını sağlar. Böylece, Azure’da bir donanım veya yazılım hatası oluşursa yalnızca sanal makinelerinizin bir alt kümesinin etkilenmesi ve genel çözümünüzün kullanılabilir ve çalışır durumda kalması sağlanır. 
   
      ![avset](media/move-vm-overview/AVset.PNG)

3. **Kullanılabilirlik kümesi dağıtılan her bir katmandaki Vm'leri** -bir katmandaki her VM üzerinde yapılandırılmış [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview). Bir Azure bölgesi içinde kullanılabilirlik alanı, hata etki alanı ve bir güncelleme etki alanı birleşimidir. Örneğin, bir Azure bölgesinde üç bölgelerindeki üç veya daha fazla VM oluşturursanız, sanal makinelerinizin etkili bir şekilde üç hata etki alanları ve üç güncelleştirme etki alanları arasında dağıtılır. Azure platform güncelleştirme etki alanlarında, farklı bölgelerdeki sanal makineleri aynı anda güncelleştirilmez emin olmak için bu dağıtım tanır.

      ![Bölge deploymnt](media/move-vm-overview/zone.PNG)



 ## <a name="move-vms-as-is-to-a-target-region"></a>Bir hedef bölgeye olduğu gibi sanal makineleri taşıma

Yukarıdaki bilgilere bağlı temel bahsedilen [mimarileri](#typical-architectures-for-a-multi-tier-deployment), heres nasıl dağıtımları görünür için hedef bölgede olduğundan taşıma kez gerçekleştirdiğiniz gibi.


1. **Tek Örnekli VM'ler çeşitli katmanlarda dağıtılan** 

     ![tek bölge. PNG](media/move-vm-overview/single-zone.PNG)

2. **Kullanılabilirlik kümesi dağıtılan her katmanındaki VM'ler**

     ![crossregionAset.PNG](media/move-vm-overview/crossregionAset.PNG)


3. **Kullanılabilirlik alanı dağıtılan her katmanındaki VM'ler**
      

     ![AzoneCross.PNG](media/move-vm-overview/AzoneCross.PNG)

## <a name="move-vms-to-increase-availability"></a>Sanal makinelerin kullanılabilirliğini artırmak için Taşı

1. **Tek Örnekli VM'ler çeşitli katmanlarda dağıtılan** 

     ![tek bölge. PNG](media/move-vm-overview/single-zone.PNG)

2. **Kullanılabilirlik kümesi dağıtılan her bir katmandaki Vm'leri** -kullanılabilirlik ayrı kullanılabilirlik alanına, sanal Makineye yönelik çoğaltmayı etkinleştirmek Azure Site Recovery kullanarak seçtiğinizde kümesi Vm'lerinizin yerleştirmek için yapılandırmayı seçebilirsiniz. Taşıma işlemini tamamladıktan sonra % 99,9 oranında kullanılabilirlik SLA'sı olacaktır.

      ![aset Azone.PNG](media/move-vm-overview/aset-Azone.PNG)


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, sanal makinelerin taşınmasında genel Kılavuzu okuyun. Bu adım adım yürütme bilmek daha fazlasını okuyun:


> [!div class="nextstepaction"]
> * [Azure Vm'lerini başka bir bölgeye Taşı](azure-to-azure-tutorial-migrate.md)

> * [Kullanılabilirlik alanına Azure sanal makineleri taşıma](move-azure-VMs-AVset-Azone.md)

