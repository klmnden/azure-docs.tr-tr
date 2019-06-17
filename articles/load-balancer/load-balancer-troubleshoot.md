---
title: Azure Load Balancer sorunlarını giderme
titlesuffix: Azure Load Balancer
description: Azure Load Balancer ile bilinen sorunlarını giderme
services: load-balancer
documentationcenter: na
author: chadmath
manager: cshepard
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: c5f92d564a93823fd9c0f932fa95f20d4e827761
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60734487"
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure Load Balancer sorunlarını giderme

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Bu sayfa, ortak Azure Load Balancer sorular için sorun giderme bilgileri sağlar. Yük Dengeleyici bağlantısına kullanılamadığında, en yaygın belirtiler aşağıdaki gibidir: 
- Yük dengeleyicinin arkasındaki VM'ler için sistem durumu araştırmaları yanıt vermiyor 
- Yük dengeleyicinin arkasındaki VM'ler için yapılandırılan bağlantı noktasındaki trafiğe yanıt vermiyor

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Belirti: Yük dengeleyicinin arkasındaki VM'ler için sistem durumu araştırmaları yanıt vermiyor
Arka uç sunucularına yük dengeleyici kümesiyle katılmak araştırma onay geçmesi gerekir. Sistem durumu araştırmaları hakkında daha fazla bilgi için bkz: [anlama yük dengeleyici araştırmalarını](load-balancer-custom-probe-overview.md). 

Yük Dengeleyici arka uç havuzu Vm'leri aşağıdaki nedenlerden dolayı araştırmaları için yanıt vermiyor olabilir: 
- Yük Dengeleyici arka havuzu sanal makinesi iyi durumda olmayan 
- Yük Dengeleyici arka havuzu sanal makine araştırma bağlantı noktasında dinleme yapmıyor 
- Yük Dengeleyici arka uç havuzuna VM üzerindeki bağlantı noktasının güvenlik duvarı veya bir ağ güvenlik grubu engelliyor 
- Yük Dengeleyici diğer yapılandırma hataları

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>1\. neden: Yük Dengeleyici arka havuzu sanal makinesi iyi durumda olmayan 

**Doğrulama ve çözümleme**

Bu sorunu çözmek için katılımcı VM'ler için oturum açın ve VM durumu sağlıklı olup olmadığını denetleyin ve yanıt verebilir **PsPing** veya **Telnet** havuzdaki başka bir VM'den. VM, sağlam değil veya araştırmasına yanıt verip vermediğini, sorunu düzeltmek ve Yük Dengeleme de katılabilmesi için önce VM'yi sağlıklı bir duruma alın.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>2\. neden: Yük Dengeleyici arka havuzu sanal makine araştırma bağlantı noktasında dinleme yapmıyor
VM, iyi durumda ancak araştırmasına yanıt vermiyorsa, ardından bir araştırma bağlantı noktasını VM veya VM katılan üzerinde açık değil olası neden olabilir, bağlantı noktası üzerinde dinleme yapmıyor.

**Doğrulama ve çözümleme**

1. Arka uç VM oturum açın. 
2. Bir komut istemi açın ve aşağıdaki komutu çalıştırarak var doğrulamak için araştırma bağlantı noktasında dinleyen bir uygulamanın alır:   
            Netstat - an
3. Bağlantı noktası durumu olarak listelenmemişse **DİNLEME**, doğru bağlantı noktasını yapılandırın. 
4. Alternatif olarak, başka bir bağlantı olarak listelenen seçin **DİNLEME**ve güncelleştirme dengeleyici yapılandırmasını uygun şekilde yükleyin.              

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>3\. neden: Yük Dengeleyici arka uç havuzuna VM üzerindeki bağlantı noktasının güvenlik duvarı veya bir ağ güvenlik grubu engelliyor  
Araştırma bağlantı noktasını VM üzerindeki güvenlik duvarını engelliyor veya bir veya daha fazla ağ alt ağında veya VM üzerinde yapılandırılan güvenlik grupları, araştırma bağlantı noktasını erişmesine izin vermemesi durumunda VM sistem durumu araştırmasına yanıt silemiyor.          

**Doğrulama ve çözümleme**

