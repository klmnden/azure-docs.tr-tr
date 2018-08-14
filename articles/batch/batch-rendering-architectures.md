---
title: Azure işleme - başvuru mimarileri
description: Buluta geçiş tarafından mimarileri, şirket içi genişletmek için Azure Batch ve diğer Azure hizmetlerini kullanmak için Grup oluşturma
services: batch
author: davefellows
manager: jeconnoc
ms.author: danlep
ms.date: 08/13/2018
ms.topic: conceptual
ms.openlocfilehash: 0fe101ee6eb88094034b90c4d39f06ba509c9512
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "40099934"
---
# <a name="reference-architectures-for-azure-rendering"></a>Azure işleme için başvuru mimarileri

Üst düzey mimari diyagramlar genişletmek senaryoları için bu makale veya şirket içi Grup Azure'a işleme "patlama". Örnekler, Azure işlem, ağ ve depolama hizmetleri için farklı seçenekleri açıklar.

## <a name="hybrid-with-nfs-or-cfs"></a>NFS veya CFS karma

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure sanal ağı.

* **Depolama** - giriş ve Çıkış dosyalarını: NFS ya da Azure Vm'lerini kullanarak CFS eşitlenmiş Azure dosya eşitleme veya RSync aracılığıyla şirket içi depolama ile.

  ![Bulut Patlaması - karma NFS veya CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Karma Blobfuse ile

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure sanal ağı.

* **Depolama** - giriş ve Çıkış dosyalarını: Blob Depolama, işlem kaynaklarına Azure Blobfuse aracılığıyla bağlanır.

  ![Bulut Patlaması - Blobfuse ile karma](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hibrit bilgi işlem ve depolama

Aşağıdaki diyagramda, işlem ve depolama için bağlı tamamen karma bir senaryo gösterir ve aşağıdaki Azure hizmetlerini içerir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure sanal ağı.

* **Depolama** -şirketler arası: Avere vFXT. İsteğe bağlı, arşivleme Azure Data Box aracılığıyla dosyaları Blob Depolama için şirket içi.

  ![Bulut Patlaması - karma hesaplama ve depolama](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Sonraki adımlar

* Kullanma hakkında daha fazla bilgi edinin [işleme yöneticileri](batch-rendering-render-managers.md) Azure toplu işlem.

* Seçenekleri hakkında daha fazla bilgi [Azure'da işleme](batch-rendering-service.md).