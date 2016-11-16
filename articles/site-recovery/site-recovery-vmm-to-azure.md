---
title: "Azure portalını kullanarak VMM bulutlarındaki Hyper-V sanal makinelerini Azure&quot;a çoğaltma | Microsoft Belgeleri"
description: "VMM bulutlarındaki Hyper-V VM&quot;lerinin Azure&quot;a yönelik çoğaltma, yük devretme ve kurtarma işlemlerini gerçekleştirmek üzere Azure Site Recovery&quot;nin Azure portalı kullanılarak nasıl dağıtılacağını açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/31/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2a1a835855499da50d746e87cd27ad4141241f48


---
# <a name="replicate-hyperv-virtual-machines-in-vmm-clouds-to-azure-using-the-azure-portal"></a>Azure portalını kullanarak VMM bulutlarındaki Hyper-V sanal makinelerini Azure'a çoğaltma
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Azure klasik](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell klasik](site-recovery-deploy-with-powershell.md)
> 
> 

Azure Site Recovery hizmetine hoş geldiniz!

Site Recovery, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunan bir Azure hizmetidir. Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenler. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Azure Site Recovery nedir?](site-recovery-overview.md) bölümünden daha fazla bilgi edinebilirsiniz.

Bu makalede, Azure portalındaki Azure Site Recovery'yi kullanarak System Center VMM bulutlarında yönetilen şirket içi Hyper-V sanal makinelerini Azure'a nasıl çoğaltacağınız açıklanmıştır.

Bu makaleyi okuduktan sonra yorumlarınızı en alttaki Disqus yorumlarında paylaşabilirsiniz. Teknik sorular için [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nu kullanın.

## <a name="quick-reference"></a>Hızlı başvuru
Tam bir dağıtım için makaledeki tüm adımları uygulamanızı öneririz. Ancak zamanınız kısıtlıysa burada daha fazla bilgi içeren bağlantılarla birlikte kısa bir özet bulabilirsiniz.

| **Alan** | **Ayrıntılar** |
| --- | --- |
| **Dağıtım senaryosu** |Azure portalını kullanarak VMM bulutlarındaki Hyper-V VM’lerini Azure'a çoğaltma |
| **Şirket içi gereksinimleri** |Bir veya daha fazla bulutla System Center 2012 R2 üzerinde çalışan bir veya daha fazla VMM sunucusu.<br/><br/> Bulutlar, bir veya daha fazla VMM konak grubu içermelidir.<br/><br/> En azından Hyper-V rolüne sahip Windows Server 2012 R2’yi veya en son güncelleştirmeleri içeren Microsoft Hyper-V Server 2012 R2’yi çalıştıran buluttaki en az bir Hyper-V sunucusu.<br/><br/> VMM sunucuları ve Hyper-V konaklarının internete ve doğrudan veya bir ara sunucu aracılığıyla belirli URL’lere erişebilmesi gerekir. [Tüm ayrıntılar](#on-premises-prerequisites). |
| **Şirket içi sınırlamaları** |HTTPS tabanlı ara sunucu desteklenmez |
| **Sağlayıcı/aracı** |Çoğaltılan VM’ler için Azure Site Recovery Sağlayıcısı gerekir.<br/><br/> Hyper-V konakları için Kurtarma Hizmetleri aracısı gerekir.<br/><br/> Bunları dağıtım sırasında yüklersiniz. |
|  **Azure gereksinimleri** |Azure hesabı<br/><br/> Kurtarma hizmetleri kasası<br/><br/> Kasa bölgesindeki LRS veya GRS depolama hesabı<br/><br/> Standart depolama hesabı<br/><br/> Kasa bölgesinde Azure sanal ağı. [Tüm ayrıntılar](#azure-prerequisites). |
|  **Azure sınırlamaları** |GRS kullanırsanız oturum açmak için başka bir LRS hesabına ihtiyacınız olur<br/><br/> Azure portalında oluşturulan depolama hesapları, kaynak grupları arasında taşınamaz.<br/><br/> Premium depolama desteklenmiyor. |
|  **VM çoğaltması** |VM’ler Azure önkoşullarına uymalıdır](site-recovery-best-practices.md#azure-virtual-machine-requirements)<br/><br/> |
|  **Çoğaltma sınırlamaları** |Statik bir IP adresiyle Linux çalıştıran VM'leri çoğaltamazsınız.<br/><br/> Belirli diskleri çoğaltmanın dışında tutamazsınız. |
| **Dağıtım adımları** |1) Azure’u hazırlama (abonelik, depolama, ağ) -> 2) Şirket içi ile ilgili hazırlıkları yapma (VMM ve ağ eşlemesi) -> 3) Kurtarma Hizmetleri kasası oluşturma -> 4) VMM ve Hyper-V konaklarını ayarlama -> 5) Çoğaltma ayarlarını yapılandırma -> 6) Çoğaltmayı etkinleştirme -> 7) Çoğaltma ve yük devretmeyi test etme. |

## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery
Azure’da, kaynak oluşturmaya ve bunlarla çalışmaya yönelik iki farklı [dağıtım modeli](../resource-manager-deployment-model

> ) mevcuttur: Azure Resource Manager ve klasik. Azure’da ayrıca iki portal bulunur: Klasik Azure portalı ve Azure portalı. Bu makalede, Azure portalında nasıl dağıtım yapılacağı açıklanmıştır. 
> 
> 

Azure portalındaki Site Recovery yeni özellikler içerir:

* İş sürekliliği ve olağanüstü durum kurtarmayı (BCDR) tek bir konumdan ayarlayabilmeniz ve yönetebilmeniz için Azure Backup ve Azure Site Recovery hizmetleri tek bir Kurtarma Hizmetleri kasasında birleştirilmiştir. Birleştirilmiş bir pano sayesinde şirket içi konumlarında ve Azure genel bulutunda işlemleri izleyebilir ve yönetebilirsiniz.
* Bulut Çözümü Sağlayıcısı (CSP) ile sağlanan Azure aboneliklerine sahip kullanıcılar, artık Site Recovery işlemlerini Azure portalında yönetebilir.
* Azure portalında makineleri Azure Resource Manager depolama hesaplarına çoğaltabilirsiniz. Yük devretme durumunda Site Recovery tarafından Azure'da Resource Manager tabanlı VM'ler oluşturulur.
* Site Recovery, klasik depolama hesaplarına çoğaltma desteği sağlamaya devam eder. Yük devretme işleminde Site Recovery, klasik modeli kullanarak VM'ler oluşturur.

