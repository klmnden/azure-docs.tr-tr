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
ms.openlocfilehash: 0334f9fd2d749b88580ff3857d705de2ae961902
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60407797"
---
Sanal ağ geçidi, ağ geçidi alt ağı adlı belirli bir alt ağ kullanır. Ağ geçidi alt ağı, sanal ağınızı yapılandırırken belirttiğiniz sanal ağ IP adresi aralığının bir parçasıdır. Sanal ağ geçidi kaynaklarının ve hizmetlerinin kullandığı IP adreslerini içerir. Azure’ın ağ geçidi kaynaklarını dağıtması için alt ağ, 'GatewaySubnet' olarak adlandırılmalıdır. Ağ geçidi kaynaklarının dağıtılacağı farklı bir alt ağ belirtemezsiniz. 'GatewaySubnet' adlı bir alt ağınız yoksa, kendi VPN ağ geçidinizi oluştururken bu başarısız olur.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Gerekli IP adresi sayısı, oluşturmak istediğiniz VPN ağ geçidi yapılandırmasına bağlıdır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. /27 veya /28 kullanan bir ağ geçidi alt ağı oluşturmanızı öneririz.

Adres alanının bir alt ağ ile çakıştığını veya alt ağın sanal ağınız için adres alanında bulunmadığını belirten bir hata görürseniz VNet adres aralığınızı denetleyin. Sanal ağınız için oluşturduğunuz adres aralığında yeterli IP adresi kullanılabilir olmayabilir. Örneğin, varsayılan alt ağınız tüm adres aralığını kapsıyorsa, ek alt ağlar oluşturmak için bir IP adresi kalmamıştır. IP adreslerini serbest bırakmak için mevcut adres alanında alt ağlarınızı ayarlayabilir veya ek bir adres aralığı belirtip orada ağ geçidi alt ağını oluşturabilirsiniz.