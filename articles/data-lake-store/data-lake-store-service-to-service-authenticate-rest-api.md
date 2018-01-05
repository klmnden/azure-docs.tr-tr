---
title: "Hizmetten hizmete kimlik doğrulaması: Azure Active Directory'yi kullanarak REST API'si Data Lake Store ile | Microsoft Docs"
description: "Hizmetten hizmete kimlik doğrulaması REST API kullanarak Azure Active Directory kullanarak Data Lake Store ile elde öğrenin"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 754e65c4bcf8574a16b9620e2f21938ecc62b735
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-rest-api"></a>REST API kullanarak Data Lake Store ile hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’yı kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Data Lake Store ile hizmet kimlik doğrulaması yapmak için REST API kullanma hakkında bilgi edinin. REST API kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması için bkz: [son kullanıcı kimlik doğrulaması REST API kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Web" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [hizmeti için kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması
Bu senaryoda, uygulama işlemlerini gerçekleştirmek için kendi kimlik bilgilerini sağlar. Bunun için aşağıdaki kod parçacığında gösterilene benzer bir POST isteği yayımlamanız gerekir: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Bir yetkilendirme belirteci isteği çıktısını içerir (belirtilmiştir `access-token` çıkışı) daha sonra REST API çağrıları ile geçirdiğiniz. Kimlik Doğrulama belirtecini bir metin dosyasına kaydedin; Data Lake Store REST çağrılarını yaparken gerekir.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Bu makalede, **etkileşimli olmayan** yaklaşım kullanılmıştır. Etkileşimli olmayan seçeneği (hizmet-hizmet çağrıları) hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini kullanarak gerçekleştirilen hizmet-hizmet çağrıları](https://msdn.microsoft.com/library/azure/dn645543.aspx). 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, REST API kullanarak Azure Data Lake Store ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmak için REST API kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [REST API kullanarak Data Lake Store üzerinde hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)
* [REST API kullanarak Data Lake Store üzerinde veri işlemleri](data-lake-store-data-operations-rest-api.md)


