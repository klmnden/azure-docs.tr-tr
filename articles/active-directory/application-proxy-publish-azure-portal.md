---
title: "Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama | Microsoft Belgeleri"
description: "Azure portalında Azure AD uygulama proxy'si ile şirket içi uygulamaları bulutta yayımlayın."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
<<<<<<< HEAD
ms.openlocfilehash: e00a939f2b20ab8e0a2ddf0ff91e59db440213ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: 3639c7d8c3c1e716eaf1a0af0506f6d0d2ad0493
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama

Azure Active Directory (AD) uygulama proxy'si, internet üzerinden erişilebilmesi için şirket içi uygulamaları yayımlama tarafından uzaktan çalışanlar destek yardımcı olur. Ağınızın dışından güvenli uzaktan erişim'sağlamak için bu uygulamaları Azure portalı üzerinden yayımlayabilirsiniz.

Bu makalede, uygulama ara sunucusu ile bir şirket içi uygulamayı yayımlamak için adım adım anlatılmaktadır. Bu makalede tamamladıktan sonra kullanıcılarınızın uygulamanızın uzaktan erişim kuramaz. Ve uygulamanın çoklu oturum açma, kişiselleştirilmiş bilgileri ve güvenlik gereksinimleri gibi ek özellikleri yapılandırmak hazır olması.

Uygulama proxy'si yeniyseniz, bu özellik ile makalesi hakkında daha fazla bilgi [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>Uzaktan erişim için şirket içi uygulama yayımlama

Uygulama proxy'si ile uygulamalarınızı yayımlamak için aşağıdaki adımları izleyin. Önceden indirilen henüz ve kuruluşunuz için bir bağlayıcı yapılandırılmış, gidin [uygulama ara sunucusu ile başlayın ve Bağlayıcısı'nı yüklemek](active-directory-application-proxy-enable.md) ilk ve uygulamanızı yayımlayın.

> [!TIP]
> İlk kez uygulama proxy'si test ediyorsanız parola tabanlı kimlik doğrulaması için ayarlanmış bir uygulama seçin. Uygulama proxy'si diğer kimlik doğrulama türlerini destekler, ancak parola tabanlı uygulamalar kolay hale getirmek ve hızlı bir şekilde çalışıyor. 

1. Bir yönetici olarak oturum açın [Azure portal](https://portal.azure.com/).
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **yeni uygulama**.

  ![Kurumsal uygulama ekleme](./media/application-proxy-publish-azure-portal/add-app.png)

3. Seçin **tüm**seçeneğini belirleyip **şirket içi uygulama**.  

  ![Kendi uygulama ekleme](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Uygulamanız ile ilgili şu bilgileri sağlayın:

   - **Ad**: erişim paneli ve Azure portalında görünür uygulamanın adı. 

   - **İç URL**: gelen özel ağınızdan uygulamaya erişmek için kullandığınız URL. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalar ile aynı sunucuda farklı siteleri yayımlamak ve her biri kendi ad ve erişim kuralları verin.

     > [!TIP]
     > Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız https://yourapp/app üzerindeyse ve https://yourapp/media üzerindeki görüntüleri kullanıyorsa yolu https://yourapp/ olarak yayımlamanız gerekir. Bu iç URL kullanıcılarınızın giriş sayfası olması gerekmez. Daha fazla bilgi için bkz: [yayımlanan uygulamalar için özel bir ana sayfa ayarlamak](application-proxy-office365-app-launcher.md).

   - **Dış URL**: adresi kullanıcılarınızın ağınızın dışından uygulamasından erişmek için gider. Varsayılan uygulama proxy'si etki alanını kullanmak istemiyorsanız, bilgiyi [Azure AD uygulama proxy'si özel etki alanlarında](active-directory-application-proxy-custom-domains.md).
   - **Ön kimlik doğrulamasını**: nasıl uygulama proxy'si, bunları erişim uygulamanıza vermeden önce kullanıcıların doğrular. 

     - Azure Active Directory: Uygulama Proxy’si, kullanıcıları Azure AD'de oturum açmaya yönlendirir. Burada, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması gerçekleştirilir. Koşullu erişim ve çok faktörlü kimlik doğrulaması gibi Azure AD güvenlik özelliklerden yararlanabilmeniz bu seçenek varsayılan olarak tutmanızı önerir.
     - Geçiş: Kullanıcıların Azure uygulamaya erişmek için Active Directory'ye karşı kimlik doğrulaması gerekmez. Arka uç kimlik doğrulaması gereksinimleri hala ayarlayabilirsiniz.
   - **Bağlayıcı grup**: bağlayıcılar, uygulamanız için uzaktan erişim işlemek ve bağlayıcı grupları bağlayıcılar ve bölge, ağ veya amacı uygulamaların düzenlemenize yardım. Bağlayıcı gruplarda henüz yoksa, uygulamanız atandığı **varsayılan**.

   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Gerekirse, ek ayarları yapılandırın. Çoğu uygulama için bu ayarları varsayılan durumlarına tutmanız gerekir. 
   - **Arka uç uygulaması zaman aşımı**: Bu değer ayarlanırsa **uzun** yalnızca uygulamanızı kimlik doğrulaması ve bağlanmak için yavaş olduğunda. 
   - **Üst bilgileri URL'leri**: Bu değer olarak tutun **Evet** kimlik doğrulama isteğini özgün ana bilgisayar üst bilgi uygulamanızın gerektirdiği sürece.
   - **Uygulama gövdesindeki URL'leri**: Bu değer olarak tutun **Hayır** kodlanmış HTML bağlantılar diğer şirket içi uygulamalara sahip ve özel etki alanlarını kullanmadığınız sürece. Daha fazla bilgi için bkz: [bağlantı uygulama proxy'si ile çeviri](application-proxy-link-translation.md).
   
   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. **Add (Ekle)** seçeneğini belirleyin.


## <a name="add-a-test-user"></a>Test kullanıcı ekleme 

Uygulamanız doğru bir şekilde yayımlanan test etmek için bir test kullanıcı hesabı ekleyin. Bu hesap zaten şirket ağı içinde uygulamasından erişmek için izinlere sahip olduğunu doğrulayın.

1. Geri hızlı başlangıç dikey penceresinde, seçin **test etmek için bir kullanıcı atamak**.

  ![Test etmek için bir kullanıcı atama](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Kullanıcıları ve grupları dikey seçin **Ekle**.

  ![Bir kullanıcı veya Grup Ekle](./media/application-proxy-publish-azure-portal/add-user.png)

3. Ekle atama dikey seçin **kullanıcılar ve gruplar** eklemek istediğiniz hesabı seçin. 
4. Seçin **atamak**.

## <a name="test-your-published-app"></a>Yayımlanan uygulamanızı test etme

Tarayıcınızda, Yayımla adımında yapılandırdığınız dış URL'ye gidin. Başlangıç ekranında bakın ve ayarladığınız test hesapla oturum açabilmesi gerekir.

![Yayımlanan uygulamanızı test etme](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Bağlayıcılar karşıdan](active-directory-application-proxy-enable.md) ve [bağlayıcı grupları oluşturma](active-directory-application-proxy-connectors-azure-portal.md) ayrı ağlar ve konumları uygulamaları yayımlamak için.

- [Çoklu oturum açmayı kurduğunuzda](application-proxy-sso-azure-portal.md) yeni yayımlanan uygulamanız için
