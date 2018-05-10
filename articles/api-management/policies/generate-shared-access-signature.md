---
title: Azure API management ilke örnek - paylaşılan erişim imzası oluşturma | Microsoft Docs
description: Azure API management ilke örnek - paylaşılan erişim imzası ifadeler kullanarak oluşturmak ve isteği yeniden yazma-URI İlkesi ile Azure depolama iletmek nasıl gösterir...
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
ms.openlocfilehash: fadad6f83f1a855f3ca1509a6a5e02c822db512d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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

