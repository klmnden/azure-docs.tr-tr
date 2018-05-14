---
title: Azure'da VMware çoğaltma işlemini Site kurtarma ve bulut çözümü sağlayıcısı (CSP) programı kullanarak çoklu kiracı ortamında ayarlama | Microsoft Docs
description: Oluşturma ve CSP Kiracı aboneliği yönetme ve çok kiracılı kurulumunda Azure Site Recovery dağıtma açıklar
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 58ea2e7c387137f974425464ef2c9d17f438ba7c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="set-up-vmware-replication-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>VMware çoğaltma bulut çözümü sağlayıcısı (CSP) programı ile çoklu kiracı ortamında ayarlama

[CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) Office 365, Enterprise Mobility Suite ve Microsoft Azure dahil olmak üzere Microsoft bulut Hizmetleri için daha iyi birlikte hikayeleri destekler. CSP, iş ortakları uçtan uca ilişki müşterilerle sahip ve için birincil ilişki için bağlantı noktası olur. İş ortakları, müşteriler için Azure abonelikleri dağıtmak ve kendi katma değer, özelleştirilmiş teklifleri aboneliklerle birleştirin.

İle [Azure Site Recovery](site-recovery-overview.md), iş ortakları CSP aracılığıyla doğrudan müşteriler için olağanüstü durum kurtarma yönetebilirsiniz. Alternatif olarak, Site Recovery ortamları ayarlama için CSP kullanın ve müşterilerin kendi olağanüstü durum kurtarma gereksinimlerini Self Servis bir şekilde yönetmenize olanak tanır. Her iki durumda da, Site Recovery müşterilerine arasındaki ilişkiler ortaklarıdır. İş ortakları müşteri ilişkisine hizmet ve müşteriler Site Recovery kullanımı için fatura.

Bu makalede, nasıl bir iş ortağı olarak oluşturabilir ve bir çok kiracılı VMware çoğaltma senaryo için CSP, aracılığıyla Kiracı Aboneliklerini yönetmek açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

VMware çoğaltmayı ayarlama için aşağıdakileri yapmanız gerekir:

- [Hazırlama](tutorial-prepare-azure.md) bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gibi Azure kaynakları.
- [Hazırlama](vmware-azure-tutorial-prepare-on-premises.md) VMware sunucularını ve Vm'leri şirket.
- Her bir kiracı için Kiracı VM'ler ile iletişim kurabilen bir ayrı yönetim sunucusu ve vCenter sunucusu oluşturun. Yalnızca bir iş ortağı olarak, bu yönetim sunucusuna erişim hakları olmalıdır. Daha fazla bilgi edinmek [çok kiracılı ortamlarda](vmware-azure-multi-tenant-overview.md).

## <a name="create-a-tenant-account"></a>Bir kiracı hesabı oluşturun

