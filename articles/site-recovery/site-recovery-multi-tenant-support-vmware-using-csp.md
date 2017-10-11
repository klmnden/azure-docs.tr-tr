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
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 97edbe67c25036dc1156f0f0ca5431a617d7a004
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>CSP aracılığıyla Azure VMware sanal makineleri çoğaltmak için Azure Site Recovery çok müşterili desteği

Azure Site Recovery Kiracı abonelikler için çok kiracılı ortamlarda destekler. Oluşturulan ve Microsoft bulut çözümü sağlayıcısı (CSP) programı aracılığıyla yönetilen Kiracı abonelikler için çoklu kiracı de destekler. Bu makalede, uygulamayı ve yönetmeyi çok kiracılı VMware Azure senaryolar için rehberlik ayrıntıları verilmektedir. Ayrıca, oluşturma ve CSP Kiracı aboneliği yönetme de kapsar.

Bu kılavuz, VMware sanal makinelerin Azure'a çoğaltılması için varolan belgelerindeki yoğun çizer. Daha fazla bilgi için bkz: [çoğaltmak VMware sanal makinelerini Site Recovery ile Azure'a](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlarda
Üç ana çok kiracılı modeli vardır:

* **Barındırma hizmetleri sağlayıcısı (HSP) paylaşılan**: iş ortağı fiziksel altyapı sahip ve aynı altyapı üzerinde birden çok Kiracı sanal makineleri barındırmak için paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama alanı ve benzeri) kullanır. Kiracı Self Servis bir çözüm olarak olağanüstü durum kurtarma sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Barındırma hizmeti sağlayıcısı ayrılmış**: iş ortağı fiziksel altyapı sahip ancak ayrı bir altyapı her bir kiracının sanal makineleri barındırmak için özel kaynakları (birden çok Vcenter, fiziksel datastores vb.) kullanır. Kiracı Self Servis bir çözüm olarak sahip olabilir veya iş ortağı olağanüstü durum kurtarma yönetim yönetilen bir hizmet olarak sağlayabilir.

* **Hizmetler Sağlayıcısı (MSP) yönetilen**: Müşteri sanal makineleri barındıran fiziksel altyapı sahibi ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

## <a name="shared-hosting-multi-tenant-guidance"></a>Paylaşılan barındırma çok kiracılı Kılavuzu
Bu bölümde ayrıntılı paylaşılan barındırma senaryo kapsar. Diğer iki senaryo paylaşılan barındırma senaryo kümeleridir ve aynı ilkeleri kullanın. Paylaşılan barındırma Kılavuzu sonunda farklar açıklanmaktadır.

Temel bir çok kiracılı senaryosunda çeşitli kiracılar yalıtmak için gereksinimdir. Bir kiracı başka bir kiracı barındırılan gözlemlemek olmamalıdır. İş ortağı tarafından yönetilen bir ortamda, bu gereksinim Burada, kritik olabilir bir Self Servis ortamında, olduğu gibi önemli değil. Bu kılavuz, Kiracı yalıtımı gerekli olduğunu varsayar.

Aşağıdaki diyagramda mimari sunulur:

