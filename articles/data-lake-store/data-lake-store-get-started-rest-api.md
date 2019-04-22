---
title: 'REST API: Hesap yönetimi işlemleri Azure Data Lake depolama Gen1 | Microsoft Docs'
description: Data Lake depolama Gen1 hesabında hesap yönetim işlemlerini gerçekleştirmek için Azure Data Lake depolama Gen1 ve WebHDFS REST API'sini kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 97fe33309f36cd7545f8c9d6c2d34671641caa1f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880177"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>Azure Data Lake depolama Gen1 hesap yönetim işlemlerini REST API'sini kullanma
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, Azure Data Lake depolama Gen1 hesap yönetim işlemlerini REST API kullanarak gerçekleştirmeyi öğrenin. Silme, vb. bir Data Lake depolama Gen1 hesabı bir Data Lake depolama Gen1 hesabı oluşturma, hesap yönetim işlemlerini içerir. Data Lake depolama Gen1 REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri gerçekleştirmek yönergeler için bkz: [REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri Data Lake depolama Gen1](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](https://curl.haxx.se/)**. Bu makalede, bir Data Lake depolama Gen1 hesabına yönelik REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızın (etkileşimli) son kullanıcı kimlik doğrulaması için bkz. [son kullanıcı kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-end-user-authenticate-rest-api.md).
* Uygulamanızın (etkileşimli olmayan) hizmetten hizmete kimlik doğrulaması için bkz [hizmetten hizmete kimlik doğrulaması .NET SDK kullanarak Data Lake depolama Gen1 ile](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı oluşturun
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/create) tanımlanan REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. Değiştirin  **\<yourstoragegen1name >** Data Lake depolama Gen1 adınızla.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Yukarıdaki komutta; \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komuta yönelik istek yükü, yukarıdaki `-d` parametresi için sağlanan **input.json** dosyasına dahildir. Söz konusu input.json dosyasının içeriği aşağıdaki kod parçacığına benzer:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabını Sil
Bu işlem, [burada](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete) tanımlanan REST API çağrısını temel alır.

Bir Data Lake depolama Gen1 hesabını silmek için aşağıdaki cURL komutunu kullanın. Değiştirin  **\<yourstoragegen1name >** , Data Lake depolama Gen1 hesap adına sahip.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview

Aşağıdaki kod parçacığı gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Sonraki adımlar
* [REST API kullanılarak gerçekleştirilen dosya sistemi işlemleri Data Lake depolama Gen1](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 REST API Başvurusu](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Data Lake depolama Gen1 ile uyumlu açık kaynak büyük veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

