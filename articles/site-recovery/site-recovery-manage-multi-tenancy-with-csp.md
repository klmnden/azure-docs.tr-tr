---
title: "Azure Site kurtarma çoklu kiracı bulut çözümü sağlayıcısı (CSP) programı ile yönetme | Microsoft Docs"
description: "Oluşturma ve CSP Kiracı aboneliği yönetme ve çok kiracılı kurulumunda Azure Site Recovery dağıtma açıklar"
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
ms.date: 12/14/2017
ms.author: manayar
ms.openlocfilehash: a49da33c8038ad467389c66e59727c7e195baf82
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="manage-multi-tenancy-with-the-cloud-solution-provider-csp-program"></a>Çoklu kiracı bulut çözümü sağlayıcısı (CSP) programı ile yönetme

[CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) Office 365, Enterprise Mobility Suite ve Microsoft Azure dahil olmak üzere Microsoft bulut Hizmetleri için daha iyi birlikte hikayeleri destekler. CSP ile iş ortakları uçtan uca ilişki müşterilerle sahip ve için birincil ilişki için bağlantı noktası olur. İş ortakları, müşteriler için Azure abonelikleri dağıtmak ve kendi katma değer, özelleştirilmiş teklifleri aboneliklerle birleştirin.

Azure Site Recovery ile iş ortakları, müşteriler CSP aracılığıyla doğrudan tam olağanüstü durum kurtarma çözümü yönetebilirsiniz. Veya Site Recovery ortamları ayarlama ve müşterilerin kendi olağanüstü durum kurtarma gereksinimlerini bir Self Servis şekilde yönetmek için CSP kullanabilirsiniz. Her iki durumda da, Site Recovery müşterilerine arasındaki ilişkiler ortaklarıdır. İş ortakları ve müşteriler Site Recovery kullanımı için fatura Müşteri ilişkisine hizmeti.

Bu makalede, bir iş ortağı nasıl oluşturabilir ve CSP, Kiracı aboneliği için çok kiracılı VMware Kurulum yönetebilirsiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- [Hazırlama](tutorial-prepare-azure.md) bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gibi Azure kaynakları.
- [Hazırlama](tutorial-prepare-on-premises-vmware.md) VMware şirket içi VMware sunucularını ve Vm'leri.
- Her bir kiracı için Kiracı VM'ler ve iş ortağının vCenter ile iletişim kurabilen bir ayrı yönetim sunucusu oluşturun. Yalnızca iş ortağı bu sunucuya erişim hakları vardır. Farklı çok kiracılı ortamlarda hakkında daha fazla ayrıntı başvurmak için [çok kiracılı VMware](site-recovery-multi-tenant-support-vmware-using-csp.md) Kılavuzu.

## <a name="create-a-tenant-account"></a>Bir kiracı hesabı oluşturun