* Güvenlik Duvarı etkinse, yoklama bağlantı noktası izin verecek şekilde yapılandırılıp yapılandırılmadığını denetleyin. Aksi durumda, yoklama bağlantı noktası üzerinde trafiğe izin veren ve yeniden test etmek için güvenlik duvarını yapılandırın. 
* Ağ güvenlik grupları listesinden araştırma bağlantı noktasında gelen veya giden trafiği engelleme olup olmadığını denetleyin. 
* Ayrıca, kontrol bir **Reddet tüm** NIC VM veya LB araştırmaları & trafiğine izin veren varsayılan kuralı daha yüksek bir önceliğe sahip bir alt ağ üzerinde ağ güvenlik grupları kuralı (ağ güvenlik grupları izin vermesi gerekir, yük dengeleyici IP 168.63.129.16). 
* Bu kurallardan herhangi birine araştırma trafiğini engelliyorsanız kaldırın ve kuralları, araştırma trafiğine izin verecek şekilde yeniden yapılandırın.  
* VM sistem durumu yoklamalara yanıt vermeye artık başlatılmış olup olmadığını test edin. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4\. neden: Yük Dengeleyici diğer yapılandırma hataları
Önceki tüm nedenler doğrulanabilir ve doğru şekilde çözümlenen gibi görünüyor ve durum yoklaması için sonra el ile arka uç VM'den hala yanıtlamıyor bağlantısını için test etme ve bağlantı anlamak için bazı izlemeleri toplamak.

**Doğrulama ve çözümleme**

* Kullanım **Psping** bir araştırma bağlantı noktası yanıtını test etmek için sanal ağ içindeki diğer Vm'lere (örnek: -t 10.0.0.4:3389.\psping.exe) ve sonuçları kaydedin. 
* Kullanım **Telnet** bir araştırma bağlantı noktası yanıtını test etmek için sanal ağ içindeki diğer Vm'lere (örnek:.\tcping.exe 10.0.0.4 3389) ve sonuçları kaydedin. 
* Yanıt bu ping testlerinde sonra alınırsa
    - Hedef arka havuzu sanal makinesi üzerinde eşzamanlı bir Netsh trace ve başka bir test sanal makinesi aynı sanal ağdan çalıştırın. Artık, bir süre için PsPing test çalıştırması, bazı ağ izleri toplamak ve ardından testi durdurma. 
    - Ağ yakalamayı analiz etme ve gelen ve giden paketlerin ping sorgu için ilgili olup olmadığını görebilirsiniz. 
        - Arka uç havuzu sanal makinesi hiçbir gelen paket kullanılıyorsa var. büyük olasılıkla bir ağ güvenlik grupları veya trafiği engelleme UDR yanlış yapılandırma 
        - Arka uç havuzu sanal makinesi hiçbir giden paketlerin kullanılıyorsa, VM'nin ilgisiz sorunları (örneğin, yoklama bağlantı noktası engelleme uygulama) için denetlenecek gerekir. 
    - Araştırma paketleri (büyük olasılıkla UDR ayarları aracılığıyla) başka bir hedef için yük dengeleyici ulaşmadan önce isabetten, doğrulayın. Bu, hiçbir zaman arka uç VM'den ulaşmak trafiği neden olabilir. 
