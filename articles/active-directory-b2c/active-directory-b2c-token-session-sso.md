---
title: Oturum ve çoklu oturum açma yapılandırması - Azure Active Directory B2C | Microsoft Docs
description: Oturum ve Azure Active Directory B2C, çoklu oturum açma yapılandırması.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 674a20fc96cf5b86219222d746525a3559ae9d09
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681106"
---
# <a name="session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Oturum ve Azure Active Directory B2C, çoklu oturum açma yapılandırması

Bu özellik, hassas bir denetim üzerinde sağlar bir [kullanıcı akışı başına](active-directory-b2c-reference-policies.md), biri:

- Azure AD B2C tarafından yönetilen web uygulaması oturumları ömrü.
- Çoklu oturum açma (SSO) davranışı birden fazla uygulama ve kullanıcı Azure AD B2C kiracınızda akar.

## <a name="session-behavior"></a>Oturum davranışı

Azure AD B2C'yi destekleyen [Openıd Connect kimlik doğrulama protokolü](active-directory-b2c-reference-oidc.md) güvenli oturum açma web uygulamalarını etkinleştirmek için. Web uygulaması oturumları yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumunun ömrü (dakika)** - başarılı kimlik doğrulamadan sonra kullanıcının tarayıcısında depolanan Azure AD B2C'in oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - (Sınırlar dahil) en az 15 dakika =.
    - (Sınırlar dahil) en fazla 1440 dakika =.
- **Web uygulaması oturumu zaman aşımı** - bu anahtarı ayarlanırsa **mutlak**, kullanıcı tarafından belirtilen zaman aralığı yeniden sonra kimlik doğrulaması için zorlanır **Web uygulaması oturumunun ömrü (dakika)** geçen. Bu anahtar ayarlanırsa **çalışırken** (varsayılan ayar), kullanıcı web uygulamanızı sürekli olarak etkin olduğu sürece, kullanıcının oturum açmış durumda kalır.

Aşağıdaki kullanım örnekleri, bu özellikleri kullanarak etkinleştirilir:

- Uygun web uygulaması oturumu ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılamak yaşam süresi yok.
- Kimlik doğrulamasından sonra ayarlanmış bir süre içinde web uygulamanızı yüksek güvenlikli bir parçası olan bir kullanıcı etkileşimi zorlar. 

Kullanıcı akışları parola sıfırlama için bu ayarlar kullanılamaz.

## <a name="single-sign-on-sso-configuration"></a>Çoklu oturum açma (SSO) yapılandırması

B2C kiracınızda birden çok uygulama ve kullanıcı akışları varsa, Kullanıcı etkileşimlerine arasında yönetebileceğiniz kullanarak **çoklu oturum açma yapılandırması** özelliği. Özelliği aşağıdaki ayarlardan birini ayarlayabilirsiniz:

- **Kiracı** -Bu ayar varsayılan ayardır. Bu ayar kullanılarak sağlayan birden çok uygulama ve kullanıcı akışları B2C kiracınızda aynı kullanıcı oturumuna paylaşmak için. Bir uygulamaya bir kullanıcı oturum açtıktan sonra Örneğin, kullanıcı aynı zamanda sorunsuz bir şekilde başka bir Contoso eriştiği üzerine ilaç, içine oturum açabilirsiniz.
- **Uygulama** -diğer uygulamalar bağımsız bir uygulama için yalnızca bir kullanıcı oturumu korumak bu ayarı sağlar. Örneğin, Contoso ilaç için (aynı kimlik bilgileri ile), oturum açmak için kullanıcının kullanıcı zaten Contoso alışveriş imzalansa bile isterseniz, başka bir uygulama aynı B2C Kiracı. 
- **İlke** -Bu ayar, bir kullanıcı oturumu şemayı kullanan uygulamaların bağımsız bir kullanıcı akışı için özel olarak korumak sağlar. Kullanıcı zaten açık ve çok faktörlü kimlik doğrulaması (MFA) adım tamamlandı, kullanıcı akışa bağlı oturumu süresi dolmadığı sürece Örneğin, kullanıcı erişim daha yüksek güvenlik için birden çok uygulama bölümlerini verilebilir.
- **Devre dışı bırakılmış** -bu ayarı, ilkenin her yürütme tüm kullanıcı akışını çalıştırmak için kullanıcı zorlar.

Kullanıcı akışları parola sıfırlama için bu ayarlar kullanılamaz. 

