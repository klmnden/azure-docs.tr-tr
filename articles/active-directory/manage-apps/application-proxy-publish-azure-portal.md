---
title: Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama | Microsoft Belgeleri
description: Azure portalında Azure AD uygulama proxy'si ile şirket içi uygulamaları bulutta yayımlayın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 8eb629396629a92503907439a64cca9d70747010
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594134"
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama

Azure Active Directory (AD) uygulama proxy'si, internet üzerinden erişilebilmesi için şirket içi uygulamaları yayımlama tarafından uzaktan çalışanlar destek yardımcı olur. Ağınızın dışından güvenli uzaktan erişim'sağlamak için bu uygulamaları Azure portalı üzerinden yayımlayabilirsiniz.

Bu makalede, uygulama ara sunucusu ile bir şirket içi uygulamayı yayımlamak için adım adım anlatılmaktadır. Bu makalede tamamladıktan sonra kullanıcılarınızın uygulamanızın uzaktan erişim kuramaz. Ve uygulamanın çoklu oturum açma, kişiselleştirilmiş bilgileri ve güvenlik gereksinimleri gibi ek özellikleri yapılandırmak hazır olması.

Uygulama proxy'si yeniyseniz, bu özellik ile makalesi hakkında daha fazla bilgi [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](application-proxy.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, zaten yüklü ve kayıtlı bir bağlayıcı varsayar. Bu adımları yapmanız gerekiyorsa, bkz: [uygulama ara sunucusu ile başlayın ve Bağlayıcısı'nı yüklemek](application-proxy-enable.md).

## <a name="publish-an-on-premises-app-for-remote-access"></a>Uzaktan erişim için şirket içi uygulama yayımlama

Uygulama proxy'si ile uygulamalarınızı yayımlamak için aşağıdaki adımları izleyin. Önceden indirilen henüz ve kuruluşunuz için bir bağlayıcı yapılandırılmış, gidin [uygulama ara sunucusu ile başlayın ve Bağlayıcısı'nı yüklemek](application-proxy-enable.md) ilk ve uygulamanızı yayımlayın.

> [!TIP]
> İlk kez uygulama proxy'si test ediyorsanız parola tabanlı kimlik doğrulaması için ayarlanmış bir uygulama seçin. Uygulama proxy'si diğer kimlik doğrulama türlerini destekler, ancak parola tabanlı uygulamalar kolay hale getirmek ve hızlı bir şekilde çalışıyor. 

