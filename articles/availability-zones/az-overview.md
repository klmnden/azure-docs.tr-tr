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
ms.service: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: ac89562a5156282bb0fe8a6aa6761d6acb97f785
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Kullanılabilirlik bölgeleri Azure (Önizleme) genel bakış

Veri merkezi düzeyi arızasına karşı korumak için kullanılabilirlik bölgeleri yardımcı. Bir Azure bölgesi içinde bulunur ve her biri kendi bağımsız sahip güç kaynağı, ağ ve soğutma. Dayanıklılık sağlamak için en az üç ayrı bölgelere etkinleştirilmiş tüm bölgelerde yoktur. Kullanılabilirlik bölgeleri fiziksel ve mantıksal ayırma bir bölge içinde uygulamaları ve verileri bölge düzeyinde hatalarından korur. 

![bir bölgenin bir bölgede giderek kavramsal görünümü](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri destekler bölgeleri

- Doğu ABD 2
- Batı Avrupa
- Fransa Orta

## <a name="services-that-support-availability-zones"></a>Kullanılabilirlik bölgeleri Destek Hizmetleri

Kullanılabilirlik bölgeleri destekler Azure hizmetler şunlardır:

- Linux Sanal Makineleri
- Windows Sanal Makineleri
- Zonal sanal makine ölçekleme kümeleri
- Yönetilen Diskler
- Load Balancer
- Genel IP adresi

## <a name="supported-virtual-machine-size-families"></a>Desteklenen sanal makine boyutu aileleri

- Av2
- Dv2
- DSv2


## <a name="get-started-with-the-availability-zones-preview"></a>Kullanılabilirlik bölgeleri Önizleme kullanmaya başlama

Kullanılabilirlik bölgeleri Önizleme, Doğu ABD 2, Batı Avrupa ve belirli Azure hizmetlerinin Fransa merkezi bölgelerde kullanılabilir. 

1. [Kullanılabilirlik bölgeleri önizlemek için kaydolun](http://aka.ms/azenroll). 
2. Azure aboneliğinizde oturum açın.
3. Kullanılabilirlik bölgeyi destekleyen bir bölge seçin.
4. Kullanılabilirlik bölgeleri hizmetiniz ile kullanmaya başlamak için aşağıdaki bağlantılardan birini kullanın. 
    - [Bir sanal makine oluşturun](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Zonal sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md)
    - [PowerShell kullanarak yönetilen Disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Yük Dengeleyici](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç şablonları](http://aka.ms/azqs)
