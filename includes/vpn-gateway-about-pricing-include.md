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
ms.openlocfilehash: ae36adc78d8c87d85c64fd61cb3a50dfcae60b85
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188250"
---
İki şey için ödeme yaparsınız: sanal ağ geçidi için saatlik işlem maliyetleri ve sanal ağ geçidinden çıkış veri aktarımı. Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

**Sanal ağ geçidi işlem maliyetleri**<br>Her sanal ağ geçidi saatlik bir işlem maliyetine sahiptir. Fiyat bir sanal ağ geçidi oluştururken belirttiğiniz ağ geçidi SKU’sunu temel alır. Maliyet, ağ geçidinin kendisi içindir ve ağ geçidinden geçen veri aktarımlarına eklenir. Etkin-Etkin kurulum maliyeti Aktif-Pasif ile aynıdır.

**Veri aktarım maliyetleri**<br>Veri aktarım maliyetleri, kaynak sanal ağ geçidinden çıkış trafiğine göre hesaplanır.

* Şirket içi VPN cihazınıza trafik gönderiyorsanız İnternet çıkış veri aktarım hızına göre ücretlendirilir.
* Farklı bölgelerdeki sanal ağlar arasında trafik gönderiyorsanız fiyatlandırma bölgeye göre hesaplanır.
* Yalnızca aynı bölgedeki sanal ağlar arasında trafik gönderiyorsanız veri maliyeti yoktur. Aynı bölgedeki sanal ağlar arasında gerçekleşen trafik ücretsizdir.