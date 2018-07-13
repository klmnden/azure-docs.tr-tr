---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ab668a905b435287a4eaf96ff04b2fa5b54deb1d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38764731"
---
Başlattığınızda veya bir Azure sanal makine (VM) üzerinde çalışan bir uygulamaya Bağlan çeşitli nedenleri vardır. Uygulama çalışmıyor nedenleri veya beklenen bağlantı noktalarında dinleme, engellenen dinleme bağlantı noktasını veya ağ doğru uygulama geçirme trafik kuralları. Bu makalede, bulmak ve sorunu düzeltmek için sistemli bir yaklaşım açıklanmaktadır.

Sanal makinenizde RDP veya SSH kullanarak bağlanma sorunu yaşıyorsanız, önce aşağıdaki makalelerden birine bakın:

* [Bir Windows tabanlı Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantılarında sorun giderme](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../articles/resource-manager-deployment-model.md). Bu makale her iki modelin de nasıl kullanıldığını kapsıyor olsa da, Microsoft en yeni dağıtımların Resource Manager modelini kullanmasını önermektedir.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="quick-start-troubleshooting-steps"></a>Hızlı Başlangıç, sorun giderme adımları
Bir uygulamaya bağlanma konusunda sorunlar varsa, aşağıdaki genel sorun giderme adımlarını deneyin. Her adımdan sonra uygulamanızı yeniden bağlanmayı deneyin:

