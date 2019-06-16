---
title: Birden çok kiracıyı destekleyen VMware VM'LERİNDE olağanüstü durum kurtarma için Azure (Azure Site Recovery kullanarak CSP) için genel bakış | Microsoft Docs
description: Çok kiracılı ortam (CSP) programında azure'a VMWare olağanüstü durum kurtarma için Azure Site Recovery desteğine genel bakış sağlar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: d227b8d038dd686bde9b031ca2c58adc7dd6d76b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60718130"
---
# <a name="overview-of-multi-tenant-support-for-vmware-disaster-recovery-to-azure-with-csp"></a>VMware olağanüstü durum kurtarma için Azure CSP ile çok kiracılı desteği'ne genel bakış

[Azure Site Recovery](site-recovery-overview.md) Kiracı abonelikler için çok kiracılı ortamlarını destekler. Oluşturulan ve Microsoft bulut çözümü sağlayıcısı (CSP) programı aracılığıyla yönetilen Kiracı abonelikler için çok kiracılı modeli de destekler.

Bu makalede, uygulama ve çok kiracılı Vmware'den Azure'a çoğaltma yönetmeye genel bakış sağlar.

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda

Üç ana çok kiracılı modeli vardır:

* **Paylaşılan barındırma hizmet sağlayıcısı (HSP)** : İş ortağı fiziksel altyapı sahip ve aynı altyapıda paylaşılan kaynaklar (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) birden çok Kiracı Vm'leri barındırmak için kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma Yönetimi sağlayabilir veya Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir.

