---
title: "Azure MFA uygulama parolaları kullanmak üzere nasıl? | Microsoft Belgeleri"
description: "Bu sayfa, kullanıcıların uygulama parolaları nedir ve ne için ile kullanıldıkları Azure MFA Algıla anlamasına yardımcı olur."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 1ecc2bdef5ff7ef8ed8dded7dc12428ce9657821
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması'ndaki uygulama parolaları nedir?
Exchange Active Sync kullanan Apple yerel e-posta istemcisi gibi belirli bir tarayıcı olmayan uygulamaları, şu anda çok faktörlü kimlik doğrulamasını desteklemez. Çok faktörlü kimlik doğrulaması kullanıcı başına etkinleştirilir.  Başka bir deyişle, bir kullanıcı, çok faktörlü kimlik doğrulamasını kullanamaz:

- Kullanıcı için multi-Factor authentication etkinleştirildi
- Kullanıcı, bir tarayıcı olmayan uygulama kullanmaya çalışıyor.

Bir uygulama parolası kullanıcının uygulamayı kullanmasına izin verir.

Bir uygulama parolası sahip olduktan sonra özgün parolanızı bu tarayıcı olmayan uygulamaları ile yerine kullanın. İki aşamalı doğrulama için kaydolduğunuzda, herkesin ikinci doğrulama de gerçekleştiremezsiniz parolanızla oturum oturum vermez Microsoft'a söyleyen. İki aşamalı doğrulama için istenemez çünkü telefonunuzda Apple yerel e-posta istemcisi olarak oturum açamaz. Bu sorun için çözüm kullanmadığınız daha güvenli bir uygulama parolası günlük oluşturmaktır. Uygulama parolaları, iki aşamalı doğrulamayı destekleyemez yalnızca bu uygulamalar içindir. Uygulamalar çok faktörlü kimlik doğrulamasını atlamak ve çalışmaya devam uygulama parolasını kullanın.


