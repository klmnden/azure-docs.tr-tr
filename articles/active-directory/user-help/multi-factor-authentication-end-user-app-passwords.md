---
title: Uygulama parolaları Azure MFA kullanmaya nasıl? | Microsoft Docs
description: Bu sayfa, kullanıcıların uygulama parolaları nedir ve ne ile kullanıldıkları Azure mfa'yı dikkate anlamasına yardımcı olur.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: lizross
ms.custom: end-user
ms.openlocfilehash: 290458e95aaed0cc85d83539d9d870c334df45df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059434"
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication, uygulama parolaları nedir?
Çok faktörlü kimlik doğrulaması kullanan Exchange Active Sync, Apple yerel e-posta istemcisi gibi belirli tarayıcı olmayan uygulamalar şu anda desteklemez. Çok faktörlü kimlik doğrulaması, kullanıcı başına etkinleştirilir. Bu bir kullanıcı için multi-Factor authentication etkinleştirildi ve tarayıcı olmayan uygulamaları kullanmaya çalışıyorsunuz, bunlar bunu yapamıyor anlamına gelir. Bir uygulama parolası bunun olabilmesini sağlar. Koşullu erişim ilkeleri ve değil kullanıcı başına MFA aracılığıyla çok faktörlü kimlik doğrulaması zorunlu kılarsanız, uygulama parolaları oluşturamazsınız. Erişimi denetlemek için koşullu erişim ilkeleri kullanan uygulamalar, uygulama parolaları gerekmez.

Bir uygulama parolası aldıktan sonra bunu yerine bu tarayıcı olmayan uygulamaları ile özgün parolanızı kullanın. İki aşamalı doğrulama için kaydolduğunuzda, herhangi bir kişinin ikinci doğrulama de gerçekleştiremezsiniz parolanızla oturum bulunmamasını Microsoft'a belirten olmasıdır. İki aşamalı doğrulama için istenemez çünkü telefonunuzda Apple yerel e-posta istemcisi oturum açamazsınız. Bu günlük, ancak yalnızca iki aşamalı doğrulama desteği bu uygulamalar için kullanmadığınız daha güvenli bir uygulama parolası oluşturmak için çözümüdür. Uygulamalar, çok faktörlü kimlik doğrulamasını atlamak ve çalışmaya devam uygulama parolasını kullanın.