* **Barındırma hizmet sağlayıcısı adanmış**: İş ortağı fiziksel altyapı sahip, ancak ayrı bir altyapı üzerinde her bir kiracının sanal makineleri barındırmak için adanmış kaynaklar (birden çok vCenters, fiziksel veri depoları vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma Yönetimi sağlayabilir veya Kiracı Self Servis bir çözüm olarak bulunabilir.

* **Yönetilen Hizmetler Sağlayıcısı (MSP)** : Müşteri sanal makineleri barındıran fiziksel altyapı sahip olan ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

## <a name="shared-hosting-services-provider-hsp"></a>Paylaşılan barındırma hizmet sağlayıcısı (HSP)

Diğer iki senaryo paylaşılan barındırma senaryo kümeleridir ve bunların aynı ilkeleri kullanın. Paylaşılan barındırma Kılavuzu sonunda farklar açıklanmaktadır.

Temel bir çok kiracılı senaryosunda kiracılar yalıtılmış gereksinimidir. Bir kiracı ne, başka bir kiracıda barındırılan gözlemleyin olmamalıdır. İş ortağı tarafından yönetilen bir ortamda, bu gereksinim, burada kritik olabilir bir Self Servis ortamında olduğu gibi önemli değil. Bu makalede, Kiracı yalıtımı gerekli olduğunu varsayar.

Aşağıdaki diyagramda mimari gösterilmektedir.

![Bir vCenter ile paylaşılan HSP](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Paylaşılan barındırma bir vCenter sunucusu ile**

Diyagramda, her müşteri ayrı yönetim sunucusu vardır. Bu yapılandırma kiracıya özgü Vm'leri Kiracı erişimi sınırlar ve Kiracı yalıtımını etkinleştirir. VMware VM çoğaltma Vm'leri bulma ve aracıları yüklemek için yapılandırma sunucusunu kullanır. VCenter erişim denetimi kullanarak VM bulmayı sınırlandırma eklenmesi ile çok kiracılı ortamları için aynı ilkeler geçerlidir.

Veri yalıtımı gereksinim tüm hassas altyapı bilgileri (örneğin, erişim kimlik bilgileri) kiracılara belirlenmemiş kaldığı anlamına gelir. Bu nedenle, tüm bileşenleri yönetim sunucusunun iş ortağı özel denetimi altında kalmasını öneririz. Yönetim sunucusu bileşenleri şunlardır:

* Yapılandırma sunucusu
* İşlem sunucusu
* Ana hedef sunucusu

Ayrı bir genişleme işlem sunucusu ayrıca iş ortağının altında denetimidir.

## <a name="configuration-server-accounts"></a>Configuration server hesapları

Her yapılandırma sunucusu çoklu kiracı senaryosunda, iki hesap kullanır:

- **vCenter hesabının**: Bu hesap, Kiracı Vm'leri bulmak için kullanılır. Kendisine atanmış vCenter erişim izinleri var. Erişim sızıntılarını önlemeye yardımcı olmak için iş ortakları Yapılandırma Aracı'nda bu kimlik girin öneririz.

- **Sanal makine erişim hesabı**: Bu hesap, Kiracı sanal makineleri, otomatik bir anında iletme ile Mobility hizmeti aracısı yüklemek için kullanılır. Genellikle, bir kiracı için bir iş ortağı sağlayabilir bir etki alanı hesabı veya iş ortağı doğrudan yönetebileceği bir hesabı olur. Bir kiracı ayrıntılarını iş ortağıyla doğrudan paylaşmak istediğiniz değil, sınırlı süreli erişim kimlik bilgilerini yapılandırma sunucusuna girebilirsiniz. Veya, iş ortağının yüklemişse, Mobility Hizmeti Aracısı el ile yükleyebilirsiniz.

## <a name="vcenter-account-requirements"></a>vCenter hesabı gereksinimleri

Yapılandırma sunucusu, özel bir rol atanmış olan bir hesap ile yapılandırın.

- Rol ataması gerekir vCenter erişim hesabını her vCenter nesne için uygulanır ve yayılan alt nesneler için yok. Diğer nesnelere erişim yayma yanlışlıkla erişim sağladığından, bu yapılandırma Kiracı yalıtımı sağlar.

    ![Alt nesneleri seçeneği yay](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- Kullanıcı hesabı ve veri merkezi nesnesi rolü atayın ve bunları alt nesneleri yaymak için alternatif bir yaklaşım olan. Ardından hesabı verin bir **erişemez** rol (örneğin, diğer kiracılara ait VM'ler) her nesne için olması gereken belirli bir kiracıya erişilemez. Bu yapılandırma, yavaşlatan bir yöntemdir. Her yeni alt nesne da otomatik olarak bir üst nesneden devralındıysa erişim verilir çünkü yanlışlıkla erişim denetimlerini gösterir. Bu nedenle, bir ilk yaklaşım kullanmanızı öneririz.

### <a name="create-a-vcenter-account"></a>Bir vCenter hesabı oluşturun

1. Önceden tanımlanmış kopyalayarak yeni bir rol oluşturmak *salt okunur* rolünü ve ardından (örneğin, bu örnekte gösterildiği gibi Azure_Site_Recovery) uygun bir ad verin.
2. Bu rol için aşağıdaki izinler atayın:

   * **Veri deposu**: Ayırma boşluk, göz atma veri deposu, düşük düzeyli dosya işlemleri, dosya, sanal makine dosyalarını güncelleştirme Kaldır
   * **Ağ**: Ağ atama
   * **Kaynak**: VM güç VM geçişi, VM üzerinde desteklenen geçiş kaynak havuzuna atayın
   * **Görevleri**: Görev, güncelleme görevi oluşturun
   * **VM - yapılandırma**: Tümü
   * **VM - etkileşim** > yanıt soru, cihaz bağlantısı, CD yapılandırma ortamı, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme
   * **VM - Envanter** > oluşturma varolandan Yeni Oluştur, kaydetme, kaydı kaldırma
   * **VM - sağlama** > sanal makine indirmeye izin, izin sanal makine dosyalarını karşıya yükleme
   * **VM - anlık görüntü Yönetimi** > anlık görüntüleri kaldırma

       ![Rolü Düzenle iletişim kutusu](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. (Kiracı yapılandırma sunucusunda kullanılan) vCenter hesabı için erişim düzeyleri gibi çeşitli nesneler için ata:

>| Object | Rol | Açıklamalar |
>| --- | --- | --- |
>| vCenter | Salt okunur | Yalnızca farklı nesneleri yönetmek için vCenter erişime izin vermek gerekli. Tüm yönetim işlemleri vcenter için kullanılan veya hesabı bir kiracıya sağlanacak hiçbir zaman edecekse bu izni kaldırabilirsiniz. |
>| Veri merkezi | Azure_Site_Recovery |  |
>| Konak ve konak kümesi | Azure_Site_Recovery | Böylece yalnızca erişilebilir ana Kiracı Vm'leri yük devretmeden önce ve sonra yeniden çalışma nesne düzeyinde erişim olmasını yeniden sağlar. |
>| Veri deposu ve veri deposu küme | Azure_Site_Recovery | Önceki aynıdır. |
>| Ağ | Azure_Site_Recovery |  |
>| Yönetim sunucusu | Azure_Site_Recovery | CS makine dışında tüm bileşenleri (CS, PS ve MT) erişimi içerir. |
>| Kiracı VM | Azure_Site_Recovery | Herhangi bir yeni Kiracı Vm'leri belirli bir kiracının aynı zamanda bu erişim elde veya bunlar Azure Portalı aracılığıyla bulunabilir olmayacaktır sağlar. |

VCenter hesabı erişimi tamamlanmıştır. Bu adım, yeniden çalışma işlemlerini tamamlamak için en düşük izinlere gereksinimi karşılar. Bu izinler, mevcut ilkeleri ile de kullanabilirsiniz. Yalnızca mevcut izinlerinizi daha önce ayrıntılı rol izinleri, 2. adımda içerecek şekilde değiştirin.

### <a name="failover-only"></a>Yalnızca Yük devretme
Yalnızca Yük devretme gönderinizi olağanüstü durum kurtarma işlemlerini kısıtlamak için (diğer bir deyişle, yeniden çalışma özellikleri olmadan), bu özel durumları ile önceki yordamı kullanın:

- Atama yerine *Azure_Site_Recovery* vCenter erişim hesabı rol atama yalnızca bir *salt okunur* bu hesaba rol. VM çoğaltma ve yük devretme bu izin kümesi sağlar ve yeniden çalışma izin vermiyor.
- Önceki işlemin her şey, olduğu gibi kalır. Kiracı yalıtımı sağlamak ve VM bulma kısıtlamak için her izin hala yalnızca nesne düzeyinde atanmış olup yayılan alt nesneler için yok.

### <a name="deploy-resources-to-the-tenant-subscription"></a>Kiracı aboneliği için kaynakları dağıtma

1. Azure portalında bir kaynak grubu oluşturun ve sonra bir kurtarma Hizmetleri kasası normal işlem başına dağıtın.
2. Kasa kayıt anahtarını indir
3. CS, Kiracı için kasa kayıt anahtarını kullanarak kaydedin.
4. İki erişim hesapları, vCenter sunucusuna erişmek için hesabınız ve sanal Makineye erişmek için hesap kimlik bilgilerini girin.

    ![Yönetici yapılandırması sunucu hesapları](./media/vmware-azure-multi-tenant-overview/config-server-account-display.png)

### <a name="register-servers-in-the-vault"></a>Sunucuları kasaya kaydedin.

1. Daha önce oluşturduğunuz kasa Azure portalında oluşturduğunuz vCenter hesabı kullanarak yapılandırma sunucusunu, vCenter Server'a kaydeder.
2. "Altyapıyı Hazırlama" işlemi için Site Recovery normal işlemini tamamlayın.
3. Sanal makinelerin çoğaltılması artık hazırsınız. Yalnızca kiracının sanal makineleri içinde görüntülendiğini doğrulayın **çoğaltmak** > **sanal makineleri**.

## <a name="dedicated-hosting-solution"></a>Ayrılmış barındırma çözümü

Aşağıdaki diyagramda gösterildiği gibi mimari, adanmış bir barındırma çözümü yalnızca bu Kiracı için her bir kiracının altyapı ayarlandığını farktır.

![Mimari paylaşılan hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Birden çok vCenters ayrılmış barındırma senaryosunda**

## <a name="managed-service-solution"></a>Yönetilen hizmet çözümü

Aşağıdaki diyagramda gösterildiği gibi yönetilen hizmet çözümü mimari fark her bir kiracının altyapısı ayrıca diğer kiracıların altyapısından ayrı fiziksel olmasıdır. Bu senaryo genellikle Kiracı altyapıya sahip ve istediği olağanüstü durum kurtarma yönetimi için bir çözüm sağlayıcısı yok.

![Mimari paylaşılan hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Birden çok vCenters hizmet senaryosuyla yönetilen**

## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md) Site recovery'de rol tabanlı erişim denetimi ile ilgili.
- Bilgi edinmek için nasıl [Azure'da VMware vm'lerinin olağanüstü durum kurtarma ayarlama](vmware-azure-tutorial.md).
- Daha fazla bilgi edinin [VMWare Vm'leri için CSP ile çok kiracılı](vmware-azure-multi-tenant-csp-disaster-recovery.md).
