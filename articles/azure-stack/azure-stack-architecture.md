---
title: "Microsoft Azure yığın Geliştirme Seti mimarisi | Microsoft Docs"
description: "Microsoft Azure yığın Geliştirme Seti mimarisi görüntüleyin."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e8ff17e069bd9237bf06cd79b4c222c50a999eda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Microsoft Azure yığın Geliştirme Seti mimarisi

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Azure yığın Geliştirme Seti Azure yığınının tek düğümlü dağıtımıdır. Tüm bileşenleri tek bir konak makinesi üzerinde çalışan sanal makineler yüklenir. 

## <a name="logical-architecture-diagram"></a>Mantıksal mimari diyagramı
Aşağıdaki diyagramda Azure yığın Geliştirme Seti ve bileşenlerinin mantıksal mimarisi gösterilmektedir.

![](media/azure-stack-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Sanal makine rolleri
Azure yığın Geliştirme Seti, konakta aşağıdaki sanal makineleri kullanarak hizmetleri sunar:

| Ad | Açıklama |
| ----- | ----- |
| **AzS ACS01** | Azure yığın depolama hizmetleri.|
| **AzS ADFS01** | Active Directory Federasyon Hizmetleri (ADFS).  |
| **AzS BGPNAT01** | Kenar yönlendirici ve Azure yığını için NAT ve VPN özellikleri sağlar. |
| **AzS CA01** | Azure yığın rol hizmetleri için sertifika yetkilisi Hizmetleri.|
| **AzS DC01** | Active Directory, DNS ve DHCP hizmetleri için Microsoft Azure yığın.|
| **AzS ERCS01** | Acil durumda Kurtarma Konsolu VM. |
| **AzS GWY01** | Sınır ağ geçidi VPN siteden siteye bağlantıları için Kiracı ağları gibi hizmetleri.|
| **AzS NC01** | Ağ denetleyicisi Azure yığın Ağ Hizmetleri yönetir.  |
| **AzS SLB01** | Kiracılar ve Azure yığın altyapı hizmetleri için Azure yığınında çoğaltıcı Hizmetleri Dengeleme yükleyin.  |
| **AzS SQL01** | Azure yığın altyapı rolleri için iç verileri depolar.  |
| **AzS WAS01** | Azure yığın yönetim portalı ve Azure Resource Manager Hizmetleri.|
| **AzS WASP01**| Azure yığın (Kiracı) Kullanıcı Portalı ve Azure Resource Manager Hizmetleri.|
| **AzS XRP01** | İşlem, ağ ve depolama kaynak sağlayıcıları dahil olmak üzere Microsoft Azure yığın için altyapı yönetim denetleyicisi.|


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın dağıtma](azure-stack-deploy.md)

[Denemek için ilk senaryoları](azure-stack-first-scenarios.md)

