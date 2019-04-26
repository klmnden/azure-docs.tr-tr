---
title: 'Hizmetten hizmete kimlik doğrulaması: REST API kullanarak Azure Active Directory ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması REST API kullanarak Azure Active Directory kullanarak elde öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c48f7d7608b2b70f4ae41e2af5792cff72bb0dd2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60195791"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması REST API'sini kullanma
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’yı kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

Bu makalede, Azure Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için REST API kullanma hakkında bilgi edinin. REST API kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1 bkz [REST API kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Active Directory "Web" uygulaması oluşturma**. Adımları tamamlamış olmanız gerekir [hizmetten hizmete kimlik doğrulaması Azure Active Directory kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması
Bu senaryoda, uygulama işlemleri gerçekleştirmek için kendi kimlik bilgilerini sağlar. Bunun için aşağıdaki kod parçacığında gösterilene benzer bir POST isteği yayımlamanız gerekir: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

İstek çıktısını bir yetkilendirme belirteci içerir (çağrılarınızla `access-token` aşağıdaki çıktıda) REST API çağrılarınıza sonradan geçirin. Kimlik Doğrulama belirtecini bir metin dosyasına kaydedin; Data Lake depolama Gen1 REST çağrı yaparken gerekir.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Bu makalede, **etkileşimli olmayan** yaklaşım kullanılmıştır. Etkileşimli olmayan seçeneği (hizmet-hizmet çağrıları) hakkında daha fazla bilgi için bkz. [Kimlik bilgilerini kullanarak gerçekleştirilen hizmet-hizmet çağrıları](https://msdn.microsoft.com/library/azure/dn645543.aspx). 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Data Lake depolama Gen1 ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz REST API kullanarak. Data Lake depolama Gen1 ile çalışmak için REST API kullanma hakkında konuşmak Aşağıdaki makaleler artık göz atabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini REST API'sini kullanma](data-lake-store-get-started-rest-api.md)
* [Data Lake depolama Gen1 REST API kullanarak veri işlemleri](data-lake-store-data-operations-rest-api.md)

