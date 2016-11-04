---
title: Windows Server VM’ye bağlanma | Microsoft Docs
description: Azure portal ve Resource Manager dağıtım yöneticisini kullanarak bir Windows VM’ye nasıl bağlanacağınızı ve oturum açacağınızı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2016
ms.author: cynthn

---
# Windows çalıştıran bir Azure Virtual Machine’e bağlanma ve oturum açma
Uzak Masaüstü (RDP) oturumu başlatmak için Azure portalda **Bağlan**düğmesini kullanacaksınız. Önce sanal makineye bağlandıktan sonra oturum açın.

## Sanal makineye bağlanma
1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2. Hub menüsünde, **Virtual Machines**’e tıklayın.
3. Listeden sanal makineyi seçin.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
   
    ![VM'nize nasıl bağlanacağınızı gösteren Azure portal ekran görüntüsü.](./media/virtual-machines-windows-connect-logon/connect.png)
   
   > [!TIP]
   > Portal'daki **Bağlan** düğmesi gri ise ve Azure’a bir [Express Route](../expressroute/expressroute-introduction.md) veya [Siteden Siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) aracılığıyla bağlı değilseniz RDP kullanmadan önce IP adresi oluşturmalı ve VM’nize atamalısınız. [Azure’da genel IP adresleri](../virtual-network/virtual-network-ip-addresses-overview-arm.md) hakkında daha fazlasını okuyabilirsiniz.
   > 
   > 

## Sanal makinede oturum açma
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## Sonraki adımlar
Bağlanmayı denediğinizde sorun yaşıyorsanız, bkz. [Uzak Masaüstü bağlantı sorunlarını giderme](virtual-machines-windows-troubleshoot-rdp-connection.md). Bu makale ortak sorunları tanılama ve çözme boyunca size yol gösterecektir.

<!--HONumber=Sep16_HO3-->


