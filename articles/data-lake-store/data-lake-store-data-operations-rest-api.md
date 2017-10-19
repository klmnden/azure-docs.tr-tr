---
title: "REST API: Azure Data Lake Store'daki dosya sistemi işlemleri | Microsoft Docs"
description: "Data Lake Store üzerinde dosya sistemi işlemleri yapmak için WebHDFS REST API'lerini kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 62cfc713c3b7ce8f7c0a7ff4dd5e80802f1843b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-rest-api"></a>Azure Data Lake Store'da REST API'si kullanılarak gerçekleştirilen dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Bu makalede, Azure Data Lake Store üzerinde dosya sistemi işlemleri gerçekleştirmek üzere WebHDFS REST API'lerini ve Data Lake Store REST API'lerini nasıl kullanacağınızı öğreneceksiniz. Data Lake Store'da hesap yönetim işlemlerini REST API'si kullanarak gerçekleştirme talimatları için bkz. [Data Lake Store'da REST API'si kullanılarak gerçekleştirilen hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md).

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store hesabı**. [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın.

* **[cURL](http://curl.haxx.se/)**. Bu makalede, bir Data Lake Store hesabına yönelik olarak REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızın (etkileşimli) son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-rest-api.md).
* Uygulamanızın (etkileşimli olmayan) servisler arası kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-folders"></a>Klasör oluşturma
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komut, Data Lake Store hesabınızın kök klasörü altında **mytempdir** adlı bir dizin oluşturur.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

    {"boolean":true}

## <a name="list-folders"></a>Klasörleri listeleme
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "<GUID>",
            "group": "<GUID>"
        }]
    }
    }

## <a name="upload-data"></a>Karşıya veri yükleme
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

Yukarıdaki söz diziminde **-T** parametresi karşıya yüklediğiniz dosyanın konumudur.

Çıkış aşağıdaki kod parçacığına benzer:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data"></a>Verileri okuma
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Data Lake Store hesabından veri okuma, iki adımlı bir işlemdir.

* İlk olarak `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN` uç noktası için bir GET isteği gönderirsiniz. Bu çağrı, sonraki GET isteğini göndermek için bir konum döndürür.
* Ardından `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true` uç noktası için GET isteğini gönderirsiniz. Bu çağrı, dosyanın içeriğini görüntüler.

Ancak giriş parametrelerinde birinci ve ikinci adım arasında fark olmadığından, ilk isteği göndermek için `-L` parametresini kullanabilirsiniz. `-L` seçeneği, temelde iki isteği tek istekte birleştirir ve cURL'nin isteği yeni konumda yeniden gerçekleştirmesini sağlar. Son olarak, aşağıdaki kod parçacığında gösterildiği gibi, tüm istek çağrılarının çıktısı görüntülenir. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı yeniden adlandırmak için aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file"></a>Dosyayı silme
Bu işlem, [burada](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı silmek için aşağıdaki cURL komutunu kullanın. **\<yourstorename>** ifadesini, Data Lake Store adınızla değiştirin.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Aşağıdaki gibi bir çıktı görmeniz gerekir:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="next-steps"></a>Sonraki adımlar
* [REST API kullanılarak gerçekleştirilen Data Lake Store'daki hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store REST API Başvurusu](https://docs.microsoft.com/rest/api/datalakestore/)
* [Azure Data Lake Store ile uyumlu Açık Kaynak Büyük Veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)

