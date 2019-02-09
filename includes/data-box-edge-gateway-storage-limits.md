---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/30/2019
ms.author: alkohli
ms.openlocfilehash: 94fe099984fae77c65658d7085a8540ff4f2448b
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967692"
---
Bu bölümde, Azure dosyaları, Azure blok BLOB'ları ve veri kutusu ağ geçidi/veri kutusu Edge hizmetine uygunsa Azure sayfa blobları için Azure depolama hizmeti için sınırlar ve gerekli adlandırma kurallarını açıklar. Depolama sınırları dikkatle gözden geçirin ve tüm önerilere uyun.

Azure depolama hizmet sınırları ve adlandırma paylaşımları, kapsayıcılar ve dosyalar için en iyi yöntemler üzerinde en son bilgiler için bkz:

- [Adlandırma ve kapsayıcıları başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blok blobları ve sayfa blob kuralları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Herhangi bir dosya ya da Azure depolama hizmeti limitlerin ya da Azure dosya/Blob adlandırma kurallarına uygun olmayan dizin varsa, ardından bu dosyaları veya dizinleri veri kutusu ağ geçidi/veri kutusu Edge hizmeti aracılığıyla Azure Depolama'ya alınan değil.