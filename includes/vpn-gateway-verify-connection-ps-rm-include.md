---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 49f48c2d0bf865cf8c8fde831e7a597f8701d213
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66150014"
---
İle veya olmadan 'Get-AzVirtualNetworkGatewayConnection' cmdlet'ini kullanarak bağlantınızın başarılı olduğunu doğrulayabilirsiniz. '-Debug'. 

1. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde "Tümünü" çalıştırmak için "A" seçeneğini belirleyin. Örnekte bulunan "-Name", test etmek istediğiniz bağlantının adıdır.

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu "Bağlandı" olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.
   
   ```
   "connectionStatus": "Connected",
   "ingressBytesTransferred": 33509044,
   "egressBytesTransferred": 4142431
   ```