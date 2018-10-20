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
ms.openlocfilehash: 84bfec375878fe31e085f7de3cd4a41ace645c41
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458657"
---
Bağlantınızın başarılı olduğunu, 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet’ini '-Debug' ile veya '-Debug' olmadan kullanarak doğrulayabilirsiniz. 

1. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde "Tümünü" çalıştırmak için "A" seçeneğini belirleyin. Örnekte bulunan "-Name", test etmek istediğiniz bağlantının adıdır.

  ```azurepowershell-interactive
  Get-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
  ```
2. cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu "Bağlandı" olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```