---
title: "VMware VM çoğaltması için Azure (Azure Site RECOVERY'yi kullanarak CSP) çok müşterili desteği'ne genel bakış | Microsoft Docs"
description: "Azure Site Recovery Destek genel bir bakış Kiracı için CSP program aracılığıyla bir çok kiracılı ortamı Aboneliklerde sağlar."
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 9b4fbb34686a12f992b344ac61420c9ba99ee405
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="overview-of-multi-tenant-support-for-vmware-replication-to-azure-with-csp"></a>CSP ile Azure'da VMware çoğaltma işlemini çok müşterili desteği'ne genel bakış

[Azure Site Recovery](site-recovery-overview.md) Kiracı abonelikler için çok kiracılı ortamlarda destekler. Oluşturulan ve Microsoft bulut çözümü sağlayıcısı (CSP) programı aracılığıyla yönetilen Kiracı abonelikler için çoklu kiracı de destekler. 

Bu makalede, uygulamayı ve yönetmeyi çok kiracılı VMware Azure çoğaltma için bir genel bakış sağlar. 

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda

Üç ana çok kiracılı modeli vardır:

* **Barındırma hizmetleri sağlayıcısı (HSP) paylaşılan**: fiziksel altyapı ortak sahibi ve kullandığı paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) birden çok Kiracı sanal makineleri barındırmak için aynı altyapı üzerinde. Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Barındırma hizmeti sağlayıcısı ayrılmış**: iş ortağı fiziksel altyapı sahip, ancak ayrı bir altyapı her bir kiracının sanal makineleri barındırmak için özel kaynakları (birden çok Vcenter, fiziksel datastores vb.) kullanır. Kiracı Self Servis bir çözüm olarak sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Hizmetler Sağlayıcısı (MSP) yönetilen**: Müşteri sanal makineleri barındıran fiziksel altyapı sahibi ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

## <a name="shared-hosting-services-provider-hsp"></a>Paylaşılan barındırma hizmeti sağlayıcısı (HSP)

 Diğer iki senaryo paylaşılan barındırma senaryo kümeleridir ve aynı ilkeleri kullanın. Paylaşılan barındırma Kılavuzu sonunda farklar açıklanmaktadır.

Temel bir çok kiracılı senaryosunda kiracılar yalıtılmış gereksinimdir. Bir kiracı başka bir kiracı barındırılan gözlemlemek olmamalıdır. İş ortağı tarafından yönetilen bir ortamda, bu gereksinim Burada, kritik olabilir bir Self Servis ortamında, olduğu gibi önemli değil. Bu makalede, Kiracı yalıtımı gerekli olduğunu varsayar.

Aşağıdaki diyagramda mimari gösterilir.

![Bir vCenter ile paylaşılan HSP](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Paylaşılan barındırma bir vCenter sunucusu ile**

Diyagramdaki her bir müşteri bir ayrı yönetim sunucusu vardır. Bu yapılandırma Kiracı özel VM'ler Kiracı erişimi sınırlar ve Kiracı yalıtımını etkinleştirir. VMware VM çoğaltma yapılandırma sunucusu sanal makineleri bulmak ve aracıları yüklemek için kullanır. Çok kiracılı ortamlarda, vCenter erişim denetimi kullanarak VM bulmayı sınırlandırma eklenmesi ile aynı ilkeleri uygulanır.

Veri yalıtımı gereksinim tüm hassas altyapı bilgileri (örneğin, erişim kimlik bilgileri) kiracılar için belirlenmemiş kaldığı anlamına gelir. Bu nedenle, management server'ın tüm bileşenleri iş ortağı özel denetimi altında kalmasını öneririz. Yönetim sunucusu bileşenleri şunlardır:

* Yapılandırma sunucusu)
* İşlem sunucusu
* Ana hedef sunucusu

Ayrı bir ölçeklendirilmiş işlem sunucusu da iş ortağının altında denetimdir.

## <a name="configuration-server-accounts"></a>Yapılandırma sunucusu hesapları

Her yapılandırma sunucusu çoklu kiracı senaryoda iki hesap kullanır:

- **vCenter erişim hesabı**: Bu hesap, Kiracı sanal makineleri bulmak için kullanılır. Kendisine atanmış vCenter erişim izinleri vardır. Erişim sızıntıları kaçınmak için iş ortakları Yapılandırma aracında bu kimlik bilgileri kendilerini girin öneririz.

- **Sanal makine erişim hesabı**: Bu hesap, Kiracı sanal makineleri, otomatik bir anında iletme ile Mobility hizmeti aracısı yüklemek için kullanılır. Genellikle, bir kiracı için bir iş ortağı sağlayabilir bir etki alanı hesabı veya iş ortağı doğrudan yönetebilir bir hesabı olur. Bir kiracı ayrıntıları ortağıyla doğrudan paylaşmak istiyorsanız değil, zaman sınırlı erişim ile kimlik bilgileri yapılandırma sunucusuna girebilirsiniz. Ya da iş ortağının Yardımı ile bunlar Mobility Hizmeti Aracısı el ile yükleyebilirsiniz.

## <a name="vcenter-account-requirements"></a>vCenter hesabı gereksinimleri

