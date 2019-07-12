---
title: Bir Azure sanal makinesi için ayrıntılı SSH giderme | Microsoft Docs
description: SSH gidermeyle ilgili daha fazla bilgi için bir Azure sanal makinesine bağlanma sorunu daha ayrıntılı
keywords: SSH bağlantısını reddetti, ssh hatası, azure, SSH bağlantısı başarısız ssh
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 153117488cf94eb304eeb63ba6dca92a6c6ff27d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696224"
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-to-a-linux-vm-in-azure"></a>Azure'da bir Linux VM'ye bağlanma sorunları için sorun giderme adımları ayrıntılı SSH
SSH istemcisi VM üzerinde SSH hizmete erişmek mümkün olmayabilir birçok olası nedeni vardır. Daha izlediyseniz [sorun giderme adımları genel SSH](troubleshoot-ssh-connection.md), başka bir bağlantı sorunu gidermek gerekir. Bu makalede, SSH bağlantısını nerede başarısız olduğunu ve nasıl çözümleyeceğiniz belirlemek için ayrıntılı sorun giderme adımlarını size yol gösterir.

## <a name="take-preliminary-steps"></a>Başlangıç adımları
Aşağıdaki diyagramda, söz konusu olan bileşenleri gösterilmektedir.

![SSH hizmeti bileşenlerini gösteren diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Aşağıdaki adımlar hatanın kaynağını yalıtmak ve çözümleri veya geçici çözümler dağıtamadığını bulmak yardımcı olur.

1. Portalda VM durumunu denetleyin.
   İçinde [Azure portalında](https://portal.azure.com)seçin **sanal makineler** > *VM adı*.

   VM durumu bölmesi göstermelidir **çalıştıran**. İşlem, depolama ve ağ kaynakları için son etkinliğin show aşağı kaydırın.

2. Seçin **ayarları** uç noktaları, IP adresleri, ağ güvenlik grupları ve diğer ayarları incelemek için.

   VM içinde görüntüleyebilirsiniz SSH trafiği için tanımlanmış bir uç nokta olmalıdır **uç noktaları** veya  **[ağ güvenlik grubu](../../virtual-network/security-overview.md)** . Resource Manager kullanılarak oluşturulan vm'lerde uç noktaları, bir ağ güvenlik grubunda depolanır. Kural ağ güvenlik grubuna uygulanır ve alt ağında başvurulan doğrulayın.

Ağ bağlantısını doğrulamak için yapılandırılan Uç noktalara denetleyin ve, HTTP veya başka bir hizmet gibi başka bir protokol üzerinden VM'ye bağlanıp bağlanamadığınızı görün.

Bu adımlardan sonra SSH bağlantısını tekrar deneyin.

## <a name="find-the-source-of-the-issue"></a>Sorunun kaynağı bulunamadı
Sorunları veya yanlış yapılandırmalarını aşağıdaki alanlarda nedeniyle Azure sanal makinesinde SSH hizmetine bağlanmak SSH istemcisi bilgisayarınızda başarısız olabilir:

* [SSH istemcisi bilgisayar](#source-1-ssh-client-computer)
* [Kuruluş edge cihazı](#source-2-organization-edge-device)
* [Bulut Hizmeti uç noktası ve erişim denetimi listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Linux tabanlı bir Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1\. kaynak: SSH istemcisi bilgisayar
Bilgisayarınızı hata kaynağı olarak ortadan kaldırmak için başka bir şirket içi, Linux tabanlı bir bilgisayarda SSH bağlantıları zorlaştırabilir doğrulayın.

![SSH istemcisi bilgisayar bileşenleri vurgulanmaktadır diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Bağlantı başarısız olursa, bilgisayarınızda aşağıdaki sorunlar için denetleyin:

* Gelen veya giden SSH trafiği (TCP 22) engelleyen bir yerel güvenlik duvarı ayarı
* SSH bağlantıları engelliyor istemci proxy yazılımını yerel olarak yüklü
* Ağ izleme SSH bağlantıları engelliyor yazılımını yerel olarak yüklü
* Trafiği izlemek veya trafiği belirli türlerini izin vermek/engellemek diğer güvenlik yazılım türleri

Şu koşullardan biri geçerliyse, geçici olarak devre dışı yazılım ve bağlantı bilgisayarınızda engellenme nedenini bulmak için bir şirket içi bilgisayarınıza bir SSH bağlantısı deneyin. Ardından SSH bağlantılarına izin verecek şekilde yazılım ayarları düzeltmek için ağ yöneticinizle birlikte çalışın.

Sertifika kimlik doğrulaması kullanıyorsanız, giriş dizininizde .ssh klasörü bu izinlere sahip olduğunuzu doğrulayın:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (ya da bunlarda depolanan, özel anahtarlara sahip diğer tüm dosyaları)
* Chmod 644 ~/.ssh/known_hosts (SSH yoluyla bağlı olduğunuz konakları içerir)

## <a name="source-2-organization-edge-device"></a>2\. kaynak: Kuruluş edge cihazı
Hata kaynağı olarak kuruluş edge Cihazınızı kaldırmak için doğrudan Internet'e bağlı bir bilgisayar, Azure VM ile SSH bağlantıları yapabilir doğrulayın. VM bir siteden siteye VPN veya Azure ExpressRoute bağlantısı üzerinden erişiyorsanız atlamak [kaynak 4: Ağ güvenlik grupları](#nsg).

![Kuruluş edge cihazı vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Doğrudan olan bir bilgisayar yoksa, Internet'e bağlı kendi kaynak grubunda yeni bir Azure VM oluşturma veya Bulut hizmeti ve bu yeni VM kullanın. Daha fazla bilgi için [Azure'da Linux çalıştıran bir sanal makine oluşturma](../linux/quick-create-cli.md). Test ile işiniz bittiğinde kaynak grubu veya VM ve bulut hizmetini silin.

Doğrudan Internet'e bağlı bir bilgisayar ile bir SSH bağlantısı oluşturun, kuruluş edge cihazınız için denetleyin:

* Internet ile SSH trafiği engelleyen bir iç güvenlik duvarı
* SSH bağlantıları engelleyen bir proxy sunucusu
* Yetkisiz giriş algılama ya da ağ izleme uç ağınızdaki SSH bağlantıları engelliyor cihazlarda çalışan yazılım

Internet ile SSH trafiğine izin vermek için kuruluş uç cihazlarınıza ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3\. kaynak: Bulut Hizmeti uç noktası ve ACL
> [!NOTE]
> Bu kaynak, yalnızca klasik dağıtım modeli kullanılarak oluşturulmuş olan VM'ler için geçerlidir. Resource Manager kullanılarak oluşturulan VM'ler için atlamak [kaynak 4: Ağ güvenlik grupları](#nsg).

ACL ve bulut Hizmeti uç noktası hatası kaynağı olarak ortadan kaldırmak için aynı sanal ağdaki başka bir Azure VM'ye SSH kullanarak bağlanabildiğinizi doğrulayın.

![Bulut Hizmeti uç noktası ve ACL vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Aynı sanal ağdaki başka bir VM'ye sahip değilseniz, bir kolayca oluşturabilirsiniz. Daha fazla bilgi için [CLI kullanarak Azure'da bir Linux VM oluşturma](../linux/quick-create-cli.md). Ek sanal makine, test ile işiniz bittiğinde silin.

Aynı sanal ağdaki bir VM ile bir SSH bağlantısı oluşturun, aşağıdaki alanları kontrol edin:

* **Hedef VM üzerinde giden SSH trafiği uç nokta yapılandırması.** SSH hizmeti VM üzerinde dinlediği TCP bağlantı noktası uç noktasının özel TCP bağlantı noktası eşleşmesi gerekir. (Varsayılan bağlantı noktası 22'dir). Azure portalında SSH TCP bağlantı noktası numarasını seçerek doğrulayın **sanal makineler** > *VM adı* > **ayarları**  >   **Uç noktaları**.
* **Hedef sanal makineye SSH trafiği uç noktası için ACL.** Bir ACL, kaynak IP adresine göre Internet'ten gelen trafiğe izin verileceğini veya belirtmenize olanak sağlar. Yanlış ACL'ler uç noktaya gelen SSH trafiğine engel olabilir. Bu genel IP adreslerini Ara sunucunuzun'ten gelen trafiği emin olmak için ACL'ler denetleyin veya başka bir uç sunucusu izin verilir. Daha fazla bilgi için [ağ erişim denetim listeleri (ACL'ler)](../../virtual-network/virtual-networks-acl.md).

Sorun kaynağı olarak uç nokta ortadan kaldırmak için geçerli uç noktasını kaldırın, başka bir uç noktası oluşturma ve SSH adı (TCP bağlantı noktası 22 genel ve özel bağlantı noktası numarası için) belirtin. Daha fazla bilgi için [Azure sanal makinesinde uç noktaları ayarlama](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>4\. kaynak: Ağ güvenlik grupları
Ağ güvenlik grupları izin verilen gelen ve giden trafik daha ayrıntılı denetim sağlar. Alt ağlar kapsayan ve bulut Hizmetleri, bir Azure sanal ağında kuralları oluşturabilirsiniz. Internet'ten SSH trafiğine izin verildiğinden emin olun, ağ güvenlik grubu kurallarını denetleyin.
Daha fazla bilgi için [ağ güvenlik grupları hakkında](../../virtual-network/security-overview.md).

NSG yapılandırmasını doğrulamak için IP doğrulamak da kullanabilirsiniz. Daha fazla bilgi için [Azure ağ izlemeye genel bakış](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>5\. kaynak: Linux tabanlı Azure sanal makinesi
Son olası sorunlar Azure sanal makine kendini kaynağıdır.

![Linux tabanlı Azure sanal makine vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Henüz yapmadıysanız, yönergeleri [bir parola Linux tabanlı sanal makineleri sıfırlamayı](../linux/reset-password.md).

Bilgisayarınızdan tekrar bağlanmayı deneyin. Yine başarısız olursa olası sorunlardan bazıları şunlardır:

* SSH hizmeti hedef sanal makine üzerinde çalışmıyor.
* SSH hizmeti TCP bağlantı noktası 22 dinleme yapmıyor. Test, telnet istemcisi yerel bilgisayarınıza yüklemek ve çalıştırmak için "telnet *cloudServiceName*. cloudapp.net 22". Bu adım, sanal makinenin SSH uç noktası için gelen ve giden iletişimi izin belirler.
* Hedef sanal makinede yerel bir güvenlik duvarı gelen veya giden SSH trafiği engelleyen kuralları vardır.
* Yetkisiz giriş algılama veya Azure sanal makinesinde çalışan yazılım izleme ağ SSH bağlantıları engelliyor.

## <a name="additional-resources"></a>Ek kaynaklar
Uygulama erişimi sorunlarını giderme hakkında daha fazla bilgi için bkz. [bir Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md)
