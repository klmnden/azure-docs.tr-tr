---
title: Windows sanal makinelerde SAP kullanma | Microsoft Docs
description: "Microsoft Azure Windows sanal makinelerle (VM'ler) SAP kullanma hakkında Temizle"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 1d6326d5f79ea4e975abd1aa90b0d4a635f4c31a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makinelerde SAP kullanma
Bulut Bilgi İşlem, BT sektöründeki küçük ölçekli şirketlerden büyük ve çok uluslu kuruluşlara kadar çok geniş ölçekte kullanılan ve her geçen gün daha önemli hale gelen bir terimdir. Microsoft Azure, Microsoft tarafından sunulan ve birçok yeni özelliğe sahip olan Bulut Hizmetleri Platformudur. Bu özellikler sayesinde müşteriler artık teknik veya mali kısıtlamalara takılmadan uygulamalarını Bulut Hizmetleri olarak hazırlayabilir ve kullanımdan kaldırabilir. Şirketler, zamanlarını ve bütçelerini donanım altyapısına harcamak yerine uygulamanın kendisine, iş süreçlerine ve hem müşteriler hem de kullanıcılar için fayda sağlamaya odaklanabilir.

Microsoft Azure sanal makinelerle Microsoft olarak hizmet (Iaas) platform kapsamlı bir altyapı sunar. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Aşağıdaki teknik incelemeler, planlamanızı ve azure'da Windows sanal makinelerde SAP NetWeaver tabanlı uygulamalar açıklanmaktadır. SAP NetWeaver tabanlı uygulamalar üzerinde uygulayabilirsiniz [Linux sanal makineleri](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver azure'da - HA
Başlık: SAP NetWeaver azure'da - kümeleme SAP ASCS/SCS SIOS DataKeeper ile azure'da Windows Server Yük devretme kullanarak örnekleri

Özet: ' Bu belge, Azure üzerinde yüksek oranda kullanılabilir bir SAP ASCS/SCS yapılandırma ayarlamak için SIOS DataKeeper kullanmayı açıklar. SAP hatası bileşenleriyle SAP ASCS/SCS veya kuyruğa Çoğaltma Hizmetleri gibi paylaşılan diskleri gerektiren Windows Server Yük devretme yapılandırmaları kendi tek noktası korur. Bu SAP bileşenleri SAP sistem işlevleri için gereklidir. Bu nedenle yüksek kullanılabilirlik işlevselliği bu bileşenlerin bir sunucu veya tam ve Hyper-V ortamları için Windows Küme yapılandırmaları ile yapılan bir VM başarısızlığını karşılayabilir emin olmak için yararlanılmasını gerekir. Ağustos 2015'ten itibaren Windows için gerekli olacak paylaşılan disklerin yüksek oranda kullanılabilir yapılandırmaları Bu kritik SAP bileşenler için gerekli temel Azure kendisi üzerinde sağlayamaz. Ancak SIOS tarafından DataKeeper ürün Yardımı ile Azure Iaas platformunda SAP ASCS/SCS için gerektiği gibi Windows Server Yük devretme yapılandırmaları oluşturulabilir. Bu yazı adım adım bir yaklaşım bir Windows Server Yük devretme yapılandırması SIOS Datakeeper Azure tarafından sağlanan paylaşılan disk ile yüklemeyi açıklar. Kağıt bir en iyi şekilde çalışması yüksek kullanılabilirlik yapılandırması yapan Azure, Windows ve SAP tarafında yapılandırmaları Ayrıntılar açıklanmaktadır. Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynaklar üzerinde platformları temsil eden bir SAP notlar ve SAP yükleme belgelerini tamamlar.

Güncelleştirilmiş: Ağustos 2015

[Bu kılavuzu hemen indirin](http://go.microsoft.com/fwlink/?LinkId=613056)

