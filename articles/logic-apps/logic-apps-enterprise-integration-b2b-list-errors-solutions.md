---
title: Hatalar ve çözümleri B2B senaryoları - Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps B2B senaryoları için hatalar ve çözümleri bulun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: f0591b47ce7ba6837f300088c856c0098fb66710
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60998837"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>B2B hatalar ve çözümleri için Azure Logic Apps

Bu makalede Logic Apps B2B senaryolarda oluşabilir ve bu hataları düzeltmek için uygun eylemleri öneren hata gidermenize yardımcı olur.

## <a name="agreement-resolution"></a>Sözleşme çözümleme

### <a name="no-agreement-found"></a>Sözleşme bulunamadı 

|   |   |  
|---|---|
| Hata açıklaması | Sözleşme Sözleşme çözümleme parametrelerle bulunamadı. | 
| Bir kullanıcı eylemi | Üzerinde anlaşılan iş kimlikleri ile tümleştirme hesabı sözleşmesi eklenmesi gerekir. </br>İş kimlikleri giriş iletisi kimlikleri eşleşmesi gerekir. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Sözleşme ile kimlikleri bulunamadı

|   |   | 
|---|---|
| Hata açıklaması | Sözleşme ile kimlikleri bulunamadı: 'AS2Identity':: 'Partner1' ve 'AS2Identity':: 'Partner3' | 
| Bir kullanıcı eylemi | Geçersiz AS2-gelen veya AS2-anlaşma için yapılandırılmış. </br>AS2 iletisinin düzeltin "AS2-gelen" veya "AS2-için" üstbilgileri veya anlaşma yapılandırmalarla AS2 iletisi üst bilgilerinde AS2 kimlikleri eşleştirmek için sözleşme. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>AS2 ileti üstbilgileri eksik  

|   |   |  
|---|---|
| Hata açıklaması | Geçersiz AS2 üstbilgileri. Biri "AS2-için" veya "AS2-gelen" üstbilgileri boştur. | 
| Bir kullanıcı eylemi | AS2 içermeyen bir AS2 iletisi alındı-gelen veya AS2-için veya her iki üstbilgi. </br> AS2 iletisinin AS2 denetleyin-gelen ve AS2-üst bilgiler ve doğru bunları sözleşmesi yapılandırmasına göre. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>AS2 ileti gövdesi ve üst bilgileri eksik    

|   |   |  
|---|---|
| Hata açıklaması | İstek içeriği null veya boş. | 
| Bir kullanıcı eylemi | İleti gövdesi içermeyen bir AS2 iletisi alındı. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>AS2 ileti şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlenen hata: şifre çözme başarısız] | 
| Bir kullanıcı eylemi | Ekleme @base64ToBinary göndermeden önce AS2Message için iş ortağı için. |
|||

Örneğin:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>MDN şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlenen hata: şifre çözme başarısız] | 
| Bir kullanıcı eylemi | Ekleme @base64ToBinary göndermeden önce MDN için iş ortağı için. | 
|||

Örneğin:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>İmzalama sertifikası eksik

|   |   |  
|---|---|
| Hata açıklaması| AS2 tarafı için imzalama sertifikası yapılandırılmadı. </br>AS2-öğesinden: partner1 AS2-için: partner2 | 
| Bir kullanıcı eylemi | AS2 anlaşması ayarları doğru imza sertifikası ile yapılandırın. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 ve EDIFACT

### <a name="leading-or-trailing-space-found"></a>Başta veya sonda boşluk bulundu    
    
|   |   | 
|---|---|
| Hata açıklaması | Ayrıştırma sırasında hatayla karşılaşıldı. EDIFACT işlem Kimliğine sahip ' 123456'987654 ' kimlikli değişimin (Grup içermeyen), 'Partner1' gönderen Kimliğine sahip ' kümesinin, 'Partner2' alıcı Kimliğine aşağıdaki hatalarla askıya alınıyor: <p>"Önde gelen sondaki ayırıcı bulundu" |
| Bir kullanıcı eylemi | Baştaki ve sondaki boşlukları izin verecek şekilde yapılandırılması için anlaşma ayarlar. </br>Başında ve sonunda boşluk izin vermek için anlaşma ayarlarını düzenleyin. |
|   |   |

![alan izin ver](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Yinelenen onay anlaşmasında etkinleştirdi.

|   |   | 
|---|---| 
| Hata açıklaması | Yinelenen Denetim Numarası |
| Bir kullanıcı eylemi | Bu hata, alınan ileti yinelenen denetim numaralarına sahip olduğunu gösterir. </br>Denetim numarası düzeltin ve iletisini yeniden gönder. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Sözleşme'deki eksik şema

|   |   | 
|---|---| 
| Hata açıklaması | Ayrıştırma sırasında hatayla karşılaşıldı. X12 Kimliğine sahip '564220001' yer alan '56422', '000056422' gönderen kimliğine ' 12345678 kimlikli Değişimdeki', kimlikli işlevsel grubun 87654321 ' alıcı Kimliğine' işlem kümesi aşağıdaki hatalarla askıya: <p>"İleti bir belge türü bilinmiyor ve sözleşmede yapılandırılan mevcut şemalardan hiçbirine çözümlenmedi" |
| Bir kullanıcı eylemi | Şema sözleşmesi ayarları yapılandırın.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Yanlış şema Sözleşmesi

|   |   | 
|---|---| 
| Hata açıklaması | İletinin belge türü bilinmiyor ve ileti, sözleşmede yapılandırılan mevcut şemalardan hiçbirine çözümlenmedi. |
| Bir kullanıcı eylemi | Doğru şemayı sözleşmesi ayarları yapılandırın. |
|   |   |

## <a name="flat-file"></a>Düz dosya

### <a name="input-message-with-no-body"></a>Hiçbir gövdesi olan giriş iletisi

|   |   | 
|---|---|
| Hata açıklaması | InvalidTemplate. Eylem 'Flat_File_Decoding' şablon dili ifadeleri işlenemiyor, '1', sütun '1902' satırında giriş: ' Gerekli 'content' özelliği, bir değer alındı null bekliyor. Yol ''.'. |
| Bir kullanıcı eylemi | Giriş iletisi gövdesi içermiyor bu hata gösterir. |
|   |   | 

