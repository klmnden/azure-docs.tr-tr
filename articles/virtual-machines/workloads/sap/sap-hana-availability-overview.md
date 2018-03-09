---
title: "SAP HANA kullanılabilirlik Azure vm'lerde - genel bakış | Microsoft Docs"
description: "SAP HANA işlemleri Azure yerel vm'lerde"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b22f89750234a4715d2b7fd2df6ad8740b1d085
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-high-availability-guide-for-azure-virtual-machines"></a>Azure sanal makineler için SAP HANA yüksek kullanılabilirlik Kılavuzu
Azure SAP HANA Azure VM'de gibi görev kritik veritabanları dağıtmak için sağlayan çok sayıda yetenekleri sağlar. Bu belgede, Azure sanal makinelerinde barındırılan SAP HANA örnekleri için kullanılabilirlik elde etmek nasıl Rehber sağlanır. Belgenin içinde Azure üzerinde SAP HANA kullanılabilirliğini artırmak için Azure altyapı uygulanabilir birkaç senaryo açıklanmaktadır. 

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, böyle bir altyapı (ıaas) temel Azure ile ilgili olarak aşina olduğunuzu varsayar: 

- Nasıl sanal makine ya da sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtılır.
- Azure platformlar arası komut satırı JavaScript nesne gösterimi (JSON) şablonları kullanma seçeneğiniz de dahil olmak üzere arabirimi (CLI).

Bu kılavuz Ayrıca, SAP HANA örnekleri yükleme ve yönetme ve SAP HANA örnekleri işletim konusunda bilgi sahibi olduğunuzu varsayar. Özellikle ayarlayın ve işlemleri HANA sistem çoğaltma veya görevler geçici yedekleme ve geri yükleme SAP HANA veritabanlarının ister.

SAP HANA konularında Azure iyi bir genel bakış vermek diğer makaleler şunlardır:

- [Azure vm'lerinde Tek Örnekli SAP HANA el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP HANA sistem Azure VM'ler çoğaltmasında Kurulumu](sap-hana-high-availability.md)
- [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Makale ve SAP HANA hakkında bilgi sahibi olmanız gerekir içeriği gibi listelenir:

- [SAP HANA için yüksek kullanılabilirlik](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Hakkında SSS: SAP HANA için yüksek kullanılabilirlik](https://archive.sap.com/documents/docs/DOC-66702)
- [SAP HANA için sistem çoğaltma gerçekleştirme](https://archive.sap.com/documents/docs/DOC-47702)
- [Yenilikler HANA 2.0 SP 01 SAP: yüksek kullanılabilirlik](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [SAP HANA sistem çoğaltma için ağ önerileri](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [SAP HANA sistem çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA hizmet otomatik yeniden başlatma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [SAP HANA sistem çoğaltma yapılandırma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)


Azure'da VM dağıtımıyla tanıdık olmasının ötesinde, ayrıca makaleyi okuduktan öneririz [azure'da Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) Azure'da kullanılabilirlik Mimarinizi tanımlama ile devam etmeden önce.

## <a name="service-level-agreements-for-different-azure-components"></a>Farklı Azure bileşenleri için hizmet düzeyi sözleşmeleri
Azure ağ, depolama ve sanal makineleri gibi farklı bileşenler için farklı kullanılabilirlik SLA sahiptir. Tüm bu SLA belgelenen ve başlayarak bulunabilir [Microsoft Azure hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/) sayfası. Kullanıma varsa [sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), Azure iki farklı yapılandırmalara sahip iki farklı SLA sağladığını unutmayın. SLA'ları gibi tanımlanır:

- Kullanarak tek bir VM [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) işletim sistemi için disk ve tüm veri diskleri % 99,9 aylık bir çalışma süresi yüzdesini sağlar
- Düzenlenen birden çok (en az iki) VM bir [Azure kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) % 99,95 aylık süresi yüzdesi sağlayın

Kullanılabilirlik SLA'ları Azure bileşenleri karşı gereksinim sağlar ve kullanılabilirlik elde etmek için SAP HANA ile uygulamak için gereken farklı senaryoları hakkında karar ölçü sağlamak için gereklidir.

## <a name="next-steps"></a>Sonraki adımlar
Belgeleri devam edin:

- [Bir Azure bölgesi içinde SAP HANA kullanılabilirlik](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [SAP HANA kullanılabilirlik Azure bölgeler arasında](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 















  


