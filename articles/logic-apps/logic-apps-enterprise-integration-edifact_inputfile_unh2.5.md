---
title: EDIFACT iletilerini UNH 2.5 segements - Azure Logic Apps ile işleme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile UNH2.5 parçalarla EDIFACT belgelerini çözümleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/27/2017
ms.openlocfilehash: 926c9ebe8675d8b50d4544be813ae0b15492ae35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60681687"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Azure Logic apps'te UNH2.5 parçalarla EDIFACT belgelerini

EDIFACT belgede UNH2.5 olduğunda şema arama için kullanılıyor. 

Örnek: UNH alan **EAN008** EDIFACT iletisi  
UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'  

İletisini işlemek için izlemeniz gereken adımlar 
1. Şemayı Güncelleştir
2. Sözleşme ayarlarını kontrol edin  

## <a name="update-the-schema"></a>Şemayı Güncelleştir
İletiyi işlemek için bir şema UNH2.5 kök düğümü adı ile dağıtmanız gerekebilir.  Verilen bir örnek için şema kök adı olacaktır **EFACT_D03B_ORDERS_EAN008**  

Farklı bir UNH2.5 segmenti ile her D03B_ORDERS için ayrı bir şema dağıtmak gerekir.  

## <a name="add-schema-to-the-edifact-agreement"></a>EDIFACT sözleşmesini şema ekleme
### <a name="edifact-decode"></a>EDIFACT kodunu çözme
Gelen ileti kodunu çözmek için şema yapılandırma EDIFACT sözleşmesi alma ayarları
1. Şemayı tümleştirme hesabına ekleyin    
2. Şema yapılandırma ayarlarını alma sözleşmesindeki EDIFACT içinde. 
3. EDIFACT Sözleşmesi'ni seçip tıklayın **JSON olarak Düzenle**.  UNH2.5 değer alma sözleşmesindeki Ekle **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT kodlama
Gelen ileti kodlamak için EDIFACT anlaşma gönderme ayarlarında şema yapılandırma
1. Şemayı tümleştirme hesabına ekleyin    
2. Şema EDIFACT anlaşma gönderme ayarlarında yapılandırın. 
3. EDIFACT Sözleşmesi'ni seçip tıklayın **JSON olarak Düzenle**.  Gönderme anlaşması'nda UNH2.5 değer Ekle **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Sonraki Adımlar
* [Tümleştirme hesabı sözleşmeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmalar hakkında bilgi edinin")  