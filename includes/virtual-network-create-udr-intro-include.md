---
title: include dosyası
description: include dosyası
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743402"
---
Sistem yollarının kullanımı dağıtımınız için trafiği otomatik olarak kolaylaştırsa da, bir sanal gereç yoluyla paketlerin yönlendirilmesini denetlemek isteyeceğiniz durumlar vardır. Bunun için paketlerin belirli bir alt ağa akmak yerine bir sonraki atlamada sanal gerecinize gitmelerini belirten ve sanal gereç olarak çalışan VM için IP iletimini etkinleştiren kullanıcı tanımlı yollar oluşturabilirsiniz.

Sanal gereçler kullanıldığı durumlarda bazıları şunlardır:

* İzleme trafiği bir yetkisiz erişim algılama sistemi (Kimlikler)
* Bir güvenlik duvarı ile trafiği denetleme

UDR ve IP iletme hakkında daha fazla bilgi için ziyaret [kullanıcı tanımlı yollar ve IP iletimi](../articles/virtual-network/virtual-networks-udr-overview.md).