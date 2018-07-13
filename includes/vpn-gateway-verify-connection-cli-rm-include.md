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
ms.openlocfilehash: 1c3103889bd11df45710cc0d6afaeeba022c9ace
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38763014"
---
[az network vpn-connection show](/cli/azure/network/vpn-connection#show) komutunu kullanarak bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. Örnekte bulunan "-name", test etmek istediğiniz bağlantının adıdır. Bağlantı henüz kurulma aşamasındayken, bağlantı durumu "Bağlanıyor" olarak gösterilir. Bağlantı kurulduktan sonra durum "Bağlandı" olarak değişir.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```