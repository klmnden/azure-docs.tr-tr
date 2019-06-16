---
title: 'Son kullanıcı kimlik doğrulaması: REST API kullanarak Azure Active Directory ile Azure Data Lake depolama Gen1 | Microsoft Docs'
description: REST API kullanarak Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Azure Data Lake depolama Gen1 elde öğrenin
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
ms.openlocfilehash: 0ef65c23ee1bf4f064695779b71c8616427da204
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60877831"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>Son kullanıcı kimlik doğrulaması REST API kullanarak Azure Data Lake depolama Gen1 ile
> [!div class="op_single_selector"]
> * [Java kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’yı kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API’sini kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Data Lake depolama Gen1 son kullanıcı kimlik doğrulaması yapmak için REST API kullanma hakkında bilgi edinin. REST API kullanarak hizmetten hizmete kimlik doğrulaması ile Data Lake depolama Gen1 bkz [Data Lake depolama Gen1 ile hizmetten hizmete kimlik doğrulaması REST API kullanarak](data-lake-store-service-to-service-authenticate-rest-api.md).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Active Directory "Yerel" uygulama oluşturma**. Adımları tamamlamış olmanız gerekir [Azure Active Directory kullanarak son kullanıcı kimlik doğrulaması ile Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

* **[cURL](https://curl.haxx.se/)** . Bu makalede, bir Data Lake depolama Gen1 hesabına yönelik REST API çağrılarının nasıl yapılacağını göstermek üzere cURL kullanılmıştır.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Uygulamanızı Azure AD kullanarak oturum açmak için kullanıcının istiyorsanız, son kullanıcı kimlik doğrulaması için önerilen yaklaşımdır. Uygulamanızı Azure kaynaklarıyla aynı erişim düzeyini oturum açmış kullanıcı olarak erişebilir. Kullanıcının kimlik bilgilerini düzenli olarak uygulamanızın erişimi sürdürmek sırada olmalıdır.

Son kullanıcı oturum açma olması sonucunu, uygulamanızın bir erişim belirteci ve yenileme belirteci verilmesidir. Data Lake depolama Gen1 veya Data Lake Analytics yapılan her bir isteğe bağlı erişim belirteci ve varsayılan olarak bir saat için geçerli olduğundan. Düzenli olarak kullandıysanız varsayılan olarak, iki haftaya kadar geçerli olduğundan ve yenileme belirtecinin yeni bir erişim belirteci almak için kullanılır. Son kullanıcı oturum açma için iki farklı yaklaşım kullanabilirsiniz.

Bu senaryoda, uygulama kullanıcıdan oturum açmasını ister ve tüm işlemler, kullanıcı bağlamında gerçekleştirilir. Aşağıdaki adımları gerçekleştirin:

1. Uygulamanızı kullanarak kullanıcıyı şu URL'ye yönlendirin:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

   > [!NOTE]
   > \<REDIRECT-URI>, bir URL içinde kullanılmak üzere kodlanmalıdır. İçin https://localhost , kullanın `https%3A%2F%2Flocalhost` )

    Bu öğreticinin amaçları doğrultusunda, yukarıdaki URL'deki yer tutucu değerlerini değiştirebilir ve bir web tarayıcısının adres çubuğuna yapıştırabilirsiniz. Azure oturum açma bilgilerinizi kullanarak kimlik doğrulaması gerçekleştirmeye yönlendirileceksiniz. Başarıyla oturum açtığınızda yanıt, tarayıcının adres çubuğunda görüntülenir. Yanıt şu biçimde olacaktır:

        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Yanıttaki yetki kodunu alın. Bu öğreticide, web tarayıcısı ve onu gönderisinde talep ve belirteç uç noktasına geçişi Adres çubuğundan, aşağıdaki kod parçacığında gösterildiği gibi yetkilendirme kodu kopyalayabilirsiniz:

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

3. Bir erişim belirteci içeren bir JSON nesnesi yanıttır (örneğin, `"access_token": "<ACCESS_TOKEN>"`) ve bir yenileme belirteci (örneğin, `"refresh_token": "<REFRESH_TOKEN>"`). Uygulamanız bir erişim belirtecinin süresi olduğunda başka bir erişim belirteci almak için Azure Data Lake depolama Gen1 ve yenileme belirteci erişirken erişim belirtecini kullanır.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4. Erişim belirtecinin süresi dolduğunda, aşağıdaki kod parçacığında gösterildiği gibi yenileme belirtecini kullanarak yeni bir erişim belirteci isteyebilirsiniz:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Etkileşimli kullanıcı kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Yetki kodu izin akışı](https://msdn.microsoft.com/library/azure/dn645542.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Lake depolama Gen1 ile kimlik doğrulaması için hizmetten hizmete kimlik doğrulaması kullanmayı öğrendiniz REST API kullanarak. Şimdi, Azure Data Lake depolama Gen1 ile çalışmak için REST API kullanma hakkında konuşmak aşağıdaki makaleleri da bakabilirsiniz.

* [Data Lake depolama Gen1 hesap yönetim işlemlerini REST API'sini kullanma](data-lake-store-get-started-rest-api.md)
* [Data Lake depolama Gen1 REST API kullanarak veri işlemleri](data-lake-store-data-operations-rest-api.md)

