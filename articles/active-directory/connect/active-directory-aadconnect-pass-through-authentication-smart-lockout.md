---
title: "Azure AD Connect: Doğrudan kimlik doğrulaması - akıllı kilitleme | Microsoft Docs"
description: "Bu makalede, şirket içi hesaplarınızı Azure Active Directory (Azure AD) doğrudan kimlik doğrulama bulutta yanılma parola saldırılarından nasıl koruduğunu açıklanmaktadır."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 771741fd7da8c9b6932851851aaca148f9596643
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Azure Active Directory doğrudan kimlik doğrulaması: Akıllı kilitleme

## <a name="overview"></a>Genel Bakış

Azure AD yanılma parola saldırılarına karşı korur ve Office 365 ve SaaS uygulamalarını dışında kilitli orijinal kullanıcıların engeller. Adlı bu özelliği, **akıllı kilitleme**, oturum açma yönteminiz olarak doğrudan kimlik doğrulaması kullandığınızda desteklenir. Akıllı kilitleme tüm kiracılar için varsayılan olarak etkindir ve her zaman kullanıcı hesaplarınızı koruyorsanız; etkinleştirmek için gerek yoktur.

Başarısız oturum açma girişimleri izlemek tarafından ve belirli bir sonra Akıllı kilitleme çalışır **kilitleme eşiği**başlayan bir **kilitleme süresi**. Bir saldırgan kilitleme süresi sırasında oturum açma denemeleri reddedilir. Saldırı devam ederse, sonraki başarısız oturum açma uzun kilitleme süreleri sonucunda kilitleme süresi sona erdikten sonra çalışır.

>[!NOTE]
>Kilitleme eşiği 10 başarısız oturum açma girişimlerinin varsayılandır ve kilitleme süresi varsayılan değer 60 saniyedir.

Akıllı kilitleme, oturum açma işlemleri orijinal kullanıcılar ve saldırganların ve çoğu durumda saldırganlar çıkışı yalnızca kilitleri arasında ayırır. Bu işlevsellik, orijinal kullanıcıları kötü amaçlı olarak kilitleme saldırganlar engeller. Oturum açma davranışı, kullanıcıların aygıtlarını & tarayıcılar ve diğer sinyaller orijinal kullanıcılar saldırganlar arasında ayrım yapmak için kullanırız. Biz sürekli bizim algoritmaları geliştirme.

