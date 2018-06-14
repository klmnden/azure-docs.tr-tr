---
title: Azure VM'ler için mevcut bir kullanılabilirlik ekleme desteklenebilirlik ayarlama | Microsoft Docs
description: Azure VM'ler var olan bir kullanılabilirlik kümesine ekleme desteklenebilirlik.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: delhan
ms.openlocfilehash: 8bf2a55563772e26239445732b2b08df677436ef
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
ms.locfileid: "23987729"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Azure VM'ler var olan bir kullanılabilirlik kümesine ekleme desteklenebilirlik

Yeni sanal makineler (VM'ler) eklediğiniz zaman sınırlamaları var olan bir kullanılabilirlik kümesine bazen karşılaşabilirsiniz. Aşağıdaki grafikte aynı kullanılabilirlik kümesinde karıştırabilirsiniz hangi VM dizisi ayrıntılarını verir.

VM'ler farklı türlerini karma olarak desteklenebilirlik matris şöyledir:

Seri & kullanılabilirlik kümesi|İkinci VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|İlk VM|||||||
|A||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Av2||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|D||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Dv2||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Dv3||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|

Diğer tüm serisi aynı kullanılabilirlik belirli donanım gerektirdiğinden kümesi içinde bulunamadı.

A8/A9 VM boyutu ayrılmış RDMA arka uç ağ üzerindeki requirment nedeniyle karıştırılamaz.
