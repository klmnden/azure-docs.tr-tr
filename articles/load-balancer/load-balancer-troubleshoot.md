---
title: "Azure yük dengeleyici sorunlarını giderme | Microsoft Docs"
description: "Azure yük dengeleyici ile ilgili bilinen sorunlar sorun giderme"
services: load-balancer
documentationcenter: na
author: RamanDhillon
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: bc059221656a695bb43af0dca06df941ca77c73d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure yük dengeleyici sorun giderme

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Bu sayfa, ortak Azure yük dengeleyici sorular için sorun giderme bilgileri sağlar. Yük dengeleyiciye kullanılamadığında, en yaygın belirtiler aşağıdaki gibidir: 
- Yük dengeleyicinin arkasındaki VM'ler için sistem durumu araştırmalarının yanıt vermiyor 
- Yük dengeleyicinin arkasındaki VM'ler yapılandırılan bağlantı noktasındaki trafiği yanıt vermiyor

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Belirti: Yük dengeleyicinin arkasındaki VM'ler için sistem durumu araştırmalarının yanıt vermiyor
Arka uç sunucular yük dengeleyici kümesinde katılmayı araştırma onay geçmesi gerekir. Sistem durumu araştırmalarının hakkında daha fazla bilgi için bkz: [anlama yük dengeleyici yoklamaları](load-balancer-custom-probe-overview.md). 

Yük Dengeleyici arka uç havuzu VM'ler için aşağıdaki nedenlerden biriyle nedeniyle araştırmalar yanıt vermiyor olabilir: 
- Yük Dengeleyici arka uç havuzu VM sağlam değil 
- Yük Dengeleyici arka uç havuzu VM araştırma bağlantı noktasında dinleme yapmıyor 
- Yük Dengeleyici arka uç havuzu VM'ler üzerindeki bağlantı noktasının güvenlik duvarı ya da bir ağ güvenlik grubu engelleme 
- Yük Dengeleyici diğer yapılandırma hataları

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Neden 1: Yük dengeleyici arka uç havuzu VM sağlam değil 

**Doğrulama ve çözümleme**

Bu sorunu çözmek için katılan VM'ler için oturum açın ve VM durumu sağlıklı olup olmadığını denetleyin ve yanıt verebilir **PsPing** veya **TCPing** havuzundaki başka bir VM'den. VM sağlam değil veya için araştırma yanıt verip vermediğini, sorunu gidermek ve Yük Dengeleme katılabilmesi için önce VM sağlıklı bir duruma geri alın.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>Neden 2: Yük dengeleyici arka uç havuzu VM araştırma bağlantı noktasında dinleme yapmıyor
VM'yi sağlıklı olduğundan, ancak yoklama yanıt vermiyor, ardından bir araştırma bağlantı noktası VM veya VM katılan üzerinde açık değil olası bir nedeni olabilir Bu bağlantı noktasında dinleme değil.

**Doğrulama ve çözümleme**

1. VM arka uç için oturum açın. 
2. Bir komut istemi açın ve aşağıdaki komutu çalıştırın olmadığını doğrulamak için araştırma bağlantı noktasını dinleyen bir uygulamanın:   
            Netstat - bir
3. Bağlantı noktası durumu olarak listelenmemişse **DİNLEME**, doğru bağlantı noktasını yapılandırın. 
4. Alternatif olarak, olarak listelenen başka bir bağlantı noktası seçin **DİNLEME**ve dengeleyici yapılandırması uygun şekilde yük güncelleştirme.              

###<a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>3. neden: Güvenlik duvarı ya da bir ağ güvenlik grubu bağlantı noktası üzerinde yük dengeleyici arka uç havuzu VM'ler engelliyor  
VM üzerindeki güvenlik duvarında sonda bağlantı noktası engelliyor veya bir veya daha fazla ağ güvenlik grubu veya VM alt ağ üzerinde yapılandırılmış, bağlantı noktası ulaşmak araştırma izin vermeyen, VM durumu araştırması yanıt alamıyor.          

**Doğrulama ve çözümleme**

