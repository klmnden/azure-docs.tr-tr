---
title: "Azure AD Connect: Doğrudan kimlik doğrulaması - akıllı kilitleme | Microsoft Docs"
description: "Şirket içi hesaplarınızı Azure Active Directory (Azure AD) doğrudan kimlik doğrulama bulutta yanılma parola saldırılarından nasıl koruduğunu bu makalede"
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: billmath
ms.openlocfilehash: fc46fe1d68538757ba5a8c5aa1acb4b51f8a171b
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Azure Active Directory doğrudan kimlik doğrulaması: Akıllı kilitleme

## <a name="overview"></a>Genel Bakış

Azure Active Directory (Azure AD) yanılma parola saldırılarına karşı korur ve Office 365 ve SaaS uygulamalarını dışında kilitli orijinal kullanıcıların engeller. Adlı bu özelliği, *akıllı kilitleme*, oturum açma yönteminiz olarak doğrudan kimlik doğrulaması kullandığınızda desteklenir. Akıllı kilitleme tüm kiracılar için varsayılan olarak etkindir ve kullanıcı hesaplarınızı sürekli olarak korur.

Akıllı kilitleme, başarısız oturum açma denemeleri izler. Belirli bir sonra *kilitleme eşiği*, başladığı bir *kilitleme süresi*. Akıllı kilitleme kilitleme süresi sırasında kullanıcılardan oturum girişimleri reddeder. Saldırı devam ederse, sonraki başarısız oturum açma kilitleme süresi sonucu uzun kilitleme süreleri sona erdikten sonra çalışır.

>[!NOTE]
>Varsayılan kilitleme eşiği 10 başarısız denemedir. Varsayılan kilitleme süresi 60 saniyedir.

Akıllı kilitleme, oturum açma işlemleri orijinal kullanıcıların oturum açmalardan saldırganlar arasındaki ayırır. Çoğu durumda, yalnızca saldırganlar kilitler. Bu işlevsellik, orijinal kullanıcıları kötü amaçlı olarak kilitleme saldırganlar engeller. Oturum açma davranışı, kullanıcıların aygıtlarını ve tarayıcılar akıllı kilitleme kullanır ve orijinal kullanıcılar saldırganlar arasında ayrım yapmak diğer işaret eder. Algoritmalar sürekli geliştirildi.

Yani, kullanıcılarınızın Active Directory hesapları kilitleme saldırganlar engellemeniz gerekiyor geçişli kimlik doğrulaması parola doğrulama isteklerini şirket içi Active Directory'ye iletir. Active Directory sahip kendi hesap kilitleme ilkeleri, özellikle [hesap kilitleme eşiği](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) ve [sıfırlama hesap kilitleme sayacını](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx) ilkeleri. Azure AD kilitleme eşiği ve kilitleme süresi değerleri saldırıları bulutta şirket içi Active Directory düşmeden önce filtrelemek için uygun şekilde yapılandırın.

>[!NOTE]
>>Akıllı kilitleme özelliğini ücretsiz ve _üzerinde_ tüm müşteriler için varsayılan olarak. Ancak, Azure AD kilitleme eşiği ve grafik API'sini kullanarak kilitleme süresi değerleri değiştirme kiracınız için Azure AD Premium P2 etkinleştirilmesi gerekir. 

Kullanıcılarınızın şirket içi Active Directory hesapları da korunduğundan emin olmak için emin olmanız gerekir:

   * Azure AD kilitleme eşiği _daha az_ Active Directory hesap kilitleme eşiği değerinden. Böylece Active Directory hesap kilitleme eşiği en az iki veya üç kez Azure AD kilitleme eşikten daha uzun değerlerini ayarlayın.
   * Azure AD kilitleme süresi (saniye cinsinden gösterilir) _uzun_ Active Directory (dakika cinsinden gösterilir) süreden sonra Hesap kilidi sayacını sıfırla daha.

>[!IMPORTANT]
>Şu anda Akıllı kilitleme yeteneği bunlar kilitlenen, yönetici kullanıcıların bulut hesaplarının kilidini açamazsınız. Yönetici kilitleme süresi dolmak üzere beklemeniz gerekir.

## <a name="verify-your-active-directory-account-lockout-policies"></a>Active Directory hesap kilitleme ilkeleri doğrulayın

