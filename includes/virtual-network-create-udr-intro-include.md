---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170885"
---
Sistem yollarının kullanımı dağıtımınız için trafiği otomatik olarak kolaylaştırsa da, bir sanal gereç yoluyla paketlerin yönlendirilmesini denetlemek isteyeceğiniz durumlar vardır. Bunun için paketlerin belirli bir alt ağa akmak yerine bir sonraki atlamada sanal gerecinize gitmelerini belirten ve sanal gereç olarak çalışan VM için IP iletimini etkinleştiren kullanıcı tanımlı yollar oluşturabilirsiniz.

Sanal gereçler kullanıldığı durumlarda bazıları şunlardır:

* İzleme trafiği bir yetkisiz erişim algılama sistemi (Kimlikler)
* Bir güvenlik duvarı ile trafiği denetleme

UDR ve IP iletme hakkında daha fazla bilgi için ziyaret [kullanıcı tanımlı yollar ve IP iletimi](../articles/virtual-network/virtual-networks-udr-overview.md).

