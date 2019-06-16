---
title: Azure Site Recovery ve bulut çözümü sağlayıcısı (CSP) programını kullanarak çok kiracılı ortamında VMware olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Çok müşterili bir ortamda Azure Site Recovery ile VMware olağanüstü durum kurtarma ayarlamayı açıklar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 77b64f09b7fd1429eb23c4407c729dfc0aafdf2b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60461026"
---
# <a name="set-up-vmware-disaster-recovery-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>Bulut çözümü sağlayıcısı (CSP) programı ile çok kiracılı ortamında VMware olağanüstü durum kurtarmayı ayarlama

[CSP programı](https://partner.microsoft.com/en-US/cloud-solution-provider) Office 365, Enterprise Mobility Suite ve Microsoft Azure gibi Microsoft bulut Hizmetleri için birlikte daha iyi hikayeler destekler. CSP, iş ortakları uçtan uca ilişkisi olan müşterilerin kendi ve birincil ilişki için bağlantı noktası haline gelir. İş ortakları, müşterilerin Azure abonelikleri dağıtın ve abonelikleri kendi katma değer, özelleştirilmiş teklifleri ile birleştirin.

İle [Azure Site Recovery](site-recovery-overview.md), iş ortakları CSP aracılığıyla doğrudan müşteriler için olağanüstü durum kurtarma yönetebilirsiniz. Alternatif olarak, Site Recovery ortamı ayarlamak için CSP kullanın ve müşterilerin kendi olağanüstü durum kurtarma ihtiyaçlarınızı Self Servis bir şekilde yönetmesine olanak tanır. Her iki senaryoda, Site Recovery ile müşterileri arasındaki ilişkiler ortaklarıdır. İş ortakları, müşteri ilişkisi hizmet ve Site Recovery kullanım için müşterilerin fatura.

Bu makalede, nasıl bir iş ortağı olarak oluşturabilir ve bir çok kiracılı VMware çoğaltma senaryosu, CSP aracılığıyla Kiracı Aboneliklerini yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

VMware çoğaltma işlemini ayarlama için aşağıdakileri yapmanız gerekir:

- [Hazırlama](tutorial-prepare-azure.md) bir Azure aboneliği, bir Azure sanal ağı ve bir depolama hesabı dahil olmak üzere, Azure kaynaklarının.
- [Hazırlama](vmware-azure-tutorial-prepare-on-premises.md) şirket içi VMware sunucularını ve Vm'leri.
- Her Kiracı için Kiracı Vm'leri ile iletişim kurabilen bir ayrı yönetim sunucusu ve vCenter sunucularınızın oluşturun. Yalnızca bir iş ortağı olarak, bu yönetim sunucusuna erişim hakları olmalıdır. Daha fazla bilgi edinin [çok kiracılı ortamlarda](vmware-azure-multi-tenant-overview.md).

## <a name="create-a-tenant-account"></a>Bir kiracı hesabı oluşturun

1. Aracılığıyla [Microsoft Partner Center](https://partnercenter.microsoft.com/), CSP hesabınızda oturum açın.
2. Üzerinde **Pano** menüsünde **müşteriler**.
3. Açılan sayfada tıklatın **Ekle müşteri** düğmesi.
4. İçinde **yeni müşteri** sayfasında, Kiracı için hesap bilgileri ayrıntıları girin.

    ![Hesap bilgileri sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Ardından **sonraki: Abonelikleri**.
6. Abonelik Seçimi sayfasında, seçin **Microsoft Azure** onay kutusu. Diğer abonelikler artık veya başka bir zaman ekleyebilirsiniz.
7. Üzerinde **gözden geçirme** sayfasında Kiracı ayrıntılarını doğrulayın ve ardından **Gönder**.
8. Kiracı hesabı oluşturduktan sonra varsayılan hesap ve parolayı Bu abonelik için ayrıntıları görüntüleyen bir onay sayfası görüntülenir. Bilgileri kaydedin ve daha sonra gerektiğinde, Azure portalı oturum açma sayfası aracılığıyla parolayı değiştirin.

Kiracı olarak bu bilgileri paylaşabilir veya oluşturabilir ve gerekirse ayrı bir hesap paylaşın.

## <a name="access-the-tenant-account"></a>Kiracı hesabına erişme

Kiracının abonelik Microsoft iş ortağı merkezi panosuna erişebilirsiniz.

1. Üzerinde **müşteriler** sayfasında, Kiracı hesabı adına tıklayın.
2. İçinde **abonelikleri** sayfası Kiracı hesabını, mevcut hesabı abonelikleri izleyebilir ve daha fazla abonelik gerektiği gibi ekleyin.
3. Kiracının olağanüstü durum kurtarma işlemlerini yönetmek için seçin **tüm kaynakları (Azure portalı)** . Bu kiracının Azure aboneliklerine erişim verir.

    ![Tüm kaynaklar bağlantı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. Erişim üst kısmında Azure Active Directory bağlantısını tıklatarak doğrulayabilirsiniz Azure portal'ın sağ.

    ![Azure Active Directory bağlantısı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

Şimdi gerçekleştirmek ve Azure portalında bir kiracının tüm Site Recovery işlemlerini yönetme. Kiracı aboneliği, yönetilen olağanüstü durum kurtarma için CSP aracılığıyla erişmek için yukarıda açıklanan işlemi izleyin.

## <a name="assign-tenant-access-to-the-subscription"></a>Aboneliği için Kiracı erişim atama

1. Olağanüstü durum kurtarma altyapısı ayarlandığından emin olun. İş ortakları, olağanüstü durum kurtarma yönetilip yönetilmediği veya Self Servis bağımsız olarak CSP Portalı aracılığıyla Kiracı aboneliklerine erişin. Kasası ayarlama ve Kiracı aboneliklerine altyapılar kaydedebilir.
2. Kiracıyla sağlamak [oluşturduğunuz hesabı](#create-a-tenant-account).
3. Size yeni bir kullanıcı CSP Portalı aracılığıyla Kiracı aboneliği için şu şekilde ekleyebilirsiniz:

    (a) kiracının CSP aboneliği sayfasına gidin ve ardından **kullanıcıları ve lisansları** seçeneği.

      ![Kiracının CSP aboneliği sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

    (b) artık ilgili ayrıntıları girerek ve izinlerini seçmek veya bir CSV dosyasındaki kullanıcı listesi yükleyerek yeni bir kullanıcı oluşturun.
    
    c) yeni bir kullanıcı oluşturduktan sonra Azure portalına geri dönün. İçinde **abonelik** sayfasında, ilgili aboneliği seçin.

    d) select **erişim denetimi (IAM)** ve ardından **rol atamaları**.

    e) tıklayın **rol ataması Ekle** ilgili erişim düzeyine sahip bir kullanıcı eklemek için. CSP Portalı aracılığıyla oluşturulan kullanıcılar rol atamalarını sekmesinde görüntülenir.

      ![Kullanıcı ekleme](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Çoğu yönetim işlemleri için *katkıda bulunan* rolüdür yeterli. Bu erişim düzeyine sahip kullanıcılar bir Abonelikteki erişim düzeylerini değiştirme dışında her şeyi yapabilir (hangi *sahibi*-erişim düzeyi gereklidir).
- Site Recovery ayrıca sahip üç [önceden tanımlı kullanıcı rolleri](site-recovery-role-based-linked-access-control.md), daha fazla erişim düzeyleri gerektiği şekilde kısıtlamak için kullanılabilir.

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda

Üç ana çok kiracılı modeli vardır:

* **Paylaşılan barındırma hizmet sağlayıcısı (HSP)** : İş ortağı fiziksel altyapı sahip ve aynı altyapıda paylaşılan kaynaklar (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) birden çok Kiracı Vm'leri barındırmak için kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma Yönetimi sağlayabilir veya Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir.

* **Barındırma hizmet sağlayıcısı adanmış**: İş ortağı fiziksel altyapı sahip, ancak ayrı bir altyapı üzerinde her bir kiracının sanal makineleri barındırmak için adanmış kaynaklar (birden çok vCenters, fiziksel veri depoları vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma Yönetimi sağlayabilir veya Kiracı Self Servis bir çözüm olarak bulunabilir.

* **Yönetilen Hizmetler Sağlayıcısı (MSP)** : Müşteri sanal makineleri barındıran fiziksel altyapı sahip olan ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

Bu makalede anlatıldığı gibi Kiracı abonelikleri ayarlayarak, müşterilerin herhangi bir ilgili çok kiracılı modeli etkinleştirme hızlıca başlatabilirsiniz. Farklı çok kiracılı modelleri hakkında daha fazla bilgi ve şirket içi etkinleştirme erişim denetimleri [burada](vmware-azure-multi-tenant-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [rol tabanlı erişim denetimi](site-recovery-role-based-linked-access-control.md) Azure Site Recovery dağıtımları yönetmek için.
- Vmware'den azure'a hakkında daha fazla bilgi [çoğaltma mimarisi](vmware-azure-architecture.md).
- [Öğreticiyi gözden](vmware-azure-tutorial.md) VMware Vm'lerini Azure'a çoğaltma için.
Daha fazla bilgi edinin [çok kiracılı ortamlarda](vmware-azure-multi-tenant-overview.md) VMware Vm'lerini Azure'a çoğaltma için.
