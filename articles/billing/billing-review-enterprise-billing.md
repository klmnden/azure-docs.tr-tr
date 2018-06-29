---
title: REST API verilerle faturalama gözden geçirme Azure işletme kaydı | Microsoft Docs
description: Azure REST API'lerini Kurumsal kayıt faturalama bilgileri gözden geçirmek için nasıl kullanılacağını öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: 046b2e31aaefa5916a42b3652f9e6a8fdceff367
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064113"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>REST API'lerini kullanarak kurumsal kayıt faturalama gözden geçirin

Azure API Yardım raporlama gözden geçirmenizi ve Azure maliyetleriniz yönetin.

Burada, bir kurumsal hesap kaydıyla ilişkili geçerli fatura almak öğrenin.

Geçerli fatura almak için:
``` http
GET https://consumption.azure.com/v2/enrollments/{enrollmentID}/usagedetails
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Oluşturma isteği  

`{enrollmentID}` Parametresi gereklidir ve kurumsal hesap (EA) için kayıt kimliği içermelidir.

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir ayarla `Bearer` [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based). |  

Bu örnek, geçerli fatura dönemi için ayrıntılarını döndürür zaman uyumlu bir çağrı gösterir. Performans nedenleriyle bilgi geçen ay için zaman uyumlu çağrıları döndür.  Ayrıca, çağırabilirsiniz [API zaman uyumsuz olarak](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) 36 ay için verileri döndürmek için.


## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) hesabınız için ayrıntılı maliyetlerin bir listesini içeren başarılı bir yanıt döndürdü.

``` json
{
    "id": "${id}",
    "data": [
        {
            "cost": ${cost}, 
            "departmentId": ${departmentID},
            "subscriptionGuid" : ${subscriptionGuid} 
            "date": "${date}",
            "tags": "${tags}",
            "resourceGroup": "${resourceGroup}"
        } // ...
    ],
    "nextLink": "${nextLinkURL}"
}
```  

Her öğe **veri** bir ücret temsil eder:

|Yanıt özelliği|Açıklama|
|----------------|----------|
|**Maliyet** | Tutar veri merkezi konumu için uygun bir para birimi dolu. |
|**subscriptionGuid** | Abonelik için genel benzersiz kimliği. | 
|**departmentId** | Varsa bölüm kimliği. |
|**Tarih** | Tarih ücret faturalandırılır. |
|**etiketler** | Abonelikle ilişkili etiketleri içeren JSON dizesi. |
|**resourceGroup**|Maliyet ücrete nesnesini içeren kaynak grubunun adı. |
|**nextLink**| Ne zaman ayarlama, sonraki "sayfasının" Ayrıntıları URL'sini belirtir. Sayfa sonuncu olduğunda boş. |  
||
  
Bölüm kimlikleri, kaynak grupları, etiketler ve ilgili alanları EA yönetici tarafından tanımlanır.  

Bu örnek kadar kısaltılmıştır; bkz: [kullanım ayrıntılarını almak](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) her yanıtı alanının eksiksiz bir açıklaması. 

Diğer durum kodları hata koşulları belirtin. Bu durumlarda, isteğin neden başarısız yanıt nesnesi açıklanmaktadır.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Sonraki adımlar 
- Gözden geçirme [Kurumsal raporlamaya genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Araştırmak [REST API faturalama Enterprise](https://docs.microsoft.com/rest/api/billing/)   
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
