---
title: Linux sanal makinelerinde SAP kullanma | Microsoft Docs
description: Microsoft Azure’daki Linux sanal makinelerinde (VM) SAP çözümlerini kullanma hakkında bilgi edinin
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: gwallace
editor: ''
tags: azure-service-management
keywords: ''
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: aba060680871fb727103efd6068ca2fb4d84432e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709961"
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a>Azure'da Linux sanal makinelerinde SAP kullanma
Bulut Bilgi İşlem, BT sektöründeki küçük ölçekli şirketlerden büyük ve çok uluslu kuruluşlara kadar çok geniş ölçekte kullanılan ve her geçen gün daha önemli hale gelen bir terimdir. Microsoft Azure, Microsoft tarafından sunulan ve birçok yeni özelliğe sahip olan Bulut Hizmetleri Platformudur. Bu özellikler sayesinde müşteriler artık teknik veya mali kısıtlamalara takılmadan uygulamalarını Bulut Hizmetleri olarak hazırlayabilir ve kullanımdan kaldırabilir. Şirketler, zamanlarını ve bütçelerini donanım altyapısına harcamak yerine uygulamanın kendisine, iş süreçlerine ve hem müşteriler hem de kullanıcılar için fayda sağlamaya odaklanabilir.

Microsoft, Microsoft Azure sanal makineler, hizmet (Iaas) platformu olarak kapsamlı bir altyapı sunar. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Aşağıdaki teknik incelemeler, planlama ve azure'daki Windows sanal makinelerinde SAP NetWeaver tabanlı uygulamaların uygulama açıklanır. SAP NetWeaver tabanlı uygulamalar üzerinde de uygulayabilirsiniz [Windows sanal makineleri](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a>Azure SUSE Linux sanal makinelerinde SAP NetWeaver
Başlık: Microsoft Azure SUSE Linux VM’lerde SAP NetWeaver’ı test etme

Özet: Azure Linux Vm'lerinde SAP NetWeaver çalıştırmak, bu anda resmi SAP desteği yoktur. Yine de müşterilerin bazı testler yapmak isteyebilirsiniz veya SAP destek için gerek yoktur sürece Azure Linux Vm'lerinde SAP demo veya eğitim sistemleri çalıştırmak için göz önünde bulundurabilirsiniz. Bu makalede ayarlama Azure SUSE Linux Vm'lerde SAP çalıştırmak için yardımcı olur ve olası yaygın görülen tehlikeleri önlemek için temel bazı ipuçları sağlar.

Güncelleştirme: Aralık 2015

[Bu makalede buradan ulaşabilirsiniz](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

