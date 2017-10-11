---
title: "Test yük devretme Azure Site Recovery | Microsoft Docs"
description: "Şirket içinden Azure'a yük devretme testi çalıştırma hakkında bilgi edinin"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: 54f62af6abcdd38254fd5379b95baa05656dc90b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="test--failover-to-azure-in-site-recovery"></a>Azure Site kurtarma için yük devretme sınaması



Bu makalede bilgiler sağlanmaktadır ve yük devretme testi ya da bir DR detayı sanal makinelerin ve fiziksel sunucuları, yapmaya yönelik yönergeler kurtarma sitesi olarak Azure kullanarak Site Recovery ile korunur.

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

Çoğaltma stratejinizi doğrulamak veya herhangi bir veri kaybı veya kapalı kalma süresi olmadan bir olağanüstü durum kurtarma ayrıntıya gerçekleştirmek için yük devretme testi çalıştırın. Yük devretme testi yapılması etkilerini devam eden çoğaltmayı veya üretim ortamınıza sahip değil. Yük devretme sınaması yapılabilir ya da üzerinde bir sanal makine veya bir [kurtarma planı](site-recovery-create-recovery-plans.md). Yük devretme testi tetiklendiğinde, sanal makineleri bağlanacağı hangi test ağ belirtmeniz gerekir. Yük devretme testi tetiklenir sonra ilerleme durumunu izleyebilirsiniz **işleri** sayfası.  


