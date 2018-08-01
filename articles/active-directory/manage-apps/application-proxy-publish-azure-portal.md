---
title: Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama | Microsoft Belgeleri
description: Azure AD uygulama ara sunucusu ile şirket içi uygulamaları buluta, Azure portalında yayımlayın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1224642bb7e6fc0c51b3f839a78449132db5b4bb
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364266"
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama

Azure Active Directory (AD) uygulama proxy'si internet üzerinden erişilecek şirket içi uygulamalar yayımlayarak uzak çalışanları desteklemenize yardımcı olur. Gelen ağınızın dışından güvenli uzaktan erişim sağlamak için Azure portalı üzerinden bu uygulamaları yayımlayabilirsiniz.

Bu makalede uygulama ara sunucusu ile şirket içi uygulama yayımlama adımlarında size kılavuzluk eder. Bu makaleyi tamamladıktan sonra kullanıcılarınızın uygulamanızı uzaktan erişmek mümkün olacaktır. Ve uygulamanın çoklu oturum açma, kişiselleştirilmiş bilgiler ve güvenlik gereksinimleri gibi ek özellikleri yapılandırmak hazır olacaksınız.

Uygulama Ara sunucusuna yeniyseniz, bu makalede özellikle hakkında daha fazla bilgi [güvenli uzaktan erişim sağlamak şirket içi uygulamalara](application-proxy.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, zaten yüklü ve kayıtlı bir bağlayıcı varsayılır. Bu adımları uygulamanız gerekiyorsa, bkz. [bağlayıcıyı yükleyin ve uygulama ara sunucusu ile çalışmaya başlama](application-proxy-enable.md).

## <a name="publish-an-on-premises-app-for-remote-access"></a>Uzaktan erişim için şirket içi uygulama yayımlama

Uygulama proxy'si kullanarak uygulamalarınızı yayımlamak için aşağıdaki adımları izleyin. Önceden indirdiğiniz henüz ve kuruluşunuz için bir bağlayıcı yapılandırılmış, Git [bağlayıcıyı yükleyin ve uygulama ara sunucusu ile çalışmaya başlama](application-proxy-enable.md) önce ve sonra uygulamanızı yayımlayın.

> [!TIP]
> İlk kez uygulama proxy'si test ediyorsanız parola tabanlı kimlik doğrulaması için ayarlanmış bir uygulama seçin. Uygulama proxy'si diğer kimlik doğrulaması türünü destekler, ancak parola tabanlı getirmek kolay ve hızlı bir şekilde çalışan uygulamalardır. 

1. Yönetici olarak oturum açın [Azure portalında](https://portal.azure.com/).
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **yeni uygulama**.

  ![Kurumsal uygulama ekleme](./media/application-proxy-publish-azure-portal/add-app.png)

3. Seçin **tüm**, ardından **şirket içi uygulama**.  

  ![Kendi uygulamanızı ekleyin](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Uygulamanız ile ilgili şu bilgileri sağlayın:

   - **Ad**: uygulamanın erişim panelinde hem de Azure portalında görünecek adı. 

   - **İç URL**: uygulamaya özel ağınızın içinden erişmek için kullandığınız URL. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalar ile aynı sunucuda farklı siteleri yayımlayabilir; her biri kendi adını ve erişim kuralları belirleyebilirsiniz.

     > [!TIP]
     > Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız, ise https://yourapp/app ve konumunda bulunan görüntüleri kullanır https://yourapp/media, yayımlamanız gerekir sonra https://yourapp/ yolu olarak. Bu iç URL, kullanıcıların görmesi giriş sayfası olması gerekmez. Daha fazla bilgi için [yayımlanan uygulamalar için özel bir ana sayfa ayarlamak](application-proxy-configure-custom-home-page.md).

   - **Dış URL**: adres, kullanıcılarınızın uygulamadan ağınızın dışından erişmek için gidin. Varsayılan uygulama ara sunucusu etki alanı kullanmayı istemiyorsanız okuyun [Azure AD uygulama proxy'sinde özel etki alanları](application-proxy-configure-custom-domain.md).
   - **Ön kimlik doğrulaması**: uygulama proxy'nizin kullanıcıların erişim uygulamanıza vermeden önce. 

     - Azure Active Directory: Uygulama Proxy’si, kullanıcıları Azure AD'de oturum açmaya yönlendirir. Burada, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması gerçekleştirilir. Azure AD koşullu erişim ve çok faktörlü kimlik doğrulaması gibi güvenlik özelliklerini yararlanabilir, böylece bu seçenek varsayılan olarak tutma öneririz.
     - Geçiş: Kullanıcıların Azure uygulamaya erişmek için Active Directory karşı kimlik doğrulaması gerekmez. Kimlik doğrulama gereksinimleri arka uçtaki yine de ayarlayabilirsiniz.
   - **Bağlayıcı grubu**: bağlayıcılar, uygulamanız için uzaktan erişim işlemek ve bağlayıcı gruplarını bağlayıcılar ve bölgeyi, ağ veya amaçlı uygulamaların düzenlemenize yardımcı. Bağlayıcı gruplarda henüz sahip değilseniz, uygulamanızın atanan **varsayılan**.

>[!NOTE]
>Websocket desteği ve atanmış bir bağlayıcı grubu ile daha yüksek veya connector sürümü 1.5.612.0 sahip emin olun, yalnızca uygulamanız bağlanmak için websockets kullanıyorsa, bu bağlayıcıları kullanır.

   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Gerekirse, ek ayarları yapılandırın. Çoğu uygulama için bu ayarları varsayılan durumlarına tutmanız gerekir. 
   - **Arka uç uygulama zaman aşımı**: Bu değer kümesine **uzun** uygulamanız kimlik doğrulaması ve bağlanmak yavaş ise. 
   - **Üst bilgilerinde URL'leri**: Bu değer olarak tutmak **Evet** özgün ana bilgisayar üst bilgisi'kimlik doğrulama isteği, uygulamanızın gerektirdiği durumlar haricinde.
   - **Uygulama gövdesindeki URL'leri**: Bu değer olarak tutmak **Hayır** sürece diğer şirket içi uygulamalara yönelik sabit kodlanmış HTML bağlantıları ve özel etki alanları kullanmayın. Daha fazla bilgi için [çeviri uygulama ara sunucusu ile bağlantı](application-proxy-configure-hard-coded-link-translation.md).
   
   ![Uygulamanızı yapılandırma](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. **Add (Ekle)** seçeneğini belirleyin.


## <a name="add-a-test-user"></a>Bir test kullanıcısı Ekle 

Uygulamanızın doğru şekilde yayımlandığını test etmek için bir test kullanıcı hesabı ekleyin. Bu hesap kurumsal ağ içinde uygulamadan erişim izni olduğunu doğrulayın.

1. Hızlı Başlangıç dikey penceresine üzerinde seçin **test etmek için kullanıcı atama**.

  ![Test etmek için kullanıcı atama](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Kullanıcıları ve grupları dikey penceresini seçin **Ekle**.

  ![Bir kullanıcı veya Grup Ekle](./media/application-proxy-publish-azure-portal/add-user.png)

3. Atama Ekle dikey penceresinde seçin **kullanıcılar ve gruplar** ardından eklemek istediğiniz hesabı seçin. 
4. **Ata**'yı seçin.

## <a name="test-your-published-app"></a>Yayımlanan uygulamanızı test edin

Yayımlama adımında yapılandırdığınız dış URL'yi tarayıcınızda gidin. Başlangıç ekranına bakın ve ayarladığınız test hesabıyla oturum açabilir.

![Yayımlanan uygulamanızı test edin](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Bağlayıcı indirme](application-proxy-enable.md) ve [bağlayıcı grupları oluşturma](application-proxy-connector-groups.md) ayrı ağlar ve konumları uygulama yayımlayın.

- [Çoklu oturum açmayı ayarlama](application-proxy-configure-single-sign-on-password-vaulting.md) yeni yayımlanan uygulama
