---
title: SAP HANA kullanılabilirliğine Azure Vm'leri - genel bakış | Microsoft Docs
description: Yerel Azure sanal makineler'de SAP HANA işlemleri açıklar.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1db56ad31991b85ffad415818c7c67f0ee30808d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60708456"
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>Azure sanal makineler için SAP HANA yüksek kullanılabilirlik

Azure Vm'leri üzerinde SAP HANA gibi görev açısından kritik veritabanları dağıtmak için çok sayıda Azure özelliklerini kullanabilirsiniz. Bu makalede, Azure Vm'lerinde barındırılan SAP HANA örnekleri için kullanılabilirlik elde etmek nasıl hakkında yönergeler sağlanır. Bu makalede, uygulayabileceğiniz çeşitli senaryolar anlatılmaktadır azure'da SAP HANA kullanılabilirliği artırmak için Azure altyapısı kullanılarak. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, azure'da bir hizmet (Iaas) temel olarak altyapısıyla ilgili bilgi sahibi olduğunuz varsayılır dahil olmak üzere: 

- Sanal makine veya sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtma
- Azure platformlar arası komut satırı seçeneği JavaScript nesne gösterimi (JSON) şablonlarını kullanma dahil olmak üzere arabirimi (Azure CLI) kullanarak.

Bu makalede ayrıca, SAP HANA örnekleri yükleme ve yönetme ve SAP HANA örnekleri işletim bildiğinizi varsayar. Kurulum ve HANA sistem çoğaltması işlemleri ile bilgi sahibi olmanız özellikle önemlidir. Bu, yedekleme ve geri yükleme için SAP HANA veritabanları gibi görevleri içerir.

Bu makaleler, Azure üzerinde SAP HANA kullanma hakkında kapsamlı bilgi sağlar:

- [Tek örnek SAP hana Azure vm'lerde el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Azure vm'lerde SAP HANA sistem çoğaltması ayarlama](sap-hana-high-availability.md)
- [Azure Vm'leri üzerinde SAP HANA ' yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Bu makale SAP HANA hakkında bilginiz olması için de iyi bir fikirdir:

- [SAP HANA için yüksek kullanılabilirlik](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [SSS: SAP HANA için yüksek kullanılabilirlik](https://archive.sap.com/documents/docs/DOC-66702)
- [İçin SAP HANA sistem çoğaltmasını gerçekleştirme](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SPS 01 neler yeni: yüksek kullanılabilirlik](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [SAP HANA sistem çoğaltması için ağ önerileri](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA sistem çoğaltması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA sistem çoğaltması yapılandırın](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Azure'da Vm'leri dağıtma ile ilgili bilgi sahibi olması dışında kullanılabilirlik mimarisi Azure'da tanımlamadan önce okumanızı öneririz [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="service-level-agreements-for-azure-components"></a>Azure bileşenleri için hizmet düzeyi sözleşmeleri

Azure, ağ, depolama ve VM'lerin gibi farklı bileşenleri için farklı kullanılabilirlik SLA'larını sahiptir. Tüm SLA'ları belgelenmiştir. Daha fazla bilgi için [Microsoft Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/). 

[Sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) iki farklı yapılandırmaları için iki farklı SLA'lar açıklar:

- Kullanan tek bir VM [Azure premium SSD](../../windows/disks-types.md) işletim sistemi diski ve tüm veri diskleri için. Bu seçenek bir yüzde 99,9 aylık açık kalma süresi sağlar.
- Düzenlenen birden çok (en az iki) Vm'leri bir [Azure kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Bu seçenek bir yüzde 99,95 aylık çalışma süresi sağlar.

Azure bileşenleri sağlayabilir SLA'ları karşı kullanılabilirlik gereksinimi ölçün. Senaryolarınız için gerekli kullanılabilirlik düzeyini sağlamak SAP HANA seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [SAP HANA kullanılabilirliği tek Azure bölgesi içinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region).
- Hakkında bilgi edinin [SAP HANA kullanılabilirliği Azure bölgeleri arasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions). 















  


