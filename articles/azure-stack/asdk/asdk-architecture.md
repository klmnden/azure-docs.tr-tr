---
title: Azure yığın Geliştirme Seti mimarisi | Microsoft Docs
description: Azure yığın Geliştirme Seti (ASDK) mimarisini açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 68da3ac0eb135f5956dfea76e186d9c57beea79c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975870"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Microsoft Azure yığın Geliştirme Seti mimarisi
Azure yığın Geliştirme Seti (ASDK), Azure yığınının bir tek düğümlü dağıtımıdır. Tüm bileşenleri tek bir konak makinesi üzerinde çalışan sanal makineler yüklenir. 

## <a name="logical-architecture-diagram"></a>Mantıksal mimari diyagramı
Aşağıdaki diyagramda ASDK ve bileşenlerinin mantıksal mimarisi gösterilmektedir.

![ASDK mimarisi](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Sanal makine rolleri
ASDK Geliştirme Seti ana bilgisayarda barındırılan şu sanal makineleri kullanarak hizmetleri sunar:

| Ad | Açıklama |
| ----- | ----- |
| **AzS-ACS01** | Azure yığın depolama hizmetleri.|
| **AzS-ADFS01** | Active Directory Federasyon Hizmetleri (ADFS).  |
| **AzS-BGPNAT01** | Kenar yönlendirici ve Azure yığını için NAT ve VPN özellikleri sağlar. |
| **AzS-CA01** | Azure yığın rol hizmetleri için sertifika yetkilisi Hizmetleri.|
| **AzS-DC01** | Active Directory, DNS ve DHCP hizmetleri için Microsoft Azure yığın.|
| **AzS-ERCS01** | Acil durumda Kurtarma Konsolu VM. |
| **AzS-GWY01** | Sınır ağ geçidi VPN siteden siteye bağlantıları için Kiracı ağları gibi hizmetleri.|
| **AzS-NC01** | Ağ denetleyicisi Azure yığın Ağ Hizmetleri yönetir.  |
| **AzS-SLB01** | Kiracılar ve Azure yığın altyapı hizmetleri için Azure yığınında çoğaltıcı Hizmetleri Dengeleme yükleyin.  |
| **AzS-SQL01** | Azure yığın altyapı rolleri için iç verileri depolar.  |
| **AzS-WAS01** | Azure yığın yönetim portalı ve Azure Resource Manager Hizmetleri.|
| **AzS-WASP01**| Azure yığın (Kiracı) Kullanıcı Portalı ve Azure Resource Manager Hizmetleri.|
| **AzS-XRP01** | İşlem, ağ ve depolama kaynak sağlayıcıları dahil olmak üzere Microsoft Azure yığın için altyapı yönetim denetleyicisi.|


## <a name="next-steps"></a>Sonraki adımlar
[Temel ASDK yönetim görevleri hakkında bilgi edinin](asdk-admin-basics.md)