Kendisine atanmış bir özel rolü olan bir hesap ile yapılandırma sunucusu yapılandırmanız gerekir. 

- Rol ataması gerekir vCenter erişim hesabını her vCenter nesne için uygulanır ve alt nesnelere yayılan değil. Yanlışlıkla erişim'deki diğer nesnelere erişim yayma sağladığından bu yapılandırmayı Kiracı yalıtımı sağlar.

    ![Propagate alt nesneleri seçeneği](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- Kullanıcı hesabı ve veri merkezi nesnede rolü atayın ve alt nesneler için yayılması için alternatif yaklaşımdır bakın. Hesabı verin bir **erişim yok** rolü (örneğin, diğer kiracılar ait VM'ler) her nesne için olması gereken belirli bir kiracı için erişilemez. Bu sıkıcı bir yapılandırmadır. Her yeni alt nesne da otomatik olarak üst öğeden devralındı erişim izni olduğundan yanlışlıkla erişim denetimleri kullanıma sunar. Bu nedenle, ilk yaklaşım kullanmanızı öneririz.

### <a name="create-a-vcenter-account"></a>Bir vCenter hesabı oluşturun

1. Önceden tanımlanmış kopyalayarak yeni bir rol oluşturmak *salt okunur* rolü ve bir kolay ad (örneğin, Azure_Site_Recovery, bu örnekte gösterildiği gibi) verin.
2. Aşağıdaki izinleri bu role atayın:

    * **Veri deposu**: tahsis alanı, Gözat veri deposu, alt düzey dosya işlemleri, Kaldır dosyası, sanal makine dosyaları güncelleştirmesi
    * **Ağ**: ağ atama
    * **Kaynak**: kaynak havuzu, VM gücü geçirme VM atamak geçirme gücü VM
    * **Görevleri**: görev, güncelleştirme görevi oluşturma
    * **VM - yapılandırma**: tüm
    - **VM - etkileşim** > yanıt soru, cihaz bağlantısı, yapılandırma CD medyasından, yapılandırma disket ortamı, kapatma, açma, VMware araçları yükleme
    - **VM - stok** > oluşturma varolandan, yeni oluştur, kaydetme, kaydı
    - **VM - sağlama** > izin sanal makine indirme, izin sanal makine dosyaları karşıya yükleme
    - **VM - anlık görüntü Yönetimi** > anlık görüntüleri kaldırma

        ![Rol Düzenle iletişim kutusu](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. Erişim düzeyleri (Kiracı yapılandırma sunucusunda kullanılan) vCenter hesabına çeşitli nesneler için aşağıdaki gibi atayın:

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

### <a name="failover-only"></a>Yalnızca Yük devretme
Yalnızca Yük devretme kadar olağanüstü durum kurtarma işlemlerini kısıtlamak için (diğer bir deyişle, yeniden çalışma özellikleri olmayan), aşağıdaki istisnalar önceki yordamı kullanın:

- Atama yerine *Azure_Site_Recovery* vCenter erişim hesabı'nı rol atama yalnızca bir *salt okunur* bu hesaba rol. Bu izin kümesi VM çoğaltma ve yük devretme sağlar ve geri dönme izin vermez.
- Önceki işleminde bir şey olduğu gibi kalır. Kiracı yalıtımı sağlamak ve VM bulma kısıtlamak için her izni yalnızca nesne düzeyinde atanmıştır ve alt nesneler için yayılan değil.


## <a name="dedicated-hosting-solution"></a>Barındırma çözümü ayrılmış

Aşağıdaki çizimde gösterildiği gibi mimari adanmış bir barındırma çözümüne yalnızca Kiracı için her bir kiracının altyapı ayarlandığını farktır. Kiracılar ayrı Vcenter yalıtılmış olduğundan, barındırma sağlayıcısı hala paylaşılan barındırma için sağlanan CSP adımları izlemeniz gerekir, ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP Kurulum değişmeden kalır.

![architecture-shared-hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Birden çok Vcenter ile ayrılmış barındırma senaryosu**

## <a name="managed-service-solution"></a>Yönetilen hizmet çözümü

Aşağıdaki çizimde gösterildiği gibi bir yönetilen hizmet çözümüne mimari fark her bir kiracının altyapı ayrıca diğer kiracılar altyapısından ayrı fiziksel olmasıdır. Kiracı altyapı sahibi ve olağanüstü durum kurtarma yönetmek için bir çözüm sağlayıcısı istediğinde bu senaryo genellikle bulunmaktadır. Yeniden, kiracılar farklı altyapılar aracılığıyla fiziksel olarak yalıtılmış olduğundan, CSP adımları izlemek için iş ortağı gereksinimlerini paylaşılan barındırma için sağlanan ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP sağlama değişmeden kalır.

![architecture-shared-hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Birden çok Vcenter hizmet senaryoyla yönetilen**

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md) Site kurtarma rol tabanlı erişim denetimi hakkında.
Bilgi nasıl [VMware Vm'lerini olağanüstü durum kurtarma Azure ayarlama](vmware-azure-tutorial.md)
[CSP ile çoklu kiracı olan VMWare VM'ler için olağanüstü durum kurtarma ayarlayın](vmware-azure-multi-tenant-csp-disaster-recovery.md)