> [!NOTE]
> Office 2013 istemcileri (Outlook dahil) destekleyen yeni kimlik doğrulama protokolleri ve iki aşamalı doğrulamayla birlikte kullanılabilir. Uygulama parolaları Office 2013 istemcilerle kullanım için gerekli değildir.  Daha fazla bilgi için bkz: [Office 2013 modern kimlik doğrulaması genel önizlemesi Duyuruldu](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Uygulama parolaları kullanma
Uygulama parolaları hakkında bilinmesi gereken bazı şeyler şunlardır:

* Kendi uygulama parolaları oluşturmayın. Bunlar otomatik olarak oluşturulur.
* Şu anda kullanıcı başına 40 parolalık bir sınırı yoktur. 
* Sınıra ulaştıktan sonra bir uygulama parolası oluşturmak denerseniz, yeni bir tane oluşturmadan önce varolan uygulama parolalarınızdan birini silmeniz gerekir.
* Bir uygulama parolası aygıt başına, uygulama başına değil kullanın. Örneğin, dizüstü bilgisayarınız için bir uygulama parolası oluşturmanız ve tüm Dizüstü bilgisayarınızdaki uygulamalar için bu uygulama parolasını kullanın. Daha sonra masaüstünüzde tüm uygulama için kullanılacak ikinci bir uygulama parolası oluşturun. 
* Bir uygulama parolası kaydettiğiniz iki aşamalı doğrulamayı ilk kez verilir.  Ek olanları ihtiyacınız varsa, bunları oluşturabilirsiniz.



## <a name="creating-and-deleting-app-passwords"></a>Oluşturma ve uygulama parolaları silme
İlk oturum açma işleminiz sırasında kullanabileceğiniz bir uygulama parolası verilir.  Ayrıca, oluşturabilir ve daha sonra uygulama parolalarını Sil. Uygulama parolalarını Sil nasıl çok faktörlü kimlik doğrulaması nasıl kullandığınıza bağlıdır. Uygulama parolaları yönetmek için nereye belirlemek için aşağıdaki soruları yanıtlayın: 

1. Kişisel Microsoft hesabınız için iki aşamalı doğrulama kullanıyor musunuz? Yanıt Evet ise, için başvurmalıdır [uygulama parolaları ve iki aşamalı doğrulamayı](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) Yardım makalesi. Öyle değilse, iki soru devam edin.

2. İş veya Okul hesabınız için iki aşamalı doğrulamayı kullanmak için Tamam. Bunu Office 365 uygulamalarda oturum açmak için kullandığınız? Yanıt Evet ise, için başvurmalıdır [Office 365 için bir uygulama parolası oluşturmanız](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) Yardım. Öyle değilse, soru üç devam edin. 

3. Microsoft Azure ile iki aşamalı doğrulama kullanıyor musunuz? Yanıt Evet ise, devam [Azure portalında uygulama parolaları yönetme](#manage-app-passwords-in-the-Azure-portal) bu makalenin. Öyle değilse, dört soru devam edin.

4. İki aşamalı doğrulamayı kullandığınızda emin değil misiniz? Devam [MyApps portal ile uygulama parolaları yönetme](#manage-app-passwords-with-the-myapps-portal) bu makalenin. 


## <a name="manage-app-passwords-in-the-azure-portal"></a>Azure portalında uygulama parolaları yönetme
İki aşamalı doğrulamayı Azure ile kullanırsanız, Azure portalı üzerinden uygulama parolaları oluşturmak istediğiniz.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Azure portalında uygulama parolaları oluşturmak için
1. Klasik Azure portalında oturum açın.
2. En üstte, kullanıcı adını sağ tıklatın ve ek güvenlik doğrulama'yı seçin.
3. En üstte düzenleyebileceğinizi sayfasında uygulama parolaları seçin
4. **Oluştur**'a tıklayın.
5. Uygulama parolası için bir ad girin ve tıklayın **sonraki**
6. Uygulama parolasını panoya kopyalayın ve uygulamanıza yapıştırın.
   
   ![Bulut](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Azure portalında uygulama parolaları silmek için
1. Klasik Azure portalında oturum açın.
2. En üstte, kullanıcı adını sağ tıklatın ve ek güvenlik doğrulama'yı seçin.
3. Ek güvenlik doğrulaması yanındaki üst seçin **uygulama parolaları.**
4. Silmek istediğiniz uygulama parolası yanındaki seçin **silmek**.
5. Silme işlemini onaylamak **Evet**.
6. Uygulama parolası silindikten sonra tıklayabilirsiniz **kapatmak**.


## <a name="manage-app-passwords-with-the-myapps-portal"></a>Uygulama parolaları MyApps portalı ile yönetin.
Çok faktörlü kimlik doğrulaması kullanımını emin değilseniz, daha sonra her zaman oluşturabilir ve myapps portalı üzerinden uygulama parolalarını Sil.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Myapps Portalı'nı kullanarak bir uygulama parolası oluşturmak için
1. Oturum [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Sağ üst köşedeki adınızın ve seçin **profil**.
3. Seçin **ek güvenlik doğrulaması**.
   ![Ek güvenlik doğrulaması - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Seçin **uygulama parolaları**.
   ![Uygulama parolaları - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. **Oluştur**'a tıklayın.
6. Uygulama parolası için bir ad girin ve tıklayın **sonraki**.
7. Uygulama parolasını panoya kopyalayın ve uygulamanıza yapıştırın.
   ![Bir uygulama parolası oluşturma](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Myapps Portalı'nı kullanarak bir uygulama parolası silmek için
1. Oturum [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. En üstte profilini seçin.
3. Seçin **ek güvenlik doğrulaması**.

   ![Ek güvenlik doğrulaması - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Seçin **uygulama parolaları**.

   ![Uygulama parolaları - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Silmek istediğiniz uygulama parolası yanındaki tıklatın **silmek**.

   ![Bir uygulama parolası Sil](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Parola tıklayarak silmek istediğinizi onaylamak **Evet**.
7. Uygulama parolası silindikten sonra tıklayabilirsiniz **kapatmak**.

## <a name="next-steps"></a>Sonraki adımlar

- [İki basamaklı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- Denemenin [Microsoft Authenticator uygulaması](microsoft-authenticator-app-how-to.md) oturum açma işlemleri metinleri veya aramaları almasını yerine uygulama bildirimleri ile doğrulayın. 
