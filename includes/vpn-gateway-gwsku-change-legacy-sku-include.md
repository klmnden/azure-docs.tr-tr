---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c232e1ce183c6935d625b5bc9987a4981865ae4
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188251"
---
Resource Manager dağıtım modeliyle çalışıyorsanız, yeni ağ geçidi SKU'ları değiştirebilirsiniz. Eski ağ geçidi SKU'sunu yeni bir SKU'ya değiştirdiğinizde, mevcut VPN ağ geçidini silin ve yeni bir VPN ağ geçidi oluşturun.

İş akışı:

1. Sanal ağ geçidine ilişkin tüm bağlantıları kesin.
2. Eski VPN ağ geçidini silin.
3. Yeni VPN ağ geçidini oluşturun.
4. Şirket içi VPN cihazlarınızdaki VPN ağ geçidi IP adresini (Siteden Siteye bağlantılar için) yenisi ile güncelleştirin.
5. Bu ağ geçidine bağlanacak tüm VNet-VNet yerel ağ geçitleri için ağ geçidi IP adresi değerini güncelleştirin.
6. Bu VPN ağ geçidi yoluyla sanal ağa bağlanan P2S istemcileri için yeni istemci VPN yapılandırma paketlerini indirin.
7. Sanal ağ geçidine ilişkin tüm bağlantıları yeniden oluşturun.

Dikkat edilmesi gerekenler:

* Yeni SKU'lara taşımak için VPN gateway'inizi Resource Manager dağıtım modelinde olmalıdır.
* Klasik VPN ağ geçidi varsa, bu ağ geçidi için eski eski SKU'ları kullanarak devam etmeniz gerekir, ancak, eski SKU'ları arasında yeniden boyutlandırabilirsiniz. Yeni SKU'lara geçiş yapamazsınız.
* Eski bir SKU'dan yeni bir SKU'ya değiştirdiğinizde, bağlantı kapalı kalma süresi gerekir.
* Yeni bir ağ geçidi için SKU değiştirirken, VPN ağ geçidiniz için genel IP adresi değişir. Bu, daha önce kullandığınız aynı genel IP adresi nesnesi belirtseniz dahi gerçekleşir.