---
title: Azure veri kutusu ağ geçidi sınırlar | Microsoft Docs
description: Microsoft Azure veri kutusu ağ geçidi için sistem sınırlarını ve önerilen boyut açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: e80b03f696a78887676e9f16750055a4dcfac230
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60755257"
---
# <a name="azure-data-box-gateway-limits"></a>Azure veri kutusu ağ geçidi sınırları

Limitler, dağıtmanıza ve Microsoft Azure veri kutusu ağ geçidi çözümünüz olarak düşünün. 


## <a name="data-box-gateway-service-limits"></a>Veri kutusu Gateway hizmet limitleri

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Veri kutusu ağ geçidi cihaz sınırları

Veri kutusu ağ geçidi cihazı için sınırlar aşağıdaki tabloda açıklanmaktadır.

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosyaları |100 milyon <br> Sınır ~ her 2 TB disk alanı ile en fazla 100 milyon sınırında 25 milyon dosya |
|Hayır. cihaz başına paylaşılma sayısı |24 |
|Hayır. Azure depolama kapsayıcısı başına paylaşılma sayısı |1 |
|Bir paylaşıma yazılan en büyük dosya boyutu|2 TB sanal cihaz için en büyük dosya boyutu 500 GB'dir. <br> En çok 5 TB ulaşana kadar önceki oranını veri disk boyutu ile maksimum dosya boyutu artar. |

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
