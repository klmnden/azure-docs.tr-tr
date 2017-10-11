---
title: SAP Linux sanal makineleri kullanma | Microsoft Docs
description: "Microsoft Azure’daki Linux sanal makinelerinde (VM) SAP çözümlerini kullanma hakkında bilgi edinin"
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 66eb53f99ce4b30ec283243deb3649c9ca897a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a>Azure'daki Linux sanal makinelerde SAP kullanma
Bulut Bilgi İşlem, BT sektöründeki küçük ölçekli şirketlerden büyük ve çok uluslu kuruluşlara kadar çok geniş ölçekte kullanılan ve her geçen gün daha önemli hale gelen bir terimdir. Microsoft Azure, Microsoft tarafından sunulan ve birçok yeni özelliğe sahip olan Bulut Hizmetleri Platformudur. Bu özellikler sayesinde müşteriler artık teknik veya mali kısıtlamalara takılmadan uygulamalarını Bulut Hizmetleri olarak hazırlayabilir ve kullanımdan kaldırabilir. Şirketler, zamanlarını ve bütçelerini donanım altyapısına harcamak yerine uygulamanın kendisine, iş süreçlerine ve hem müşteriler hem de kullanıcılar için fayda sağlamaya odaklanabilir.

Microsoft Azure sanal makinelerle Microsoft olarak hizmet (Iaas) platform kapsamlı bir altyapı sunar. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Aşağıdaki teknik incelemeler, planlamanızı ve azure'da Windows sanal makinelerde SAP NetWeaver tabanlı uygulamalar açıklanmaktadır. SAP NetWeaver tabanlı uygulamalar üzerinde uygulayabilirsiniz [Windows sanal makineleri](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a>SAP NetWeaver Azure SUSE Linux sanal makinelerde
Başlık: Microsoft Azure SUSE Linux VM'ler üzerinde SAP NetWeaver test etme

Özet: Bu anda SAP NetWeaver Azure Linux VM'ler üzerinde çalıştırılan için resmi SAP desteği yoktur. Yine de müşteriler bazı test yapmak isteyebileceğiniz veya SAP destek uzmanıyla gerek yoktur sürece SAP demo veya eğitim sistemleri Azure Linux VM'ler üzerinde çalıştırmak için düşünebilirsiniz. Bu makalede Azure SUSE Linux VM'ler ayarlama SAP çalıştırmak için yardımcı olur ve ortak olası Tuzaklar önlemek için temel bazı ipuçları sağlar.

Güncelleştirilmiş: Aralık 2015

[Bu makalede burada bulunabilir.](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