1. Aracılığıyla [Microsoft Partner Center](https://partnercenter.microsoft.com/), CSP hesabınızda oturum açın.
2. Üzerinde **Pano** menüsünde, select **müşteriler**.
3. Açılan sayfada tıklatın **Ekle müşteri** düğmesi.
4. İçinde **yeni müşteri** sayfasında, Kiracı için hesap bilgilerini ayrıntıları doldurun.

    ![Hesap bilgileri sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Ardından **sonraki: abonelikleri**.
6. Abonelik Seçimi sayfasında seçin **Microsoft Azure** onay kutusu. Diğer abonelikler şimdi veya herhangi bir anda ekleyebilirsiniz.
7. Üzerinde **gözden geçirme** sayfasında, Kiracı ayrıntıları onaylayın ve ardından **gönderme**.
8. Kiracı hesabınızı oluşturduktan sonra varsayılan hesabı ve parolası için bu abonelik ayrıntılarını görüntüleme onay sayfası görüntülenir. Bilgileri kaydedin ve daha sonra gerektiği gibi Azure portalı oturum açma sayfası aracılığıyla parolayı değiştirin.

Kiracı olarak bu bilgileri paylaşabilir veya oluşturabilir ve gerekirse ayrı bir hesap paylaşın.

## <a name="access-the-tenant-account"></a>Kiracı hesabına erişme

Kiracının abonelik Microsoft iş ortağı merkezi Pano erişebilirsiniz.

1. Üzerinde **müşteriler** sayfasında, Kiracı hesap adına tıklayın.
2. İçinde **abonelikleri** sayfa kiracı hesabını, mevcut hesabı abonelikleri izlemek ve daha fazla abonelik gerektiği gibi ekleyin.
3. Kiracının olağanüstü durum kurtarma işlemlerini yönetmek için seçin **tüm kaynaklar (Azure portalı)**. Bu kiracının Azure abonelikleri erişim sağlar.

    ![Tüm kaynaklar bağlantı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. Üst Azure Active Directory bağlantısını tıklatarak erişim doğrulayabilirsiniz Azure portalının sağ.

    ![Azure Active Directory bağlantısı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

Artık gerçekleştirmek ve tüm Site Recovery işlemlerini Azure portalında Kiracı için yönetebilirsiniz. Kiracı aboneliği, yönetilen olağanüstü durum kurtarma için CSP erişmek için yukarıda açıklanan süreci izleyin.

## <a name="assign-tenant-access-to-the-subscription"></a>Aboneliği için Kiracı erişimi atayın

1. Olağanüstü durum kurtarma altyapı kurulduğundan emin olun. İş ortakları, olağanüstü durum kurtarma yönetilip veya Self-Servis bağımsız olarak CSP Portalı aracılığıyla Kiracı aboneliklerine erişin. Kasasını oluşturup ve Kiracı aboneliklerine altyapısına kaydedin.
2. Kiracı ile sağlayın [oluşturduğunuz hesabı](#create-a-tenant-account).
3. Yeni bir kullanıcı CSP Portalı aracılığıyla Kiracı aboneliği için aşağıdaki gibi ekleyebilirsiniz:

    bir) kiracının CSP aboneliği sayfasına gidin ve ardından **kullanıcılar ve lisansları** seçeneği.

      ![Kiracının CSP abonelik sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

      b) şimdi ilgili ayrıntıları girerek ve izinleri seçerek veya bir CSV dosyasındaki kullanıcı listesi yükleyerek yeni bir kullanıcı oluşturun.
    c) yeni bir kullanıcı oluşturduktan sonra Azure portalına geri dönün. İçinde **abonelik** sayfasında, ilgili aboneliğini seçin.
    d) select **erişim denetimi (IAM)** ve ardından **Ekle**, ilgili erişim düzeyine sahip bir kullanıcı eklemek için. CSP Portalı aracılığıyla oluşturulmuş olan kullanıcıların otomatik olarak bir erişim düzeyi tıklattığınızda, açılan sayfasında görüntülenir.

      ![Kullanıcı ekleme](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Çoğu yönetim işlemleri için *katkıda bulunan* rolüdür yeterli. Bu erişim düzeyi kullanıcılarla bir abonelikte erişim düzeylerini değiştirme dışında her şeyi yapabilir (kendisi için *sahibi*-düzeyinde erişim gereklidir).
- Site Recovery de sahip üç [kullanıcı rolleri önceden tanımlanmış](site-recovery-role-based-linked-access-control.md), daha fazla erişim düzeyleri gereken şekilde kısıtlamak için kullanılabilir.

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda

Üç ana çok kiracılı modeli vardır:

* **Barındırma hizmetleri sağlayıcısı (HSP) paylaşılan**: fiziksel altyapı ortak sahibi ve kullandığı paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) birden çok Kiracı sanal makineleri barındırmak için aynı altyapı üzerinde. Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Barındırma hizmeti sağlayıcısı ayrılmış**: iş ortağı fiziksel altyapı sahip, ancak ayrı bir altyapı her bir kiracının sanal makineleri barındırmak için özel kaynakları (birden çok Vcenter, fiziksel datastores vb.) kullanır. Kiracı Self Servis bir çözüm olarak sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Hizmetler Sağlayıcısı (MSP) yönetilen**: Müşteri sanal makineleri barındıran fiziksel altyapı sahibi ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

Bu makalede açıklanan Kiracı aboneliklerine ayarlayarak, ilgili çok kiracılı modellerinden herhangi biri Müşteriler etkinleştirme hızla başlatabilirsiniz. Farklı çok kiracılı modelleri hakkında daha fazla bilgi edinin ve etkinleştirme şirket içi erişim denetimleri [burada](vmware-azure-multi-tenant-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [rol tabanlı erişim denetimi](site-recovery-role-based-linked-access-control.md) Azure Site Recovery dağıtımları yönetmek için.
- VMware Azure hakkında daha fazla bilgi [çoğaltma mimarisi](vmware-azure-architecture.md).
- [Öğretici gözden](vmware-azure-tutorial.md) VMware Vm'lerini Azure'a çoğaltma için.
Daha fazla bilgi edinmek [çok kiracılı ortamlarda](vmware-azure-multi-tenant-overview.md) VMware Vm'lerini Azure'a çoğaltma için.
