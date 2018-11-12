---
title: Azure dijital İkizlerini API kimlik doğrulaması anlama | Microsoft Docs
description: Azure dijital İkizlerini bağlanıp API'leri için kimlik doğrulaması için kullanma
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: f85ab05e785ea559962490b43e75b196d1602159
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016225"
---
# <a name="connect-and-authenticate-to-apis"></a>Bağlanıp API'lerine kimlikleri

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır. Azure AD, çeşitli modern mimarileri için kimlik doğrulamasını destekler. Bunların tümünde, OAuth 2.0 veya Openıd Connect endüstri standardı protokollerine dayalıdır. Ayrıca, geliştiriciler, Azure AD'ye tek kiracılı ve satır iş kolu (LOB) uygulamaları oluşturmak için kullanabilirsiniz. Geliştiriciler, Azure AD çok kiracılı uygulamalar geliştirmek için de kullanabilirsiniz.

Azure AD genel bakış için ziyaret [temelleri sayfa](https://docs.microsoft.com/azure/active-directory/fundamentals/index) için adım adım kılavuzlar, kavramlar ve hızlı başlangıçları.

Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir. Ayrıntılı yönergeler ve ekran görüntüleri için bkz. [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

[Beş birincil uygulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) Azure AD tarafından desteklenir:

* Tek sayfalı uygulama (SPA): bir kullanıcının Azure AD tarafından güvenliği sağlanan bir tek sayfalı uygulama için oturum açmanız gerekir.
* Web uygulamasına Web tarayıcısı: bir kullanıcı Azure AD tarafından güvenliği sağlanan bir web uygulamasına oturum açması gerekiyor.
* Web API'si yerel uygulamaya: telefon, tablet veya PC çalıştıran yerel bir uygulama, Azure AD tarafından güvenliği sağlanan bir web API'sini kaynakları almak için bir kullanıcının kimliğini doğrulaması gerektiğinde.
* Web uygulaması web API'si için: bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir web uygulaması gerekir.
* Web API arka plan programı ya da sunucu uygulamasında: bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir arka plan programı uygulaması veya web kullanıcı Arabirimi ile bir sunucu uygulaması gerekir.

Windows Azure kimlik doğrulama kitaplığı, Active Directory belirteçlerini almak için birçok yol sunar. Kitaplık ve kod örnekleri hakkında daha fazla bilgi için bkz: [bu makalede](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Dijital İkizlerini bir orta katman web API'si çağırma

Geliştiriciler dijital İkizlerini çözümleri tasarlama, bunlar genellikle bir orta katman uygulama veya API oluşturun. Daha sonra dijital İkizlerini API uygulaması veya API aşağı yönde çağırır. Kullanıcılar, Orta katmanda uygulamaya ilk kimlik doğrulaması ve ardından bir on-behalf-of belirteci akış aşağı akış çağırmak için kullanılır. On-behalf-of akışı düzenlemek nasıl hakkında yönergeler için bkz: [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Kod örnekleri üzerinde görüntüleyebilirsiniz [bu sayfayı](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Postman istemciyle test etme

Dijital İkizlerini API'lerini kullanmaya başlamak için Postman gibi bir istemci bir API ortam kullanabilirsiniz. Postman, karmaşık HTTP isteklerine hızlı bir şekilde oluşturmanıza yardımcı olur. Aşağıdaki adımlarda, dijital İkizlerini Postman UI içinde çağırmak için gerekli olan Azure AD belirteçlerini almak gösterilmektedir.


1. Git https://www.getpostman.com/ uygulamayı indirmek için.
1. Bağlantısındaki [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) bir Azure AD uygulaması oluşturmak için. Veya mevcut bir kayıt yeniden kullanabilirsiniz. 
1. Altında **gerekli izinler**, "Azure dijital İkizlerini" girin ve seçin **Temsilcili izinler**. Ardından **izinler**.
1. Uygulama bildirimi açın ve ayarlayın **oauth2AllowImplicitFlow** true.
1. Bir yanıt URL'si için yapılandırma [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Seçin **yetkilendirme** sekmesinde **OAuth 2.0**ve ardından **yeni erişim belirteci Al**.

    |**Alan**  |**Değer** |
    |---------|---------|
    | İzin verme türü | Örtük |
    | Geri çağırma URL'si | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Kimlik doğrulama URL'si | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/Authorize?Resource=0b07f429-9F4B-4714-9392-cc5e8e80c8b0 |
    | İstemci Kimliği | Uygulama Kimliği oluşturulmuş veya adım 2'den başka bir amaçla kullanılması Azure AD uygulamasını kullanın. |
    | Kapsam | Boş bırakın. |
    | Durum | Boş bırakın. |
    | İstemci kimlik doğrulaması | Temel kimlik doğrulama üst bilgi olarak gönderin. |

1. Seçin **belirteç iste**.

    >[!NOTE]
    >"2 OAuth tamamlanamadı" hata iletisini alırsanız, aşağıdakileri deneyin:
    > * Postman, kapatın ve yeniden açın ve yeniden deneyin.
   
1. Aşağı kaydırın ve select **kullanım belirteci**.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
