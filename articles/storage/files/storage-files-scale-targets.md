---
title: "Azure dosyaları ölçeklenebilirlik ve performans hedefleri | Microsoft Docs"
description: "Kapasite, istek hızı ve gelen ve giden bant genişliği sınırları dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin."
services: storage
documentationcenter: na
author: wmgries
manager: klaasl
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/17/2017
ms.author: wgries
ms.openlocfilehash: 35d430b683224d1035cccdfb58b9db415d47ea3b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure dosyaları ölçeklenebilirlik ve performans hedefleri
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, endüstri standardı SMB protokolü erişilebilir bulutta sunar. Bu makalede, Azure dosyaları ve Azure dosya eşitleme (Önizleme) için ölçeklenebilirlik ve performans hedefleri anlatılmaktadır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri Gelişmiş hedeflerine, ancak diğer değişkenleri dağıtımınızdaki etkilenebilir. Örneğin, bir dosya için işleme Ayrıca, kullanılabilir ağ bant genişliği yalnızca Azure dosyaları hizmetini barındıran sunucular sınırlı olabilir. Azure dosyaları performans ve ölçeklenebilirlik gereksinimlerinizi karşılayacak olup olmadığını belirlemek için kullanım şekillerini sınama öneririz. Biz de zaman içinde bu sınırları artırmak için çalışıyoruz. Lütfen görüşlerinizi bize bildirme, ya da altında veya üzerinde açıklamalarında çekinmeyin [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), hangi sınırları hakkında bize artırmak görmek istediğiniz.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri
Üst kaynak bir Azure dosya paylaşımı için bir Azure depolama hesabıdır. Bir depolama hesabı Azure dosyaları dahil olmak üzere birden çok depolama hizmetleri tarafından verileri depolamak için kullanılan Azure depolama havuzunu temsil eder. Depolama hesaplarında verilerini depolayan diğer Azure Blob storage, Azure kuyruk depolama ve Azure Table depolama hizmetleridir. Aşağıdaki hedefleri verileri bir depolama hesabına depolama tüm depolama hizmetleri Uygula:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Depolama alanı hesabı kullanımı diğer depolama hizmetlerinden, Azure dosya paylaşımları depolama hesabınızdaki etkiler. Azure Blob storage ile en fazla depolama hesabı kapasitesi ulaştıysanız, Azure dosya paylaşımınızı en fazla paylaşım boyutunun altında olsa bile, örneğin, yeni dosyalarda Azure dosya paylaşımınızı oluşturmak mümkün olmaz.

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
Bu her zaman mümkün değildir ancak Azure dosya eşitleme ile biz sınırsız kullanım için tasarlamak mümkün olduğunca çalıştınız. Aşağıdaki tablo bizim sınırları ve hangi hedefleri gerçekte sabit kısıtlamalardır gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="see-also"></a>Ayrıca bkz.
- [Bir Azure dosyaları dağıtımını planlama](storage-files-planning.md)
- [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Diğer depolama hizmetleri için ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md)