---
title: Azure işleme başvuru mimarileri - Azure Batch
description: Buluta geçiş tarafından mimarileri, şirket içi genişletmek için Azure Batch ve diğer Azure hizmetlerini kullanmak için Grup oluşturma
services: batch
ms.service: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 02/07/2019
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: ae4680c948ce8e1efd32207dc37821d61182f2d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60774186"
---
# <a name="reference-architectures-for-azure-rendering"></a>Azure işleme için başvuru mimarileri

Üst düzey mimari diyagramlar genişletmek senaryoları için bu makale veya şirket içi Grup Azure'a işleme "patlama". Örnekler, Azure işlem, ağ ve depolama hizmetleri için farklı seçenekleri açıklar.

## <a name="hybrid-with-nfs-or-cfs"></a>NFS veya CFS karma

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** - giriş ve çıkış dosyaları: NFS veya CFS kullanarak Azure Vm'leri, Azure dosya eşitleme veya RSync aracılığıyla şirket içi depolama ile eşitlenir. Alternatif olarak: Avere vFXT giriş veya çıkış dosyalarını NFS kullanarak şirket içi NAS cihazlardan.

  ![Bulut Patlaması - karma NFS veya CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Karma Blobfuse ile

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** - giriş ve çıkış dosyaları: Blob depolama, işlem kaynaklarına Azure Blobfuse aracılığıyla bağlanır.

  ![Bulut Patlaması - Blobfuse ile karma](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hibrit bilgi işlem ve depolama

Aşağıdaki diyagramda, işlem ve depolama için bağlı tamamen karma bir senaryo gösterir ve aşağıdaki Azure hizmetlerini içerir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** -şirketler arası: Avere vFXT. İsteğe bağlı şirket içi arşivleme Azure Data Box dosyaları Blob depolama alanına veya Avere FXT NAS hızlandırması için şirket içinde.

  ![Bulut Patlaması - karma hesaplama ve depolama](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Sonraki adımlar

* Kullanma hakkında daha fazla bilgi edinin [işleme yöneticileri](batch-rendering-render-managers.md) Azure toplu işlem.

* Seçenekleri hakkında daha fazla bilgi [Azure'da işleme](batch-rendering-service.md).