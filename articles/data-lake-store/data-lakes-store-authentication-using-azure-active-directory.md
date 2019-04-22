---
title: Azure Data Lake depolama Gen1 Azure Active Directory'yi kullanarak kimlik doğrulaması | Microsoft Docs
description: Azure Data Lake depolama Gen1 ile kimlik doğrulaması yapmayı öğrenin Azure Active Directory'yi kullanarak
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
ms.openlocfilehash: f83cf183bee930dd07c707b0eb49125cecd70b84
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58884644"
---
# <a name="authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kimlik doğrulaması ile Azure Data Lake depolama Gen1

Azure Data Lake depolama Gen1 Azure Active Directory kimlik doğrulaması için kullanır. Data Lake depolama Gen1 ile çalışan bir uygulama geliştirme önce uygulamanızın Azure Active Directory (Azure AD) ile kimlik doğrulaması nasıl karar vermeniz gerekir.

## <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri

* **Son kullanıcı kimlik doğrulaması** -son kullanıcının Azure kimlik bilgileri ile Data Lake depolama Gen1 kimliğini doğrulamak için kullanılır. Data Lake depolama Gen1 ile çalışmak için oluşturduğunuz uygulama, bu kullanıcı kimlik bilgilerini ister. Sonuç olarak, bu kimlik doğrulama mekanizmasıdır *etkileşimli* ve uygulama oturum açmış kullanıcının bağlamında çalışır. Daha fazla bilgi ve yönergeler için bkz. [son kullanıcı kimlik doğrulaması için Data Lake depolama Gen1](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Hizmetten hizmete kimlik doğrulaması** -Data Lake depolama Gen1 ile kendi kimliğini doğrulamak için bir uygulama istiyorsanız bu seçeneği kullanın. Böyle durumlarda, bir Azure Active Directory (AD) uygulama oluşturmak ve Data Lake depolama Gen1 ile kimlik doğrulaması için Azure AD uygulaması anahtarı kullanın. Sonuç olarak, bu kimlik doğrulama mekanizmasıdır *etkileşimli olmayan*. Daha fazla bilgi ve yönergeler için bkz. [hizmetten hizmete kimlik doğrulaması için Data Lake depolama Gen1](data-lake-store-service-to-service-authenticate-using-active-directory.md).

Aşağıdaki tabloda, son kullanıcı ve hizmetten hizmete kimlik doğrulama mekanizmaları için Data Lake depolama Gen1 nasıl desteklenen gösterilmektedir. İşte, tabloyu nasıl olduğunu okuyun.

* Kimlik doğrulama seçeneği desteklenir ve bağlantılar ✔ * simgesi gösterir, kimlik doğrulama seçeneği nasıl yapılacağı açıklanır makale için. 
* ✔ Sembol, kimlik doğrulama seçeneği desteklenip desteklenmediğini gösterir. 
* Boş hücreleri kimlik doğrulama seçeneği desteklenmediğini gösterir.


|Bu kimlik doğrulama seçeneği kullan...                   |.NET         |Java     |PowerShell |Azure CLI | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Son kullanıcı (olmadan MFA **)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(kullanım dışı)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Son kullanıcı (MFA ile)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Hizmetler (istemci anahtarını kullanarak)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Hizmetler (istemci sertifikası kullanarak) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* Tıklayın <b>✔\* </b> simgesi. Bu bir bağlantıdır.</i><br>
<i>** MFA için multi-Factor authentication anlamına gelir.</i>

Bkz: [Azure Active Directory için kimlik doğrulama senaryoları](../active-directory/develop/authentication-scenarios.md) Azure Active Directory kimlik doğrulaması için kullanma hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)


