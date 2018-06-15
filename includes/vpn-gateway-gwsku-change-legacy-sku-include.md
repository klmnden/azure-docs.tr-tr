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
ms.openlocfilehash: 2c1a4a1931bc2e38b0bee5f90518b01fdf4767a1
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "30196825"
---
Resource Manager dağıtım modeliyle çalışıyorsanız, yeni ağ geçidi SKU'ları değiştirebilirsiniz. Yeni bir SKU bir eski ağ geçidinden SKU değiştirdiğinizde, var olan VPN ağ geçidini silin ve yeni bir VPN ağ geçidi oluşturun.

İş akışı:

1. Sanal ağ geçidine ilişkin tüm bağlantıları kesin.
2. Eski VPN ağ geçidini silin.
3. Yeni VPN ağ geçidini oluşturun.
4. Şirket içi VPN cihazlarınızdaki VPN ağ geçidi IP adresini (Siteden Siteye bağlantılar için) yenisi ile güncelleştirin.
5. Bu ağ geçidine bağlanacak tüm VNet-VNet yerel ağ geçitleri için ağ geçidi IP adresi değerini güncelleştirin.
6. Bu VPN ağ geçidi yoluyla sanal ağa bağlanan P2S istemcileri için yeni istemci VPN yapılandırma paketlerini indirin.
7. Sanal ağ geçidine ilişkin tüm bağlantıları yeniden oluşturun.

Dikkate alınacak noktalar:

* Yeni SKU'ları taşımak için VPN ağ geçidinizi Resource Manager dağıtım modelinde olması gerekir.
* Klasik bir VPN ağ geçidi varsa, bu ağ geçidi için eski eski SKU'ları kullanmaya devam gerekir, ancak, eski SKU'ları arasında yeniden boyutlandırabilirsiniz. Yeni SKU'ları değiştiremezsiniz.
* Yeni bir SKU eski bir SKU değiştirdiğinizde bağlantı kapalı kalma süresi gerekir.
* Yeni bir ağ geçidi için SKU değiştirirken, VPN ağ geçidinizin genel IP adresini değiştirir. Daha önce kullandığınız aynı ortak IP adresi nesnesinin belirttiğiniz olsa bile bu gerçekleşir.