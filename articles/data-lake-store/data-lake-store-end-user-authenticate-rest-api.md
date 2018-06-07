---
title: "Son kullanıcı kimlik doğrulaması: Azure Active Directory'yi kullanarak REST API'si Data Lake Store ile | Microsoft Docs"
description: Son kullanıcı kimlik doğrulaması REST API kullanarak Azure Active Directory kullanarak Data Lake Store ile elde öğrenin
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 7b339c989a21abff34b885a8cba219aba701ca79
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34624259"
---
# <a name="end-user-authentication-with-data-lake-store-using-rest-api"></a>REST API kullanarak Data Lake Store ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake Store ile son kullanıcı kimlik doğrulaması yapmak için REST API kullanma hakkında bilgi edinin. REST API kullanarak Data Lake Store ile hizmet kimlik doğrulaması için bkz: [hizmeti için kimlik doğrulaması REST API kullanarak Data Lake Store ile](data-lake-store-service-to-service-authenticate-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. ' Ndaki adımları tamamlanmış gerekir [son kullanıcı kimlik doğrulaması Azure Active Directory kullanarak Data Lake Store ile](data-lake-store-end-user-authenticate-using-active-directory.md).

* **[cURL](http://curl.haxx.se/)**. Bu makalede, bir Data Lake Store hesabına yönelik olarak REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Azure AD kullanarak uygulamanıza oturum açmak için kullanıcının istiyorsanız, son kullanıcı kimlik doğrulaması için önerilen yaklaşımdır. Uygulamanız oturum açma kullanıcı aynı düzeyde erişim ile Azure kaynaklarına erişebilir. Kullanıcı kimlik bilgilerini düzenli olarak erişim, uygulamanız için sırayla sağlaması gerekir.

Son kullanıcı oturum açma sahip sonucunu uygulamanız bir erişim belirteci ve bir yenileme belirteci verilmesidir. Erişim belirteci Data Lake Store veya Data Lake Analytics yapılan her isteği bağlı ve varsayılan olarak bir saat için geçerli olduğundan. Düzenli olarak kullanılan iki haftaya varsayılan olarak, geçerli olduğundan ve yenileme belirtecini yeni bir erişim belirteci almak için kullanılabilir. Son kullanıcı oturum açma için iki farklı yaklaşım kullanabilirsiniz.

Bu senaryoda, uygulama kullanıcıdan oturum açmasını ister ve tüm işlemler, kullanıcı bağlamında gerçekleştirilir. Aşağıdaki adımları gerçekleştirin:

1. Uygulamanızı kullanarak kullanıcıyı şu URL'ye yönlendirin:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI>, bir URL içinde kullanılmak üzere kodlanmalıdır. Bu nedenle, için https://localhost, kullanın `https%3A%2F%2Flocalhost`)
   > 
   > 
   
    Bu öğreticinin amaçları doğrultusunda, yukarıdaki URL'deki yer tutucu değerlerini değiştirebilir ve bir web tarayıcısının adres çubuğuna yapıştırabilirsiniz. Azure oturum açma bilgilerinizi kullanarak kimlik doğrulaması gerçekleştirmeye yönlendirileceksiniz. Başarıyla oturum açtığınızda yanıt, tarayıcının adres çubuğunda görüntülenir. Yanıt şu biçimde olacaktır:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Yanıttaki yetki kodunu alın. Bu öğretici için yetkilendirme kodu postasında istekte belirteç uç noktası için geçişi ve web tarayıcısının Adres çubuğundan aşağıdaki kod parçacığında gösterildiği gibi kopyalayabilirsiniz:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > Bu durumda, \<REDIRECT-URI> öğesinin kodlanması gerekmez.
   > 
   > 

3. Yanıt, bir erişim belirteci içeren bir JSON nesnesidir (örneğin, `"access_token": "<ACCESS_TOKEN>"`) ve bir yenileme belirtecidir (örneğin, `"refresh_token": "<REFRESH_TOKEN>"`). Uygulamanız Azure Data Lake Store'a erişmek için erişim belirtecini ve bir erişim belirtecinin süresi olduğunda başka bir erişim belirteci almak için yenileme belirtecini kullanır.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4. Erişim belirtecinin süresi dolduğunda aşağıdaki kod parçacığında gösterildiği gibi yenileme belirtecini kullanarak yeni bir erişim belirteci isteyebilirsiniz:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Etkileşimli kullanıcı kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Yetki kodu izin akışı](https://msdn.microsoft.com/library/azure/dn645542.aspx).
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, REST API kullanarak Azure Data Lake Store ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz. Şimdi, Azure Data Lake Store ile çalışmak için REST API kullanma hakkında konuşun aşağıdaki makaleleri da bakabilirsiniz.

* [REST API kullanarak Data Lake Store üzerinde hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)
* [REST API kullanarak Data Lake Store üzerinde veri işlemleri](data-lake-store-data-operations-rest-api.md)