## <a name="site-recovery-in-your-business"></a>İşletmenizde Site Recovery
Kuruluşlar, planlı ve plansız kesinti süreleri boyunca uygulamalarla verilerin çalışır durumda ve kullanılabilir olmasına yönelik izlenecek yolu belirleyip mümkün olan en kısa sürede normal çalışma koşullarına dönmeyi sağlayan bir BCDR stratejisine gereksinim duyar. Site Recovery'nin sağladığı avantajlar şunlardır:

* Hyper-V VM’lerinde çalışan iş uygulamaları için şirket dışında koruma.
* Çoğaltma, yük devretme ve kurtarma işlemlerini izlemek, yönetmek ve ayarlamak için tek bir konum.
* Azure'a kolayca yük devretme ve Azure'dan şirket içi konumunuzdaki Hyper-V konak sunucularından yeniden çalışma (kurtarma).
* Katmanlı uygulama iş yüklerinin birlikte yük devredebilmesi için birden çok VM içeren kurtarma planları.

## <a name="scenario-architecture"></a>Senaryo mimarisi
Senaryo bileşenleri şu şekildedir:

* **VMM sunucusu**: Bir veya birden çok bulut içeren şirket içi VMM sunucusu.
* **Hyper-V ana bilgisayarı veya kümesi**: VMM bulutlarında yönetilen Hyper-V ana bilgisayar sunucuları ve kümeleri.
* **Azure Site Recovery Sağlayıcısı ve Kurtarma hizmetleri aracısı**: Dağıtım sırasında Azure Site Recovery Sağlayıcısı'nı VMM sunucusuna, Microsoft Azure Kurtarma Hizmetleri aracısını Hyper-V konak sunucularına yüklersiniz. VMM sunucusundaki Sağlayıcı, çoğaltma işlemini düzenlemek için HTTPS 443 üzerinden Site Recovery ile iletişim kurar. Varsayılan olarak, Hyper-V ana bilgisayar sunucusundaki aracı HTTPS 443 üzerinden verileri Azure depolama alanına çoğaltır.
* **Azure**: Çoğaltılan verileri depolamak için bir Azure aboneliğine ve Azure depolama hesabına sahip olmanız gerekir. Ayrıca, Azure VM'lerinin yük devretme sonrasında bir ağa bağlanması için Azure sanal ağı gerekir.

![E2A Topolojisi](./media/site-recovery-vmm-to-azure/architecture.png)

## <a name="azure-prerequisites"></a>Azure önkoşulları
İşte Azure'da ihtiyaç duyacaklarınız:

