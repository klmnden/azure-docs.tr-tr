---
title: Avrupalı müşteriler - Azure Active Directory için kimlik verileri depolama | Microsoft Docs
description: Azure Active Directory kimlik doğrulamayla ilgili veriler, Avrupalı müşteriler için depoladığı hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93ac5ef5f03f800a8f90259db3e382b3bc5c5e2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235157"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Kimlik Azure Active Directory'de Avrupalı müşteriler için veri depolama
Kimlik verileri Azure AD tarafından Office 365 ve Azure gibi bir Microsoft Online hizmetine abone olurken, kuruluşunuz tarafından sağlanan adresini temel alarak bir coğrafi konumda depolanır. Kimlik verilerinizi depolandığı hakkında daha fazla bilgi için kullandığınız [bulunan verileriniz nerede?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) Microsoft Trust Center bölümünü.

Avrupa'daki bir adresi sağlanan müşteriler, Azure AD kimlik verilerini Avrupa veri merkezleri içinde en tutar. Bu belge, Avrupa dışında Azure AD Hizmetleri tarafından depolanan tüm verileri hakkında bilgiler sağlar.

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure multi factor authentication (MFA)
    
- Telefon aramaları kullanarak tüm iki öğeli kimlik doğrulama veya SMS ABD veri merkezleri aracılığıyla kaynaklanan ve ayrıca genel sağlayıcıları tarafından yönlendirilir.
- Anında iletme bildirimleri uygulama kaynaklanan BİZDEN veri merkezleri için Microsoft Authenticator'ı kullanarak. Ayrıca, cihaz satıcısı belirli hizmetleri play ve hizmetlerin Avrupa dışında belki de gelebilir.
- OATH kodları her zaman ABD'de doğrulanır. 

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C'yi (Azure AD B2C)

Azure AD B2C ilkesi yapılandırma verileri ve anahtar kapsayıcılarının ABD veri merkezlerinde depolanır. Bu, tüm kullanıcı kişisel verilerini içermez. İlke yapılandırmaları hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C: Yerleşik ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) makalesi.

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Azure AD B2B depoları davet kullanma ile bağlantı ve URL bilgileri ABD veri merkezinde yönlendirme. Ayrıca, B2B davetleri aboneliğini iptal kullanıcıların e-posta adresi depolanır ABD veri merkezleri içinde.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Microsoft Azure Active Directory etki alanı Hizmetleri (Azure AD DS)

Azure AD DS, kullanıcı verilerini müşterinin seçtiği Azure Sanal Ağı ile aynı konumda depolar. Ağınız Avrupa dışındaysa veriler Avrupa dışında çoğaltılır ve depolanır.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Hizmetler ve Azure AD ile tümleştirilen uygulamalar kimlik verilerini erişiminiz. Her bir hizmet ve bu belirli hizmet ve uygulama tarafından kimlik veriler nasıl işlenir ve şirketinizin veri depolama gereksinimlerini karşıladıkları belirlemek için kullandığınız uygulama değerlendirin.

Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin [Verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü inceleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Özellikler ve İşlevler yukarıda açıklanan ilgili daha fazla bilgi için şu makalelere bakın:
- [Multi-Factor Authentication nedir?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)

- [Azure AD self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview)

- [Azure Active Directory B2C nedir?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)

- [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)

- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