Parola doğrulama isteklerini, şirket içi Active Directory (AD) üzerine doğrudan kimlik doğrulama ileten olduğundan, kullanıcılarınızın AD hesapları kilitleme saldırganların önlemek gerekir. Kendi AD hesap kilitleme ilkeleri olduğundan (özellikle [ **hesap kilitleme eşiği** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) ve [ **hesap kilitleme sayaç sonra sıfırlamailkeleri** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), Azure AD kilitleme eşiği ve kilitleme süresi değerleri uygun şekilde saldırıları bulutta şirket içi düşmeden önce filtrelemek için yapılandırmanız gereken AD.

>[!NOTE]
>Akıllı kilitleme özelliğini ücretsiz ve _üzerinde_ tüm müşteriler için varsayılan olarak. Ancak, Azure AD kilitleme eşiği ve grafik API'sini kullanarak kilitleme süresi değerleri değiştirme kiracınız en az bir Azure AD Premium P2 lisansına sahip olması gerekir. Bir Azure AD Premium P2 lisansı gerekmez _kullanıcı başına_ doğrudan kimlik doğrulaması ile akıllı kilitleme özelliğini almak için.

Kullanıcılarınızın emin olmak için şirket içi AD hesapları iyi korumalı, emin olmanız gerekir:

1.  Azure AD kilitleme eşiği _daha az_ Reklamın hesap kilitleme eşiği değerinden. AD'ın hesap kilitleme eşiği en az iki ya da Azure AD kilitleme eşiği üç kez şekilde değerleri ayarlamanızı öneririz.
2.  Azure AD kilitleme süresi (saniye cinsinden gösterilir) _uzun_ Reklamın sıfırlama hesap kilitleme sayaç (dakika cinsinden gösterilir) sonra daha.

>[!IMPORTANT]
>Şu anda Akıllı kilitleme yeteneği bunlar kilitlenen, yönetici kullanıcıların bulut hesaplarının kilidini açamazsınız. Kilitleme süresi dolmak üzere beklemeniz gerekecektir.

## <a name="verify-your-ad-account-lockout-policies"></a>AD hesap kilitleme ilkeleri doğrulayın

AD hesap kilitleme ilkeleri doğrulamak için aşağıdaki yönergeleri kullanın:

1.  Grup İlkesi yönetim aracını açın.
2.  Tüm kullanıcılar, örneğin, varsayılan etki alanı ilkesi uygulanan Grup İlkesi düzenleyin.
3.  Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Güvenlik Ayarları\Hesap İlkeleri\Hesap kilitleme ilkesi için gidin.
4.  Hesap kilitleme eşiği ve hesap kilitleme sayaç sonra sıfırlama değerleri doğrulayın.

![AD ve hesap kilitleme ilkeleri](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-needs-premium-license"></a>Kiracı'nın akıllı kilitleme değerleri yönetme (Premium lisansı gerekir) grafik API'sini kullanın

>[!IMPORTANT]
>Azure AD kilitleme eşiği ve grafik API'sini kullanarak kilitleme süresi değerleri değiştirerek bir Azure AD Premium P2 özelliğidir. Ayrıca, kiracınızın genel yönetici olmanız gerekir.

Kullanabileceğiniz [Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) okuyun, ayarlayın ve Azure AD akıllı kilitleme değerleri güncelleştirmek için. Ancak, bu işlemlerin program aracılığıyla da yapabilirsiniz.

### <a name="read-smart-lockout-values"></a>Okuma akıllı kilitleme değerleri

Kiracı'nın akıllı kilitleme değerleri okumak için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. "Değiştirme izinleri"'i tıklatın ve "Directory.ReadWrite.All" izni seçin.
3. Grafik API'si istek gibi yapılandırma: kümesi versiyonunu "BETA", istek türü için "GET" ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kiracı'nın akıllı kilitleme değerleri görmek için "Sorgu Çalıştır"'i tıklatın. Önce kiracının değerleri ayarlamadıysanız boş bakın.

### <a name="set-smart-lockout-values"></a>Akıllı kilitleme değerlerini Ayarla

(İlk kez yalnızca), kiracının akıllı kilitleme değerlerini ayarlamak için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. "Değiştirme izinleri"'i tıklatın ve "Directory.ReadWrite.All" izni seçin.
3. Grafik API'si istek gibi yapılandırma: sürüm "BETA" olarak ayarlamak, istek türü "POST" ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kopyalayın ve aşağıdaki JSON isteği "İstek gövdesi" alanına yapıştırın.
5. Kiracı'nın akıllı kilitleme değerlerini ayarlamak için "Sorgu Çalıştır"'i tıklatın.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Bunları kullanmıyorsanız, bırakabilirsiniz **BannedPasswordList** ve **EnableBannedPasswordCheck** değerler boş olarak ("") ve "false" sırasıyla.

Doğru kullanarak, kiracının akıllı kilitleme değerleri ayarladığınızdan emin olun [adımları](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Akıllı kilitleme değerleri güncelleştirin

(Önceden daha önce ayarlamış olmanız durumunda), kiracının akıllı kilitleme değerleri güncelleştirmek için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. "Değiştirme izinleri"'i tıklatın ve "Directory.ReadWrite.All" izni seçin.
3. [Bu adımları, kiracının akıllı kilitleme değerleri okumak için](#read-smart-lockout-values). Kopya `id` "PasswordRuleSettings" olarak "görünen adı" öğesinin değeri (GUID).
4. Grafik API'si istek gibi yapılandırma: kümesi versiyonunu "BETA", istek türü için "Düzeltme Eki" ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -adım 3 için GUID kullanmak `<id>`.
5. Kopyalayın ve aşağıdaki JSON isteği "İstek gövdesi" alanına yapıştırın. Akıllı kilitleme değerleri uygun şekilde değiştirin.
6. Kiracı'nın akıllı kilitleme değerleri güncelleştirmek için "Sorgu Çalıştır"'i tıklatın.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Doğru kullanarak, kiracının akıllı kilitleme değerleri güncelleştirilmiş doğrulamak [adımları](#read-smart-lockout-values).

## <a name="next-steps"></a>Sonraki adımlar
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
