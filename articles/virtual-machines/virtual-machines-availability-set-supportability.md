---
title: Azure Vm'leri mevcut bir kullanılabilirlik kümesine ekleme desteklenebilirliği | Microsoft Docs
description: Azure Vm'leri mevcut bir kullanılabilirlik kümesine ekleme desteklenebilirliği ayarlayın.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 7a5e97b66fec040b4ec32caa8d58cf9b50169a33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443713"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Azure Vm'leri, var olan bir kullanılabilirlik kümesine ekleme desteklenebilirliği

Yeni sanal makineler (VM'ler) eklediğiniz zaman zaman zaman var olan bir kullanılabilirlik kümesine kısıtlamalarla karşılaşabilirsiniz. Aşağıdaki grafikte, aynı kullanılabilirlik kümesinde karıştırabilirsiniz hangi VM serisi ayrıntıları.

Farklı türlerdeki Vm'leri karıştırmak için desteklenebilirlik matris şu şekildedir:

Seriler ve kullanılabilirlik kümesi|İkinci VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|İlk VM|||||||
|A||Tamam|Tamam|Tamam|Tamam|Tamam|
|Av2||Tamam|Tamam|Tamam|Tamam|Tamam|
|D||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv2||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv3||Tamam|Tamam|Tamam|Tamam|Tamam|

Diğer tüm serisi aynı kullanılabilirlik bunlar belirli bir donanım gerektirdiğinden kümesinde bulunamadı.

A8/A9 VM boyutu, adanmış RDMA arka uç ağı üzerinde gereksinimi nedeniyle karıştırılamaz.
