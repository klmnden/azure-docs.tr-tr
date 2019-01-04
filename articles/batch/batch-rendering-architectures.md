---
title: Azure işleme başvuru mimarileri - Azure Batch
description: Buluta geçiş tarafından mimarileri, şirket içi genişletmek için Azure Batch ve diğer Azure hizmetlerini kullanmak için Grup oluşturma
services: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 08/13/2018
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: d5102ba94e2b7808a457df00a87b35ef7022c454
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543504"
---
# <a name="reference-architectures-for-azure-rendering"></a>Azure işleme için başvuru mimarileri

Üst düzey mimari diyagramlar genişletmek senaryoları için bu makale veya şirket içi Grup Azure'a işleme "patlama". Örnekler, Azure işlem, ağ ve depolama hizmetleri için farklı seçenekleri açıklar.

## <a name="hybrid-with-nfs-or-cfs"></a>NFS veya CFS karma

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure sanal ağı.

* **Depolama** - giriş ve çıkış dosyaları: NFS veya CFS kullanarak Azure Vm'leri, Azure dosya eşitleme veya RSync aracılığıyla şirket içi depolama ile eşitlenir.

  ![Bulut Patlaması - karma NFS veya CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Karma Blobfuse ile

Aşağıdaki diyagramda, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterilmektedir:

* **İşlem** -Azure Batch havuzu veya sanal makine ölçek kümesi.

* **Ağ** -şirket içi: Azure ExpressRoute veya VPN. Azure: Azure sanal ağı.

* **Depolama** - giriş ve çıkış dosyaları: Blob depolama, işlem kaynaklarına Azure Blobfuse aracılığıyla bağlanır.

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