* Sanal makineyi yeniden başlatın
* Uç noktayı yeniden / güvenlik duvarı kurallarını / ağ güvenlik grubu (NSG) kuralları
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../articles/virtual-network/manage-network-security-group.md)
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Farklı Azure sanal ağı gibi farklı konumdan bağlanma
* Sanal makineyi yeniden dağıtın
  * [Windows VM yeniden dağıtma](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Linux VM'yi yeniden dağıtma](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Sanal makine yeniden oluşturun

Daha fazla bilgi için [uç noktası bağlantısı (RDP/SSH/HTTP hataları vb.) sorun giderme](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Ayrıntılı sorun giderme genel bakış
Bir Azure sanal Makinesi'nde çalışan uygulamaya erişim sorunlarını gidermek için dört ana alan vardır.

![sorun giderme aplikaci nelze spustit.](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. Azure sanal makine üzerinde çalışan uygulama.
   * Uygulama doğru şekilde çalışıyor mu?
2. Azure sanal makinesi.
   * VM, düzgün çalışmasını ve isteklere yanıt mi?
3. Azure ağ uç noktaları.
   * Hizmet uç noktaları Klasik dağıtım modelinde sanal makineler için bulut.
   * Ağ güvenlik grupları ve Resource Manager dağıtım modelinde sanal makineler için gelen NAT kuralları.
   * Kullanıcıların bir Flow VM/uygulama beklenen bağlantı noktalarında trafiği?
4. Internet edge cihazı.
   * Güvenlik duvarı kuralları yerinde doğru akan trafiği engelliyor?

Uygulama siteden siteye VPN veya ExpressRoute bağlantısı üzerinden erişen istemci bilgisayarları için sorunlara neden olabilecek ana uygulama ve Azure sanal makinesi alanlardır.

Sorun ve doğrusunu kaynağını belirlemek için bu adımları izleyin.

## <a name="step-1-access-application-from-target-vm"></a>1. adım: hedef VM'den uygulamaya erişim
Uygun bir istemci programında uygulama üzerinde çalıştığı sanal makineden erişmeyi deneyin. Yerel ana bilgisayar adı, yerel IP adresi veya geri döngü adresi (127.0.0.1) kullanın.

![Sanal makineden doğrudan uygulama Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Örneğin, uygulama bir web sunucusu ise, VM'de bir tarayıcı açın ve sanal makinede barındırılan bir web sayfasına erişmek deneyin.

Uygulamaya erişebilmesi için Git [2. adım](#step2).

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Uygulamanın hedef sanal makinede çalışıyor.
* Uygulamanın beklenen TCP ve UDP bağlantı noktalarında dinliyor.

Hem Windows hem de Linux tabanlı sanal makineler kullanın **netstat - a** etkin dinleme bağlantı noktalarını göstermek için komutu. Uygulamanızı dinleniyor olması beklenen bağlantı noktaları için çıktıyı inceleyin. Uygulamayı yeniden başlatın veya beklenen bağlantı noktaları gerektiğinde kullanıp uygulamayı yerel olarak yeniden erişmeye yapılandırın.

## <a id="step2"></a>2. adım: uygulama aynı sanal ağdaki başka bir VM'den erişim.
Farklı bir VM'den ancak sanal makinenin konak adı veya Azure tarafından atanan genel, özel veya sağlayıcı IP adresini kullanarak aynı sanal ağda, uygulamaya erişmeyi deneyin. Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için bulut hizmeti genel IP adresini kullanmayın.

![uygulamayı farklı bir VM'den Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Uygulama bir web sunucusu ise, örneğin, aynı sanal ağdaki farklı bir VM'de bir tarayıcı bir web sayfasına erişmek deneyin.

Uygulamaya erişebilmesi için Git [3. adım](#step3).

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Gelen istek ve yanıt giden trafiği hedef sanal makine konak güvenlik duvarını izin verir.
* Yetkisiz giriş algılama veya hedef sanal makine üzerinde çalışan yazılımı izleme ağ trafiğine izin verir.
* Trafik, bulut Hizmetleri uç noktaları veya ağ güvenlik grupları vermiş olursunuz:
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../articles/virtual-network/manage-network-security-group.md)
* Vm'nizde yolundaki test arasında VM ve Güvenlik Duvarı ' nı veya yük dengeleyici gibi sanal makinenizin çalıştıran ayrı bir bileşen trafiğe izin verir.

Bir Windows tabanlı sanal makinede güvenlik duvarı kuralları, uygulamanızın gelen ve giden trafik hariç olup olmadığını belirlemek için Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı'nı kullanın.

## <a id="step3"></a>3. adım: uygulamasından sanal ağın dışında erişim
VM olarak uygulamanın çalıştığı uygulama sanal ağ dışındaki bir bilgisayardan erişmeyi deneyin. Farklı bir ağ özgün istemci bilgisayarınız kullanın.

![sanal ağ dışındaki bir bilgisayardan uygulamayı başlatın](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Uygulama bir web sunucusu ise, örneğin, web sayfası sanal ağ içinde olmayan bir bilgisayar üzerinde çalışan bir tarayıcıdan erişmeyi deneyin.

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için:
  
  * VM için uç nokta yapılandırması özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını gelen trafiğe izin verdiğinden emin olun.
  * Uç nokta erişim denetim listelerini (ACL'ler) Internet'ten gelen trafiği engellemediğini doğrulayın.
  * Daha fazla bilgi için [uç noktaları ayarlama bir sanal makineye nasıl](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Resource Manager dağıtım modeli kullanılarak oluşturulan sanal makineler için:
  
  * VM için gelen NAT kuralı yapılandırması özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını gelen trafiğe izin verdiğinden emin olun.
  * Ağ güvenlik grupları giden yanıt trafiğini ve gelen talep verme doğrulayın.
  * Daha fazla bilgi için [bir ağ güvenlik grubu nedir?](../articles/virtual-network/security-overview.md)

Sanal makine veya uç nokta yük dengeli bir küme üyesi ise:

* Araştırma Protokolü (TCP veya UDP) ve bağlantı noktası numarası doğru olduğundan emin olun.
* Araştırma protokolü ve bağlantı noktası, yük dengeli küme protokolü ve bağlantı noktası farklı:
  * Uygulama araştırma Protokolü (TCP veya UDP) ve bağlantı noktası numarası dinlediğini doğrulayın (kullanın **netstat – bir** hedef VM'de).
  * Konak güvenlik duvarı hedef VM'de giden araştırma yanıt trafiğini ve gelen bir araştırma isteğinin izin doğrulayın.

Uygulamaya erişebilmesi için Internet edge cihazınıza izin verdiğinden emin olun:

* Azure sanal makinesi, istemci bilgisayardan giden uygulama isteği akışına.
* Azure sanal makinesinden gelen uygulama yanıt trafiğini.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Adım 4 uygulama erişemiyorsanız kullanan IP doğrulama ayarlarını denetlemek için. 

Daha fazla bilgi için [Azure ağ izlemeye genel bakış](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Ek kaynaklar
[Bir Windows tabanlı Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantılarında sorun giderme](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

