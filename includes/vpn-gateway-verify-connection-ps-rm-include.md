---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 508685954e23a357fa5fc48b0e89c5e7daedb5c6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765941"
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