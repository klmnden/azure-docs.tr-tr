---
title: Arka plan programı uygulama arama web API'lerini (üretim taşıma) - Microsoft kimlik platformu
description: Daemon uygulamasının nasıl oluşturulacağını öğrenin çağrıları veritabanını web API'leri (üretim taşıma)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 627dab0cb23800664c5fb5b3df9c61f5071d4b87
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545396"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Web çağıran arka plan programı uygulama API'ler - üretim ortamına taşıyın

Almak ve bir hizmetten hizmete çağrı için bir belirteç kullanma öğrendiğinize göre uygulamanızı üretim aşamasına geçmeye nasıl bilgi edinebilirsiniz.

## <a name="deployment---case-of-multi-tenant-daemon-apps"></a>Dağıtım - çok kiracılı arka plan programları durumu

Birden fazla kiracıda çalışabilen bir arka plan programı uygulaması oluşturma, bir ISV iseniz emin olmak ihtiyacınız Kiracı yöneticileri:

- Uygulama için bir hizmet sorumlusu sağlar
- Uygulamaya izin verir

Bu işlemleri gerçekleştirmek nasıl müşterilere açıklamak gerekir. Daha fazla bilgi için bkz. [tamamını bir kiracı için onay isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için birkaç bağlantıları aşağıda verilmiştir:

### <a name="net"></a>.NET

- Zaten varsa, hızlı başlangıç kılavuzundan [bir belirteç almak ve Microsoft Graph API'sini çağırmak uygulamanın kimliğini kullanarak bir konsol uygulamasından](./quickstart-v2-netcore-daemon.md).
- İçin başvuru belgeleri:
  - Örnekleme [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)
  - Çağırma [AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)
- Diğer örnekler/öğreticiler:
  - [Microsoft-kimlik-platform-konsol-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) Microsoft Graph sorgulama Kiracı kullanıcıları görüntüler basit bir .NET Core arka plan programı konsol uygulaması sunar.

    ![topoloji](media/scenario-daemon-app/daemon-app-sample.svg)

    Aynı örnek sertifikalarla değişim de gösterir.

    ![topoloji](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-identity-Platform-ASPNET-WebApp-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) verileri Microsoft Graph kullanıcı adına yerine uygulama kimliğini kullanarak bir ASP.NET MVC web uygulaması özellikleri. Örnek ayrıca, yönetici onayı işlemi gösterilmektedir.

    ![topoloji](media/scenario-daemon-app/damon-app-sample-web.svg)

### <a name="python"></a>Python

MSAL Python şu anda genel Önizleme aşamasındadır. Daha fazla bilgi için bkz. [MSAL Python istemci kimlik bilgileri deposu örnek](https://github.com/AzureAD/azure-activedirectory-library-for-python/blob/dev/sample/client_credentials_sample.py).

### <a name="java"></a>Java

MSAL Python şu anda genel Önizleme aşamasındadır. Daha fazla bilgi için bkz. [MSAL Java depo örnekleri](https://github.com/AzureAD/azure-activedirectory-library-for-java/tree/dev/src/samples).