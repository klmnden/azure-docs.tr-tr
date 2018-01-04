---
title: "Azure API management ilke örnek - paylaşılan erişim imzası oluşturma | Microsoft Docs"
description: "Azure API management ilke örnek - paylaşılan erişim imzası ifadeler kullanarak oluşturmak ve isteği yeniden yazma-URI İlkesi ile Azure depolama iletmek nasıl gösterir..."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
<<<<<<< HEAD
ms.openlocfilehash: e1f17f9f4e17a3eebb55e4ec1905aec19a2165a5
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
=======
ms.openlocfilehash: 9b0d37e4f7930389d3399e51de905db2b2ce8c27
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="generate-shared-access-signature"></a>Paylaşılan erişim imzası oluşturma

Bu makalede oluşturmak nasıl oluşturulduğunu gösteren bir Azure API management ilke örnek gösterilmektedir [paylaşılan erişim imzası](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) ifadeler kullanarak ve isteği yeniden yazma-URI İlkesi ile Azure depolama iletebilir. Ayarlamak veya ilke kodu düzenlemek için açıklanan adımları izleyin [ayarlama veya düzenleme bir ilke](../set-edit-policies.md). Diğer örnekleri görmek için bkz: [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Koda Yapıştır **gelen** bloğu.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