* Güvenlik Duvarı etkinse, araştırma bağlantı noktası izin verecek şekilde yapılandırılıp yapılandırılmadığını denetleyin. Aksi durumda, araştırma bağlantı noktasındaki trafiğe izin ve yeniden test etmek için Güvenlik Duvarı'nı yapılandırın. 
* Ağ güvenlik grupları listesinden araştırma bağlantı noktasındaki gelen veya giden trafiğe girişim olup olmadığını denetleyin. 
* Ayrıca, denetleyin bir **Reddet tüm** ağ güvenlik grupları kural NIC VM veya LB araştırmalar & trafiğine izin veren varsayılan kural daha yüksek önceliğe sahip alt ağ üzerinde (ağ güvenlik grupları izin vermelidir yük dengeleyici IP 168.63.129.16). 
* Bu kurallardan herhangi birinin araştırma trafiğini engelliyor, kaldırmak ve araştırma trafiğine izin verme kuralları yeniden yapılandırın.  
* VM için sistem durumu araştırmalarının yanıt şimdi başlatılmış olup olmadığını test edin. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4. neden: Diğer yapılandırma hataları yük dengeleyici
Önceki tüm nedenlerini doğrulandı ve doğru şekilde çözümlenen göründüğü ve sistem durumu araştırması için sonra el ile arka uç VM hala yanıtlamıyor için bağlanabilirliği test edin ve bağlantı anlamak için bazı izlemeleri toplamak.

**Doğrulama ve çözümleme**

* Kullanım **Psping** araştırma bağlantı noktası yanıt test VNet içindeki diğer VM'ler birinden (örnek:.\psping.exe -t 10.0.0.4:3389) ve sonuçları kaydedin. 
* Kullanım **TCPing** araştırma bağlantı noktası yanıt test VNet içindeki diğer VM'ler birinden (örnek:.\tcping.exe 10.0.0.4 3389) ve sonuçları kaydedin. 
* Yanıt bu ping testlerinde sonra alınırsa
    - Eşzamanlı Netsh trace hedef arka uç havuzu VM üzerinde ve başka bir test VM aynı sanal ağdan çalıştırın. Şimdi, belirli bir süre için PsPing testi, bazı ağ izleri toplamak ve testi durdurma. 
    - Ağ Yakalama çözümlemek ve ping sorguya ilgili gelen ve giden paketlerin olup olmadığını görebilirsiniz. 
        - Hiçbir gelen paket arka uç havuzu VM uyulması gereken, var. olası bir ağ güvenlik grupları veya trafiği engelleme UDR yanlış yapılandırma 
        - Hiçbir giden paketlerin arka uç havuzu VM uyulması gereken, VM ilgisiz sorunları (için eample, araştırma bağlantı noktası engelleme uygulama) için denetlenmesi gerekir. 
    - Araştırma paketleri (büyük olasılıkla ayarlarıyla UDR) başka bir hedef için yük dengeleyici ulaşmadan önce zorlanır durumunda doğrulayın. Bu arka uç VM hiçbir zaman ulaşması trafiğine neden olabilir. 
* Araştırma türü (örneğin, TCP HTTP) değiştirin ve ağ güvenlik grupları ACL'ler ve güvenlik duvarı sorun araştırma yanıtı yapılandırmayla ise doğrulamak için karşılık gelen bağlantı noktasını yapılandırın. Sistem durumu araştırma yapılandırması hakkında daha fazla bilgi için bkz: [Yük Dengeleme uç noktası sistem durumu araştırma Yapılandırması](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Belirti: VM'ler yük dengeleyicinin arkasına yapılandırılmış veri bağlantı noktası üzerinde trafiği yanıt vermiyor

VM sağlıklı olarak listelenir ve sistem durumu araştırmalarının için yanıt verir ancak hala Yük Dengeleme katılmıyor veya veri trafiği yanıt vermeyen bir arka uç havuzu, şunlardan birini nedeniyle olabilir: 
* Yük Dengeleyici arka uç havuzu VM veri bağlantı noktasında dinleme yapmıyor 
* Ağ güvenlik grubu bağlantı noktası üzerinde yük dengeleyici arka uç havuzu VM engelleme  
* Yük Dengeleyici aynı VM ve NIC erişme 
* Katılımcı yük dengeleyici arka uç havuzundan VM Internet yük dengeleyici VIP erişme 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>Neden 1: Yük dengeleyici arka uç havuzu VM veri bağlantı noktasında dinleme yapmıyor 
Bir VM için veri trafiğini yanıt vermezse, hedef bağlantı katılımcı VM açık olmadığından bu olabilir veya VM Bu bağlantı noktasında dinleme yapmıyor. 

**Doğrulama ve çözümleme**