| **Önkoşul** | **Ayrıntılar** |
| --- | --- |
| **Azure hesabı** |Bir [Microsoft Azure](http://azure.microsoft.com/) hesabınızın olması gerekir. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Site Recovery fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Azure depolama alanı** |Çoğaltılan verileri depolamak için bir standart Azure depolama hesabınızın olması gerekir. LRS ve GRS depolama hesabı kullanabilirsiniz. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. [Daha fazla bilgi edinin](../storage/storage-redundancy.md). Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.<br/><br/>Premium depolama desteklenmiyor.<br/><br/> Çoğaltılan veriler Azure depolama alanında saklanır ve Azure VM'leri yük devretme gerçekleştiğinde oluşturulur. <br/><br/> Azure depolama alanı [hakkında bilgi edinin](../storage/storage-introduction.md). |
| **Azure ağı** |Yük devretme gerçekleştiğinde Azure VM'lerinin bağlanabileceği bir Azure sanal ağınızın olması gerekir. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. |

## <a name="onpremises-prerequisites"></a>Şirket içi önkoşullar
Şirket içinde şunlara ihtiyaç duyarsınız:

| **Önkoşul** | **Ayrıntılar** |
| --- | --- |
| **VMM** |System Center 2012 R2 üzerinde çalışan bir veya daha fazla VMM sunucusu. Her VMM sunucusunun bir veya daha fazla yapılandırılmış bulut içermesi gerekir. Bir bulutun şunları içermesi gerekir:<br/><br/> Bir veya birden çok VMM ana bilgisayar grubu.<br/><br/> Her ana bilgisayar grubunda bir veya daha fazla Hyper-V sunucusu ya da kümesi.<br/><br/>VMM bulutlarını ayarlama hakkında [daha fazla bilgi edinin](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Hyper-V ana bilgisayar sunucularının, Hyper-V rolü içeren **Windows Server 2012 R2** veya **Microsoft Hyper-V Server 2012 R2** sürümünde ya da bunların sonraki sürümlerinde çalıştırılması ve en son güncelleştirmelerinin yüklenmiş olması gerekir.<br/><br/> Bir Hyper-V sunucusunun bir veya daha fazla VM içermesi gerekir.<br/><br/> Çoğaltmak istediğiniz VM'leri içeren Hyper-V ana bilgisayar sunucusunun veya kümesinin bir VMM bulutunda yönetilmesi gerekir.<br/><br/>Hyper-V sunucularının, doğrudan veya bir ara sunucu üzerinden İnternet'e bağlanması gerekir.<br/><br/>Hyper-V sunucularında [2961977](https://support.microsoft.com/kb/2961977) makalesinde belirtilen düzeltmelerin yüklü olması gerekir.<br/><br/>Hyper-V ana bilgisayar sunucularının Azure'a veri çoğaltabilmesi için internet erişimi gerekir. |
| **Sağlayıcı ve aracı** |Azure Site Recovery dağıtımı sırasında Azure Site Recovery Sağlayıcısı'nı VMM sunucusuna, Kurtarma Hizmetleri aracısını Hyper-V konağına yüklersiniz. Sağlayıcı ve aracının doğrudan İnternet üzerinden veya bir ara sunucu aracılığıyla Azure'a bağlanması gerekir. HTTPS tabanlı ara sunucular desteklenmez. Hyper-V ana bilgisayarlarının ve VMM sunucusundaki ara sunucunun şunlara yönelik erişime izin vermesi gerekir: <br/><br/> ``*.hypervrecoverymanager.windowsazure.com`` <br/><br/> ``*.accesscontrol.windows.net``<br/><br/> ``*.backup.windowsazure.com``<br/><br/> ``*.blob.core.windows.net``<br/><br/> ``*.store.core.windows.net``<br/><br/> VMM sunucusunda IP adresi tabanlı güvenlik duvarı kurallarına sahipseniz bu kuralların Azure ile iletişim kurmaya izin verip vermediğini denetleyin. [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin vermeniz gerekir.<br/><br/> Aboneliğinizin Azure bölgesi ve Batı ABD için IP adresi aralıklarına izin verin.<br/><br/> Ek olarak, VMM sunucusundaki ara sunucunun ``https://www.msftncsi.com/ncsi.txt`` adresine erişmesi gerekir |

## <a name="protected-machine-prerequisites"></a>Korumalı makine önkoşulları
| **Önkoşul** | **Ayrıntılar** |
| --- | --- |
| **Korumalı VM'ler** |Bir VM'ye yük devretmeden önce, Azure VM'sine atanan adın [Azure önkoşullarına](site-recovery-best-practices.md#azure-virtual-machine-requirements) uygun olduğundan emin olun. VM için çoğaltma işlemini etkinleştirdikten sonra adı değiştirebilirsiniz. <br/><br/> Korumalı makinelerdeki bağımsız disk kapasitesinin 1023 GB'tan fazla olmaması gerekir. Bir VM 16 adede kadar disk barındırabilir (16 TB'ye kadar).<br/><br/> Paylaşılan disk konuk kümeleri desteklenmez.<br/><br/> Birleşik Genişletilebilir Bellenim Arabirimi (UEFI)/Genişletilebilir Bellenim Arabirimi (EFI) önyüklemesi desteklenmez.<br/><br/> Kaynak VM, NIC grubu oluşturma özelliğine sahipse Azure'a yük devretme işleminin ardından tek bir NIC'ye dönüştürülür.<br/><br/>Statik bir IP adresiyle Linux çalıştıran VM'leri koruma işlemi desteklenmez. |

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
Dağıtıma hazırlanmak için şunları yapmanız gerekir:

1. Yük devretmeden sonra Azure VM’lerinin içinde bulunacağı [bir Azure ağı ayarlayın](#set-up-an-azure-network).
2. Çoğaltılan veriler için [Azure depolama hesabı ayarlama](#set-up-an-azure-storage-account).
3. Site Recovery dağıtımı için [VMM sunucusunu hazırlama](#prepare-the-vmm-server).
4. [Ağ eşlemesi için hazırlanma](#prepare-for-network-mapping). Site Recovery dağıtımı sırasında ağ eşlemesini yapılandırabilmeniz için ağları ayarlayın.

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama
Yük devretme işleminden sonra oluşturulan Azure VM'lerinin bağlanacağı bir Azure ağınızın olması gerekir.

* Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.
* Azure ağını, yük devri yapılan Azure VM'lerinde kullanmak istediğiniz kaynak modeline bağlı olarak [Resource Manager modunda](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) veya [klasik modda](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ayarlarsınız.
* Başlamadan önce bir ağ ayarlamanızı öneririz. Aksi takdirde, Site Recovery dağıtımı sırasında yapmanız gerekir.

> [!NOTE]
> Site Recovery dağıtımında kullanılan ağlar için aynı abonelik içindeki kaynak grupları arasında veya abonelikler arasında [ağ geçişi](../resource-group-move-resources.md) desteklenmez.
> 
> 

### <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama
* Azure'a çoğaltılan verileri tutmak için standart bir Azure depolama hesabına sahip olmanız gerekir. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
* Yük devri yapılan Azure VM'lerinde kullanmak istediğiniz kaynak modeline bağlı olarak [Resource Manager modunda](../storage/storage-create-storage-account.md) veya [klasik modda](../storage/storage-create-storage-account-classic-portal.md) bir hesap ayarlarsınız.
* Başlamadan önce bir hesap ayarlamanızı öneririz. Aksi takdirde, Site Recovery dağıtımı sırasında yapmanız gerekir.

> [!NOTE]
> Site Recovery dağıtımında kullanılan depolama hesapları için aynı abonelik içindeki kaynak grupları arasında veya abonelik arasında [depolama hesapları geçişi](../resource-group-move-resources.md) desteklenmez.
> 
> 

### <a name="prepare-the-vmm-server"></a>VMM sunucusunu hazırlama
* VMM sunucusunun [önkoşullar](#on-premises-prerequisites) ile uyum sağladığından emin olun.
* Site Recovery dağıtımı sırasında, VMM sunucusundaki tüm bulutların Azure portalda kullanılabilir olması gerektiğini belirtebilirsiniz. Portalda yalnızca belirli bulutların görünmesini istiyorsanız VMM yönetici konsolunda ilgili bulut için bu ayarı etkinleştirebilirsiniz.

### <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma
Site Recovery dağıtımı sırasında ağ eşlemesini ayarlamanız gerekir. Şunları sağlamak için kaynak VMM VM ağları ile hedef Azure ağları arasında ağ eşlemesi:

* Aynı ağda yük devretme işlemini gerçekleştiren makineler, yük devretmeyi aynı şekilde veya aynı kurtarma planında gerçekleştirmiyor olsalar bile birbirlerine bağlanabilir.
* Hedef Azure ağında bir ağ geçidi ayarlanırsa Azure sanal makineleri şirket içi sanal makinelere bağlanabilir.
* Ağ eşlemesini ayarlamak için şunları yapmanız gerekir:
  
  * Kaynak Hyper-V ana bilgisayar sunucusundaki VM'lerin bir VMM VM ağına bağlı olduğundan emin olun. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
  * [Yukarıda](#set-up-an-azure-network) açıklandığı gibi bir Azure ağına sahip olmanız gerekir.
* Ağ eşlemesinin nasıl çalıştığı hakkında [daha fazla bilgi edinin](site-recovery-network-mapping.md).

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Yeni** > **Yönetim** > **Kurtarma Hizmetleri** seçeneklerine tıklayın. Alternatif olarak, **Gözat** > **Kurtarma Hizmetleri** kasaları > **Ekle** seçeneklerine tıklayabilirsiniz.
   
    ![Yeni kasa](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa bunlardan birini seçin.
4. [Kaynak grubu oluşturun](../resource-group-template-deploy-portal.md) veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Makineler bu bölgeye çoğaltılır. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki Coğrafi Kullanılabilirlik kısmına bakın.
5. Pano'dan kasaya hızlıca erişmek istiyorsanız **Panoya sabitle** > **Kasa oluştur** seçeneğine tıklayın.
   
    ![Yeni kasa](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Yeni kasa **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** dikey penceresinde görünür.

## <a name="getting-started"></a>Başlarken
Mümkün olduğunca hızlı bir şekilde dağıtma işlemini gerçekleştirmenize yardımcı olmak için Site Recovery, bir çalışmaya başlama deneyimi sağlar. Başlarken deneyimi, önkoşulları denetler ve Site Recovery dağıtımı adımlarını doğru sırayla gerçekleştirmeniz için size kılavuzluk eder.

Başlarken bölümünde, çoğaltmak istediğiniz makinelerin türünü ve bu makineleri nereye çoğaltacağınızı seçersiniz. Şirket içi sunucular, Azure depolama hesapları ve ağlar ayarlarsınız. Çoğaltma ilkelerini oluşturup kapasite planlamasını gerçekleştirirsiniz. Altyapınız hazırlandıktan sonra VM'ler için çoğaltma işlemini etkinleştirirsiniz. Yük devretmeyi belirli makineler için çalıştırabilirsiniz veya birden çok makinede yük devretmek için kurtarma planları oluşturabilirsiniz.

Site Recovery'yi nasıl dağıtmak istediğinizi seçerek Başlarken deneyimine adım atın. Başlarken akışı, çoğaltma gereksinimlerinize bağlı olarak kısmen değişiklik gösterebilir.

## <a name="step-1-choose-your-protection-goals"></a>1. Adım: Koruma hedeflerinizi seçme
Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinde kasanızı seçin ve **Ayarlar**'a tıklayın.
2. **Başlarken** bölümünde **Site Recovery** > **1. Adım: Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.
   
    ![Hedefleri seçme](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. **Koruma hedefi** kısmında **Azure'a** seçeneğini belirleyin ve **Evet, Hyper-V ile**'yi seçin. Hyper-V ana bilgisayarlarını ve kurtarma sitesini yönetmek için VMM kullandığınızı doğrulamak için **Evet** seçeneğini belirleyin. Daha sonra, **Tamam**'a tıklayın.
   
    ![Hedefleri seçme](./media/site-recovery-vmm-to-azure/choose-goals2.png)

## <a name="step-2-set-up-the-source-environment"></a>2. Adım: Kaynak ortamını ayarlama
Azure Site Recovery Sağlayıcısı'nı VMM sunucusuna yükleyin ve sunucuyu kasaya kaydedin. Azure Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleyin.

1. **2. Adım: Altyapıyı Hazırlama** > **Kaynak** seçeneklerine tıklayın.
   
    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source1.png)
2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın.
   
    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source2.png)
3. **Sunucu Ekle** dikey penceresinde, **Sunucu türü** alanında **System Center VMM sunucusunun** görünüp görünmediğini ve VMM sunucusunun [önkoşulları ve URL gereksinimlerini](#on-premises-prerequisites) karşılayıp karşılamadığını kontrol edin.
4. Azure Site Recovery Sağlayıcısı yükleme dosyasını indirin.
5. Kayıt anahtarını indirin. Kurulumu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
   
    ![Kaynağı ayarlama](./media/site-recovery-vmm-to-azure/set-source3.png)
6. VMM sunucusunda Azure Site Recovery Sağlayıcısı'nı yükleyin.

### <a name="set-up-the-azure-site-recovery-provider"></a>Azure Site Recovery Sağlayıcısı'nı ayarlama
1. Sağlayıcı kurulum dosyasını çalıştırın.
2. **Microsoft Update** kısmından güncelleştirmeleri seçebilirsiniz. Böylece, Sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir.
3. **Yükleme** bölümünde varsayılan Sağlayıcı yükleme konumunu kabul edin veya değiştirin ve **Yükle**'ye tıklayın.
   
    ![Yükleme konumu](./media/site-recovery-vmm-to-azure/provider2.png)
4. Yükleme tamamlandığında VMM sunucusunu kasaya kaydetmek için **Kaydet**'e tıklayın.
5. **Kasa Ayarları** sayfasında, kasa anahtarı dosyasını seçmek için **Gözat**'a tıklayın. Azure Site Recovery aboneliğini ve kasa adını belirtin.
   
    ![Sunucu kaydı](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. **İnternet Bağlantısı** alanında, VMM sunucusunda çalışan Sağlayıcı'nın İnternet üzerinden Site Recovery'ye nasıl bağlanacağını belirtin.
   
   * Sağlayıcı'nın doğrudan bağlanmasını istiyorsanız **Ara sunucu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
   * Mevcut ara sunucunuz kimlik doğrulaması gerektiriyorsa veya özel bir ara sunucu kullanmak istiyorsanız, **Ara sunucu kullanarak Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
   * Özel bir ara sunucu kullanıyorsanız adresi, bağlantı noktasını ve kimlik bilgilerini belirtin.
   * Ara sunucu kullanıyorsanız [önkoşullar](#on-premises-prerequisites) bölümünde açıklanan URL'lere izin vermiş olmanız gerekir.
   * Özel bir ara sunucu kullanırsanız belirtilen ara sunucu kimlik bilgileriyle otomatik olarak bir VMM RunAs hesabı (DRAProxyAccount) oluşturulacaktır. Bu hesabın kimlik doğrulamasını başarıyla gerçekleştirebilmesi için ara sunucuyu yapılandırın. VMM RunAs hesabı ayarları VMM konsolundan değiştirilebilir. **Ayarlar** kısmında, **Güvenlik** > **Run As Hesapları** seçeneklerini genişletin ve ardından DRAProxyAccount parolasını değiştirin. Bu ayarın etkili olabilmesi için VMM hizmetinin yeniden başlatılması gerekir.
     
     ![internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Veri şifreleme için otomatik olarak oluşturulan SSL sertifikasının konumunu kabul edin veya değiştirin. Bu sertifika, Azure Site Recovery portalında Azure tarafından korunan bir bulut için veri şifrelemeyi etkinleştirdiğinizde kullanılır. Bu sertifikayı güvenli bir yerde saklayın. Azure'a yük devretme işlemi çalıştırdığınızda veri şifreleme etkinse şifrenin çözülmesi gerekir.
8. **Sunucu adı** alanında, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Bir küme yapılandırmasında VMM küme rolünün adını belirtin.
9. VMM sunucusundaki tüm bulutlara yönelik meta verileri kasayla eşitlemek isterseniz **Bulut meta verilerini eşitle** seçeneğini etkinleştirin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm bulutları eşitlemek istemezseniz bu ayarı işaretlemeden bırakıp her bulutu VMM konsolundaki bulut özelliklerinde bağımsız olarak eşitleyebilirsiniz. İşlemi tamamlamak için **Kaydet**'e tıklayın.
   
    ![Sunucu kaydı](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Kayıt başlar. Kayıt tamamlandıktan sonra, sunucu kasadaki **Ayarlar** > **Sunucular** dikey penceresinde görüntülenir.

#### <a name="commandline-installation-for-the-azure-site-recovery-provider"></a>Azure Site Recovery Sağlayıcısı'na yönelik komut satırı yüklemesi
Azure Site Recovery Sağlayıcısı komut satırından yüklenebilir. Bu yöntem, Sağlayıcı'yı Windows Server 2012 R2 için Sunucu Çekirdeği'nde yüklerken kullanılabilir.

1. Sağlayıcı yükleme dosyasını ve kayıt anahtarını bir klasöre indirin. Örneğin, C:\ASR.
2. Yükseltilmiş bir komut isteminde, Sağlayıcı yükleyicisini ayıklamak için şu komutları çalıştırın:
   
            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Bileşenleri yüklemek için şu komutu çalıştırın:
   
            C:\ASR> setupdr.exe /i
4. Ardından sunucuyu kasaya kaydetmek için şu komutları çalıştırın:
   
        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Konumlar:

* **/Credentials**: Kayıt anahtarı dosyasının bulunduğu konumu belirten zorunlu parametre.  
* **/FriendlyName**: Azure Site Recovery portalında görünen Hyper-V konak sunucusunun adı için zorunlu parametre.
* * **/EncryptionEnabled**: VMM bulutlarındaki Hyper-V VM'lerini Azure'a çoğaltırken kullanılan isteğe bağlı parametre. Sanal makineleri Azure'da şifrelemek isterseniz bunu kullanın (bekleyen şifreleme). Dosya adının **.pfx** uzantısını içerdiğinden emin olun. Varsayılan olarak şifreleme kapalıdır.
* **/proxyAddress**: Ara sunucunun adresini belirten isteğe bağlı parametre.
* **/proxyport**: Ara sunucunun bağlantı noktasını belirten isteğe bağlı parametre.
* **/proxyUsername**Ara sunucu kullanıcı adını belirten isteğe bağlı parametre (ara sunucu kimlik doğrulaması gerekiyorsa).
* **/proxyPassword**: Ara sunucu ile kimlik doğrulamak için kullanılacak parolayı belirten isteğe bağlı parametre (ara sunucu kimlik doğrulaması gerekiyorsa).

### <a name="install-the-azure-recovery-services-agent-on-hyperv-hosts"></a>Azure Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleme
1. Sağlayıcı'yı ayarladıktan sonra Azure Kurtarma Hizmetleri aracısı için yükleme dosyasını indirmeniz gerekir. VMM bulutundaki tüm Hyper-V sunucularında kurulum çalıştırma
   
    ![Hyper-V siteleri](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. **Önkoşul Denetimi**’nde **İleri**'ye tıklayın. Eksik önkoşullar otomatik olarak yüklenir.
   
    ![Kurtarma Hizmetleri Aracısı Önkoşulları](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. **Yükleme Ayarları**’nda yükleme ve önbellek konumunu kabul edin veya değiştirin. Önbelleği, en az 5 GB kullanılabilir depolama alanı bulunan bir sürücüde yapılandırabilirsiniz, ancak 600 GB veya daha fazla boş alana sahip bir önbellek sürücüsü kullanmanızı öneririz. Ardından **Yükle**'ye tıklayın.
4. Yükleme tamamlandıktan sonra **Kapat**’a tıklayarak işlemi sonlandırın.
   
    ![MARS Aracısı'nı Kaydetme](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Azure Kurtarma Hizmetleri aracısına yönelik komut satırı yüklemesi
Aşağıdaki komutu kullanarak Microsoft Azure Kurtarma Hizmetleri Aracısı'nı komut satırından yükleyebilirsiniz:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyperv-hosts"></a>Hyper-V ana bilgisayarlarından Site Recovery'ye internet ara sunucusu erişimini ayarlama
Hyper-V ana bilgisayarlarında çalışan Kurtarma Hizmetleri aracısının VM çoğaltması için Azure'a yönelik internet erişimi gerekir. İnternet'e bir ara sunucu aracılığıyla erişiyorsanız ara sunucuyu aşağıdaki gibi ayarlayın:

1. Hyper-V ana bilgisayarındaki Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak Microsoft Azure Backup için masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin yolunda bir kısayol bulunur.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. **Ara Sunucu Yapılandırması** sekmesinde ara sunucu bilgilerini belirtin.
   
    ![MARS Aracısı'nı Kaydetme](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Aracının [önkoşullar](#on-premises-prerequisites) bölümünde açıklanan URL'lere erişebildiğini denetleyin.

## <a name="step-3-set-up-the-target-environment"></a>3. Adım: Hedef ortamını ayarlama
Çoğaltma için kullanılacak Azure depolama hesabını ve yük devretmenin ardından Azure VM'lerinin bağlanacağı Azure ağını belirtin.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Yük devretmenin ardından VM'ler için kullanmak istediğiniz dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.
   
   ![Depolama](./media/site-recovery-vmm-to-azure/compatible-storage.png)
4. Depolama hesabı oluşturmadıysanız ve Resource Manager’ı kullanarak bir hesap oluşturmak istiyorsanız bu işlemi satır içinde yapmak için **+Depolama hesabı** seçeneğine tıklayın.  **Depolama hesabı oluştur** dikey penceresinde hesap adını, türünü, aboneliği ve konumu belirtin. Hesabın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.
   
   ![Depolama](./media/site-recovery-vmm-to-azure/gs-createstorage.png)
   
   Şunlara dikkat edin:
   
   * Klasik modeli kullanarak bir depolama hesabı oluşturmak istiyorsanız bu işlemi Azure portalından gerçekleştirin. [Daha fazla bilgi](../storage/storage-create-storage-account-classic-portal.md)
   * Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız, şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı ayarlayın.
5. Henüz bir Azure ağı oluşturmadıysanız ve Resource Manager’ı kullanarak bir ağ oluşturmak istiyorsanız bu işlemi satır içinde yapmak için **+Ağ** seçeneğine tıklayın. **Sanal ağ oluştur** dikey penceresinde ağ adı, adres aralığı, alt ağ ayrıntıları, abonelik ve konum belirtin. Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.
   
   ![Ağ](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)
   
   Klasik modeli kullanarak bir ağ oluşturmak isterseniz bu işlemi Azure portalından gerçekleştirin. [Daha fazla bilgi edinin](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma
* Ağ eşlemesi işlevinin genel hatlarını [okuyun](#prepare-for-network-mapping). Daha ayrıntılı bir açıklama için [bunu okuyun](site-recovery-network-mapping.md).
* VMM sunucusu üzerindeki sanal makinelerin VM ağına bağlı olduğunu ve en az bir Azure sanal ağı oluşturduğunuzu doğrulayın. Tek bir Azure ağına birden çok VM ağı eşlenebilir.

Eşleme işlemini şu şekilde yapılandırın:

1. **Ayarlar** > **Site Recovery Altyapısı** > **Ağ eşlemeleri** > **Ağ Eşleme** bölümünde **+Ağ Eşlemesi** simgesine tıklayın.
   
    ![Ağ eşlemesi](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. **Ağ eşlemesi ekle** seçeneğinde, kaynak olarak VMM sunucusunu, hedef olarak ise **Azure**'u seçin.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. **Kaynak ağ** bölümünde, VMM sunucusuyla ilişkili listeden eşlemek istediğiniz kaynak şirket içi VM ağını seçin.
5. **Hedef ağ** bölümünde, çoğaltılan Azure VM'lerinin çalışmaya başladığında bulunacağı Azure ağını seçin. Daha sonra, **Tamam**'a tıklayın.
   
    ![Ağ eşlemesi](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Ağ eşlemesi başladığında gerçekleşecekler şunlardır:

* Eşleme başladığında kaynak VM ağındaki var olan VM’ler hedef ağa bağlanır. Kaynak VM ağına bağlanan yeni VM’ler, çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
* Mevcut bir ağ eşlemesini değiştirirseniz çoğaltılan sanal makineler yeni ayarlar kullanılarak bağlanır.
* Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
* Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.

## <a name="step-4-set-up-replication-settings"></a>4. Adım: Çoğaltma ayarlarını belirleme
1. Yeni bir çoğaltma ilkesi oluşturmak için **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.
   
    ![Ağ](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin.
3. **Kopyalama sıklığı** kısmında, ilk çoğaltmadan sonra değişim verilerini ne sıklıkta çoğaltacağınızı belirleyin (30 saniyede, 5 veya 15 dakikada bir).
4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını saat cinsinden belirtin. Korumalı makineler, bu süre içindeki herhangi bir noktaya kurtarılabilir.
5. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktasının hangi sıklıkta oluşturulacağını (1-12 saat) belirtin. Hyper-V iki çeşit anlık görüntü kullanır: tüm sanal makinenin artımlı anlık görüntüsünü sunan standart anlık görüntü ve sanal makine içinde uygulama verilerinin belirli bir noktadaki anlık görüntüsünü alan uygulamayla tutarlı anlık görüntü. Uygulamayla tutarlı anlık görüntüler, anlık görüntü alınırken uygulamaların tutarlı bir durumda olmasını sağlamak için Birim Gölge Kopyası Hizmeti'ni (VSS) kullanır. Uygulamayla tutarlı anlık görüntüleri etkinleştirirseniz kaynak sanal makinelerde çalışan uygulamaların performansının etkileneceğini unutmayın. Ayarladığınız değerin, yapılandırdığınız ilave kurtarma noktası sayısından daha az olduğundan emin olun.
6. **İlk çoğaltma başlangıç zamanı** bölümünde, ilk çoğaltmanın başlatılacağı zamanını belirtin. Çoğaltma işlemi internet bant genişliğiniz üzerinden gerçekleştirileceğinden, çoğaltma zamanını yoğun saatlerin dışında olacak şekilde ayarlamak isteyebilirsiniz.
7. **Azure'da depolanan verileri şifrele** bölümünde, Azure depolama alanındaki bekleyen verilerin şifrelenip şifrelenmeyeceğini belirtin. Daha sonra, **Tamam**'a tıklayın.
   
    ![Çoğaltma ilkesi](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Yeni bir ilke oluşturduğunuzda, bu ilke otomatik olarak VMM bulutuyla ilişkilendirilir. **Tamam** düğmesine tıklayın. **Ayarlar** > **Çoğaltma** > ilke adı > **VMM Bulutu ile İlişkilendir** üzerinden ilave VM Bulutlarını (ve bu bulutların içindeki VM'leri) bu çoğaltma ilkesi ile ilişkilendirebilirsiniz.
   
    ![Çoğaltma ilkesi](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>5. Adım: Kapasite planlaması
Temel altyapınızı ayarladığınıza göre kapasite planlaması yapmayı ve ilave kaynaklara ihtiyacınız olup olmadığını düşünün.

Site Recovery; kaynak ortamınız, Site Recovery bileşenleri, ağ ve depolama için doğru kaynakları ayırmanıza yardımcı olmak üzere bir kapasite planlayıcısı sunar. Planlayıcıyı; depolama alanı, disk ve VM'lerin ortalama sayısına göre tahmin oluşturmak için hızlı modda ve iş yükü düzeyinde sayılar gireceğiniz ayrıntılı modda çalıştırabilirsiniz. Başlamadan önce:

* VM'ler, VM başına disk ve disk başına depolama dahil olmak üzere çoğaltma ortamınız hakkında bilgi toplayın.
* Çoğaltılan veriler için günlük değişim (dalgalanma) hızına yönelik tahminde bulunun. Bu işlemi gerçekleştirmenize yardımcı olması için [Hyper-V Çoğaltma için Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057)'ı kullanabilirsiniz.

1. **İndir**'e tıklayarak aracı indirin ve ardından çalıştırın. Araç ile birlikte sunulan [makaleyi okuyun](site-recovery-capacity-planner.md).
2. Okumayı tamamladığınızda **Have you run the Capacity Planner?** (Capacity Planner’ı çalıştırdınız mı?) sorusuna **Evet** yanıtını verin.
   
   ![Kapasite planlaması](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Ağ bandı genişliği ile ilgili dikkat edilmesi gerekenler
Çoğaltma (ilk çoğaltma ve ardından değişim) için gereken bant genişliğini hesaplamak üzere Capacity Planner'ı kullanabilirsiniz. Çoğaltmada kullanılan bant genişliği miktarını kontrol etmek için birkaç seçenek sunulur:

* **Bant genişliğini kısıtlama**: İkincil bir siteye çoğaltılan Hyper-V trafiği belirli bir Hyper-V konağı üzerinden geçer. Ana bilgisayar sunucusunda bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliğine ince ayar uygulama**: Birkaç kayıt defteri anahtarı kullanarak çoğaltma için kullanılan bant genişliği üzerinde etki oluşturabilirsiniz.

#### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama
1. Hyper-V ana bilgisayar sunucusunda Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak Microsoft Azure Backup için masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin yolunda bir kısayol bulunur.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. **Azaltma** sekmesinde **Yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir** seçeneğini belirleyin ve çalışma saatleri ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar saniye başına 512 Kbps ila 102 Mbps arasındadır.
   
    ![Bant genişliğini kısıtlama](./media/site-recovery-vmm-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

#### <a name="influence-network-bandwidth"></a>Ağ bant genişliği üzerinde etki oluşturma
**UploadThreadsPerVM** kayıt defteri değeri, bir diskin veri aktarımı (başlangıç ve değişim çoğaltması) için kullanılan iş parçacıklarının sayısını denetler. Daha yüksek bir değer, çoğaltma işlemi için kullanılan ağ bant genişliğini artırır. **DownloadThreadsPerVM** kayıt defteri değeri, yeniden çalışma sırasındaki veri aktarımı için kullanılan iş parçacıklarının sayısını belirler.

1. Kayıt defterinde **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication** hedefine gidin.
   
   * Disk çoğaltması için kullanılan iş parçacıklarını denetlemek için **UploadThreadsPerVM** değerini değiştirin (veya anahtar yoksa anahtar oluşturun).
   * Azure'dan gelen yeniden çalışma trafiği için kullanılan iş parçacıklarını denetlemek için **DownloadThreadsPerVM** değerini değiştirin (veya anahtar yoksa anahtar oluşturun).
2. Varsayılan değer 4'tür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.

## <a name="step-6-enable-replication"></a>6. Adım: Çoğaltmayı etkinleştirme
Şimdi aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın. Çoğaltmayı ilk kez etkinleştirdikten sonra ek makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada **+Çoğalt**'a tıklayın.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. **Kaynak** dikey penceresinde Hyper-V konaklarının bulunduğu VMM sunucusunu ve bulutu seçin. Daha sonra, **Tamam**'a tıklayın.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. **Hedef** kısmında aboneliği, yük devretme sonrası dağıtım modelini ve çoğaltılan veriler için kullandığınız depolama hesabını seçin.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Kullanmak istediğiniz depolama hesabını seçin. Sahip olduğunuz hesaplardan farklı bir depolama hesabı kullanmak isterseniz [yeni bir hesap](#set-up-an-azure-storage-account) oluşturabilirsiniz. Resource Manager modelini kullanarak bir depolama hesabı oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir depolama hesabı oluşturmak istiyorsanız bu işlemi [Azure portalından](../storage/storage-create-storage-account-classic-portal.md) gerçekleştirin. Daha sonra, **Tamam**'a tıklayın.
5. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Her makine için Azure ağını ayrı ayrı seçmek üzere **Daha sonra yapılandır**'ı seçin. Sahip olduğunuz ağlardan farklı bir ağ kullanmak isterseniz [yeni bir ağ](#set-up-an-azure-network) oluşturabilirsiniz. Resource Manager modelini kullanarak bir ağ oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir ağ oluşturmak isterseniz bu işlemi [Azure portalından](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) gerçekleştirin. Bir alt ağ (varsa) seçin. Daha sonra, **Tamam**'a tıklayın.
6. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication5.png)
7. **Özellikler** > **Özellikleri yapılandır** seçeneklerinde, seçili VM'ler için işletme sistemini ve OS diskini seçin. Daha sonra, **Tamam**'a tıklayın. Daha sonra ek özellikleri ayarlayabilirsiniz.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication6.png)
8. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandır** seçeneklerinde, korumalı VM'lere uygulamak istediğiniz çoğaltma ilkesini seçin. Daha sonra, **Tamam**'a tıklayın. **Ayarlar** > **Çoğaltma ilkeleri** > ilke adı > **Ayarları Düzenle**'de çoğaltma ilkesini değiştirebilirsiniz. Uyguladığınız değişiklikler, zaten çoğaltılmakta olan makinelere ve yeni makinelere uygulanır.
   
   ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/enable-replication7.png)

**Ayarlar** > **İşler** > **Site Recovery işleri** üzerinden **Korumayı Etkinleştir** işinin ilerleyişini izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

### <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme
Kaynak makinenin özelliklerini doğrulamanızı öneririz. Azure VM adının [Azure sanal makine gereksinimlerini](site-recovery-best-practices.md#azure-virtual-machine-requirements) karşılaması gerektiğini unutmayın.

1. **Ayarlar** > **Korunan Öğeler** > **Çoğaltılan Öğeler** seçeneklerine tıklayıp ayrıntılarını görmek istediğiniz makineyi seçin.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. **Özellikler** kısmında VM'nin çoğaltma ve yük devretme bilgilerini inceleyebilirsiniz.
   
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. **İşlem ve Ağ** > **İşlem özellikleri** seçeneklerinden Azure VM adını ve hedef boyutu belirtebilirsiniz. Gerekirse [Azure gereksinimleri](site-recovery-best-practices.md#azure-virtual-machine-requirements) ile uyum sağlamak için adı değiştirin. Azure VM'sine atanan IP adresi, alt ağ ve hedef ağ ile ilgili bilgileri de görüntüleyip değiştirebilirsiniz. Şunlara dikkat edin:
   
   * Hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme işlemi başarısız olur. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.
   * Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:
     
     * Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
     * Kaynak sanal makinenin bağdaştırıcı sayısı hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
     * Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.     
     * VM'nin birden çok bağdaştırıcısı varsa bunların tümü aynı ağa bağlanır.
     
     ![Çoğaltmayı etkinleştirme](./media/site-recovery-vmm-to-azure/test-failover4.png)
4. **Diskler** kısmında çoğaltılacak VM'deki işletim sistemi ve veri disklerini görebilirsiniz.

## <a name="step-7-test-your-deployment"></a>7. Adım: Dağıtımınızı test etme
Dağıtımı test etmek için tek bir sanal makine için yük devretme testi veya bir ya da daha fazla sanal makine içeren bir kurtarma planı çalıştırabilirsiniz.

### <a name="prepare-for-failover"></a>Yük devretme hazırlığı
* Yük devretme testi çalıştırmak için, Azure üretim ağınızdan yalıtılmış olan yeni bir Azure ağı oluşturmanızı öneririz. Bu, Azure'da yeni bir ağ oluşturulurken görülen varsayılan davranıştır. Yük devretme testlerini çalıştırma hakkında [daha fazla bilgi edinin](site-recovery-failover.md#run-a-test-failover).
* Azure'a yük devrederken en iyi performansı elde etmek için korunan makineye Azure Aracısı'nı yükleyin. Önyüklemeyi hızlandırır ve sorun gidermeye yardım eder. [Linux](https://github.com/Azure/WALinuxAgent) veya [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) aracısını yükleyin.
* Dağıtımınızı tam olarak test etmek için, çoğaltılan makinelerin istendiği gibi çalışacağı bir altyapıya sahip olmanız gerekir. Active Directory ve DNS'yi test etmek isterseniz DNS içeren bir etki alanı denetleyicisi olarak bir sanal makine oluşturabilir ve Azure Site Recovery'yi kullanarak bunu Azure'a çoğaltabilirsiniz. Daha fazla bilgi edinmek için [Active Directory'ye yönelik yük devretme testi ile ilgili dikkat edilmesi gerekenler](site-recovery-active-directory.md#considerations-for-test-failover) bölümünü okuyun.
* Yük devretme testi yerine planlanmamış bir yük devretme çalıştırmak isterseniz şunlara dikkat edin:
  
  * Mümkün olduğu durumlarda, planlanmamış bir yük devretmeyi çalıştırmadan önce birincil makineleri kapatmanız gerekir. Bu işlem, kaynak ve çoğaltılan makinelerin aynı anda çalışmamasını garantiler.
  * Planlanmamış yük devretme çalıştırdığınızda birincil makinelerden veri çoğaltma işlemi durduğundan, planlanmamış yük devretme başladıktan sonra hiçbir veri değişikliği aktarılmaz. Ayrıca, kurtarma planı üzerinde planlanmamış bir yük devretme çalıştırırsanız hata gerçekleşse bile tamamlanana kadar çalışır.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma
Yük devretmeden sonra RDP kullanarak Azure VM'lerine bağlanmak isterseniz aşağıdakileri yaptığınızdan emin olun:

**Yük devretmeden önce şirket içi makinede**:

* İnternet üzerinden erişim için RDP'yi etkinleştirin, **Genel** için TCP ile UDP kurallarının eklendiğinden ve tüm profiller için **Windows Güvenlik Duvarı** -> **İzin verilen uygulamalar ve özellikler** seçeneklerinde RDP'ye izin verildiğinden emin olun.
* Konumdan konuma bağlantı üzerinden erişim için makinede RDP'yi etkinleştirin ve **Etki alanı** ile **Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulamalar ve özellikler** seçeneklerinde RDP'ye izin verildiğinden emin olun.
* Şirket içi makinede [Azure VM aracısını](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yükleyin.
* İşletim sisteminin SAN ilkesinin OnlineAll olarak ayarlandığından emin olun. [Daha fazla bilgi](https://support.microsoft.com/kb/3031135)
* Yük devretmeyi çalıştırmadan önce IPSec hizmetini devre dışı bırakın.

**Yük devretme sonrasında Azure VM'de**:

* RDP protokolü (3389 bağlantı noktası) için genel bir uç nokta ekleyin ve oturum açma kimlik bilgilerini belirtin.
* Genel adres kullanarak sanal makineye bağlanmanızı engelleyecek etki alanı ilkesi olmadığından emin olun.
* Bağlanmayı deneyin. Bağlanamıyorsanız VM'nin çalıştığını doğrulayın. Sorun gidermeye ilişkin daha fazla ipucu için bu [makaleyi](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) okuyun.

Yük devretmeden sonra Secure Shell istemcisi kullanarak Linux çalıştıran bir Azure VM'sine erişmek isterseniz aşağıdakileri uygulayın:

**Yük devretmeden önce şirket içi makinede**:

* Azure VM'de Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılacak şekilde ayarlandığından emin olun.
* Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.

**Yük devretme sonrasında Azure VM'de**:

* Yük devredilen VM'deki ve VM'nin bağlandığı Azure alt ağındaki ağ güvenlik grubu kurallarının, SSH bağlantı noktasına gelen bağlantılara izin vermesi gerekir.
* SSH bağlantı noktasına (varsayılan olarak TCP bağlantı noktası 22) gelen bağlantılara izin vermek için genel bir uç nokta oluşturulması gerekir.
* VM'ye bir VPN bağlantısı üzerinden erişilirse (Express Route veya konumdan konuma VPN) SSH üzerinden doğrudan VM'ye bağlanmak için istemci kullanılabilir.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
1. Tek bir VM'de yük devretme için **Ayarlar** > **Çoğaltılan Öğeler** altında VM > **+Yük Devretme Testi**'ne tıklayın.
2. Kurtarma planında yük devretme için **Ayarlar** > **Kurtarma Planları** seçeneklerinde plana sağ tıklayıp **Yük Devretme Testi**'ne tıklayın. Kurtarma planı oluşturmak için [aşağıdaki yönergeleri uygulayın](site-recovery-create-recovery-plans.md).
3. **Yük Devretme Testi** kısmında, yük devretme gerçekleştikten sonra Azure VM'lerinin bağlandığı Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM'ye tıklayarak veya **Ayarlar** > **Site Recovery işleri** seçeneklerinde **Yük Devretme Testi**'ne tıklayarak ilerlemeyi izleyebilirsiniz.
5. Yük devretme **Testi tamamla** aşamasına ulaştığında şunları yapın:
   
   1. Azure portalında çoğaltılan sanal makineyi görüntüleyin. Sanal makinenin başarılı bir şekilde başlatıldığını doğrulayın.
   2. Sanal makinelere şirket içi ağınızdan erişiyorsanız sanal makineye yönelik Uzak Masaüstü bağlantısını başlatabilirsiniz.
   3. Bitirmek için **Testi Tamamla**'ya tıklayın.
   4. Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın.
   5. **Yük devretme testi tamamlandı** seçeneğine tıklayın. Otomatik olarak kapatmak ve sanal makine testini silmek için test ortamını temizleyin.
   6. Bu aşamada, Site Recovery tarafından yük devretme testi sırasında otomatik olarak oluşturulan tüm öğeler ve VM'ler silinir. Yük devretme testi için oluşturduğunuz ilave öğeler silinmez.
      
      > [!NOTE]
      > Bir yük devretme testi iki haftadan fazla sürerse zorla tamamlanır.
      > 
      > 
6. Yük devretme tamamlandıktan sonra çoğaltılan makineyi Azure portalı > **Sanal Makineler** kısmında da görmeniz gerekir. VM'nin uygun boyutta olduğundan, uygun bir ağa bağlandığından ve çalıştığından emin olmanız gerekir.
7. [Yük devretme sonrasındaki bağlantılar için hazırlık yaptıysanız](#prepare-to-connect-to-Azure-VMs-after-failover) Azure VM'ye bağlanabilmeniz gerekir.

## <a name="monitor-your-deployment"></a>Dağıtımınızı izleme
Site Recovery dağıtımınızın durumunu, yapılandırma ayarlarını ve sistem durumunu izlemeniz için yapmanız gerekenler:

1. **Temel Bileşenler** panosuna erişmeniz için kasa adına tıklayın. Bu panoda Site Recovery işlerini, çoğaltma durumunu, kurtarma planlarını, sunucu sistem durumunu ve olayları izleyebilirsiniz.  **Temel Bileşenler** panosunu, Site Recovery ve Backup kasalarının durumu dahil olmak üzere sizin için en faydalı olan kutucukları ve düzenlemeleri gösterecek şekilde özelleştirebilirsiniz.
   
    ![Temel Bileşenler](./media/site-recovery-vmm-to-azure/essentials.png)
2. **Sistem durumu** kutucuğunda, konum sunucularındaki (VMM veya yapılandırma sunucuları) sorunları ve Site Recovery tarafından son 24 saat içinde tetiklenen olayları izleyebilirsiniz.
3. **Çoğaltılan Öğeler**, **Kurtarma Planları** ve **Site Recovery İşleri** kutucuklarında çoğaltma işlemini yönetebilir ve izleyebilirsiniz. **Ayarlar** > **İşler** > **Site Recovery İşleri** seçeneklerinden işlerin ayrıntılarına ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](site-recovery-failover.md).




<!--HONumber=Nov16_HO2-->