> [!NOTE]
> (Outlook gibi) office 2013 istemcilerindeki yeni kimlik doğrulama protokolleri destekleyen ve iki aşamalı doğrulamayla birlikte kullanılabilir.  Bu etkinleştirildikten sonra uygulama parolaları Office 2013 istemcilerindeki ile kullanmak için gerekli olmadığını anlamına gelir.  Daha fazla bilgi için [Office 2013 modern kimlik doğrulaması genel önizlemesi Duyuruldu](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Uygulama parolaları kullanma
Uygulama parolalarını nasıl unutmayın gereken bazı noktalar verilmiştir.

* Kendi uygulama parolaları oluşturmayın. Bunun yerine, bunlar otomatik olarak oluşturulur. Yalnızca uygulama için bir sefer uygulama parolası girmek olduğundan, hatırlayabileceğiniz bir yapmak yerine otomatik olarak oluşturulan daha karmaşık bir parola kullanmak daha güvenlidir.
* Şu anda kullanıcı başına 40 parolalık bir sınır yoktur. Sınıra ulaştıktan sonra bir oluşturmaya çalışırsanız, yeni bir tane oluşturmadan önce varolan uygulama parolalarından birini silmeniz istenir.
* Cihaz başına, uygulama başına değil bir uygulama parolasını kullanmanız gerekir. Örneğin, dizüstü bilgisayarınız için bir uygulama parolası oluşturmanız ve tüm uygulamalarınızı Dizüstü bilgisayarınızdaki bu uygulama parolasını kullanın. Daha sonra masaüstünüzde tüm uygulamalarınız için kullanılacak ikinci bir uygulama parolası oluşturun.
* Bir uygulama parolası kayıt iki aşamalı doğrulamayı ilk kez verilir.  Bulunmakla ihtiyacınız varsa, bunları oluşturabilirsiniz.



## <a name="creating-and-deleting-app-passwords"></a>Oluşturma ve uygulama parolaları silme
İlk oturum açma işleminiz sırasında kullanabileceğiniz bir uygulama parolası verilir.  Ayrıca, aynı zamanda oluşturabilir ve daha sonra uygulama parolalarını Sil.  Bunu nasıl yapacağınız, çok faktörlü kimlik doğrulamasını nasıl kullandığınıza bağlıdır. Uygulama parolaları yönetmek için nereye belirlemek için aşağıdaki soruları yanıtlayın:

1. Kişisel Microsoft hesabınız için iki aşamalı doğrulama kullanıyor musunuz? Yanıt Evet ise, başvurmanız gerekir [uygulama parolaları ve iki aşamalı doğrulama](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) Yardım makalesi. Hayır ise, iki soru devam edin.

2. İş veya Okul hesabınız için iki aşamalı doğrulamayı kullanmak için Tamam. Bu Office 365 uygulamalarında oturum açmak için kullanıyorsunuz? Yanıt Evet ise, başvurmanız gerekir [Office 365 için bir uygulama parolası oluşturmanız](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) Yardım. Hayır ise, üç soruya devam edin.

3. Microsoft Azure ile iki aşamalı doğrulama kullanıyor musunuz? Yanıt Evet ise, devam [Azure portalında uygulama parolaları yönetme](#manage-app-passwords-in-the-Azure-portal) bu makalenin. Öyle değilse, dört soru devam edin.

4. İki aşamalı doğrulama kullandığınızda emin değil misiniz? Devam [MyApps portalında ile uygulama parolaları yönetme](#manage-app-passwords-with-the-myapps-portal) bu makalenin.


## <a name="manage-app-passwords-in-the-azure-portal"></a>Azure portalında uygulama parolaları yönetme
Azure ile iki aşamalı doğrulama kullanıyorsanız, Azure portalı üzerinden uygulama parolaları oluşturmak istiyorsunuz.



## <a name="manage-app-passwords-with-the-myapps-portal"></a>MyApps portalında uygulama parolalarıyla yönetin.
Çok faktörlü kimlik doğrulamasını nasıl kullanacağınız emin değilseniz, ardından, her zaman oluşturabilir ve myapps portalı üzerinden uygulama parolalarını Sil.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>MyApps portalında kullanarak bir uygulama parolası oluşturmak için
1. Oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sağ üst köşedeki adınıza tıklayın ve seçin **profili**.
3. Seçin **ek güvenlik doğrulaması**.
   ![Ek güvenlik doğrulaması - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Seçin **uygulama parolaları**.
   ![Uygulama parolaları - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. **Oluştur**’a tıklayın.
6. Uygulama parolası için bir ad girin ve tıklayın **sonraki**.
7. Uygulama parolasını panoya kopyalayın ve uygulamanıza yapıştırın.
   ![Bir uygulama parolası oluşturma](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>MyApps portalında kullanarak bir uygulama parolasını silmek için
1. Oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sayfanın üstünde profilini seçin.
3. Seçin **ek güvenlik doğrulaması**.

   ![Ek güvenlik doğrulaması - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Seçin **uygulama parolaları**.

   ![Uygulama parolaları - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Silmek istediğiniz uygulama parolası yanındaki tıklayın **Sil**.

   ![Bir uygulama parolasını silme](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Parola silmek istediğinizi onaylayın **Evet**.
7. Uygulama parolası silindikten sonra tıklayabilirsiniz **kapatmak**.

## <a name="next-steps"></a>Sonraki adımlar

- [İki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- Denemenin [Microsoft Authenticator uygulamasını](microsoft-authenticator-app-how-to.md) oturum açma işlemlerini uygulama bildirimleri, metinleri veya çağrıları almak yerine ile doğrulayın.