1. VM arka uç için oturum açın. 
2. Bir komut istemi açın ve aşağıdaki komutu çalıştırın olmadığını doğrulamak için veri bağlantı noktasını dinleyen bir uygulamanın:  
            Netstat - bir 
3. Bağlantı noktası durumuyla listelenmemişse "DİNLEME" uygun dinleyici bağlantı noktasını yapılandırın 
4. Bağlantı noktasını dinleme olarak işaretlenmişse, bu bağlantı noktası olası sorunlar için hedef uygulama denetleyin. 

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>2. neden: Ağ güvenlik grubu bağlantı noktası üzerinde yük dengeleyici arka uç havuzu VM engelliyor  

Bir veya daha fazla ağ güvenlik grupları veya VM alt ağ üzerinde yapılandırdıysanız, VM yanıt verip vermediğini ise kaynak IP veya bağlantı noktası, engelliyor.

* VM arka uçta yapılandırılmış ağ güvenlik grupları listesi. Daha fazla bilgi için bkz.
    -  [Ağ güvenlik grupları Portalı'nı kullanarak yönetme](../virtual-network/virtual-network-manage-nsg-arm-portal.md)
    -  [PowerShell’i kullanarak ağ güvenlik gruplarını yönetme](../virtual-network/virtual-network-manage-nsg-arm-ps.md)
* Ağ güvenlik grupları listesinden kontrol edin:
    - veri bağlantı noktasındaki gelen veya giden trafiğe girişim sahiptir. 
    - bir **Reddet tüm** ağ güvenlik grubu kural NIC VM veya yük dengeleyici izin veren varsayılan kural yoklamaları daha yüksek bir önceliği olan alt ağ üzerinde ve trafik (ağ güvenlik grupları izin vermelidir 168.63.129.16, yük dengeleyici IP Sonda bağlantı noktası olan) 
* Kurallardan herhangi birinin trafiğini engelliyor, kaldırmak ve veri trafiğine izin vermek için bu kuralların yeniden yapılandırın.  
* VM için sistem durumu araştırmalarının yanıt şimdi başlatılmış olup olmadığını test edin.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>3. neden: aynı VM ve ağ arabirimi yük dengeleyici erişme 

VM yük dengeleyici arka uç içinde barındırılan uygulamanızı aynı ağ arabirimi aynı arka uç VM içinde barındırılan başka bir uygulamaya erişmeye çalıştığında, desteklenmeyen bir senaryodur ve başarısız olur. 

**Çözümleme** aşağıdaki yöntemlerden birini aracılığıyla bu sorunu çözümlemek için:
* Uygulama başına sanal makineleri ayrı arka uç havuzunu yapılandırın. 
* Her uygulamanın kendi ağ arabirimi ve IP adresi kullanarak şekilde uygulama ikili NIC Vm'lerde yapılandırın. 

### <a name="cause-4-accessing-the-internal-load-balancer-vip-from-the-participating-load-balancer-backend-pool-vm"></a>4. nedeni: iç yük dengeleyici VIP katılımcı yük dengeleyici arka uç havuzundan VM erişme

Bir ILB VIP VNet içinde yapılandırılır ve katılımcı arka uç VM'ler birini iç yük dengeleyici VIP erişmeye çalışıyor, bu hatasına neden olur. Bu senaryo desteklenmez.
**Çözümleme** değerlendirmek uygulama ağ geçidi veya diğer proxy'leri (örneğin, nginx veya haproxy) bu tür bir senaryoyu desteklemek için. Uygulama ağ geçidi hakkında daha fazla bilgi için bkz: [Application Gateway genel bakış](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Ek ağ yakalamaları
Bir destek servis talebi açmaya karar verirseniz, daha hızlı bir çözüm için aşağıdaki bilgileri toplayın. Tek bir arka uç aşağıdaki testleri gerçekleştirmek için VM seçin:
- Araştırma bağlantı noktası yanıt sınamak için sanal ağ içindeki VM'ler arka uç birinden Psping kullanın (örnek: psping 10.0.0.4:3389) ve sonuçları kaydedin. 
- Araştırma bağlantı noktası yanıt sınamak için sanal ağ içindeki VM'ler arka uç birinden TCPing kullanın (örnek: psping 10.0.0.4:3389) ve sonuçları kaydedin.
- Bu ping testlerinde yanıt alınmazsa, PsPing çalıştırın, ardından Netsh izleme durdurma yaparken eşzamanlı Netsh trace arka uç VM ve VM sanal ağ testi çalıştırın. 
  
## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).

