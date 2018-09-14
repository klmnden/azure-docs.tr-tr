---
title: "REST API: Azure Data Lake Store'daki hesap yönetimi işlemleri | Microsoft Docs"
description: Data Lake Store'da hesap yönetim işlemlerini gerçekleştirmek için Azure Data Lake Store ve WebHDFS REST API'sini kullanma
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 1b8b680736c49c0036600bf4337da6952cf38afb
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573648"
---
# <a name="account-management-operations-on-azure-data-lake-store-using-rest-api"></a>REST API kullanılarak gerçekleştirilen Azure Data Lake Store'daki hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede REST API kullanarak Data Lake Store'daki hesap yönetim işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Hesap yönetim işlemleri Data Lake Store hesabı oluşturma, Data Lake Store hesabı silme gibi işlemleri kapsar. Data Lake Store'da dosya sistemi işlemlerini REST API kullanarak gerçekleştirme talimatları için bkz. [Data Lake Store'da REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](http://curl.haxx.se/)**. Bu makalede, bir Data Lake Store hesabına yönelik olarak REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızın (etkileşimli) son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-rest-api.md).
* Uygulamanızın (etkileşimli olmayan) servisler arası kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/create) tanımlanan REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Yukarıdaki komutta; \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **input.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği aşağıdaki kod parçacığına benzer:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-store-account"></a>Data Lake Store hesabını silme
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete) tanımlanan REST API çağrısını temel alır.

Bir Data Lake Store hesabını silmek için aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Aşağıdaki kod parçacığı gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store REST API Başvurusu](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Data Lake Store ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