1. Aracılığıyla [Microsoft Partner Center](https://partnercenter.microsoft.com/), CSP hesabınızda oturum açın.

2. Üzerinde **Pano** menüsünde, select **müşteriler**.

    ![Microsoft iş ortağı merkezi müşterileri bağlantı](./media/site-recovery-manage-multi-tenancy-with-csp/csp-dashboard-display.png)

3. Açılan sayfada tıklatın **Ekle müşteri** düğmesi.

    ![Müşteri Ekle düğmesi](./media/site-recovery-manage-multi-tenancy-with-csp/add-new-customer.png)

4. Üzerinde **yeni müşteri** sayfasında, Kiracı için hesap bilgilerini ayrıntıları doldurun ve ardından **sonraki: abonelikleri**.

    ![Hesap bilgileri sayfası](./media/site-recovery-manage-multi-tenancy-with-csp/customer-add-filled.png)

5. Abonelik Seçimi sayfasında seçin **Microsoft Azure** onay kutusu. Diğer abonelikler şimdi veya herhangi bir anda ekleyebilirsiniz.

    ![Microsoft Azure aboneliği onay kutusu](./media/site-recovery-manage-multi-tenancy-with-csp/azure-subscription-selection.png)

6. Üzerinde **gözden geçirme** sayfasında, Kiracı ayrıntıları onaylayın ve ardından **gönderme**.

    ![Gözden geçirme sayfasını](./media/site-recovery-manage-multi-tenancy-with-csp/customer-summary-page.png)  

    Kiracı hesabınızı oluşturduktan sonra varsayılan hesabı ve parolası için bu abonelik ayrıntılarını görüntüleme onay sayfası görüntülenir.

7. Bilgileri kaydedin ve daha sonra gerektiği gibi Azure portalı oturum açma sayfası aracılığıyla parolayı değiştirin.  

    Kiracı olarak bu bilgileri paylaşabilir veya oluşturabilir ve gerekirse ayrı bir hesap paylaşın.

## <a name="access-the-tenant-account"></a>Kiracı hesabına erişme

Kiracının abonelik açıklandığı gibi Microsoft iş ortağı merkezi Pano erişebilirsiniz "1. adım: bir kiracı hesabı oluşturun."

1. Git **müşteriler** sayfasında ve Kiracı hesap adına tıklayın.

2. Üzerinde **abonelikleri** sayfa kiracı hesabını, mevcut hesabı abonelikleri izlemek ve daha fazla abonelik gerektiği gibi ekleyin. Kiracının olağanüstü durum kurtarma işlemlerini yönetmek için seçin **tüm kaynaklar (Azure portalı)**.

    ![Tüm kaynaklar bağlantı](./media/site-recovery-manage-multi-tenancy-with-csp/all-resources-select.png)  

    Tıklatarak **tüm kaynakları** kiracının Azure abonelikleri erişim sağlar. Üst Azure Active Directory bağlantısını tıklatarak erişim doğrulayabilirsiniz Azure portalının sağ.

    ![Azure Active Directory bağlantısı](./media/site-recovery-manage-multi-tenancy-with-csp/aad-admin-display.png)

Artık Azure Portalı aracılığıyla Kiracı için tüm site kurtarma işlemleri ve olağanüstü durum kurtarma işlemlerini yönetebilirsiniz. Kiracı aboneliği, yönetilen olağanüstü durum kurtarma için CSP erişmek için yukarıda açıklanan süreci izleyin.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Kaynaklar için Kiracı aboneliği dağıtma
1. Azure portalında bir kaynak grubu oluşturun ve sonra bir kurtarma Hizmetleri kasası normal işlem başına dağıtın.

2. Kasa kayıt anahtarını indir

3. CS kasa kayıt anahtarını kullanarak Kiracı için kaydedin.

4. İki erişim hesapları için kimlik bilgilerini girin: vCenter erişim hesabı ve VM erişim hesabı.

    ![Yönetici yapılandırması sunucu hesapları](./media/site-recovery-manage-multi-tenancy-with-csp/config-server-account-display.png)

## <a name="register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Site Recovery altyapısı kurtarma Hizmetleri kasasına kaydetme
1. Azure Portal'da, daha önce oluşturduğunuz kasa, kayıtlı CS vCenter sunucusuna kaydetmek "3. adım: kaynakları Kiracı aboneliği dağıtın." Bu amaç için vCenter erişim hesabı'nı kullanın.
2. "Altyapıyı Hazırlama" işlemi normal işlem başına Site Recovery için Son'u tıklatın.
3. Sanal makinelerin çoğaltılması artık hazırsınız. Yalnızca kiracının sanal makineleri üzerinde görüntülendiğini doğrulayın **sanal makine Seç** altında dikey **çoğaltmak** seçeneği.

    ![Kiracı VM'ler listede Select virtual machines dikey penceresi](./media/site-recovery-manage-multi-tenancy-with-csp/tenant-vm-display.png)

## <a name="assign-tenant-access-to-the-subscription"></a>Aboneliği için Kiracı erişimi atayın

Yordamının 6. adımında belirtildiği gibi Kiracı Self Servis olağanüstü durum kurtarma için hesap ayrıntılarını sağlayın "1. adım: bir kiracı hesabı oluşturma" bölümünde. İş ortağı olağanüstü durum kurtarma altyapıyı kurmasından sonra bu eylemi gerçekleştirin. Yönetilen veya Self Servis olağanüstü durum kurtarma türü olup olmadığını belirtir, iş ortakları CSP Portalı aracılığıyla Kiracı aboneliklerine erişmeniz gerekir. Bunlar, iş ortağı şirkete ait kasasını oluşturup ve Kiracı aboneliklerine altyapısına kaydedin.

İş ortakları ayrıca yeni bir kullanıcı CSP Portalı aracılığıyla Kiracı aboneliği aşağıdakileri yaparak ekleyebilirsiniz:

1. Kiracının CSP aboneliği sayfasına gidin ve ardından **kullanıcılar ve lisansları** seçeneği.

    ![Kiracının CSP abonelik sayfası](./media/site-recovery-manage-multi-tenancy-with-csp/users-and-licences.png)

    Artık ilgili ayrıntıları girerek ve izinleri seçerek veya bir CSV dosyasındaki kullanıcı listesi yükleyerek yeni bir kullanıcı oluşturabilirsiniz.

2. Yeni bir kullanıcı Azure portalına dönün ve ardından, Git oluşturduktan sonra **abonelik** dikey penceresinde, ilgili aboneliğini seçin.

3. Açılan dikey penceresinde, seçin **erişim denetimi (IAM)**ve ardından **Ekle** ilgili erişim düzeyine sahip bir kullanıcı eklemek için.      
    CSP Portalı aracılığıyla oluşturulmuş olan kullanıcıların otomatik olarak bir erişim düzeyi tıklattığınızda, açılan dikey penceresinde görüntülenir.

    ![Kullanıcı ekleme](./media/site-recovery-manage-multi-tenancy-with-csp/add-user-subscription.png)

    Çoğu yönetim işlemleri için *katkıda bulunan* rolüdür yeterli. Bu erişim düzeyi kullanıcılarla bir abonelikte erişim düzeylerini değiştirme dışında her şeyi yapabilir (kendisi için *sahibi*-düzeyinde erişim gereklidir).

  Azure Site Recovery de sahip üç [kullanıcı rolleri'önceden tanımlanmış](site-recovery-role-based-linked-access-control.md) erişim düzeyleri gerektiği gibi daha fazla sınırlamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
  [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md) Azure Site Recovery dağıtımları yönetmek için rol tabanlı erişim denetimi hakkında.

  [Çok kiracılı VMware ortamları yönetme](site-recovery-multi-tenant-support-vmware-using-csp.md)
