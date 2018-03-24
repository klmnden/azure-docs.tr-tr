---
title: Sanal ağ için Azure CLI örnekleri | Microsoft Docs
description: Sanal ağ için Azure CLI örneklerini içerir.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 9459bad1060daa0c9f6c34272cecfbc67463be66
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>Sanal ağ için Azure CLI örnekleri

Aşağıdaki tabloda Azure CLI komutları kodlarla bash için bağlantıları içerir:

| | |
|----|----|
| [Çok katmanlı uygulamalar için bir sanal ağ oluşturma](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Bir sanal ağ ile ön uç ve arka uç alt ağlar oluşturur. Arka uç alt ağ trafiği MySQL, bağlantı noktası 3306 sınırlı olsa ön uç alt ağ trafiği HTTP ve SSH, ile sınırlıdır. |
| [Eş iki sanal ağlar](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Oluşturur ve iki sanal ağ aynı bölgede bağlanır. |
| [Bir ağ sanal gereç yoluyla trafiği yönlendirme](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Ön uç ve arka uç alt ağları ve iki alt ağlar arasında trafiği yönlendirmek için bir VM sanal ağ oluşturur. |
| [Gelen ve giden VM ağ trafiği filtreleme](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Bir sanal ağ ile ön uç ve arka uç alt ağlar oluşturur. Ön uç alt ağa gelen ağ trafiğini, HTTP, HTTPS ve SSH ile sınırlıdır. Arka uç alt ağından internet giden trafiğe izin verilmez. |