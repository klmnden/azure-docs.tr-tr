---
title: "VMware VM Azure'a çoğaltma için (CSP programı) çok müşterili desteği | Microsoft Docs"
description: "Çoğaltma, yük devretme ve şirket içi VMware sanal makineleri (VM'ler) CSP program aracılığıyla kurtarma Azure Azure portalını kullanarak düzenlemek için çok kiracılı ortamında Azure Site Recovery dağıtmayı açıklar"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 532dd399d2d5fcbab616744dd02f4a95078cbb1b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>CSP aracılığıyla Azure VMware sanal makineleri çoğaltmak için Azure Site Recovery çok müşterili desteği

Azure Site Recovery Kiracı abonelikler için çok kiracılı ortamlarda destekler. Oluşturulan ve Microsoft bulut çözümü sağlayıcısı (CSP) programı aracılığıyla yönetilen Kiracı abonelikler için çoklu kiracı de destekler. Bu makalede, uygulamayı ve yönetmeyi çok kiracılı VMware Azure senaryolar için rehberlik ayrıntıları verilmektedir. Oluşturma ve Kiracı aboneliklerini yönetme hakkında ayrıntılar için bkz: [CSP ile çoklu kiracı yönetme](site-recovery-manage-multi-tenancy-with-csp.md) .

Bu kılavuz, VMware sanal makinelerin Azure'a çoğaltılması için varolan belgelerindeki yoğun çizer. Daha fazla bilgi için bkz: [çoğaltmak VMware sanal makinelerini Site Recovery ile Azure'a](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda
Üç ana çok kiracılı modeli vardır:

* **Barındırma hizmetleri sağlayıcısı (HSP) paylaşılan**: fiziksel altyapı ortak sahibi ve kullandığı paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) birden çok Kiracı sanal makineleri barındırmak için aynı altyapı üzerinde. Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Barındırma hizmeti sağlayıcısı ayrılmış**: iş ortağı fiziksel altyapı sahip, ancak ayrı bir altyapı her bir kiracının sanal makineleri barındırmak için özel kaynakları (birden çok Vcenter, fiziksel datastores vb.) kullanır. Kiracı Self Servis bir çözüm olarak sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Hizmetler Sağlayıcısı (MSP) yönetilen**: Müşteri sanal makineleri barındıran fiziksel altyapı sahibi ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

## <a name="shared-hosting-multi-tenant-guidance"></a>Paylaşılan barındırma çok kiracılı Kılavuzu
Bu bölümde ayrıntılı paylaşılan barındırma senaryo kapsar. Diğer iki senaryo paylaşılan barındırma senaryo kümeleridir ve aynı ilkeleri kullanın. Paylaşılan barındırma Kılavuzu sonunda farklar açıklanmaktadır.

Temel bir çok kiracılı senaryosunda çeşitli kiracılar yalıtmak için gereksinimdir. Bir kiracı başka bir kiracı barındırılan gözlemlemek olmamalıdır. İş ortağı tarafından yönetilen bir ortamda, bu gereksinim Burada, kritik olabilir bir Self Servis ortamında, olduğu gibi önemli değil. Bu kılavuz, Kiracı yalıtımı gerekli olduğunu varsayar.

Aşağıdaki diyagramda mimari sunulur:

