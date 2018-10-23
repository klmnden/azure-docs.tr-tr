---
title: Azure dijital İkizlerini API kimlik anlama | Microsoft Docs
description: Azure dijital İkizlerini kullanarak bağlanıp API'lerine kimlikleri
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: dc5570b188bfdc0e1be78aa2bd5c5d92e884f377
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638027"
---
# <a name="connect-and-authenticate-to-apis"></a>Bağlanıp API'lerine kimlikleri

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır. Azure AD kimlik doğrulaması için modern mimarileri çeşitli destekler, bunların tümünün OAuth 2.0 veya Openıd Connect endüstri standardı protokoller üzerinde tabanlı. Ayrıca, Azure AD, hem tek kiracılı, satır iş kolu (LOB) uygulamaları, aynı zamanda çok kiracılı uygulamalar geliştirmek isteyen geliştiricilerin oluşturmasını sağlar.

Azure AD ziyaret genel bir bakış için [temelleri sayfa](https://docs.microsoft.com/azure/active-directory/fundamentals/index) adım adım kılavuzlar, kavramlar ve hızlı başlangıçlar için.

Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir. Ayrıntılı yönergeler ve ekran görüntüleri yönergeleri görüntülemek için [burada](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)

Bunlar [beş birincil uygulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) Azure AD tarafından desteklenen:

* Tek sayfalı uygulama (SPA): bir kullanıcının Azure AD tarafından güvenliği sağlanan bir tek sayfalı uygulama için oturum açmanız gerekir.
* Web uygulamasına Web tarayıcısı: bir kullanıcı Azure AD tarafından güvenliği sağlanan bir web uygulamasına oturum açması gerekiyor.
* Web API'si yerel uygulamaya: telefon, tablet veya PC çalıştıran yerel bir uygulama bir Web API'si Azure AD tarafından güvenli hale getirilen kaynakları almak için bir kullanıcının kimliğini doğrulaması gerektiğinde.
* Web uygulaması web API'si için: bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir web uygulaması gerekir.
* Web API arka plan programı ya da sunucu uygulamasında: bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir arka plan programı uygulaması veya web kullanıcı arabirimi ile bir sunucu uygulaması gerekir.

Windows Azure kimlik doğrulama kitaplığı, Active Directory belirteçlerini almak için birçok yol sunar. Örnekler için derinlemesine kitaplığı yanı sıra kod uygulamasına göz atın [burada](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="calling-digital-twins-from-a-middle-tier-web-api"></a>Dijital İkizlerini bir orta katman web API'si çağırma

Dijital İkizlerini çözümleri geliştiriciler genellikle üreten bir orta katman uygulama veya dijital İkizlerini API'ı aşağı yönde çağıran API oluşturmak seçin. Kullanıcılar için Orta katmanda uygulama ilk kez kimlik doğrulamasının ve aşağı akış çağrılırken bir üzerinde-behalf-of belirteç akışı ardından kullanılacaktır. On-behalf-of akışı düzenlemek nasıl hakkında ayrıntılı yönergeler ziyaret [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Kod örnekleri de görüntüleyebilirsiniz [burada](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Postman istemciyle test etme

Dijital İkizlerini API'lerini kullanmaya başlamak için Postman gibi bir istemci bir API ortam kullanabilirsiniz. Postman, karmaşık HTTP isteklerine hızlı bir şekilde oluşturmanıza yardımcı olur. Aşağıdaki yönergeler, dijital İkizlerini Postman UI içinde çağırmak için gereken bir Azure AD belirteç edinme hakkında odaklanır.


1. Gidin https://www.getpostman.com/ uygulamayı indirmek için
1. Adımları [burada](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) bir Azure Active Directory uygulaması oluşturmak için (veya mevcut bir kayıt yeniden kullanmayı da seçebilirsiniz). 
1. "Azure dijital İkizlerini" altında gerekli izinleri Ekle ve temsilci izinleri seçin. İzinler sonlandırmak için tıklamayı unutmayın.
1. Uygulama bildirimini açın ve oauth2AllowImplicitFlow true olarak ayarlayın.
1. Bir yanıt URL'si için yapılandırma [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Seçin **yetkilendirme sekmesini**, tıklayarak **OAuth 2.0**seçip **yeni erişim belirteci Al**.

    |**Alan**  |**Değer** |
    |---------|---------|
    | İzin Verme Türü | Örtük |
    | Geri çağırma URL'si | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Kimlik doğrulama URL'si | [https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0](https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0)
    | İstemci Kimliği | Uygulama kimliği için oluşturulmuş veya başka bir amaçla kullanılması adım 1'den Azure AD uygulamasını kullanın |
    | Kapsam | Boş bırakın |
    | Durum | Boş bırakın |
    | İstemci Kimlik Doğrulaması | Temel kimlik doğrulama üst bilgisi gönder |

1. Tıklayın **belirteç iste**.

    >[!NOTE]
    >"2 OAuth tamamlanamadı" hata iletisi alırsanız, aşağıdakileri deneyin:
    > * Postman'ı kapatın ve yeniden açın ve yeniden deneyin.
   
1. Aşağı kaydırıp tıklayın **kullanım belirteci**.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
