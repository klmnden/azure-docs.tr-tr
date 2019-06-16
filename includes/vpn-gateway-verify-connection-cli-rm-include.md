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
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121180"
---
[az network vpn-connection show](/cli/azure/network/vpn-connection) komutunu kullanarak bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. Örnekte bulunan "-name", test etmek istediğiniz bağlantının adıdır. Bağlantı henüz kurulma aşamasındayken, bağlantı durumu "Bağlanıyor" olarak gösterilir. Bağlantı kurulduktan sonra durum "Bağlandı" olarak değişir.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
