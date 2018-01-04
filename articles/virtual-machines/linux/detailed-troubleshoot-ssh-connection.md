---
title: "Bir Azure VM için ayrıntılı SSH giderme | Microsoft Docs"
description: "Bir Azure sanal makinesine bağlanma sorunları için sorun giderme adımları SSH ayrıntılı"
keywords: "SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: 82b2bcf5b05288888714339af15ff2796d9660bd
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-to-a-linux-vm-in-azure"></a>Ayrıntılı SSH azure'da bir Linux VM için bağlantı sorunları için sorun giderme adımları
SSH istemcisi VM SSH hizmette ulaşabilmesi olmayabilir birçok olası nedeni vardır. Daha fazla bilgi izlediyseniz [sorun giderme adımları genel SSH](troubleshoot-ssh-connection.md), daha fazla bağlantı sorunu gidermek gerekir. Bu makalede, SSH bağlantısını nerede başarısız olduğunu ve nasıl çözümleyeceğiniz belirlemek için ayrıntılı sorun giderme adımlarını size yol gösterir.

## <a name="take-preliminary-steps"></a>Ön adımlar
Aşağıdaki diyagramda oynayan bileşenlerini gösterir.

![SSH Hizmeti bileşenlerinin gösteren diyagram](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Aşağıdaki adımlar hatanın kaynak yalıtmak ve çözümleri veya geçici çözümler dağıtamadığını bulmak yardımcı olur.

1. Portalı'nda VM durumunu denetleyin.
   İçinde [Azure portal](https://portal.azure.com)seçin **sanal makineleri** > *VM adı*.

   VM için durum bölmesi göstermelidir **çalıştıran**. İşlem, depolama ve ağ kaynakları için son etkinliğin Göster gidin.

2. Seçin **ayarları** uç noktaları, IP adresleri, ağ güvenlik grupları ve diğer ayarları incelemek için.

   VM içinde görüntüleyebilirsiniz SSH trafiği için tanımlanmış bir uç nokta olmalıdır **uç noktaları** veya  **[ağ güvenlik grubu](../../virtual-network/virtual-networks-nsg.md)**. Resource Manager kullanılarak oluşturulan sanal makineleri uç noktalarını bir ağ güvenlik grubundaki depolanır. Kural ağ güvenlik grubuna uygulanan ve alt ağ içindeki başvurulan doğrulayın.

Ağ bağlantısını doğrulamak için yapılandırılmış uç denetleyin ve HTTP veya başka bir hizmeti gibi başka bir protokol üzerinden VM bağlanabildiğinizi bakın.

Bu adımlar, SSH bağlantısını yeniden deneyin.

## <a name="find-the-source-of-the-issue"></a>Sorunun kaynağını Bul
SSH istemcisi bilgisayarınızda Azure VM'de sorunları veya aşağıdaki alanlarda yapılandırma hataları nedeniyle SSH hizmetine bağlanmak başarısız olabilir:

* [SSH istemci bilgisayar](#source-1-ssh-client-computer)
* [Kuruluş sınır cihazı](#source-2-organization-edge-device)
* [Bulut Hizmeti uç noktası ve erişim denetimi listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Linux tabanlı Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>1. kaynak: SSH istemci bilgisayar
Bilgisayarınızı hata kaynağı olarak ortadan kaldırmak için başka bir şirket içi, Linux tabanlı bir bilgisayarda SSH bağlantısını yapabilirsiniz doğrulayın.

![SSH istemci bilgisayar bileşenleri vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Bağlantı başarısız olursa, bilgisayarınızda aşağıdaki sorunları denetle:

* Gelen veya giden SSH trafiği (TCP 22) engelleyen bir yerel güvenlik duvarı ayarı
* Yerel olarak SSH bağlantısını engelliyor istemci ara yazılımı yüklü
* Yerel olarak yüklü ağ SSH bağlantısını engelliyor yazılım izleme
* Trafiği izlemek ya da belirli trafik türlerine izin ver/engellemek diğer güvenlik yazılım türleri

Bu koşullardan biri geçerliyse, geçici olarak yazılımınızı devre dışı bırakın ve bağlantı bilgisayarınızda engellenme nedenini bulmak için bir şirket içi bilgisayarda bir SSH bağlantısı deneyin. Ardından SSH bağlantılara izin vermek için yazılım ayarları düzeltmek için ağ yöneticinizle birlikte çalışın.

Sertifika kimlik doğrulaması kullanıyorsanız, giriş dizininizde .ssh klasörü bu izinlere sahip olduğunuzu doğrulayın:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (ya da bunlarda depolanan özel anahtarlarınızı sahip diğer dosyalar)
* Chmod 644 ~/.ssh/known_hosts (SSH yoluyla için bağlantı kurduğunuz konakları içerir)

## <a name="source-2-organization-edge-device"></a>2. kaynak: Kuruluş sınır cihazı
Hatanın kaynak olarak kuruluş sınır cihazı ortadan kaldırmak için doğrudan Internet'e bağlı bir bilgisayar için Azure VM'yi SSH bağlantıları yapabilir doğrulayın. VM bir siteden siteye VPN veya Azure ExpressRoute bağlantısı erişiyorsa geçin [kaynak 4: ağ güvenlik grupları](#nsg).

![Kuruluş sınır cihazı vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Doğrudan olan bir bilgisayarda yoksa, Internet'e bağlı yeni bir Azure VM kendi kaynak grubunda veya Bulut hizmeti oluşturup bu yeni VM kullanabilirsiniz. Daha fazla bilgi için bkz: [Linux Azure üzerinde çalışan bir sanal makine oluşturma](quick-create-cli.md). Test ile tamamladığınızda kaynak grubu veya VM ve bulut hizmetini silin.

Internet'e doğrudan bağlı bir bilgisayar ile bir SSH bağlantısı oluşturup oluşturamayacağını kuruluş sınır cihazınız için denetleyin:

* Internet ile SSH trafiği engelleyen bir iç güvenlik duvarı
* SSH bağlantısını engelleyen bir proxy sunucu
* Yetkisiz erişim algılama veya ağ kenar ağınızdaki SSH bağlantısını engelliyor cihazlarda çalışan yazılım izleme

Internet ile SSH trafiğine izin vermek için kuruluş sınır cihazları ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3. kaynak: Bulut Hizmeti uç noktası ve ACL
> [!NOTE]
> Bu kaynağı Klasik dağıtım modeli kullanılarak oluşturulan sanal makineleri için geçerlidir. Resource Manager kullanılarak oluşturulan VM'ler için geçin [kaynak 4: ağ güvenlik grupları](#nsg).

Bulut Hizmeti uç noktası ve ACL hatası kaynağı olarak ortadan kaldırmak için aynı sanal ağdaki başka bir Azure VM SSH kullanarak bağlanabildiğinizi doğrulayın.

![Bulut Hizmeti uç noktası ve ACL vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Aynı sanal ağdaki başka bir VM sahip değilseniz, birini kolayca oluşturabilirsiniz. Daha fazla bilgi için bkz: [CLI kullanarak Azure'da bir Linux VM oluşturma](quick-create-cli.md). Ek VM, test ile bittiğinde silin.

Aynı sanal ağda bir VM ile bir SSH bağlantısı oluşturup oluşturamayacağını aşağıdaki alanları kontrol edin:

* **Hedef VM SSH trafiği için uç nokta yapılandırması.** Uç noktasının özel TCP bağlantı noktası VM SSH hizmette dinlediği TCP bağlantı noktası eşleşmesi gerekir. (Varsayılan bağlantı noktası 22'dir). Azure portalında SSH TCP bağlantı noktası numarası seçerek doğrulayın **sanal makineleri** > *VM adı* > **ayarları** > **uç noktaları**.
* **Hedef sanal makinedeki SSH trafiği uç noktası için ACL.** Bir ACL belirtmek izin verilen veya kaynak IP adresine göre Internet'ten gelen trafiği reddedilen sağlar. Yanlış yapılandırılmış ACL'ler uç noktasına gelen SSH trafiğini engelleyebilirsiniz. Bu gelen trafiğin proxy ortak IP adreslerinden emin olmak için ACL denetleyin veya başka bir uç sunucusu izin verilir. Daha fazla bilgi için bkz: [ağ erişimi denetim listeleri (ACL'ler)](../../virtual-network/virtual-networks-acl.md).

Uç nokta sorun kaynağı olarak ortadan kaldırmak için geçerli son nokta kaldırın, başka bir uç noktası oluşturma ve (TCP bağlantı noktası 22 ortak ve özel bağlantı noktası numarası) SSH adı belirtin. Daha fazla bilgi için bkz: [azure'da bir sanal makine üzerindeki uç noktaları ayarlama](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>4. kaynak: Ağ güvenlik grupları
Ağ güvenlik grupları, izin verilen gelen ve giden trafik daha ayrıntılı denetim sağlar. Alt ağlar span ve bulut hizmetlerini bir Azure sanal ağında kurallar oluşturabilirsiniz. Internet'ten SSH trafiğe izin verildiğinden emin olmak için ağ güvenlik grubu kurallarınızı denetleyin.
Daha fazla bilgi için bkz: [ağ güvenlik grupları hakkında](../../virtual-network/virtual-networks-nsg.md).

NSG yapılandırmasını doğrulamak için IP doğrulayın de kullanabilirsiniz. Daha fazla bilgi için bkz: [izlemeye genel bakış Azure ağ](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>Kaynak 5: Linux tabanlı Azure sanal makine
Son olası sorunlar Azure sanal makinesini kendisini kaynağıdır.

![Linux tabanlı Azure sanal makine vurgular diyagramı](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Henüz yapmadıysanız, yönergeleri izleyin [Linux tabanlı sanal makineler için bir parola veya SSH sıfırlamak için](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Bilgisayarınızı yeniden bağlanmayı deneyin. Yine başarısız olursa, olası sorunlardan bazıları şunlardır:

* Hedef sanal makineye SSH hizmeti çalışmıyor.
* SSH hizmeti TCP bağlantı noktası 22 dinlemiyor. Test, telnet istemcisi yerel bilgisayarınıza yüklemek ve çalıştırmak için "telnet *cloudServiceName*. cloudapp.net 22". Bu adım, sanal makinenin SSH uç noktasına gelen ve giden iletişim kurmasına olanak tanıyan varsa belirler.
* Hedef sanal makinedeki yerel güvenlik duvarı gelen veya giden SSH trafiğini engelliyor kuralları vardır.
* Yetkisiz erişim algılama veya ağ Azure sanal makinede çalışan yazılımı izleme SSH bağlantısını engelliyor.

## <a name="additional-resources"></a>Ek kaynaklar
Uygulama erişimi sorunlarını giderme hakkında daha fazla bilgi için bkz: [bir Azure sanal makine üzerinde çalışan bir uygulamaya erişim sorunlarını giderme](troubleshoot-app-connection.md)
