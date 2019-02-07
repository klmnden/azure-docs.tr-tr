---
title: Azure - FHIR için Azure API Hizmetleri FHIR hakkında sık sorulan sorular (SSS)
description: Bu SSS makalede FHIR Azure API hakkında sık sorulan soruların yanıtları
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 7403a23e236c14d77672d5b80d953b1e11088f8a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824258"
---
# <a name="frequently-asked-questions-about-azure-api-for-fhir"></a>FHIR için Azure API hakkında sık sorulan sorular

## <a name="storage-location"></a>Depolama konumu

**Verileri FHIR arkasında&reg; API'leri, Azure'da depolanan?** Evet, verileri Azure yönetilen veritabanlarında depolanır. Azure API FHIR için alttaki veri deposuna doğrudan erişim sağlamaz.

## <a name="identity-providers"></a>Kimlik sağlayıcıları

Şu anda Microsoft Azure Active Directory kimlik sağlayıcısı olarak destekliyoruz.

## <a name="supported-fhir-version"></a>Desteklenen FHIR sürümü

Şu anda 3.0.1 sürümü destekliyoruz. Bkz: [desteklenen özellikler](fhir-features-supported.md) Ayrıntılar için.

## <a name="oss-and-azure-api-for-fhir"></a>OSS ve Azure API FHIR için

Azure için açık kaynak Microsoft FHIR sunucu FHIR için Azure API arasındaki fark nedir? Azure API FHIR için OSS Microsoft FHIR sunucusunun Azure için barındırılan ve yönetilen bir sürümüdür. Yönetilen bir hizmette Microsoft tüm bakım, güncelleştirme, vb. sağlar. Azure OSS FHIR sunucusu çalıştırırken, temel alınan hizmet doğrudan erişimi sahip, ancak ayrıca PHI verileri depolamak, sunucuyu ve tüm gerekli uyumluluk iş güncelleştirme bakımı için sizin sorumluluğunuzdadır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure API hakkında sık sorulan sorular için FHIR bazıları makaleyi okudunuz. Azure için Microsoft FHIR Server'da desteklenen API özellikleri okuyun.
 
>[!div class="nextstepaction"]
>[Desteklenen FHIR özellikleri](fhir-features-supported.md)