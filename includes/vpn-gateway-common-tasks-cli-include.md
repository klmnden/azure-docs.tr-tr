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
ms.openlocfilehash: 61082333afc88abef0a8d8a57d1f1b1d893b6148
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188229"
---
### <a name="to-view-local-network-gateways"></a>Yerel ağ geçitlerini görüntülemek için

Yerel ağ geçitlerinin bir listesini görüntülemek için [az network local gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway) komutunu kullanın.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a>Paylaşılan anahtar değerlerini doğrulamak için

Paylaşılan anahtar değerinin, VPN cihaz yapılandırmanızda kullandığınız değerle aynı olduğunu doğrulayın. Aynı değilse, cihazdan alınan değeri kullanarak bağlantıyı yeniden çalıştırın veya cihazı dönüş değeriyle güncelleştirin. Değerler eşleşmelidir. Paylaşılan anahtarı görüntülemek için [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection) komutunu kullanın.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a>VPN ağ geçidi Genel IP Adresini görüntülemek için

Sanal ağ geçidinizin genel IP adresini bulmak için [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın. Kolay okunması için, bu örneğin çıktısı genel IP’lerin tablo biçiminde gösterileceği şekilde biçimlendirilmiştir.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
