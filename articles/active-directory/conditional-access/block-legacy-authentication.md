---
title: Koşullu erişim ile eski kimlik doğrulaması için Azure Active Directory (Azure AD) nasıl | Microsoft Docs
description: Azure AD koşullu erişim kullanarak eski bir kimlik doğrulama engelleyerek, güvenlik duruşunu öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.subservice: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/25/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d2841d3be584cae45ef49ad9ff20da8a232c366
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60355174"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Nasıl yapılır: Azure ad koşullu erişim bloğu eski kimlik doğrulaması   

Kullanıcılarınıza bulut uygulamalarınız için kolay erişim sunmak için Azure Active Directory (Azure AD) kimlik doğrulama protokolleri eski bir kimlik doğrulama dahil olmak üzere çok çeşitli destekler. Ancak, eski protokolleri, çok faktörlü kimlik doğrulaması (MFA) desteklemez. Mfa'yı birçok ortamlarda adresi kimlik hırsızlığı için ortak bir gereksinimdir. 


Ortamınızı kiracınızın korumasını geliştirmek için blok eski bir kimlik doğrulama için hazır ise, koşullu erişim ile bu hedefe gerçekleştirebilirsiniz. Bu makalede, söz konusu bloğu eski kimlik doğrulamasını kiracınız için koşullu erişim ilkelerini nasıl yapılandırabilirsiniz açıklanmaktadır.



## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar: 

- [Temel kavramları](overview.md) Azure AD koşullu erişim 
- [En iyi uygulamalar](best-practices.md) Azure portalında koşullu erişim ilkelerini yapılandırma



## <a name="scenario-description"></a>Senaryo açıklaması

Azure AD birkaç eski bir kimlik doğrulama dahil olmak üzere en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokolünü destekler. Eski kimlik doğrulaması temel kimlik doğrulaması kullanan protokolleri belirtir. Genellikle, bu protokolleri, herhangi bir türde ikinci faktörlü kimlik doğrulaması zorunlu kılamaz. Eski kimlik doğrulaması tabanlı uygulamaları verilebilir:

- Eski Microsoft Office uygulamaları

- SMTP POP ve IMAP gibi e-posta protokollerini kullanan uygulamalar

Tek faktörlü kimlik doğrulaması (örneğin, kullanıcı adı ve parola) yeterli bugünlerde değildir. Tahmin kolaydır ve size (insanlar) en iyi parolalar seçme hatalı parola hatalı. Parolalar, saldırıları ilaç kimlik avı ve parola gibi çeşitli de etkilenir. Parola tehditlere karşı koruma için yapabileceğiniz kolay şeylerden biri MFA uygulamaktır. Bir saldırganın bir kullanıcının parolasını elinde alır bile MFA ile parola tek başına başarıyla kimlik doğrulaması ve verilere erişmek yeterli değil.

Nasıl kiracınızın kaynaklara erişimini eski kimlik doğrulaması kullanan uygulamalar engelleyebilir miyim? Koşullu erişim ilkesi ile engellemek için önerilir. Gerekirse, yalnızca belirli kullanıcı ve belirli ağ konumlarını eski kimlik doğrulaması tabanlı uygulamaları kullanmasını sağlar.

Koşullu erişim ilkeleri, ilk-faktörlü kimlik doğrulaması tamamlandıktan sonra uygulanır. Bu nedenle, koşullu erişim, hizmet reddi (DoS) saldırıları gibi senaryolar için ilk satırı savunma olarak tasarlanmamıştır, ancak bu olaylar (örneğin oturum açma risk düzeyini, konum isteği ve benzeri) gelen sinyalleri erişimini belirlemek için kullanabilir.




## <a name="implementation"></a>Uygulama

Bu bölümde, eski bir kimlik doğrulama bloğu için bir koşullu erişim ilkesini yapılandırma açıklanmaktadır. 

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme 

Bir koşullu erişim ilkesi kaynaklarınıza erişmek için kullanılan istemci uygulamaları için bağlı bir koşul ayarlayabilirsiniz. Seçerek eski kimlik doğrulaması kullanan uygulamalar için kapsamını daraltmak istemci uygulamalar koşulunu sağlayan **diğer istemciler** için **mobil uygulamalar ve masaüstü istemciler**.

![Diğer istemciler](./media/block-legacy-authentication/01.png)

Bu uygulamalar için erişimi engellemek için seçmeniz gerekir **erişimi engelle**.