![Bir vCenter ile paylaşılan HSP](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Bir vCenter paylaşılan barındırma senaryoyla**

Yukarıdaki diyagramda görüldüğü gibi her bir müşteri ayrı yönetim sunucusu vardır. Bu yapılandırma Kiracı özel VM'ler Kiracı erişimi sınırlar ve Kiracı yalıtımını etkinleştirir. Bir VMware sanal makinesi çoğaltma senaryo yapılandırma sunucusu sanal makineleri bulmak ve aracıları yüklemek için hesapları yönetmek için kullanır. Biz, çok kiracılı ortamlarda, vCenter erişim denetimi ile VM bulma kısıtlama eklenmesi için aynı ilkeleri izleyin.

Tüm duyarlı altyapı bilgileri (örneğin, erişim kimlik bilgileri) kalır, kiracılar için belirlenmemiş veri yalıtımı gereksinim gerekir. Bu nedenle, management server'ın tüm bileşenleri iş ortağı özel denetimi altında kalmasını öneririz. Yönetim sunucusu bileşenleri şunlardır:
* Yapılandırma sunucusu (CS)
* İşlem Sunucusu (PS)
* Ana hedef sunucusu (MT) 

Bir genişleme PS da iş ortağının altında denetimdir.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Çok kiracılı senaryoda her CS iki hesap kullanır

- **vCenter erişim hesabı**: Kiracı sanal makineleri bulmak için bu hesabı kullanın. (Sonraki bölümde açıklandığı gibi) için atanan vCenter erişim izinleri vardır. Yanlışlıkla erişim sızıntıları kaçınmak için iş ortakları Yapılandırma aracında bu kimlik bilgileri kendilerini girin öneririz.

- **Sanal makine erişim hesabı**: otomatik bir anında iletme aracılığıyla Kiracı VM'ler mobility Aracısı'nı yüklemek için bu hesabı kullanın. Genellikle, bir etki alanı hesabı bir kiracı için bir iş ortağı sağlayabilir veya alternatif olarak, iş ortağı doğrudan yönetebilir, olabilir. Bir kiracı ayrıntıları ortağıyla doğrudan paylaşmak istediğiniz değil, isterse CS sınırlı süreli erişimi üzerinden kimlik bilgilerini girin veya iş ortağının Yardımı ile mobility aracıları el ile yüklemek için izin.

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
>| vCenter | Salt okunur | Yalnızca farklı nesneleri yönetmek için vCenter erişime izin vermek gerekli. Vcenter yönetim işlemleri için kullanılan veya hesap için bir kiracı sağlanacak hiçbir zaman edecekse bu izni kaldırabilirsiniz. |
>| Datacenter | Azure_Site_Recovery |  |
>| Konak ve konak kümesi | Azure_Site_Recovery | Böylece yalnızca erişilebilir ana Kiracı sanal makineleri yük devretme öncesinde ve sonrasında yeniden çalışma erişim nesne düzeyinde yeniden sağlar. |
>| Veri deposu ve veri deposu küme | Azure_Site_Recovery | Önceki aynıdır. |
>| Ağ | Azure_Site_Recovery |  |
>| Yönetim sunucusu | Azure_Site_Recovery | CS makine dışında olan tüm bileşenleri (CS, PS ve MT) erişim içerir. |
>| Kiracı VM'ler | Azure_Site_Recovery | Tüm yeni Kiracı VM'ler belirli bir kiracı aynı zamanda bu erişim alın veya bunlar Azure portalı üzerinden bulunabilirlik olmaz sağlar. |

VCenter hesap erişim tamamlanmıştır. Bu adımı yeniden çalışma işlemlerini tamamlamak için minimum izinleri gereksinimini yerine getirir. Varolan ilkeleri ile bu erişim izinleri de kullanabilirsiniz. Yalnızca varolan izinlerinizi önceden ayrıntılı rol izinleri, 2. adımda içerecek şekilde değiştirin.

Yük devretme durumu kadar olağanüstü durum kurtarma işlemlerini kısıtlamak için (diğer bir deyişle, yeniden çalışma özellikleri olmayan), bir özel durum ile yukarıdaki yordamı izleyin: atama yerine *Azure_Site_Recovery* vCenter erişim hesabı'nı rol atama yalnızca bir *salt okunur* bu hesaba rol. Bu izin kümesi VM çoğaltma ve yük devretme sağlar ve geri dönme izin vermez. Önceki işleminde bir şey olduğu gibi kalır. Kiracı yalıtımı sağlamak ve VM bulma kısıtlamak için her izni yalnızca nesne düzeyinde atanmıştır ve alt nesneler için yayılan değil.

## <a name="other-multi-tenant-environments"></a>Diğer çok kiracılı ortamlarda

Önceki bölümde bir paylaşılan barındırma çözümü için çok kiracılı ortamı ayarlanacağı açıklanmıştır. Diğer iki ana çözümler barındırma ve hizmet yönetilen ayrılmış. Bu çözümlerin mimarisi aşağıdaki bölümlerde açıklanmıştır.

### <a name="dedicated-hosting-solution"></a>Barındırma çözümü ayrılmış

Aşağıdaki çizimde gösterildiği gibi mimari adanmış bir barındırma çözümüne yalnızca Kiracı için her bir kiracının altyapı ayarlandığını farktır. Kiracılar ayrı Vcenter yalıtılmış olduğundan, barındırma sağlayıcısı hala paylaşılan barındırma için sağlanan CSP adımları izlemeniz gerekir, ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP Kurulum değişmeden kalır.

![Mimari paylaşılan hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Birden çok Vcenter ile ayrılmış barındırma senaryosu**

### <a name="managed-service-solution"></a>Yönetilen hizmet çözümü

Aşağıdaki çizimde gösterildiği gibi bir yönetilen hizmet çözümüne mimari fark her bir kiracının altyapı ayrıca diğer kiracılar altyapısından ayrı fiziksel olmasıdır. Kiracı altyapı sahibi ve olağanüstü durum kurtarma yönetmek için bir çözüm sağlayıcısı istediğinde bu senaryo genellikle bulunmaktadır. Yeniden, kiracılar farklı altyapılar aracılığıyla fiziksel olarak yalıtılmış olduğundan, CSP adımları izlemek için iş ortağı gereksinimlerini paylaşılan barındırma için sağlanan ancak Kiracı yalıtımı hakkında endişelenmeniz gerekmez. CSP sağlama değişmeden kalır.

![Mimari paylaşılan hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Birden çok Vcenter hizmet senaryoyla yönetilen**

## <a name="csp-program-overview"></a>CSP program genel bakış
[CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) Office 365, Enterprise Mobility Suite ve Microsoft Azure dahil olmak üzere tüm Microsoft bulut hizmetlerine iş ortaklarının sunduğu daha iyi birlikte hikayeleri destekler. CSP ile ortaklarımızın uçtan uca ilişki müşterilerle sahip ve için birincil ilişki için bağlantı noktası olur. İş ortakları, müşteriler için Azure abonelikleri dağıtmak ve kendi katma değer, özelleştirilmiş teklifleri aboneliklerle birleştirin.

Azure Site Recovery ile iş ortakları, müşteriler CSP aracılığıyla doğrudan tam olağanüstü durum kurtarma çözümü yönetebilirsiniz. Veya Site Recovery ortamları ayarlama ve müşterilerin kendi olağanüstü durum kurtarma gereksinimlerini bir Self Servis şekilde yönetmek için CSP kullanabilirsiniz. Her iki durumda da, Site Recovery müşterilerine arasındaki ilişkiler ortaklarıdır. İş ortakları ve müşteriler Site Recovery kullanımı için fatura Müşteri ilişkisine hizmeti.

## <a name="create-and-manage-tenant-accounts"></a>Kiracı hesapları oluşturma ve yönetme

### <a name="step-0-prerequisite-check"></a>0. adım: Önkoşul denetimi

VM önkoşullar bölümünde açıklandığı gibi aynıdır [Azure Site Recovery belgelerine](site-recovery-vmware-to-azure.md). Bu önkoşullara ek olarak, CSP aracılığıyla Kiracı Yönetimi ile devam etmeden önce yukarıda açıklanan erişim denetimleri yerinde olmalıdır. Her bir kiracı için Kiracı VM'ler ve iş ortağının vCenter ile iletişim kurabilen bir ayrı yönetim sunucusu oluşturun. Yalnızca iş ortağı bu sunucuya erişim hakları vardır.

### <a name="step-1-create-a-tenant-account"></a>1. adım: bir kiracı hesabı oluşturma

1. Aracılığıyla [Microsoft Partner Center](https://partnercenter.microsoft.com/), CSP hesabınızda oturum açın. 
 
2. Üzerinde **Pano** menüsünde, select **müşteriler**.

    ![Microsoft iş ortağı merkezi müşterileri bağlantı](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Açılan sayfada tıklatın **Ekle müşteri** düğmesi.

    ![Müşteri Ekle düğmesi](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Üzerinde **yeni müşteri** sayfasında, Kiracı için hesap bilgilerini ayrıntıları doldurun ve ardından **sonraki: abonelikleri**.

    ![Hesap bilgileri sayfası](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Abonelik Seçimi sayfasında seçin **Microsoft Azure** onay kutusu. Diğer abonelikler şimdi veya herhangi bir anda ekleyebilirsiniz.

    ![Microsoft Azure aboneliği onay kutusu](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Üzerinde **gözden geçirme** sayfasında, Kiracı ayrıntıları onaylayın ve ardından **gönderme**.

    ![Gözden geçirme sayfasını](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Kiracı hesabınızı oluşturduktan sonra varsayılan hesabı ve parolası için bu abonelik ayrıntılarını görüntüleme onay sayfası görüntülenir. 

7. Bilgileri kaydedin ve daha sonra gerektiği gibi Azure portalı oturum açma sayfası aracılığıyla parolayı değiştirin.  
 
    Kiracı olarak bu bilgileri paylaşabilir veya oluşturabilir ve gerekirse ayrı bir hesap paylaşın.

### <a name="step-2-access-the-tenant-account"></a>2. adım: Kiracı hesabı erişim

Kiracının abonelik açıklandığı gibi Microsoft iş ortağı merkezi Pano erişebilirsiniz "1. adım: bir kiracı hesabı oluşturun." 

1. Git **müşteriler** sayfasında ve Kiracı hesap adına tıklayın.

2. Üzerinde **abonelikleri** sayfa kiracı hesabını, mevcut hesabı abonelikleri izlemek ve daha fazla abonelik gerektiği gibi ekleyin. Kiracının olağanüstü durum kurtarma işlemlerini yönetmek için seçin **tüm kaynaklar (Azure portalı)**.

    ![Tüm kaynaklar bağlantı](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Tıklatarak **tüm kaynakları** kiracının Azure abonelikleri erişim sağlar. Üst Azure Active Directory bağlantısını tıklatarak erişim doğrulayabilirsiniz Azure portalının sağ.

    ![Azure Active Directory bağlantısı](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Artık Azure Portalı aracılığıyla Kiracı için tüm site kurtarma işlemleri ve olağanüstü durum kurtarma işlemlerini yönetebilirsiniz. Kiracı aboneliği, yönetilen olağanüstü durum kurtarma için CSP erişmek için yukarıda açıklanan süreci izleyin.

### <a name="step-3-deploy-resources-to-the-tenant-subscription"></a>3. adım: kaynakları Kiracı aboneliği dağıtın.
1. Azure portalında bir kaynak grubu oluşturun ve sonra bir kurtarma Hizmetleri kasası normal işlem başına dağıtın. 
 
2. Kasa kayıt anahtarını indir

3. CS kasa kayıt anahtarını kullanarak Kiracı için kaydedin.

4. İki erişim hesapları için kimlik bilgilerini girin: vCenter erişim hesabı ve VM erişim hesabı.

    ![Yönetici yapılandırması sunucu hesapları](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>4. adım: Kurtarma Hizmetleri kasasına kayıt Site Recovery altyapısı
1. Azure Portal'da, daha önce oluşturduğunuz kasa, kayıtlı CS vCenter sunucusuna kaydetmek "3. adım: kaynakları Kiracı aboneliği dağıtın." Bu amaç için vCenter erişim hesabı'nı kullanın.
2. "Altyapıyı Hazırlama" işlemi normal işlem başına Site Recovery için Son'u tıklatın.
3. Sanal makinelerin çoğaltılması artık hazırsınız. Yalnızca kiracının sanal makineleri üzerinde görüntülendiğini doğrulayın **sanal makine Seç** altında dikey **çoğaltmak** seçeneği.

    ![Kiracı VM'ler listede Select virtual machines dikey penceresi](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-the-subscription"></a>5. adım: Kiracı erişimi abonelik atayın

Yordamının 6. adımında belirtildiği gibi Kiracı Self Servis olağanüstü durum kurtarma için hesap ayrıntılarını sağlayın "1. adım: bir kiracı hesabı oluşturma" bölümünde. İş ortağı olağanüstü durum kurtarma altyapıyı kurmasından sonra bu eylemi gerçekleştirin. Yönetilen veya Self Servis olağanüstü durum kurtarma türü olup olmadığını belirtir, iş ortakları CSP Portalı aracılığıyla Kiracı aboneliklerine erişmeniz gerekir. Bunlar, iş ortağı şirkete ait kasasını oluşturup ve Kiracı aboneliklerine altyapısına kaydedin.

İş ortakları ayrıca yeni bir kullanıcı CSP Portalı aracılığıyla Kiracı aboneliği aşağıdakileri yaparak ekleyebilirsiniz:

1. Kiracının CSP aboneliği sayfasına gidin ve ardından **kullanıcılar ve lisansları** seçeneği.

    ![Kiracının CSP abonelik sayfası](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Artık ilgili ayrıntıları girerek ve izinleri seçerek veya bir CSV dosyasındaki kullanıcı listesi yükleyerek yeni bir kullanıcı oluşturabilirsiniz.

2. Yeni bir kullanıcı Azure portalına dönün ve ardından, Git oluşturduktan sonra **abonelik** dikey penceresinde, ilgili aboneliğini seçin.

3. Açılan dikey penceresinde, seçin **erişim denetimi (IAM)**ve ardından **Ekle** ilgili erişim düzeyine sahip bir kullanıcı eklemek için.      
    CSP Portalı aracılığıyla oluşturulmuş olan kullanıcıların otomatik olarak bir erişim düzeyi tıklattığınızda, açılan dikey penceresinde görüntülenir.

    ![Kullanıcı ekleme](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Çoğu yönetim işlemleri için *katkıda bulunan* rolüdür yeterli. Bu erişim düzeyi kullanıcılarla bir abonelikte erişim düzeylerini değiştirme dışında her şeyi yapabilir (kendisi için *sahibi*-düzeyinde erişim gereklidir). Erişim düzeyleri gerektiği gibi ince ayar yapabilirsiniz.
