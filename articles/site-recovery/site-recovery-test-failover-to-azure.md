---
title: Azure Site Recovery kullanarak Azure'a olağanüstü durum kurtarma tatbikatı çalıştırma | Microsoft Docs
description: Şirket içinden Azure Site Recovery hizmetini kullanarak Azure'a olağanüstü durum kurtarma tatbikatı çalıştırma hakkında bilgi edinin.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 67cbd37becb1fe87a7f4f554f574b6e5219c9243
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399927"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma 


Bu makalede Site Recovery yük devretme testi kullanarak Azure'a olağanüstü durum kurtarma tatbikatı gerçekleştirme.  

Çoğaltma ve olağanüstü durum kurtarma stratejiniz, herhangi bir veri kaybı veya kesinti süresi olmadan doğrulamak için bir yük devretme testi çalıştırın. Yük devretme testi devam eden çoğaltma veya üretim ortamınızı etkilemez. Belirli bir sanal makineye (VM) üzerinde veya yük devretme testi çalıştırma bir [kurtarma planı](site-recovery-create-recovery-plans.md) birden çok VM içeren.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
Bu yordamda, bir kurtarma planı için bir yük devretme testi çalıştırma açıklanmaktadır. Tek bir VM için yük devretme testi çalıştırmak isterseniz, açıklanan adımları izleyin [burada](tutorial-dr-drill-azure.md#run-a-test-failover-for-a-single-vm)

![Yük devretme testi](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Azure portalında Site Recovery, tıklayın **kurtarma planları** > *recoveryplan_name* > **yük devretme testi**.
2. Seçin bir **kurtarma noktası** hangi yük devretmek. Şu seçeneklerden birini kullanabilirsiniz:
    - **En son işlenen**: Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktasına plandaki tüm VM'lerin yükünü başarısız olur. En son kurtarma için belirli bir VM'ye noktası olarak görmek için **en son kurtarma noktaları** VM ayarları. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek, tüm VM'lerin yükünü planında Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktası için başarısız olur. En son kurtarma için belirli bir VM'ye noktası olarak görmek için **en son kurtarma noktaları** VM ayarları.
    - **En son**: Bu seçenek ilk önce yük devretme için her VM için bir kurtarma noktası oluşturmak için Site Recovery hizmetine gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery'ye çoğaltılan tüm verilere sahip sonra VM oluşturulduğundan, bu seçenek en düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son işlenen VM'li**: Bu seçenek, çoklu VM tutarlılığı etkin olan bir veya daha fazla VM içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM tutarlı kurtarma noktası ayarı etkin Vm'lerle devredin. Diğer VM'ler en son işlenen kurtarma noktasına yük devredebilirsiniz.  
    - **En son çoklu VM uygulamayla tutarlı**: Bu seçenek, çoklu VM tutarlılığı etkin olan bir veya daha fazla VM içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM uygulamayla tutarlı kurtarma noktası çoğaltma grubunun parçası olan Vm'leri devredin. Bunların en son uygulamayla tutarlı kurtarma noktası için diğer Vm'lere devredin.
    - **Özel**: Belirli bir VM'ye belirli bir kurtarma noktasına yük devretmek için bu seçeneği kullanın.
3. Test sanal makineleri oluşturulacağı bir Azure sanal ağı seçin.

    - Site kurtarma denemeleri oluşturmak için bir alt ağ ile aynı ada ve aynı IP adresine sağlanan Vm'leri test **işlem ve ağ** VM ayarları.
    - Aynı ada sahip bir alt ağ, yük devretme testi için kullanılan Azure sanal ağında kullanılabilir değilse, sonra test sanal makine ağdaki ilk alt alfabetik olarak oluşturulur.
    - Aynı IP adresi alt ağda kullanılabilir durumda değilse, VM alt ağ içindeki başka bir kullanılabilir IP adresi alır. [Daha fazla bilgi edinin](#create-a-network-for-test-failover).
4. Azure'a devretmek ve veri şifrelemesi etkin olduğunda, buna **şifreleme anahtarı**, sağlayıcı yüklemesi sırasında şifreleme etkin olduğunda verilmiş sertifikayı seçin. Şifreleme etkin değilse, bu adımı yoksayabilirsiniz.
5. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi. Azure portalında test çoğaltma makinesi görebilmeniz gerekir.
6. Azure VM ile RDP bağlantısı başlatmak için şunları yapmanız [genel IP adresi ekleme](https://aka.ms/addpublicip) devredilen VM'nin ağ arabiriminde.
7. Her şeyin beklendiği gibi çalıştığından, tıklayın **yük devretme testini Temizle**. Bu, yük devretme testi sırasında oluşturulan sanal makineleri siler.
8. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.


![Yük devretme testi](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Bir yük devretme tetiklendiğinde, aşağıdakiler gerçekleşir:

1. **Önkoşullar**: Bir önkoşul denetimi yük devretme için gerekli tüm koşulların karşılandığından emin olmak için çalıştırılır.
2. **Yük devretme**: Yük devretme, işler ve böylece bir Azure VM ondan oluşturulabilir veri hazır.
3. **En son**: En son kurtarma noktası seçtiyseniz, hizmete gönderilen verilerden bir kurtarma noktası oluşturulur.
4. **Başlangıç**: Bu adım, önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturur.

### <a name="failover-timing"></a>Yük devretme zamanlama

Aşağıdaki senaryolarda, yük devretme tamamlanması genellikle yaklaşık 8-10 dakika sürer. bir çok ara adım gerektirir:

* Mobility hizmetinin 9.8 eski bir sürümünü çalıştıran VMware sanal makinelerini
* Fiziksel sunucular
* VMware sanal makineleri
* Fiziksel sunucuları olarak korunan Hyper-V VM
* VMware VM burada aşağıdaki sürücüleri önyükleme sürücüleri değildir:
    * storvsc
    * vmbus
    * storflt
    * intelide
    * Atapi
* DHCP etkin sahip olmayan VMware VM olup kullanmanın edildiklerine bakılmaksızın, DHCP veya statik IP adresleri.

Diğer tüm durumlarda, hiçbir ara adım gerekli değildir ve yük devretme önemli ölçüde daha az zaman alır.


## <a name="create-a-network-for-test-failover"></a>Yük devretme testi için ağ oluşturma

Yük devretme testi için her VM'nin **İşlem ve Ağ** ayarlarından üretim kurtarma sitesi ağınızdan yalıtılmış bir ağ seçmenizi öneririz. Varsayılan olarak yeni oluşturduğunuz Azure sanal ağları diğer ağlardan yalıtılmış olur. Test ağı üretim ağınıza benzer olmalıdır:

- Test ağında üretim ağınızdaki sayıda alt ağ olmalıdır. Alt ağların adı aynı olmalıdır.
- Test ağında aynı IP adresi aralığı kullanılmalıdır.
- Test ağının DNS sunucusunu **İşlem ve Ağ** ayarlarında DNS VM için belirtilen IP adresiyle güncelleştirin. Daha fazla ayrıntı için [Active Directory için yük devretme testi ile ilgili dikkat edilmesi gerekenler](site-recovery-active-directory.md#test-failover-considerations) sayfasını okuyun.


## <a name="test-failover-to-a-production-network-in-the-recovery-site"></a>Kurtarma sitesinde bir üretim ağı için yük devretme testi

Olağanüstü durum kurtarma tatbikatı üretim ağınızda test etmek istiyorsanız, üretim ağınızdan ayrı bir test ağı kullanmanızı öneririz, ancak aşağıdakilere dikkat edin:

- Test yük devretme çalıştırdığınızda birincil VM kapatılmış olduğundan emin olun. Aksi takdirde iki VM aynı anda aynı ağda çalışan aynı kimliğe sahip olacaktır. Bu beklenmeyen sonuçlara yol açabilir.
- Yük devretme ' temizlerken vm'lere yük devretme testi için oluşturulan değişiklikler kaybolur. Bu değişiklikler birincil VM yeniden çoğaltılmaz.
- Üretim ortamınızda test, üretim uygulamanızın bir kesinti yol açar. Kullanıcılar, yük devretme testi devam ederken Vm'lerinde çalışan uygulamaları kullanmamalısınız.  



## <a name="prepare-active-directory-and-dns"></a>Active Directory ve DNS hazırlama

Uygulama testi için bir yük devretme testi çalıştırmak için test ortamında üretim Active Directory ortamınızı bir kopyasına ihtiyacınız vardır. Okuma [test Active Directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations) daha fazla bilgi için.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra RDP/SSH'yi kullanarak Azure Vm'lerine bağlanmak isterseniz tabloda özetlenen gereksinimleri izleyin.

**Yük devretme** | **Konum** | **Eylemler**
--- | --- | ---
**Windows çalıştıran azure VM** | Yük devretmeden önce şirket içi makine | Azure VM internet üzerinden erişim için RDP'yi etkinleştirin ve için TCP ve UDP kurallarının eklendiğinden emin olun **genel**, ve RDP tüm profilleri için izin verilen **Windows Güvenlik Duvarı**  >  **İzin verilen uygulamalar**.<br/><br/> Azure VM'de bir siteden siteye bağlantı üzerinden erişim için makinede RDP'yi etkinleştirin ve içinde RDP'ye izin verildiğinden emin olun **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler**, için**Etki alanı ve özel** ağlar.<br/><br/>  İşletim sisteminin SAN ilkesinin emin olun kümesine **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Bekleyen herhangi bir Windows güncelleştirme VM'de yük devretme tetiklediğinizde emin olun. Windows update, yük devretme ve VM güncelleştirme tamamlanana kadar oturum açamayacaksınız başlayabilir.
**Windows çalıştıran azure VM** | Yük devretmeden sonra Azure VM |  VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> Ağ güvenlik grubu kuralları yük devredilen VM (ve VM'nin bağlandığı Azure alt ağındaki) RDP bağlantı noktasına gelen bağlantılara izin vermesi gerekir.<br/><br/> Denetleme **önyükleme tanılaması** VM görüntüsü doğrulayın.<br/><br/> Bağlanamıyorsanız, VM'nin çalıştıran ve bunlar gözden olduğunu kontrol edin [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).
**Linux çalıştıran azure sanal makine** | Yük devretmeden önce şirket içi makine | VM'de Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlatılacak şekilde ayarlandığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran azure sanal makine** | Yük devretmeden sonra Azure VM | Yük devredilen VM (ve VM'nin bağlandığı Azure alt ağ) üzerindeki ağ güvenlik grubu kurallarının SSH bağlantı noktasına gelen bağlantılara izin vermesi gerekir.<br/><br/> VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> Denetleme **önyükleme tanılaması** için VM görüntüsü.<br/><br/>

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma tatbikatı tamamladıktan sonra diğer türleri hakkında daha fazla bilgi [yük devretme](site-recovery-failover.md).