Active Directory hesap kilitleme ilkeleri doğrulamak için aşağıdaki yönergeleri kullanın:

1.  Grup İlkesi yönetim aracını açın.
2.  Örneğin, tüm kullanıcılara uygulanan Grup İlkesi düzenleme **varsayılan etki alanı ilkesi**.
3.  Gözat **Bilgisayar Yapılandırması** > **ilkeleri** > **Windows ayarları** > **güvenlik ayarları**   >  **Hesap ilkeleri** > **hesap kilitleme ilkesi**.
4.  Doğrulayın, **hesap kilitleme eşiği** ve **sıfırlama hesap kilitleme sayacını** değerleri.

![Active Directory hesap kilitleme ilkeleri](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-requires-a-premium-license"></a>Kiracı'nın akıllı kilitleme değerleri yönetme (Premium lisansı gerekir) grafik API'sini kullanın

>[!IMPORTANT]
>Grafik API'sini kullanarak Azure AD kilitleme eşiği ve kilitleme süresi değerleri değiştirerek bir Azure AD Premium P2 özelliğidir. Ayrıca, kiracınızın genel yönetici olmanız gerekir.

Kullanabileceğiniz [Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) okuyun, ayarlayın ve Azure AD akıllı kilitleme değerleri güncelleştirmek için. Bu işlemler program aracılığıyla da yapabilirsiniz.

### <a name="view-smart-lockout-values"></a>Akıllı kilitleme değerlerini görüntüleme

Kiracı'nın akıllı kilitleme değerlerini görüntülemek için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. Seçin **değiştirme izinleri**ve ardından **Directory.ReadWrite.All** izni.
3. Grafik API'si istek gibi yapılandırma: sürüm kümesine **BETA**, istek türü **almak**ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Seçin **sorgu çalıştırma** , kiracının akıllı kilitleme değerleri görmek için. Önce kiracının değerleri ayarlamadıysanız boş bakın.

### <a name="set-smart-lockout-values"></a>Akıllı kilitleme değerlerini Ayarla

(Yalnızca ilk kez gereken), kiracının akıllı kilitleme değerlerini ayarlamak için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. Seçin **değiştirme izinleri**ve ardından **Directory.ReadWrite.All** izni.
3. Grafik API'si istek gibi yapılandırma: sürüm kümesine **BETA**, istek türü **POST**ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kopyalama ve yapıştırma içine aşağıdaki JSON istek **istek gövdesi** alan.
5. Seçin **sorgu çalıştırma** , kiracının akıllı kilitleme değerlerini ayarlamak için.

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
>Bunları kullanmıyorsanız, bırakabilirsiniz **BannedPasswordList** ve **EnableBannedPasswordCheck** değerler boş olarak (**""**) ve **yanlış** sırasıyla.

Size, kiracının akıllı kilitleme değerleri doğru adımları kullanarak ayarladığınızı doğrulayın [görünüm akıllı kilitleme değerleri](#view-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Akıllı kilitleme değerleri güncelleştirin

(Daha önce ayarlarsanız), kiracının akıllı kilitleme değerleri güncelleştirmek için aşağıdaki adımları izleyin:

1. Graph Explorer'a kiracınızın genel yönetici olarak oturum açın. İstenirse, istenen izinler için erişim verin.
2. Seçin **değiştirme izinleri**ve ardından **Directory.ReadWrite.All** izni.
3. [Kiracı'nın akıllı kilitleme değerlerini görüntülemek için bu adımları](#view-smart-lockout-values). Kopya `id` öğeyle (GUID) değerini **displayName** olarak **PasswordRuleSettings**.
4. Grafik API'si istek gibi yapılandırma: sürüm kümesine **BETA**, istek türü **düzeltme eki**ve URL `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>`. 3. adımındaki için GUID kullanmak `<id>`.
5. Kopyalama ve yapıştırma içine aşağıdaki JSON istek **istek gövdesi** alan. Akıllı kilitleme değerleri uygun şekilde değiştirin.
6. Seçin **sorgu çalıştırma** , kiracının akıllı kilitleme değerleri güncelleştirmek için.

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

Kiracı'nın akıllı kilitleme değerleri doğru adımları kullanarak güncelleştirdikten olduğunu doğrulayın [görünüm akıllı kilitleme değerleri](#view-smart-lockout-values).

## <a name="next-steps"></a>Sonraki adımlar
[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.
