---
title: Azure API management ilke örneği - paylaşılan erişim imzası oluştur | Microsoft Docs
description: Azure API management ilke örneği - paylaşılan erişim imzası ifadeleri kullanarak oluşturma ve isteği yeniden yazma-URI İlkesi ile Azure depolama gösterir...
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 2c3adaa6f4e113f09e676583c2c35b5f1fbdb622
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61062199"
---
# <a name="generate-shared-access-signature"></a>Paylaşılan erişim imzası oluşturma

Bu makale oluşturmayı gösterir bir Azure API management ilke örneği [paylaşılan erişim imzası](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) ifadeleri kullanarak ve isteği yeniden yazma-URI İlkesi ile Azure depolama iletir. Ayarlama veya ilke kodu düzenleme için açıklanan adımları izleyin [ayarlama veya düzenleme ilke](../set-edit-policies.md). Diğer örnekler için bkz [ilkesi örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodun içine yapıştırın **gelen** blok.

[!code-xml[Main](../../../api-management-policy-samples/examples/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

