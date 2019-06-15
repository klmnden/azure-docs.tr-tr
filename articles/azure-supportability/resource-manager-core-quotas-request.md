---
title: Azure Resource Manager vCPU Kotayı artırmak istekleri | Microsoft Docs
description: Azure Resource Manager vCPU kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076819"
---
# <a name="quota-increase-requests"></a>Kota artışı istekleri

Sanal makineler ve sanal makine ölçek kümeleri için Kaynak Yöneticisi'ni vCPU kotaları, her bölgede her abonelik için iki katmanda uygulanır. 

İlk katman toplam bölgesel Vcpu sınırına (tüm VM serisi) ve ikinci katman (örneğin, D serisi Vcpu) VM serisi Vcpu başına. Dilediğiniz zaman dağıtılması için yeni bir sanal makine olduğu belirli sanal makine serisi için onaylanmış vCPU kotası VM serisi için yeni ve mevcut Vcpu kullanım toplamını aşmamalıdır. Ayrıca, tüm VM serisi dağıtılmış toplam yeni ve mevcut vCPU sayısı abonelik için onaylanmış toplam bölgesel Vcpu kotası aşmamalıdır. Kotalarda biri aşılırsa, VM dağıtımı izin verilmiyor.
Azure Portalı'ndan VM serisi Vcpu kotası sınırının artışı isteyebilir. VM serisi kota artış, toplam bölgesel Vcpu sınırına tutarında otomatik olarak artırır. 

Yeni bir abonelik oluşturulurken varsayılan toplam bölgesel vcpu sayısı tek tek tüm VM serisi için varsayılan vCPU kotaları toplamına eşit olmayabilir. Bu, bir abonelikte dağıtmak istediğiniz tek tek her VM serisi için yeterli miktarda kota, ancak tüm dağıtımlar için toplam bölgesel Vcpu için yeterli kotası ile sonuçlanabilir. Bu durumda, açıkça toplam bölgesel Vcpu sınırı artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vcpu sayısı sınırı, bölge için tüm VM serisi arasında onaylı kota toplamını aşamaz.

Kotaları hakkında daha fazla bilgi [sanal makine vCPU Kotası sayfası](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet sınırlamaları](https://aka.ms/quotalimits) sayfası. 

