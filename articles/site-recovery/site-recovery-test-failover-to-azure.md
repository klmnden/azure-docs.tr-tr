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
ms.date: 10/16/2017
ms.author: pratshar
ms.openlocfilehash: 9902af83125f596f6dd5a1a6c955d00e9b5a87bc
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="test--failover-to-azure-in-site-recovery"></a>Azure Site kurtarma için yük devretme sınaması



Bu makalede, Azure Site kurtarma sınama yük devretme kullanarak, bir olağanüstü durum kurtarma ayrıntıya çalıştırmak açıklar.  

Çoğaltma ve olağanüstü durum reecovery stratejisi herhangi bir veri kaybı veya kapalı kalma süresi olmadan doğrulamak için yük devretme testi çalıştırın. Yük devretme testi devam eden çoğaltmayı veya üretim ortamınızda etkilemez. Belirli bir sanal makine (VM) veya üzerinde bir yük devretme testi çalıştırabilirsiniz bir [kurtarma planı](site-recovery-create-recovery-plans.md) birden çok VM içeren. 


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
Bu yordam, bir yük devretme sınaması için bir kurtarma planı Çalıştır açıklar. 

![Yük devretme sınaması](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Azure portalında Site Recovery içinde tıklatın **kurtarma planları** > *recoveryplan_name* > **yük devretme testi**.
2. Seçin bir **kurtarma noktası** yük devri yapılacağı. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    - **En son işlenen**: Bu seçenek tüm sanal makineleri Site Recovery tarafından işlenen en son kurtarma noktası plana başarısız olur. En son kurtarma için belirli bir VM'yi noktası görmek için kontrol **en son kurtarma noktası** VM Ayarları'nda. Bu seçenek, hiçbir zaman işlenmemiş verileri işlerken harcanan çünkü düşük RTO (Kurtarma süresi hedefi) sağlar.
    - **Son uygulama tutarlı**: Bu seçenek tüm sanal makineleri Site Recovery tarafından işlenen en son uygulama tutarlı kurtarma noktası plana başarısız olur. En son kurtarma için belirli bir VM'yi noktası görmek için kontrol **en son kurtarma noktası** VM Ayarları'nda. 
    - **En son**: Bu seçenek ilk Site Recovery hizmetine ona üzerinden başarısız olmadan önce her bir VM için bir kurtarma noktası oluşturmak üzere gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verileri sonra VM oluşturduğundan bu seçenek en düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son çoklu işlenen VM**: Bu seçenek çoklu VM tutarlılığı etkin olan bir veya daha fazla sanal makineleri içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM tutarlı bir kurtarma noktası ayarı etkin olan VM'ler devredin. Diğer Vm'leri üzerinde en son işlenen kurtarma noktası başarısız.  
    - **En son çoklu VM uygulamayla tutarlı**: Bu seçenek çoklu VM tutarlılığı etkin olan bir veya daha fazla sanal makineleri içeren kurtarma planları için kullanılabilir. Bir çoğaltma grubunun parçası olan VM'ler üzerinden son ortak çoklu VM uygulamaları tutarlı kurtarma noktası başarısız. Bunların en son uygulamaları tutarlı kurtarma noktasına diğer VM'ler devredin. 
    - **Özel**: belirli bir kurtarma noktası için belirli bir VM'de yük devri için bu seçeneği kullanın.
3. Test sanal makineleri oluşturulacak bir Azure sanal ağı seçin.

    - Site kurtarma girişimleri oluşturmak için sağlanan aynı IP adresine ve aynı ada sahip bir alt ağa VM'ler test **işlem ve ağ** VM ayarlar.
    - Aynı ada sahip bir alt ağ yük devretme sınaması için kullanılan Azure sanal ağındaki kullanılabilir değilse, ardından test VM ağdaki ilk alt alfabetik olarak oluşturulur.
    - Aynı IP adresi alt ağında kullanılabilir değilse, VM alt ağ içindeki başka bir kullanılabilir IP adresi alır. [Daha fazla bilgi edinin](#creating-a-network-for-test-failover).
4. Azure'a devretmek ve veri şifrelemesi etkin olduğunda, buna **şifreleme anahtarı**, şifreleme sağlayıcısı yükleme sırasında etkin olduğunda verilmiş sertifikayı seçin. Bu adımı yoksayabilirsiniz şifreleme etkin değil.
5. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi. Azure portalında test yineleme makine görebilmeniz gerekir.
6. Azure VM için RDP bağlantısı başlatmak için yapmanız [bir ortak IP adresi eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) devredilen VM'ye ağ arabiriminde. 
7. Her şeyin beklendiği gibi çalıştığını, tıklatın **temizleme yük devretme testi**. Yük devretme testi sırasında oluşturulan sanal makineleri siler.
8. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. 


![Yük devretme sınaması](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Yük devretme testi tetiklendiğinde aşağıdakiler gerçekleşir:

1. **Önkoşullar**: çalıştığı yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi.
2. **Yük devretme**: yük devretme işler ve böylece bir Azure VM ondan oluşturulabilir veri hazır.
3. **En son**: en son kurtarma noktası seçtiyseniz, hizmete gönderilen verilerden bir kurtarma noktası oluşturulur.
4. **Başlat**: Bu adım önceki adımda işlenen veri kullanarak bir Azure sanal makine oluşturur.

### <a name="failover-timing"></a>Yük devretme zamanlama

Aşağıdaki senaryolarda, yük devretme genellikle tamamlamak için yaklaşık 8-10 dakika sürer bir ek ara adım gerektirir:

* VMware Mobility hizmetinin 9.8 eski bir sürümünü çalıştıran VM'ler
* Fiziksel sunucuları
* VMware Linux VM'ler
* Hyper-V korumalı fiziksel sunucuları VM
* VMware VM burada aşağıdaki sürücüleri önyükleme sürücüleri değil:
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* VMware DHCP etkin, sahip olmayan VM DHCP veya statik IP adresleri kullanarak rrespective.

Tüm diğer durumlarda, hiçbir ara adım gerekli değildir ve yük devretme önemli ölçüde daha az zaman alır.


## <a name="create-a-network-for-test-failover"></a>Yük devretme sınaması için bir ağ oluşturma

Yük devretme sınaması için yalıtılmış olan bir ağ ağdan üretim kurtarma site içinde belirli seçmeniz önerilir **işlem ve ağ** her VM için ayarları. Bir Azure sanal ağı oluşturduğunuzda varsayılan olarak, bu diğer ağlardan yalıtılır. Test ağı üretim ağınızdan taklit etmelidir:

- Test ağı üretim ağınızdan alt ağlar aynı sayıda olmalıdır. Alt ağlar aynı adlara sahip olmalıdır.
- Test ağı aynı IP adresi rangek kullanmanız gerekir.
- Test ağı DNS DNS VM için belirtilen IP adresi ile güncelleştirmeniz **işlem ve ağ** ayarlar. Okuma [test Active Directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations) daha fazla ayrıntı için.


## <a name="test-failover-to-a-production-network-in-the-recovery-site"></a>Kurtarma sitesinde bir üretim ağı için yük devretme sınaması

Üretim ağınızdan bir olağanüstü durum kurtarma ayrıntıya test etmek isterseniz, üretim ağınızdan ayrı bir test ağı kullanmanız önerilir ancak aşağıdakilere dikkat edin: 

- Test yük devretme çalıştırdığınızda birincil VM kapatıldığını emin olun. Otherewise vardır, iki VM aynı anda aynı ağda çalışan aynı kimliğe sahip olacaktır. Bu, beklenmeyen sonuçlara neden olabilir.
- Yük devretmeyi temizlemek için yük devretme sınaması için oluşturulan VM'ler değişiklikler kaybolur. Bu değişiklikler birincil VM çoğaltılmaz.
- Üretim ortamınızda test üretim uygulamanız için bir kapalı kalma yol açar. Kullanıcılar, yük devretme testi devam ederken VM'ler üzerinde çalıştırılan uygulamaları kullanmamalısınız.  



## <a name="prepare-active-directory-and-dns"></a>Active Directory ve DNS hazırlama

Bir yük devretme sınaması için sınama uygulama çalıştırmak için test ortamınızda üretim Active Directory ortamınızı bir kopyasını gerekir. Okuma [test Active Directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations) daha fazla bilgi için.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra RDP kullanarak Azure vm'lerine bağlanmak isterseniz, tabloda özetlenen gereksinimleri izleyin.

**Yük devretme** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran azure VM** | Yük devretme işleminden önce şirket içi makine | Azure VM Internet üzerinden erişmek için RDP etkinleştirip için TCP ve UDP kurallarının eklendiğinden emin olun **ortak**, ve RDP tüm profilleri için izin verilen **Windows Güvenlik Duvarı**  >  **Uygulamalara izin**.<br/><br/> Azure VM'de bir siteden siteye bağlantı üzerinden erişmek için makinede RDP'yi etkinleştirir ve RDP izin verildiğinden emin olun **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler**, için**Etki alanı ve özel** ağlar.<br/><br/>  İşletim sisteminin SAN ilkesinin emin olun kümesine **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklemek zaman olmadığından emin olun. Windows update üzerinden başarısız ve VM güncelleştirme tamamlanana kadar oturum açamazsınız başlayabilir. 
**Windows çalıştıran azure VM** | Yük devretme sonrasında Azure VM |  [Bir ortak IP adresi eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) VM için.<br/><br/> RDP bağlantı noktasına gelen bağlantılara izin vermek ağ güvenlik grubu kurallarının devredilen VM'deki (ve bağlı olduğu Azure alt ağ) gerekir.<br/><br/> Denetleme **önyükleme tanılama** ekran VM görüntüsünü doğrulamak için.<br/><br/> Bağlanamıyorsanız, VM çalıştıran ve bunlar gözden olduğunu denetleyin [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).
**Linux çalıştıran azure VM** | Yük devretme işleminden önce şirket içi makine | VM'de Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılacak şekilde ayarlandığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran azure VM** | Yük devretme sonrasında Azure VM | SSH bağlantı noktasına gelen bağlantılara izin vermek ağ güvenlik grubu kurallarının devredilen VM'deki (ve bağlı olduğu Azure alt ağ) gerekir.<br/><br/> [Bir ortak IP adresi eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) VM için.<br/><br/> Denetleme **önyükleme tanılama** bir ekran görüntüsünü VM için.<br/><br/>



## <a name="next-steps"></a>Sonraki adımlar
Bir olağanüstü durum kurtarma ayrıntıya tamamladıktan sonra diğer türleri hakkında daha fazla bilgi [yük devretme](site-recovery-failover.md).
