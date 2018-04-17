---
title: include dosyası
description: include dosyası
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 10f723d5298e745520c4db41b994bd2b97aaa365
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
Sistem yollarının kullanımı dağıtımınız için trafiği otomatik olarak kolaylaştırsa da, bir sanal gereç yoluyla paketlerin yönlendirilmesini denetlemek isteyeceğiniz durumlar vardır. Bunun için paketlerin belirli bir alt ağa akmak yerine bir sonraki atlamada sanal gerecinize gitmelerini belirten ve sanal gereç olarak çalışan VM için IP iletimini etkinleştiren kullanıcı tanımlı yollar oluşturabilirsiniz.

Sanal gereçler kullanıldığı durumlarda bazıları şunlardır:

* Yetkisiz erişim algılama sistemiyle (Kimlikler) trafiğini izleme
* Bir güvenlik duvarı ile trafiği denetleme

UDR ve IP iletme hakkında daha fazla bilgi için ziyaret [kullanıcı tanımlı yollar ve IP iletimi](../articles/virtual-network/virtual-networks-udr-overview.md).

