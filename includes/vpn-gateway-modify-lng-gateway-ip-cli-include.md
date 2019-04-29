---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: e368b1590f263618969423d57cdf0531fc2bb54d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60652885"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>Yerel ağ geçidini değiştirmek için 'gatewayIpAddress'

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Ağ geçidi IP adresi, mevcut bir VPN ağ geçidi bağlantısı (varsa) kaldırılmadan değiştirilebilir. Ağ geçidi IP adresini değiştirmek için 'Site2' ve 'TestRG1' değerlerini [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway) komutunu kullanarak istediğiniz değerlerle değiştirin.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Çıktıda IP adresinin doğru olduğundan emin olun:

```
"gatewayIpAddress": "23.99.222.170",
```
