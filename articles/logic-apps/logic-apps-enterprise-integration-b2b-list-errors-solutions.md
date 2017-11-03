---
title: "Logic Apps B2B listesi hatalar ve çözümleri: Azure App Service | Microsoft Docs"
description: "Logic Apps B2B listesi hatalar ve çözümleri"
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
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1865d75f1b4c2aa18d5a3130f639572d19563b3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Logic Apps B2B listesi hatalar ve çözümleri  
Bu makalede mantığını uygulamaları B2B senaryolarda oluşabilir ve bu hataların düzeltilmesi için uygun eylemleri öneren hatalarında sorun giderme yardımcı olur.


## <a name="agreement-resolution"></a>Anlaşma çözümleme

### <a name="no-agreement-found"></a>* Hiçbir sözleşmesi bulunamadı 

|   |   |  
|---|---|
| Hata açıklaması | Anlaşma çözümleme parametrelerle bulunan hiçbir Sözleşmesi|    
| Kullanıcı eylemi | Üzerinde anlaşılan iş kimliklerle tümleştirme hesabına sözleşmesi eklenmesi gerekir.</br> İş kimlikleri için giriş iletisi kimlikleri eşleşmelidir|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>* Kimliklerle bulunan hiçbir Sözleşmesi

|   |   | 
|---|---|
| Hata açıklaması | Kimliklerle bulunan hiçbir sözleşmesi: 'AS2Identity':: 'Partner1' ve 'AS2Identity':: 'Partner3'| 
| Kullanıcı eylemi | Geçersiz AS2-gelen veya AS2-anlaşma için için yapılandırılmış. </br> Doğru AS2 ileti AS2-gelen veya AS2-üstbilgileri veya AS2 AS2 kimlikleriyle eşleşen anlaşma ileti sözleşmesi yapılandırmalarla üstbilgileri |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* AS2 ileti üstbilgilerini eksik  

|   |   |  
|---|---|
| Hata açıklaması| Geçersiz AS2 üstbilgileri. Aşağıdakilerden birini ' AS2-için ' veya ' AS2-gelen ' üstbilgileri boş| 
| Kullanıcı eylemi | AS2 içermeyen bir AS2 iletisi alındı-gelen veya AS2-için veya her iki üstbilgileri. </br> AS2 ileti AS2 denetleyin-gelen ve AS2-üstbilgileri ve doğru bunları sözleşmesi yapılandırmasını temel alarak |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Eksik AS2 ileti gövdesi ve üstbilgileri    

|   |   |  
|---|---|
| Hata açıklaması| İstek içeriği null veya boş | 
| Kullanıcı eylemi | İleti gövdesi içermeyen bir AS2 iletisi alındı |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* AS2 ileti şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlenen hata: şifre çözme başarısız oldu] | 
| Kullanıcı eylemi | Ekleme @base64ToBinary göndermeden önce AS2Message için ortağına 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* MDN şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlenen hata: şifre çözme başarısız oldu] | 
| Kullanıcı eylemi | Ekleme @base64ToBinary göndermeden önce MDN için ortağına 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>* Eksik imzalama sertifikası

|   |   |  
|---|---|
| Hata açıklaması| İmzalama sertifikası AS2 taraf için yapılandırılmadı. </br> AS2-gelen: partner1 AS2-için: partner2 | 
| Kullanıcı eylemi | İmza için doğru sertifika ile AS2 sözleşmesi ayarlarını yapılandırma |
|  |  | 

## <a name="x12-and-edifact"></a>X12 ve EDIFACT

### <a name="-leading-or-trailing-space-found"></a>* Baştaki veya sondaki boşlukları bulundu    
    
|   |   | 
|---|---|
| Hata açıklaması | Ayrıştırma sırasında ile karşılaşıldı. EDIFACT işlem kümesi ' 123456 kimliği ile 'kimlikli ' 987654 (olmadan, Grup) değişim, gönderen Kimliği 'Partner1' içindeki', 'Partner2' alıcının kimliği şu hatalarla askıya: baştaki boşlukları ayırıcısı bulundu |
| Kullanıcı eylemi | Baştaki ve sondaki boşlukları izin verecek şekilde yapılandırılması sözleşmesi ayarlar. </br> Baştaki ve sondaki boşluk izin vermek üzere anlaşma ayarlarını Düzenle |
|   |   |

![alan izin ver](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-the-agreement"></a>* Yinelenen onay anlaşmasında etkinleştirilmiş

|   |   | 
|---|---| 
| Hata açıklaması | Yinelenen denetim sayısı |
| Kullanıcı eylemi | Bu hata, yinelenen denetim numaraları alınan ileti olduğunu gösterir. </br> Denetim numarayı düzeltin ve iletiyi yeniden gönderin |
|   |   |

### <a name="-missing-schema-in-the-agreement"></a>* Eksik şema Sözleşmesi

|   |   | 
|---|---| 
| Hata açıklaması | Ayrıştırma sırasında ile karşılaşıldı. Kimliğine sahip '564220001' bulunan işlevsel grubunda Kimliği '56422', '000056422' Gönderen Kimliği ' 12345678 kimlikli değişim içinde', alıcı kimliği ' 87654321' işlem kümesi şu hatalarla askıya X12 "iletisi bir bilinmeyen belge türüne sahip bir ND anlaşmasında yapılandırılmış mevcut şemaları hiçbirine çözümlenmedi" |
| Kullanıcı eylemi | Şema sözleşmesi yapılandırmak  |
|   |   |

### <a name="-incorrect-schema-in-the-agreement"></a>* Yanlış şema Sözleşmesi

|   |   | 
|---|---| 
| Hata açıklaması | İleti bir bilinmeyen belge türüne sahip ve herhangi bir anlaşmayı yapılandırılmış mevcut şemaları çözümlenmedi. |
| Kullanıcı eylemi | Doğru şemayı sözleşmesi yapılandırmak  |
|   |   |

## <a name="flat-file"></a>Düz dosya

### <a name="-input-message-with-no-body"></a>* Hiçbir gövde ile giriş iletisi

|   |   | 
|---|---|
| Hata açıklaması | InvalidTemplate. İşlem şablonu dili '1' satır ve sütun '1902' eylemi 'Flat_File_Decoding' girişleri ifadelerinde alınamıyor: ' özelliği 'content' bekler bir değer ancak taşınmadığını null gerekli. Yol ''.'. |
| Kullanıcı eylemi | Bu hata, giriş iletisi bir gövdesi içermiyor belirtir. |
|   |   | 

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)