1. Bir yönetici olarak oturum açın [Azure portal](https://portal.azure.com/).
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **yeni uygulama**.

  ![Kurumsal uygulama ekleme](./media/application-proxy-publish-azure-portal/add-app.png)

3. Seçin **tüm**seçeneğini belirleyip **şirket içi uygulama**.  

  ![Kendi uygulamanızı ekleyin](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Uygulamanız ile ilgili şu bilgileri sağlayın:

   - **Ad**: erişim paneli ve Azure portalında görünür uygulamanın adı. 

   - **İç URL**: gelen özel ağınızdan uygulamaya erişmek için kullandığınız URL. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalar ile aynı sunucuda farklı siteleri yayımlamak ve her biri kendi ad ve erişim kuralları verin.

     > [!TIP]
     > Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanızın en ise https://yourapp/app ve konumunda bulunan görüntüleri kullanır https://yourapp/media, yayımlamanız gerekir sonra https://yourapp/ yolu olarak. Bu iç URL kullanıcılarınızın giriş sayfası olması gerekmez. Daha fazla bilgi için bkz: [yayımlanan uygulamalar için özel bir ana sayfa ayarlamak](application-proxy-configure-custom-home-page.md).

   - **Dış URL**: adresi kullanıcılarınızın ağınızın dışından uygulamasından erişmek için gider. Varsayılan uygulama proxy'si etki alanını kullanmak istemiyorsanız, bilgiyi [Azure AD uygulama proxy'si özel etki alanlarında](application-proxy-configure-custom-domain.md).
   - **Ön kimlik doğrulamasını**: nasıl uygulama proxy'si, bunları erişim uygulamanıza vermeden önce kullanıcıların doğrular. 

     - Azure Active Directory: Uygulama Proxy’si, kullanıcıları Azure AD'de oturum açmaya yönlendirir. Burada, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması gerçekleştirilir. Koşullu erişim ve çok faktörlü kimlik doğrulaması gibi Azure AD güvenlik özelliklerden yararlanabilmeniz bu seçenek varsayılan olarak tutmanızı önerir.
     - Geçiş: Kullanıcıların Azure uygulamaya erişmek için Active Directory'ye karşı kimlik doğrulaması gerekmez. Arka uç kimlik doğrulaması gereksinimleri hala ayarlayabilirsiniz.
   - **Bağlayıcı grup**: bağlayıcılar, uygulamanız için uzaktan erişim işlemek ve bağlayıcı grupları bağlayıcılar ve bölge, ağ veya amacı uygulamaların düzenlemenize yardım. Bağlayıcı gruplarda henüz yoksa, uygulamanız atandığı **varsayılan**.

>[!NOTE]
>Websocket desteği ve atanmış bir bağlayıcı grubu ile daha yüksek veya Bağlayıcısı sürüm 1.5.612.0 sahip olduğunuzdan emin olun, yalnızca uygulamanız bağlanmak için websockets kullanıyorsa, bu bağlayıcıların kullanır.

   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Gerekirse, ek ayarları yapılandırın. Çoğu uygulama için bu ayarları varsayılan durumlarına tutmanız gerekir. 
   - **Arka uç uygulaması zaman aşımı**: Bu değer ayarlanırsa **uzun** yalnızca uygulamanızı kimlik doğrulaması ve bağlanmak için yavaş olduğunda. 
   - **Üst bilgileri URL'leri**: Bu değer olarak tutun **Evet** kimlik doğrulama isteğini özgün ana bilgisayar üst bilgi uygulamanızın gerektirdiği sürece.
   - **Uygulama gövdesindeki URL'leri**: Bu değer olarak tutun **Hayır** kodlanmış HTML bağlantılar diğer şirket içi uygulamalara sahip ve özel etki alanlarını kullanmadığınız sürece. Daha fazla bilgi için bkz: [bağlantı uygulama proxy'si ile çeviri](application-proxy-configure-hard-coded-link-translation.md).
   
   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. **Add (Ekle)** seçeneğini belirleyin.


## <a name="add-a-test-user"></a>Test kullanıcı ekleme 

Uygulamanız doğru bir şekilde yayımlanan test etmek için bir test kullanıcı hesabı ekleyin. Bu hesap zaten şirket ağı içinde uygulamasından erişmek için izinlere sahip olduğunu doğrulayın.

1. Geri hızlı başlangıç dikey penceresinde, seçin **test etmek için bir kullanıcı atamak**.

  ![Test etmek için bir kullanıcı atama](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Kullanıcıları ve grupları dikey seçin **Ekle**.

  ![Bir kullanıcı veya Grup Ekle](./media/application-proxy-publish-azure-portal/add-user.png)

3. Ekle atama dikey seçin **kullanıcılar ve gruplar** eklemek istediğiniz hesabı seçin. 
4. **Ata**'yı seçin.

## <a name="test-your-published-app"></a>Yayımlanan uygulamanızı test etme

Tarayıcınızda, Yayımla adımında yapılandırdığınız dış URL'ye gidin. Başlangıç ekranında bakın ve ayarladığınız test hesapla oturum açabilmesi gerekir.

![Yayımlanan uygulamanızı test etme](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Bağlayıcılar karşıdan](application-proxy-enable.md) ve [bağlayıcı grupları oluşturma](application-proxy-connector-groups.md) ayrı ağlar ve konumları uygulamaları yayımlamak için.

- [Çoklu oturum açmayı kurduğunuzda](application-proxy-configure-single-sign-on-password-vaulting.md) yeni yayımlanan uygulamanız için