* Araştırma türü (örneğin, HTTP TCP için) değiştirin ve ağ güvenlik grupları ACL ve güvenlik duvarı araştırma yanıt yapılandırmasıyla sorunu olup olmadığını doğrulamak üzere karşılık gelen bağlantı noktasını yapılandırın. Sistem durumu araştırması yapılandırması hakkında daha fazla bilgi için bkz: [Yük Dengeleme uç noktası sistem durumu araştırması Yapılandırması](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Belirti: Vm'leri yük dengeleyicinin arkasına yapılandırılmış veri bağlantı noktası üzerinde trafiğe yanıt vermiyor

Arka uç havuzuna VM sağlıklı olarak listelenir ve sistem durumu araştırmaları için yanıt verir ancak hala Yük Dengeleme katılmıyor veya veri trafiğe yanıt vermiyorsa, aşağıdakilerden birini nedeniyle olabilir: 
* Yük Dengeleyici arka havuzu sanal makine veri bağlantı noktasında dinleme yapmıyor 
* Ağ güvenlik grubu bağlantı noktası yük dengeleyici arka havuzu sanal makinesi üzerinde engelliyor  
* Yük Dengeleyici aynı sanal makine veya NIC'den erişme 
* Internet yük dengeleyici ön uç katılan yük dengeleyici arka uç havuzundan VM erişme 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>1\. neden: Yük Dengeleyici arka havuzu sanal makine veri bağlantı noktasında dinleme yapmıyor 
VM veri trafiğe yanıt vermezse, ya da, hedef bağlantı noktası üzerinde katılan bir VM'nin, açık değil. Bunun nedeni olabilir veya VM Bu bağlantı noktasında dinleme yapmıyor. 

**Doğrulama ve çözümleme**

1. Arka uç VM oturum açın. 
2. Bir komut istemi açıp olan veri bağlantı noktası üzerinde dinleyen bir uygulamanın var. doğrulamak için aşağıdaki komutu çalıştırın:  netstat - an 
3. Bağlantı noktası durumla listede yoksa "DİNLEME" uygun dinleyici bağlantı noktasını yapılandırma 
4. Bağlantı noktasını dinleme olarak işaretlenmişse, ardından hedef uygulamanın bu bağlantı noktasında olası sorunları kontrol edin. 

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>2\. neden: Ağ güvenlik grubu bağlantı noktası yük dengeleyici arka havuzu sanal makinesi üzerinde engelliyor  

Bir veya daha fazla ağ güvenlik grubu, alt ağdaki veya VM'de yapılandırdıysanız, VM yanıt verip vermediğini ise kaynak IP veya bağlantı noktası, engelliyor.

* Arka uç VM üzerinde yapılandırılan ağ güvenlik gruplarının listesi. Daha fazla bilgi için [ağ güvenlik gruplarını yönetme](../virtual-network/manage-network-security-group.md).
* Ağ güvenlik grupları listesinden komutu kontrol edin:
    - veri bağlantı noktasında gelen veya giden trafiği engelleme sahiptir. 
    - bir **Reddet tüm** ağ üzerinde NIC'nin VM veya yük dengeleyici izin veren varsayılan kuralı araştırmaları daha yüksek bir önceliği olan bir alt ağ güvenlik grubu kuralı ve trafik (ağ güvenlik grupları izin vermelidir 168.63.129.16, yük dengeleyici IP Araştırma bağlantı noktası olan) 
* Kurallardan herhangi birinin trafiğini engelliyorsanız kaldırın ve bu kurallar veri trafiğine izin verecek şekilde yeniden yapılandırın.  
* VM sistem durumu araştırmaları için yanıt vermek artık başlatılmış olup olmadığını test edin.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>3\. neden: Yük Dengeleyici aynı VM ve ağ arabiriminden erişme 

VM yük dengeleyicinin arka uçtaki barındırılan uygulamanız aynı ağ arabirimi aynı arka uç VM içinde barındırılan başka bir uygulamaya erişmeye çalışırsa, desteklenmeyen bir senaryodur ve başarısız olur. 

**Çözüm** aşağıdaki yöntemlerden birini kullanarak bu sorunu çözebilirsiniz:
* Uygulama başına ayrı bir arka uç havuzu Vm'leri yapılandırın. 
* Her uygulama kendi ağ arabirimi ve IP adresi kullanarak şekilde uygulamayı çift NIC VM yapılandırın. 

### <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>4\. neden: İç yük dengeleyici ön uç katılan yük dengeleyici arka uç havuzundan VM erişme

Flow kaynak VM eşlendiğinde bir iç Load Balancer bir sanal ağ içinde yapılandırılır ve bir katılımcı arka uç sanal makinelerinin iç yük dengeleyici ön ucuna erişmeye çalışıyor, hataları oluşabilir. Bu senaryo desteklenmez. Gözden geçirme [sınırlamaları](load-balancer-overview.md#limitations) hakkında ayrıntılı bilgi için.

**Çözüm** proxy kullanma dahil olmak üzere bu senaryo, engellemesini kaldırmak için birkaç yolu vardır. Uygulama ağ geçidi veya 3 diğer taraf proxy'ler (örneğin, ngınx veya haproxy) değerlendirin. Application Gateway hakkında daha fazla bilgi için bkz. [Application Gateway genel bakış](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Ek ağ yakalama
Bir destek olayı açmaya karar verirseniz, daha hızlı bir çözüm için aşağıdaki bilgileri toplayın. Tek bir arka uç VM'den aşağıdaki testleri gerçekleştirmek için seçin:
- Araştırma bağlantı noktası yanıtını test etmek için bir arka uç sanal makineler VNet içinde Psping kullanın (örnek: psping 10.0.0.4:3389) ve sonuçları kaydedin. 
- Bu ping testlerinde yanıt alınmazsa, PsPing çalıştırın, ardından Netsh izleme durdurma sırasında eşzamanlı bir Netsh trace arka uç VM'den ve VNet test VM'SİNDE çalıştırın. 
  
## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).

