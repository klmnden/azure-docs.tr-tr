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
ms.openlocfilehash: 1d5b1869428cec6bf80b8518485f685e38ad5997
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324314"
---
# <a name="connect-and-authenticate-to-apis"></a>Bağlanıp API'lerine kimlikleri

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır.

Azure AD ile bilmiyorsanız, daha fazla bilgi kullanılabilir [Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-developers-guide). Windows Azure kimlik doğrulama kitaplığı, Active Directory belirteçlerini almak için birçok yol sunar. Derinlemesine için kitaplığa, göz [burada](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

Bu makalede iki senaryo genel bakışını verir: Orta katman API ve Hızlı Başlangıç'ı ve test etmek için Postman istemci uygulamasında kimlik doğrulaması ile bir üretim senaryosu.

## <a name="authentication-in-production"></a>Üretimde kimlik doğrulaması

Çözümleri geliştiriciler için dijital İkizlerini bağlanmak için iki yolu vardır.  Çözümleri geliştiriciler Azure dijital çiftleri için aşağıdaki yollarla bağlanabilirsiniz:

* İstemci uygulamanız ya da bir orta katman API oluşturabilirler. Kullanıcıların kimliğini doğrulamak ve ardından istemci uygulamalarını gerektiren [OAuth 2.0 On-Behalf-Of](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) güvenlik akış aşağı akış API'sini çağırmak için.
* Oluşturun veya mevcut bir Azure AD uygulamasının yararlanması. Belgeleri görüntüleyin [burada](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad).
    1. Belirtin **oturum açma ve yeniden yönlendirme URI'leri** (gerekirse).
    1. Uygulamada bildirim kümesi `oauth2AllowImplicitFlow` true.
    1. İçinde **gerekli izinler**, "Azure dijital İkizlerini" arayarak dijital İkizlerini Ekle Seçin **temsilci izinleri okuma/yazma erişimi** tıklatıp **izinler** düğmesi.

On-behalf-of akışı düzenlemek nasıl hakkında ayrıntılı yönergeler ziyaret [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Kod örnekleri de görüntüleyebilirsiniz [burada](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="test-with-the-postman-client"></a>Postman istemciyle test etme

1. Bir Azure Active Directory uygulaması oluşturma (veya değiştirmek için) Yukarıdaki ilk adımları izleyin. Ardından, `oauth2AllowImplicitFlow` uygulama bildiriminde true ve "Azure dijital çiftleri için." izinleri vermek için
1. Yanıt URL'si kümesine [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
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

    >[!TIP]
    >"2 OAuth tamamlanamadı" hata iletisi alırsanız, aşağıdakilerden birini deneyin:
    > * Postman'ı kapatın ve yeniden açın ve yeniden deneyin.
    > * Uygulamanızda gizli anahtar silin, yeni bir tane ve yeniden girin yukarıdaki form değeri yeniden oluşturun.

1. Aşağı kaydırıp tıklayın **kullanım belirteci**.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
