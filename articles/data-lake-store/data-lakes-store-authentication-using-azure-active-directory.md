---
title: Azure Active Directory kullanarak Data Lake Store kimlik doğrulaması | Microsoft Docs
description: Azure Active Directory'yi kullanarak Data Lake Store ile kimlik doğrulaması öğrenin
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
ms.openlocfilehash: bee65fbdc65807ac33ae425ed9d87dbf0c246d9d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625296"
---
# <a name="authentication-with-data-lake-store-using-azure-active-directory"></a>Azure Active Directory kullanarak Data Lake Store ile kimlik doğrulaması

Azure Data Lake Store, Azure Active Directory kimlik doğrulaması için kullanır. Azure Data Lake Store ile çalışan bir uygulama geliştirme önce uygulamanızı Azure Active Directory (Azure AD) ile kimlik doğrulaması yapmayı karar vermeniz gerekir.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

* **Son kullanıcı kimlik doğrulaması** -Data Lake Store ile kimlik doğrulaması için bir son kullanıcının Azure kimlik bilgileri kullanılır. Data Lake Store ile çalışmak için oluşturduğunuz uygulama bu kullanıcı kimlik bilgilerini ister. Sonuç olarak, bu kimlik doğrulama mekanizmasıdır *etkileşimli* ve uygulamanın oturum açan kullanıcının bağlamında çalışır. Daha fazla bilgi ve yönergeler için bkz: [Data Lake Store için son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Hizmetten hizmete kimlik doğrulaması** -Data Lake Store ile kendi kimliğini doğrulamak için bir uygulama istiyorsanız bu seçeneği kullanın. Böyle durumlarda, Azure Active Directory (AD) uygulama oluşturma ve Data Lake Store ile kimlik doğrulaması için Azure AD uygulaması yer alan anahtarı kullanın. Sonuç olarak, bu kimlik doğrulama mekanizmasıdır *etkileşimli olmayan*. Daha fazla bilgi ve yönergeler için bkz: [Data Lake Store için hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md).

Aşağıdaki tabloda, son kullanıcı ve hizmet kimlik doğrulama mekanizmaları için Data Lake Store nasıl desteklenen gösterilmektedir. İşte nasıl tablo okuyun.

* Kimlik doğrulama seçeneği desteklenir ve bağlantıları ✔ * simgesi gösterir, kimlik doğrulama seçeneğinin nasıl kullanılacağını gösteren bir makale için. 
* ✔ simgenin kimlik doğrulaması seçeneği desteklendiğini gösterir. 
* Boş hücrelerin kimlik doğrulaması seçeneği desteklenmiyor gösterir.


|Bu kimlik doğrulama seçeneği ile kullan...                   |.NET         |Java     |PowerShell |CLI 2.0 | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Son kullanıcı (olmadan MFA **)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(kullanım dışı)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Son kullanıcı (MFA ile)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Service-to-service (istemci anahtarını kullanarak)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Service-to-service (istemci sertifikası kullanarak) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* Tıklatın <b>✔\* </b> simgesi. Bu bir bağlantıdır.</i><br>
<i>** Çok faktörlü kimlik doğrulamasını MFA anlamına gelir</i>

Bkz: [için Azure Active Directory kimlik doğrulama senaryoları](../active-directory/develop/active-directory-authentication-scenarios.md) Azure Active Directory kimlik doğrulaması için kullanılacak hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)


