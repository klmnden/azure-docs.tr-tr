---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc1e2803a420b95e1abec0886733c5e42a8cfdb4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026651"
---
1. [Portal](http://portal.azure.com)’da, sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.
2. VNet’inizin sayfasının **Ayarlar** bölümünde, **Alt Ağlar** sayfasını genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** sayfasında, **+Alt ağ**’a tıklayarak **Alt ağ ekle** sayfasını açın. 

  ![Ağ geçidi alt ağını ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Ağ geçidi alt ağını ekleme")
4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulmuş **Adres aralığı** değerlerini, yapılandırma gereksinimlerinize uyacak şekilde ayarlayın. Yol tablosu ya da hizmet uç noktaları yapılandırmayın.

  ![Alt ağı ekleme](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Alt ağı ekleme")
5.  Tıklayın **Tamam** alt ağ oluşturmak için sayfanın alt kısmındaki.