![Erişimi engelle](./media/block-legacy-authentication/02.png)


### <a name="select-users-and-cloud-apps"></a>Kullanıcıları seçin ve bulut uygulamaları

Kuruluşunuz için eski bir kimlik doğrulama engellemek istiyorsanız, bunu seçerek gerçekleştirebilirsiniz, büyük olasılıkla düşünün:

- Tüm kullanıcılar

- Tüm bulut uygulamaları

- Erişimi engelle
 

![Atamalar](./media/block-legacy-authentication/03.png)



Azure, bu yapılandırma ihlal ettiğinden ilkenizi böyle oluşturmanızı engeller bir güvenlik özelliği olan [en iyi uygulamalar](best-practices.md) için koşullu erişim ilkeleri.
 
![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/04.png)


Güvenlik özelliği gereklidir çünkü *tüm kullanıcılar ve tüm bulut uygulamaları* tüm kuruluşunuzu kiracınıza imzalama engelleyin olasılığına sahiptir. En az bir en iyi uygulama gereksinimi karşılamak için en az bir kullanıcı hariç tutmanız gerekir. Bir dizin rolüne de hariç.

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/05.png)


Bir kullanıcı, ilkeden hariç tutarak bu güvenlik özelliği karşılayabilecek. İdeal olarak, birkaç tanımlamalıdır [Acil Durum erişimi yönetici hesaplarını Azure AD'de](../users-groups-roles/directory-emergency-access.md) ve bunları, ilkenin dışında bırakılacak.
 

## <a name="policy-deployment"></a>İlke dağıtımı

İlkeniz üretime yerleştirmeden önce ilgileniriz:
 
- **Hizmet hesapları** -hizmet hesapları veya Konferans odası telefonlar gibi cihazları tarafından kullanılan kullanıcı hesapları tanımlayın. Bu hesaplar güçlü parolalar ve bunları eklemek için dışlanmış bir grup emin olun.
 
- **Oturum açma raporları** - oturum açma raporu gözden geçirin ve Ara **diğer istemci** trafiği. En iyi kullanımı belirleyin ve neden kullanımda olduğunu araştırın. Genellikle, trafiği, modern kimlik doğrulaması veya bazı üçüncü taraf posta uygulamaları kullanmayan eski Office istemcileri tarafından oluşturulur. Kullanım bu uygulamaları uzağa taşıdığınızda veya etkisi düşükse, kullanıcılar bu uygulamaları artık kullanamaz, kullanıcılarınıza bildirmeniz için bir plan yapın.
 
Daha fazla bilgi için [dağıtımı yeni bir ilke?](best-practices.md#how-should-you-deploy-a-new-policy).



## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Kullanarak erişimini engelleme **diğer istemciler** da temel kimlik doğrulaması kullanan Exchange Online PowerShell engeller

Bir ilke için yapılandırma **diğer istemciler** SPConnect gibi belirli istemcilerden gelen tüm kuruluş engeller. Bunun nedeni, eski istemciler, beklenmedik bir şekilde kimlik doğrulaması bu blok kullanmasıdır. Sorun, eski Office istemcileri gibi önemli Office uygulamaları için geçerli değildir.

Bu ilkenin yürürlüğe 24 saate kadar sürebilir.

Diğer istemciler koşulu için tüm kullanılabilir verme denetimleri seçebilirsiniz; Bununla birlikte, son kullanıcı deneyiminin her zaman - aynıdır erişimi engellenir.

Diğer istemciler koşulunu kullanarak eski bir kimlik doğrulama engellerseniz, cihaz platformu ve konum koşulu da ayarlayabilirsiniz. Örneğin, yalnızca mobil cihazlar için eski bir kimlik doğrulama engellemek istiyorsanız, ayarlama **cihaz platformlarını** seçerek koşul:

- Android

- iOS

- Windows Phone

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/06.png)




## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkelerini yapılandırma ile henüz bilmiyorsanız bkz [Azure Active Directory koşullu erişimiyle belirli uygulamalar için mfa'yı gerekli](app-based-mfa.md) örneği.

- Modern kimlik doğrulaması desteği hakkında daha fazla bilgi için bkz. [Office 2013 ve Office 2016 istemci uygulamaları için nasıl modern kimlik doğrulama çalışıyor](https://docs.microsoft.com/en-us/office365/enterprise/modern-auth-for-office-2013-and-2016) 
