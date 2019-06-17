---
title: Azure'da VM uygulama erişim sorunlarını giderme | Microsoft Docs
description: Azure'daki sanal makinelerde çalışan uygulamalara bağlanma sorunlarını yalıtmak için ayrıntılı sorun giderme adımları kullanın.
services: virtual-machines
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: olamaz uygulamasını başlatın, olmaz programını açın, engellenmiş, yüklenemiyor yapılandırılacakları, engellenen bir bağlantı noktası dinleyecek bağlantı noktasını dinleme
ms.assetid: b9ff7cd0-0c5d-4c3c-a6be-3ac47abf31ba
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 81535d51617a419174331dbf9b18ea558913dfa9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60922320"
---
# <a name="troubleshoot-application-connectivity-issues-on-virtual-machines-in-azure"></a>Azure sanal makinelerinde uygulama bağlantı sorunlarını giderme

Başlattığınızda veya bir Azure sanal makine (VM) üzerinde çalışan bir uygulamaya Bağlan çeşitli nedenleri vardır. Uygulama çalışmıyor nedenleri veya beklenen bağlantı noktalarında dinleme, engellenen dinleme bağlantı noktasını veya ağ doğru uygulama geçirme trafik kuralları. Bu makalede, bulmak ve sorunu düzeltmek için sistemli bir yaklaşım açıklanmaktadır.

Sanal makinenizde RDP veya SSH kullanarak bağlanma sorunu yaşıyorsanız, önce aşağıdaki makalelerden birine bakın:

* [Bir Windows tabanlı Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md)
* [Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantılarında sorun giderme](troubleshoot-ssh-connection.md).

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="quick-start-troubleshooting-steps"></a>Hızlı Başlangıç, sorun giderme adımları
Bir uygulamaya bağlanma konusunda sorunlar varsa, aşağıdaki genel sorun giderme adımlarını deneyin. Her adımdan sonra uygulamanızı yeniden bağlanmayı deneyin:

* Sanal makineyi yeniden başlatın
* Uç noktayı yeniden / güvenlik duvarı kurallarını / ağ güvenlik grubu (NSG) kuralları
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../../virtual-network/manage-network-security-group.md)
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../../cloud-services/cloud-services-enable-communication-role-instances.md)
* Farklı Azure sanal ağı gibi farklı konumdan bağlanma
* Sanal makineyi yeniden dağıtın
  * [Windows VM yeniden dağıtma](redeploy-to-new-node-windows.md)
  * [Linux VM'yi yeniden dağıtma](redeploy-to-new-node-linux.md)
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

## <a name="step-1-access-application-from-target-vm"></a>1\. adım: Hedef VM uygulamadan erişim
Uygun bir istemci programında uygulama üzerinde çalıştığı sanal makineden erişmeyi deneyin. Yerel ana bilgisayar adı, yerel IP adresi veya geri döngü adresi (127.0.0.1) kullanın.

![Sanal makineden doğrudan uygulama Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Örneğin, uygulama bir web sunucusu ise, VM'de bir tarayıcı açın ve sanal makinede barındırılan bir web sayfasına erişmek deneyin.

Uygulamaya erişebilmesi için Git [2. adım](#step2).

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Uygulamanın hedef sanal makinede çalışıyor.
* Uygulamanın beklenen TCP ve UDP bağlantı noktalarında dinliyor.

Hem Windows hem de Linux tabanlı sanal makineler kullanın **netstat - a** etkin dinleme bağlantı noktalarını göstermek için komutu. Uygulamanızı dinleniyor olması beklenen bağlantı noktaları için çıktıyı inceleyin. Uygulamayı yeniden başlatın veya beklenen bağlantı noktaları gerektiğinde kullanıp uygulamayı yerel olarak yeniden erişmeye yapılandırın.

## <a id="step2"></a>2. adım: Aynı sanal ağdaki başka bir VM'den erişimi uygulama
Farklı bir VM'den ancak sanal makinenin konak adı veya Azure tarafından atanan genel, özel veya sağlayıcı IP adresini kullanarak aynı sanal ağda, uygulamaya erişmeyi deneyin. Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için bulut hizmeti genel IP adresini kullanmayın.

![uygulamayı farklı bir VM'den Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Uygulama bir web sunucusu ise, örneğin, aynı sanal ağdaki farklı bir VM'de bir tarayıcı bir web sayfasına erişmek deneyin.

Uygulamaya erişebilmesi için Git [3. adım](#step3).

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Gelen istek ve yanıt giden trafiği hedef sanal makine konak güvenlik duvarını izin verir.
* Yetkisiz giriş algılama veya hedef sanal makine üzerinde çalışan yazılımı izleme ağ trafiğine izin verir.
* Trafik, bulut Hizmetleri uç noktaları veya ağ güvenlik grupları vermiş olursunuz:
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../../cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../../virtual-network/manage-network-security-group.md)
* Vm'nizde yolundaki test arasında VM ve Güvenlik Duvarı ' nı veya yük dengeleyici gibi sanal makinenizin çalıştıran ayrı bir bileşen trafiğe izin verir.

Bir Windows tabanlı sanal makinede güvenlik duvarı kuralları, uygulamanızın gelen ve giden trafik hariç olup olmadığını belirlemek için Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı'nı kullanın.

## <a id="step3"></a>3. adım: Erişim uygulamasından sanal ağın dışında
VM olarak uygulamanın çalıştığı uygulama sanal ağ dışındaki bir bilgisayardan erişmeyi deneyin. Farklı bir ağ özgün istemci bilgisayarınız kullanın.

![sanal ağ dışındaki bir bilgisayardan uygulamayı başlatın](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Uygulama bir web sunucusu ise, örneğin, web sayfası sanal ağ içinde olmayan bir bilgisayar üzerinde çalışan bir tarayıcıdan erişmeyi deneyin.

Uygulamaya erişemezseniz, aşağıdaki ayarları doğrulayın:

* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için:
  
  * VM için uç nokta yapılandırması özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını gelen trafiğe izin verdiğinden emin olun.
  * Uç nokta erişim denetim listelerini (ACL'ler) Internet'ten gelen trafiği engellemediğini doğrulayın.
  * Daha fazla bilgi için [uç noktaları ayarlama bir sanal makineye nasıl](../windows/classic/setup-endpoints.md).
* Resource Manager dağıtım modeli kullanılarak oluşturulan sanal makineler için:
  
  * VM için gelen NAT kuralı yapılandırması özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını gelen trafiğe izin verdiğinden emin olun.
  * Ağ güvenlik grupları giden yanıt trafiğini ve gelen talep verme doğrulayın.
  * Daha fazla bilgi için [bir ağ güvenlik grubu nedir?](../../virtual-network/security-overview.md)

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
[Bir Windows tabanlı Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md)

[Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantılarında sorun giderme](troubleshoot-ssh-connection.md)


