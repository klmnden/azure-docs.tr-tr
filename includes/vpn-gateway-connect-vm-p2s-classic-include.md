---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 12/06/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 6d0737a7300b2a6025f776c1ed65a05cacf2141a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60845634"
---
Sanal ağınıza dağıtılmış VM'ye bağlanmak için Uzak Masaüstü bağlantısı oluşturun. Bilgisayar adı yerine özel IP adresine sanal makinenizde bağlantı kurabildiğimizi doğrulamak için en iyi yolu bağlanmaktır. Bu şekilde, bağlantı kurup, ad çözümlemesi olup düzgün yapılandırılmadığından görmek için test ettiğiniz. 

1. Sanal makinenizin özel IP adresini bulun. Bir VM'nin özel IP adresini bulmak için Azure portalında VM'nin özelliklerini görüntüleyin veya PowerShell'i kullanabilirsiniz.
2. Noktadan siteye VPN bağlantısı ile sanal ağınıza bağlı olduğunuzu doğrulayın. 
3. Uzak Masaüstü Bağlantısı'nı açmak için girin *RDP* veya *Uzak Masaüstü Bağlantısı* görev çubuğundaki arama kutusunu seçip **Uzak Masaüstü Bağlantısı**. Ayrıca kullanarak açabilirsiniz **mstsc** PowerShell komutu. 
3. İçinde **Uzak Masaüstü Bağlantısı**, VM'nin özel IP adresini girin. Gerekirse, seçin **Seçenekleri Göster** ek ayarları ayarlamak için ardından bağlanın.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>VM ile RDP bağlantısı sorunlarını giderme

VPN bağlantınız üzerinden bir sanal makineye bağlanmakta sorun yaşıyorsanız göz atabilirsiniz birkaç şey vardır. 

- VPN bağlantınızın başarılı olduğunu doğrulayın.
- VM için özel IP adresine bağlandığınızı doğrulayın.
- Girin **ipconfig** kurduğunuz bilgisayardaki Ethernet bağdaştırıcısına atanmış IPv4 adresini denetleyin. IP adresi, bağlanacağınız sanal ağın adres aralığında veya vpnclientaddresspool adres aralığında bir çakışan adres alanı gerçekleşir. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır.
- Özel IP adresini, ancak bilgisayar adı'nı kullanarak VM ile bağlantı kurabiliyorsanız, DNS'i düzgün yapılandırdığınızdan doğrulayın. VM’ler için ad çözümlemesinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [VM'ler için Ad Çözümlemesi](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- Sanal ağ için DNS sunucusu IP adresleri belirtildikten sonra VPN istemcisi yapılandırma paketini oluşturulduğunu doğrulayın. DNS sunucusu IP adreslerinin güncelleştirirseniz, oluşturmak ve yeni bir VPN istemcisi yapılandırma paketini yükleyin.

Daha fazla sorun giderme bilgisi için bkz. [VM ile Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).