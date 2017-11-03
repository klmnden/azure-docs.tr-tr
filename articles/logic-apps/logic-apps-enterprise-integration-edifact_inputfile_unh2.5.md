---
title: "Logic Apps EDIFACT kod çözme B2B gidermek UNH2.5 - Azure Logic Apps | Microsoft Docs"
description: "EDIFACT kod çözme Azure Logic Apps B2B UNH2.5 çözmek"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 62ad8183cc6e9f56255b2729a04ee7710d00a21a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a>UNH2.5 segment olan EDIFACT belgeler nasıl ele alınacağını
UNH2.5 EDIFACT belgede mevcut olduğunda, şema arama için kullanılıyor. 

Örnek: UNH alandır **EAN008** EDIFACT iletisi  
UNH + SSDD1 + SİPARİŞLERİ: D: 03B: KALDIRIN:**EAN008**'  

İletiyi işlemek için izlemeniz gereken adımlar 
1. Şemayı Güncelleştir
2. Anlaşma ayarlarını kontrol edin  

## <a name="update-the-schema"></a>Şemayı Güncelleştir
İletiyi işlemek için bir şema UNH2.5 kök düğümü adı ile dağıtmak için gerekir.  Verilen bir örnek için şema kök adı olacaktır **EFACT_D03B_ORDERS_EAN008**  

Farklı bir UNH2.5 segment ile her D03B_ORDERS için tek bir şema dağıtmak zorunda kalırsınız.  

## <a name="add-schema-to-the-edifact-agreement"></a>EDIFACT sözleşmesi şema ekleyin
### <a name="edifact-decode"></a>EDIFACT kod çözme
Gelen ileti kodunu çözmek için şema yapılandırma EDIFACT sözleşmesi alma ayarları
1. Şema tümleştirme hesabı Ekle    
2. Şema yapılandırma EDIFACT sözleşmesi ayarlarını alır. 
3. EDIFACT sözleşmesi tıklatıp **JSON olarak Düzenle**.  Alma sözleşmesindeki UNH2.5 değeri eklemek **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT kodlama
Gelen ileti kodlamak için şema EDIFACT sözleşmesi gönderme ayarlarını yapılandırın
1. Şema tümleştirme hesabı Ekle    
2. Şema EDIFACT sözleşmesi gönderme ayarlarını yapılandırın. 
3. EDIFACT sözleşmesi tıklatıp **JSON olarak Düzenle**.  Gönderme Sözleşmesi UNH2.5 değeri eklemek **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Sonraki Adımlar
* [Tümleştirme hesap anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  