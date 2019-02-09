---
title: Azure veri kutusu Edge sınırlar | Microsoft Docs
description: Sistem sınırlarını ve önerilen boyut için Microsoft Azure veri kutusu Edge açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/05/2019
ms.author: alkohli
ms.openlocfilehash: 30e0c37d3d0c03e77b6dab9c06c0a50bff27e8bc
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967682"
---
# <a name="azure-data-box-edge-limits-preview"></a>Azure veri kutusu Edge sınırları (Önizleme)

Limitler, dağıtmanıza ve Microsoft Azure veri kutusu Edge çözümünüz olarak düşünün.

> [!IMPORTANT]
> Data Box Edge, Önizleme aşamasındadır. Bu çözümü dağıtmadan önce [önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.


## <a name="data-box-edge-service-limits"></a>Veri kutusu Edge hizmet sınırları

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-edge-device-limits"></a>Veri kutusu Edge cihaz sınırları

Aşağıdaki tablo, veri kutusu sınır cihazı sınırları açıklar.

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosyaları |100 milyon |
|Hayır. cihaz başına paylaşılma sayısı |24 |
|Hayır. kapsayıcı başına paylaşılma sayısı |1 |
|Bir paylaşıma yazılan en büyük dosya boyutu| 5 TB |

## <a name="azure-storage-limits"></a>Azure depolama sınırları

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Uyarılar karşıya veri yükleme

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure depolama hesabı boyut ve nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway'i dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)
