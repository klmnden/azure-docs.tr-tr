---
title: SAP HANA kullanılabilirlik Azure vm'lerde - genel bakış | Microsoft Docs
description: SAP HANA işlemleri Azure yerel vm'lerde açıklar.
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
ms.openlocfilehash: 7049a4b5159687ab928cda7ddc6b1a35959529ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187166"
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>SAP HANA yüksek kullanılabilirlik için Azure sanal makineler

SAP HANA Azure vm'lerinde gibi kritik veritabanları dağıtmak için çok sayıda Azure özelliklerini kullanabilirsiniz. Bu makalede Azure Vm'lerde barındırılan SAP HANA örnekleri için kullanılabilirlik elde etmek hakkında yönergeler açıklanmaktadır. Bu makalede, uygulayabileceğiniz çeşitli senaryolar anlatılmaktadır SAP HANA Azure kullanılabilirliğini artırmak için Azure altyapı kullanarak. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, Azure, hizmet (Iaas) temel olarak altyapısıyla bildiğinizi varsayar dahil olmak üzere: 

- Nasıl sanal makine ya da sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtılır.
- Azure platformlar arası komut satırı JavaScript nesne gösterimi (JSON) şablonları kullanma seçeneğiniz de dahil olmak üzere arabirimi (Azure CLI) kullanarak.

Bu makale aynı zamanda, SAP HANA örnekleri yükleme ve yönetme ve SAP HANA örnekleri işletim bildiğinizi varsayar. Kurulum ve HANA sistem çoğaltma işlemleri ile ilgili bilgi sahibi olmanız özellikle önemlidir. Bu, yedekleme ve geri yükleme SAP HANA veritabanları gibi görevleri içerir.

Bu makaleler Azure SAP HANA kullanımının iyi bir genel bakış sunar:

- [Azure vm'lerinde Tek Örnekli SAP HANA el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP HANA sistem çoğaltma Azure VM'de ayarlama](sap-hana-high-availability.md)
- [SAP HANA Azure Vm'lerinde yedekleyin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Bu makalelerde SAP HANA hakkında bilgi sahibi olmanız için de iyi bir fikirdir:

- [SAP HANA için yüksek kullanılabilirlik](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Hakkında SSS: SAP HANA için yüksek kullanılabilirlik](https://archive.sap.com/documents/docs/DOC-66702)
- [SAP HANA için sistem çoğaltmasını gerçekleştirme](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SP 01 ne ait yeni: yüksek kullanılabilirlik](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [SAP HANA sistem çoğaltma için ağ önerileri](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA sistem çoğaltmayı yapılandırmak için](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Azure, sanal makineleri dağıtma hakkında bilgi sahibi olmak ötesinde Azure'da kullanılabilirlik Mimarinizi tanımlamadan önce okumanızı öneririz [azure'da Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="service-level-agreements-for-azure-components"></a>Azure bileşenleri için hizmet düzeyi sözleşmeleri

Azure ağ, depolama ve sanal makineleri gibi farklı bileşenler için farklı kullanılabilirlik SLA sahiptir. Tüm SLA belgelenmiştir. Daha fazla bilgi için bkz: [Microsoft Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/). 

[Sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) iki farklı yapılandırmaları için iki farklı SLA açıklar:

- Kullanan tek bir VM [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) işletim sistemi diski ve tüm veri diskleri için. Bu seçenek, yüzde 99,9 aylık açık kalma süresi sağlar.
- Düzenlenen birden çok (en az iki) VM bir [Azure kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Bu seçenek, bir aylık açık kalma süresi 99,95 oranında sağlar.

Azure bileşenleri sağlayabilir SLA'ları, kullanılabilirlik gereksinimiyle ölçün. Ardından, senaryolarınız için gerekli düzeyde kullanılabilirlik elde etmek SAP HANA seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [bir Azure bölgesi içinde SAP HANA kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region).
- Hakkında bilgi edinin [SAP HANA kullanılabilirlik Azure bölgeler arasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions). 















  