![Bir vCenter ile paylaşılan HSP](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Bir vCenter paylaşılan barındırma senaryoyla**

Yukarıdaki diyagramda görüldüğü gibi her bir müşteri ayrı yönetim sunucusu vardır. Bu yapılandırma Kiracı özel VM'ler Kiracı erişimi sınırlar ve Kiracı yalıtımını etkinleştirir. Bir VMware sanal makinesi çoğaltma senaryo yapılandırma sunucusu sanal makineleri bulmak ve aracıları yüklemek için hesapları yönetmek için kullanır. Çok kiracılı ortamlarda, vCenter erişim denetimi ile VM bulma kısıtlama eklenerek aynı ilkeleri uygulanır.

Veri yalıtımı gereksinim tüm hassas altyapı bilgileri (örneğin, erişim kimlik bilgileri) kiracılar için belirlenmemiş kalmasını gerekir. Bu nedenle, management server'ın tüm bileşenleri iş ortağı özel denetimi altında kalmasını öneririz. Yönetim sunucusu bileşenleri şunlardır:
* Yapılandırma sunucusu (CS)
* İşlem Sunucusu (PS)
* Ana hedef sunucusu (MT)

Bir genişleme PS da iş ortağının altında denetimdir.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Çok kiracılı senaryoda her CS iki hesap kullanır

- **vCenter erişim hesabı**: Kiracı sanal makineleri bulmak için bu hesabı kullanın. (Sonraki bölümde açıklandığı gibi) için atanan vCenter erişim izinleri vardır. Yanlışlıkla erişim sızıntıları kaçınmak için iş ortakları Yapılandırma aracında bu kimlik bilgileri kendilerini girin öneririz.

- **Sanal makine erişim hesabı**: otomatik bir anında iletme aracılığıyla Kiracı VM'ler mobility Aracısı'nı yüklemek için bu hesabı kullanın. Genellikle, bir Kiracı İş ortağı veya bir iş ortağı doğrudan yönetebilir için sağlayabilir bir etki alanı hesabı da olabilir. Bir kiracı ayrıntıları ortağıyla doğrudan paylaşmak istediğiniz değil, bunlar CS sınırlı süreli erişimi üzerinden kimlik bilgilerini girin veya iş ortağının Yardımı ile mobility aracıları el ile yüklemek için izin.

### <a name="requirements-for-a-vcenter-access-account"></a>Bir vCenter erişim hesabı gereksinimleri

Önceki bölümde belirtildiği gibi kendisine atanmış bir özel rolü olan bir hesap ile CS yapılandırmanız gerekir. Rol ataması vCenter erişim hesabını her vCenter nesne için uygulanır ve alt nesnelere yayılan değil. Yanlışlıkla erişim'deki diğer nesnelere erişim yayma sağladığından bu yapılandırmayı Kiracı yalıtımı sağlar.

![Propagate alt nesneleri seçeneği](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Kullanıcı hesabı ve veri merkezi nesnede rolü atayın ve alt nesneler için yayılması için alternatiftir. Hesabı verin bir *erişim yok* rolü için belirli bir kiracı için erişilebilir olması gereken her nesne (örneğin, diğer kiracılar VM). Bu yapılandırma sıkıcı ve her yeni alt nesne da otomatik olarak üst öğeden devralındı erişim izni olduğundan yanlışlıkla erişim denetimleri gösterir. Bu nedenle, ilk yaklaşım kullanmanızı öneririz.

VCenter hesap erişim yordam aşağıdaki gibidir:

1. Önceden tanımlanmış kopyalayarak yeni bir rol oluşturmak *salt okunur* rolü ve bir kolay ad (örneğin, Azure_Site_Recovery, bu örnekte gösterildiği gibi) verin.

2. Aşağıdaki izinleri bu role atayın:

    * **Veri deposu**: tahsis alanı, Gözat veri deposu, alt düzey dosya işlemleri, Kaldır dosyası, sanal makine dosyaları güncelleştirmesi

    * **Ağ**: ağ atama

    * **Kaynak**: kaynak havuzu, VM gücü geçirme VM atamak geçirme gücü VM

    * **Görevleri**: görev, güncelleştirme görevi oluşturma

    * **Sanal makine**:
        * Yapılandırma > tüm
        * Etkileşim > yanıt soru, cihaz bağlantısı, yapılandırma CD medyasından, yapılandırma disket ortamı, kapatma, açma, VMware araçları yükleme
        * Stok > oluşturma varolandan, yeni oluştur, kaydetme, kaydı
        * Sağlama > izin sanal makine indirme, izin sanal makine dosyaları karşıya yükleme
        * Anlık Görüntü Yönetimi > anlık görüntüleri kaldırma

    ![Rol Düzenle iletişim kutusu](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Erişim düzeyleri şu şekilde (Kiracı CS kullanılır) çeşitli nesneleri, vCenter hesabına atayın:

>| Nesne | Rol | Açıklamalar |
>| --- | --- | --- |
>| vCenter | Salt Okunur | Yalnızca farklı nesneleri yönetmek için vCenter erişime izin vermek gerekli. Vcenter yönetim işlemleri için kullanılan veya hesap için bir kiracı sağlanacak hiçbir zaman edecekse bu izni kaldırabilirsiniz. |
>| Veri merkezi | Azure_Site_Recovery |  |
>| Konak ve konak kümesi | Azure_Site_Recovery | Böylece yalnızca erişilebilir ana Kiracı sanal makineleri yük devretme öncesinde ve sonrasında yeniden çalışma erişim nesne düzeyinde yeniden sağlar. |
>| Veri deposu ve veri deposu küme | Azure_Site_Recovery | Önceki aynıdır. |
>| Ağ | Azure_Site_Recovery |  |
>| Yönetim sunucusu | Azure_Site_Recovery | CS makine dışında tüm bileşenleri (CS, PS ve MT) erişim içerir. |
>| Kiracı VM'ler | Azure_Site_Recovery | Tüm yeni Kiracı VM'ler belirli bir kiracı aynı zamanda bu erişim alın veya bunlar Azure portalı üzerinden bulunabilirlik olmaz sağlar. |

VCenter hesap erişim tamamlanmıştır. Bu adımı yeniden çalışma işlemlerini tamamlamak için minimum izinleri gereksinimini yerine getirir. Varolan ilkeleri ile bu erişim izinleri de kullanabilirsiniz. Yalnızca varolan izinlerinizi önceden ayrıntılı rol izinleri, 2. adımda içerecek şekilde değiştirin.

Yük devretme durumu kadar olağanüstü durum kurtarma işlemlerini kısıtlamak için (diğer bir deyişle, yeniden çalışma özellikleri olmayan), bir özel durum ile yukarıdaki yordamı izleyin: atama yerine *Azure_Site_Recovery* vCenter erişim hesabı'nı rol atama yalnızca bir *salt okunur* bu hesaba rol. Bu izin kümesi VM çoğaltma ve yük devretme sağlar ve geri dönme izin vermez. Önceki işleminde bir şey olduğu gibi kalır. Kiracı yalıtımı sağlamak ve VM bulma kısıtlamak için her izni yalnızca nesne düzeyinde atanmıştır ve alt nesneler için yayılan değil.

## <a name="other-multi-tenant-environments"></a>Diğer çok kiracılı ortamlarda

Önceki bölümde bir paylaşılan barındırma çözümü için çok kiracılı ortamı ayarlanacağı açıklanmıştır. Diğer iki ana çözümler barındırma ve hizmet yönetilen ayrılmış. Bu çözümlerin mimarisi aşağıdaki bölümlerde açıklanmıştır.

### <a name="dedicated-hosting-solution"></a>Barındırma çözümü ayrılmış

Aşağıdaki çizimde gösterildiği gibi mimari adanmış bir barındırma çözümüne yalnızca Kiracı için her bir kiracının altyapı ayarlandığını farktır. Kiracılar ayrı Vcenter yalıtılmış olduğundan, barındırma sağlayıcısı hala paylaşılan barındırma için sağlanan CSP adımları izlemeniz gerekir, ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP Kurulum değişmeden kalır.

![architecture-shared-hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Birden çok Vcenter ile ayrılmış barındırma senaryosu**

### <a name="managed-service-solution"></a>Yönetilen hizmet çözümü

Aşağıdaki çizimde gösterildiği gibi bir yönetilen hizmet çözümüne mimari fark her bir kiracının altyapı ayrıca diğer kiracılar altyapısından ayrı fiziksel olmasıdır. Kiracı altyapı sahibi ve olağanüstü durum kurtarma yönetmek için bir çözüm sağlayıcısı istediğinde bu senaryo genellikle bulunmaktadır. Yeniden, kiracılar farklı altyapılar aracılığıyla fiziksel olarak yalıtılmış olduğundan, CSP adımları izlemek için iş ortağı gereksinimlerini paylaşılan barındırma için sağlanan ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP sağlama değişmeden kalır.

![architecture-shared-hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Birden çok Vcenter hizmet senaryoyla yönetilen**

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md) Azure Site Recovery dağıtımları yönetmek için rol tabanlı erişim denetimi hakkında.

[Çoklu kiracı CSP ile yönetme](site-recovery-manage-multi-tenancy-with-csp.md)