## <a name="supported-scenarios"></a>Desteklenen senaryolar
Yük devretme testi desteklenen tüm dağıtım senaryolarında dışında [Azure'da eski VMware sitesi](site-recovery-vmware-to-azure-classic-legacy.md). Sanal makine üzerinde Azure için başarısız oldu, yük devretme sınaması da desteklenmiyor.  


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
Bu yordam, bir yük devretme sınaması için bir kurtarma planı Çalıştır açıklar. Alternatif olarak, uygun seçeneği kullanılarak tek bir makine için yük devretme testi de çalıştırabilirsiniz.

![Yük devretme sınaması](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklatın **yük devretme testi**.
1. Seçin bir **kurtarma noktası** yük. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    1.  **En son işlenen**: Site Recovery hizmeti tarafından işlenmiş olan en son kurtarma noktasına kurtarma planının tüm sanal makineleri bu seçenek yöneltilir. Bir sanal makine yük devretme yapılırken, en son işlenen kurtarma noktası zaman damgası da gösterilir. Bir kurtarma planı yük devretme yapıyorsanız, tek tek sanal makineye gidin ve bakmak **en son kurtarma noktası** bu bilgileri almak için bölme. İşlenmemiş verileri işlemek için harcanan hiçbir zaman gibi bu seçenek bir düşük RTO (Kurtarma süresi hedefi) yük devretme seçeneği sağlar.
    1.  **Son uygulama tutarlı**: Site Recovery hizmeti tarafından işlenmiş olan son uygulama tutarlı kurtarma noktasına kurtarma planının tüm sanal makineleri bu seçenek yöneltilir. Bir sanal makine yük devretme yapılırken en son uygulamayla tutarlı kurtarma noktasının zaman damgası da gösterilir. Bir kurtarma planı yük devretme yapıyorsanız, tek tek sanal makineye gidin ve bakmak **en son kurtarma noktası** bu bilgileri almak için bölme.
    1.  **En son**: Bu seçenek ilk bunları üzerinden ona başarısız olmadan önce her bir sanal makine için bir kurtarma noktası oluşturmak için Site Recovery hizmetine gönderilen tüm verileri işler. Bu seçenek, düşük RPO'ya (kurtarma noktası hedefi) yük devretme tüm verileri sonra oluşturulan sanal yük devretme tetiklendiğinde, Site Recovery hizmetine çoğaltılan makinenin sağlar.
    1.  **En son çoklu işlenen VM**: Bu seçenek yalnızca çoklu VM tutarlılığını en az bir sanal makineyle sahip kurtarma planları için kullanılabilir. En son ortak çoklu VM tutarlı Kurtarma bir çoğaltma grubu yük devretme parçası olan sanal makineleri gelin. Diğer sanal makineler yük devretme bunların en son işlenen kurtarma noktası.  
    1.  **En son çoklu VM uygulamayla tutarlı**: Bu seçenek yalnızca çoklu VM tutarlılığı ON ile en az bir sanal makinenin kurtarma planları için kullanılabilir. Çoğaltma grubu yük devretme son ortak çoklu VM uygulamaları tutarlı kurtarma için bir parçası olan sanal makineleri gelin. Diğer sanal makineler kendi son noktaya yük devretme uygulamaları tutarlı kurtarma. 
    1.  **Özel**: bir sanal makinenin yük devretme testi yapmakta olduğunuz sonra belirli bir kurtarma noktası için yük devretme için bu seçeneği kullanabilirsiniz.
1. Seçin bir **Azure sanal ağı**: Burada test sanal makineleri oluşturulacaktır bir Azure sanal ağı sağlar. Site kurtarma denemeleri test sanal makineleri aynı ad ve aynı IP sağlanan olarak kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Ardından aynı adlı alt ağ yük devretme sınaması için sağlanan Azure sanal ağda kullanılabilir durumda değilse, test sanal makine ağdaki ilk alt alfabetik olarak oluşturulur. Aynı IP alt ağda kullanılabilir durumda değilse, sanal makine başka bir IP adresi alt ağında kullanılabilir alır. Bu bölüm için okuma [daha fazla ayrıntı](#creating-a-network-for-test-failover)
1. Azure'a devretmek ve veri şifrelemesi etkin olduğunda, buna **şifreleme anahtarı** sağlayıcı yüklemesi sırasında veri şifrelemesi etkin olduğunda verilmiş sertifikayı seçin. Sanal makine üzerinde şifrelemeyi etkinleştirmediyseniz, bu adımı yoksayabilirsiniz.
1. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi. Azure portalında test yineleme makine görebilmeniz gerekir.
1. Sanal makinede bir RDP bağlantı başlatmak için gerekecektir [bir ortak IP eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) başarısız ağ arabiriminde sanal makine üzerinde. Klasik sanal makineye üzerinden başarısız oluyor sonra yapmanız [bir uç nokta ekleyin](../virtual-machines/windows/classic/setup-endpoints.md) 3389 numaralı bağlantı noktasında
1. İşiniz bittiğinde tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın. Yük devretme testi sırasında oluşturulan sanal makineleri siler.


> [!TIP]
> Site kurtarma denemeleri test sanal makineleri aynı ad ve aynı IP sağlanan olarak kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Ardından aynı adlı alt ağ yük devretme sınaması için sağlanan Azure sanal ağda kullanılabilir durumda değilse, test sanal makine ağdaki ilk alt alfabetik olarak oluşturulur. Hedef IP seçilen alt ağın parçası ise, site kurtarma, hedef IP kullanarak test yük devretme sanal makine oluşturmaya çalışır. Hedef IP, seçilen alt ağının parçası değilse, test yük devretme sanal makine seçilen alt ağında kullanılabilir herhangi bir IP'yi kullanılarak oluşturulur.
>
>

## <a name="test-failover-job"></a>Test yük devretme işi

![Yük devretme sınaması](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Yük devretme testi tetiklendiğinde, aşağıdaki adımları içerir:

1. Önkoşul denetimi: Bu adım, yük devretme için gerekli tüm koşulların karşılandığından sağlar
1. Yük devretme: Bu adım verileri işler ve böylece dışında bir Azure sanal makinesi oluşturulabilir hazır kolaylaştırır. Seçmiş olmanız durumunda **son** kurtarma noktası bu adımı oluşturur, bir kurtarma noktası hizmete gönderilen verileri.
1. Başlangıç: Bu adım önceki adımda işlenen veri kullanarak bir Azure sanal makine oluşturur.

## <a name="time-taken-for-failover"></a>Yük devretme için harcanan süre

Bazı durumlarda, sanal makinelerin yük devretme genellikle tamamlamak için yaklaşık 8-10 dakika sürer bir ek ara adım gerektirir. Bu durumların olarak olan aşağıdaki:

* Mobility hizmetinin 9.8 eski bir sürümünü kullanarak VMware sanal makineleri
* Fiziksel sunucuları
* VMware Linux sanal makineleri
* Fiziksel sunucuları olarak korunan Hyper-V sanal makineler
* Burada aşağıdaki sürücüleri önyükleme sürücüler olarak mevcut olmayan VMware sanal makineler
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* VMware sanal makineleri'de, DHCP hizmeti, DHCP veya statik kullanarak yedeklemiş etkin olmayan IP adresleri

Tüm diğer durumlarda bu ara adım gerekli değildir ve yük devretme için harcanan süre önemli ölçüde düşebilir.


## <a name="creating-a-network-for-test-failover"></a>Yük devretme sınaması için bir ağ oluşturma
Bir test yük devretme yapılırken sağladığınız üretim ağınızdan kurtarma sitesi yalıtılmış bir ağ seçmeniz önerilir **işlem ve ağ** sanal makine için ayarları. Bir Azure sanal ağı oluşturduğunuzda varsayılan olarak, diğer ağlardan yalıtılır. Bu ağın üretim ağınızdan taklit etmelidir:

1. Test ağı üretim ağınızda aynı sayıda alt ağlar olarak, üretim ağınıza ve bu alt ağ ile aynı ada sahip olması gerekir.
1. Test ağı aynı IP aralığı, üretim ağınıza kullanmanız gerekir.
1. IP altında DNS sanal makine için hedef olarak verdiğiniz IP olarak Test ağı DNS güncelleştirme **işlem ve ağ** ayarlar. Git üzerinden [test active directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations) daha fazla ayrıntı için bölüm.


## <a name="test-failover-to-a-production-network-on-recovery-site"></a>Kurtarma sitesinde bir üretim ağı için yük devretme sınaması
Bir test yük devretme yapılırken sağladığınız üretim ağınızdan kurtarma sitesini farklı bir ağ seçmeniz önerilir **işlem ve ağ** sanal makine için ayarları. Ancak gerçekten uçtan uca ağ bağlantısına başarısız doğrulamak istiyorsanız, sanal makine üzerinde aşağıdaki noktalara dikkat edin:

1. Test yük devretme yapılırken birincil sanal makine kapatma olduğundan emin olun. Bunu yapmazsanız, iki sanal makine aynı anda aynı ağda çalışan aynı kimliğe sahip olur ve istenmeyen sonuçlara neden olabilir.
1. Test yük devretme sanal makineleri yaptığınız tüm değişiklikler olduğunda kayıp Temizle, test yük devretme sanal makineler. Bu değişiklikleri birincil sanal makine çoğaltılmamış.
1. Bu şekilde test yapmanın üretim uygulamanız için bir kapalı kalma yol açar. Uygulamanın kullanıcılarını DR detayı devam ederken uygulama kullanmamayı sorulan.  



## <a name="prepare-active-directory-and-dns"></a>Active Directory ve DNS hazırlama
Bir yük devretme sınaması için sınama uygulama çalıştırmak için test ortamınızda üretim Active Directory ortamında bir kopyasını gerekir. Git üzerinden [test active directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations) daha fazla ayrıntı için bölüm.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra RDP kullanarak Azure vm'lerine bağlanmak isterseniz, tabloda özetlenen Eylemler yaptığınızdan emin olun.

**Yük devretme** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran azure VM** | Yük devretmeden önce şirket içi makinede | Azure VM internet üzerinden erişim, RDP etkinleştirmek için emin TCP ve UDP olun kuralları eklenir için **ortak**, ve bu seçeneklerinde RDP'ye izin **Windows Güvenlik Duvarı** > **izin verilen uygulamaları**, tüm profiller için.<br/><br/> Bir siteden siteye bağlantı üzerinden erişim, makinede RDP etkinleştirmek ve RDP izin verildiğinden emin olun **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler** için **etki alanı ve özel** ağlar.<br/><br/>  Emin olun işletim sisteminin SAN ilkesinin ayarlanmış **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Hiçbir Windows güncelleştirmelerini bekleyen sanal makine bir yük devretme tetiklemek zaman olmadığından emin olun. Windows update, yük devretme ve güncelleştirme tamamlanana kadar sanal makineye oturum açabilmeniz olmaz başlayabilir. <br/><br/>
**Windows çalıştıran azure VM** | Yük devretme sonrasında Azure VM'de | Klasik sanal makine için [genel bir uç nokta ekleme](../virtual-machines/windows/classic/setup-endpoints.md) RDP Protokolü (3389 numaralı bağlantı noktası)<br/><br/>  Resource Manager sanal makinesi [bir ortak IP eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) üzerindeki.<br/><br/> RDP bağlantı noktasına gelen bağlantılara izin vermek ağ güvenlik grubu kural devredilen VM ve bağlı, Azure alt ağ gerekir.<br/><br/> Bir Resource Manager sanal makine için kontrol edebilirsiniz **önyükleme tanılama** bir sanal makine ekran aramak için<br/><br/> Bağlanamıyorsanız, VM çalışıp çalışmadığını denetleyin ve bunlara Ara [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).<br/><br/>
**Linux çalıştıran azure VM** | Yük devretmeden önce şirket içi makinede | Azure VM'de Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılacak şekilde ayarlandığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran azure VM** | Yük devretme sonrasında Azure VM | Yük devredilen VM'deki ve VM'nin bağlandığı Azure alt ağındaki ağ güvenlik grubu kurallarının, SSH bağlantı noktasına gelen bağlantılara izin vermesi gerekir.<br/><br/> Klasik sanal makine için [genel bir uç nokta ekleme](../virtual-machines/windows/classic/setup-endpoints.md) , SSH bağlantı noktası (TCP bağlantı noktası 22 varsayılan olarak) gelen bağlantılara izin verecek şekilde oluşturulmalıdır.<br/><br/> Resource Manager sanal makinesi [bir ortak IP eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) üzerindeki.<br/><br/> Bir Resource Manager sanal makine için kontrol edebilirsiniz **önyükleme tanılama** bir sanal makine ekran aramak için<br/><br/>



## <a name="next-steps"></a>Sonraki adımlar
Yük devretme testi başarıyla denediyseniz sonra yapılması deneyebilirsiniz bir [yük devretme](site-recovery-failover.md).
