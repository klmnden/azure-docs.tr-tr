---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a8dec00373d991a7aeaf11f1a4d95463ab71d9a2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
1. [Portal](http://portal.azure.com)’da, sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.
2. VNet’inizin sayfasının **Ayarlar** bölümünde, **Alt Ağlar** sayfasını genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** sayfasında, **+Alt ağ**’a tıklayarak **Alt ağ ekle** sayfasını açın. 

  ![Ağ geçidi alt ağını ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Ağ geçidi alt ağını ekleme")
4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulan **Adres aralığı** değerlerini yapılandırma gereksinimlerinizle eşleşecek şekilde değiştirin ve alt ağı oluşturmak için sayfanın altındaki **Tamam** seçeneğine tıklayın.

  ![Alt ağı ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Alt ağı ekleme")