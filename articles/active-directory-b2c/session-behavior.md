---
title: Oturum davranışını - Azure Active Directory B2C'yi yapılandırma | Microsoft Docs
description: Azure Active Directory B2C'de oturum davranışını yapılandırın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7bfa34f44ca8ba53b89e4218303a7cd77cd0add9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700978"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de oturum davranışını yapılandırma

Bu özellik, hassas bir denetim üzerinde sağlar bir [kullanıcı akışı başına](active-directory-b2c-reference-policies.md), biri:

- Azure AD B2C tarafından yönetilen web uygulaması oturumları ömrü.
- Çoklu oturum açma (SSO) davranışı birden fazla uygulama ve kullanıcı Azure AD B2C kiracınızda akar.

Kullanıcı akışları parola sıfırlama için bu ayarlar kullanılamaz.

Azure AD B2C'yi destekleyen [Openıd Connect kimlik doğrulama protokolü](active-directory-b2c-reference-oidc.md) güvenli oturum açma web uygulamalarını etkinleştirmek için. Web uygulaması oturumları yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

## <a name="session-behavior-properties"></a>Oturum davranışı özellikleri

- **Web uygulaması oturumunun ömrü (dakika)** - başarılı kimlik doğrulamadan sonra kullanıcının tarayıcısında depolanan Azure AD B2C'in oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - (Sınırlar dahil) en az 15 dakika =.
    - (Sınırlar dahil) en fazla 1440 dakika =.
- **Web uygulaması oturumu zaman aşımı** - bu anahtarı ayarlanırsa **mutlak**, kullanıcı tarafından belirtilen zaman aralığı yeniden sonra kimlik doğrulaması için zorlanır **Web uygulaması oturumunun ömrü (dakika)** geçen. Bu anahtar ayarlanırsa **çalışırken** (varsayılan ayar), kullanıcı web uygulamanızı sürekli olarak etkin olduğu sürece, kullanıcının oturum açmış durumda kalır.
- **Çoklu oturum açma yapılandırması** B2C kiracınızda birden çok uygulama ve kullanıcı akışları varsa, Kullanıcı etkileşimlerine arasında yönetebileceğiniz kullanarak **çoklu oturum açma yapılandırması** özelliği. Özelliği aşağıdaki ayarlardan birini ayarlayabilirsiniz:
    - **Kiracı** -Bu ayar varsayılan ayardır. Bu ayar kullanılarak sağlayan birden çok uygulama ve kullanıcı akışları B2C kiracınızda aynı kullanıcı oturumuna paylaşmak için. Bir uygulamaya bir kullanıcı oturum açtıktan sonra Örneğin, kullanıcı aynı zamanda sorunsuz bir şekilde başka bir Contoso eriştiği üzerine ilaç, içine oturum açabilirsiniz.
    - **Uygulama** -diğer uygulamalar bağımsız bir uygulama için yalnızca bir kullanıcı oturumu korumak bu ayarı sağlar. Örneğin, Contoso ilaç için (aynı kimlik bilgileri ile), oturum açmak için kullanıcının kullanıcı zaten Contoso alışveriş imzalansa bile isterseniz, başka bir uygulama aynı B2C Kiracı. 
    - **İlke** -Bu ayar, bir kullanıcı oturumu şemayı kullanan uygulamaların bağımsız bir kullanıcı akışı için özel olarak korumak sağlar. Kullanıcı zaten açık ve çok faktörlü kimlik doğrulaması (MFA) adım tamamlandı, kullanıcı akışa bağlı oturumu süresi dolmadığı sürece Örneğin, kullanıcı erişim daha yüksek güvenlik için birden çok uygulama bölümlerini verilebilir.
    - **Devre dışı bırakılmış** -bu ayarı, ilkenin her yürütme tüm kullanıcı akışını çalıştırmak için kullanıcı zorlar.

Aşağıdaki kullanım örnekleri, bu özellikleri kullanarak etkinleştirilir:

- Uygun web uygulaması oturumu ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılamak yaşam süresi yok.
- Kimlik doğrulamasından sonra ayarlanmış bir süre içinde web uygulamanızı yüksek güvenlikli bir parçası olan bir kullanıcı etkileşimi zorlar. 

## <a name="configure-the-properties"></a>Özellikleri yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve Azure AD B2C kiracınızı içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kullanıcı akışları (ilke)**.
5. Daha önce oluşturduğunuz kullanıcı akışı açın. 
6. Seçin **özellikleri**.
7. Yapılandırma **Web uygulaması oturumunun ömrü (dakika)**, **Web uygulaması oturumu zaman aşımı**, **çoklu oturum açma yapılandırması**, ve **gerektiren kimlik belirteci oturum kapatma istekleri**  gerektiğinde.

    ![Oturum davranışını yapılandırma](./media/session-behavior/session-behavior.png)
    
8. **Kaydet**’e tıklayın.



