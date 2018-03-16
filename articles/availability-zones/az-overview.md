---
title: "Kullanılabilirlik bölgeleri genel bakış | Microsoft Docs"
description: "Bu makalede Azure kullanılabilirlik bölgelerinde genel bir bakış sağlar."
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 9274cb1fc490ab61e962e8a7918a9c44976dc755
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Kullanılabilirlik bölgeleri Azure (Önizleme) genel bakış

Veri merkezi düzeyi arızasına karşı korumak için kullanılabilirlik bölgeleri yardımcı. Bir Azure bölgesi içinde bulunur ve her biri kendi bağımsız sahip güç kaynağı, ağ ve soğutma. Dayanıklılık sağlamak için en az üç ayrı bölgelere etkinleştirilmiş tüm bölgelerde yoktur. Kullanılabilirlik bölgeleri fiziksel ve mantıksal ayırma bir bölge içinde uygulamaları ve verileri bölge düzeyinde hatalarından korur. 

![bir bölgenin bir bölgede giderek kavramsal görünümü](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri destekler bölgeleri

- Doğu ABD 2
- ABD Orta
- Batı Avrupa
- Fransa Orta

## <a name="services-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri Destek Hizmetleri

Kullanılabilirlik bölgeleri destekler Azure hizmetler şunlardır:

- Linux Sanal Makineleri
- Windows Sanal Makineleri
- Sanal Makine Ölçek Kümeleri
- Yönetilen Diskler
- Load Balancer
- Genel IP adresi
- Bölge olarak yedekli depolama
- SQL Database

## <a name="get-started-with-the-availability-zones-preview"></a>Kullanılabilirlik bölgeleri Önizleme kullanmaya başlama

Kullanılabilirlik bölgeleri Önizleme, Doğu ABD 2, BİZE Merkezi, Batı Avrupa ve belirli Azure hizmetlerinin Fransa merkezi bölgelerde kullanılabilir. 

1. [Kullanılabilirlik bölgeleri önizlemek için kaydolun](http://aka.ms/azenroll). 
2. Azure aboneliğinizde oturum açın.
3. Kullanılabilirlik bölgeyi destekleyen bir bölge seçin.
4. Kullanılabilirlik bölgeleri hizmetiniz ile kullanmaya başlamak için aşağıdaki bağlantılardan birini kullanın. 
    - [Bir sanal makine oluşturun](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
    - [PowerShell kullanarak yönetilen Disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Yük Dengeleyici](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç şablonları](http://aka.ms/azqs)
