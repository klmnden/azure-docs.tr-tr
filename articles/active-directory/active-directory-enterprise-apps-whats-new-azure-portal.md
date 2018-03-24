---
title: Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler | Microsoft Docs
description: Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikleri öğrenin.
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
editor: ''
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 8d1b24708380aeed6055912fcf3538f0e5319e2d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal Uygulama Yönetimi'nde yenilikler 

Azure Active Directory (Azure AD) ile yeni özellikler ve yetenekler yöneten uygulamalar daha basit ve verimli hale getirmek için kurumsal uygulama yönetimi araçları Gelişmiş.

Aşağıdaki Azure AD'de için bazı iyileştirmeler olan [Azure portal](https://portal.azure.com).

- Geliştirilmiş uygulama Galerisi, Basitleştirilmiş uygulama oluşturma modeliyle deneyimi ve için kullanılan tüm uygulama türleri için destek. 
- Yardımcı olabilecek bir yeni hızlı başlangıç deneyimi alma, uygulamanızın bir pilot adımıdır. 
- Self Servis ilkeleri yalnızca birkaç tıklama ile yapılandırın. 
- Uygulama proxy'si geliştirmeleri tek oturum açma yapılandırması ve yanıtlar daha önce elde size izin vererek, kendi uygulama deneyimleri duruma getirin.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Azure Active Directory Uygulama galerisinde geliştirmeleri

Gelen olup, sık kullanılan uygulamalar eklemek [uygulama Galerisi](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), buluta genişletme özel uygulamalar veya yeni uygulamalar geliştirme.  Bu yeni deneyim, başlayabiliriz tıklayarak **Ekle** üzerinde **kurumsal uygulamalar** genel bakış veya **tüm uygulamaları** dikey pencereleri.
 
  ![Bir uygulama ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Bir kez Galerisi'nde kullanıcı sağlamayı destekleyen tüm bizim öne çıkan uygulamaları Ön Orta görüntülendiğini göreceksiniz.  İlgilendiğiniz uygulamaları incelemek için farklı kategoriler her türlü göz atabilir veya hızlı bir şekilde tümleştirmek istediğiniz uygulamaları bulmak için arama deneyimini kullanabilirsiniz.

  ![Uygulama Galerisi](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Tek bir yerden özel uygulamalar ekleme

Önceden tümleştirilmiş uygulamaların Galeriden ekleme ek olarak, tüm özel uygulama yapılandırması için Klasik Yönetim Portalı'nda kullandığınız karşılaştığında şimdi yeni Portalı'nda mümkündür. Kullanılıp parolanızı veya Federasyon SSO uygulaması tümleştirme uygulaması proxy'si kullanarak şirket içi bir uygulamadan genişletme veya Uygulama Kayıt Defteri'ni kullanarak yeni bir uygulama oluşturma, için tüm bu tek bir yerden edinebilirsiniz.

  ![Kendi uygulama ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Kendi uygulamanızı ekleme başlamak için**:

1. Tıklatın **kendi bağlantısı eklemek** üst uygulama Galerisi. 
2. Önünde iki seçenek görürsünüz: **var olan bir uygulamayı dağıtmak** veya **yeni bir uygulama geliştirme**. İçin okumaya iki seçenek ve bunları nasıl kullanacağınızı arasındaki farkı öğrenin.

### <a name="deploying-existing-applications"></a>Var olan uygulamaları dağıtma

1. Zaten çalışan bir uygulama aldınız ve yalnızca üzerinde Azure AD çoklu oturum şekilde entegre veya sağlama, seçmek istediğiniz **var olan bir uygulamayı dağıtmak** seçeneği. Uygulamanız için bir ad seçin, tıklatın **Ekle**.
2. İşte bu kadar! Uygulamanızı Önden ile ilgili tüm ayrıntıları öğrenmek gereksinimi yerine, yeni uygulamanızı sol menünüzden gezinme ve herhangi bir zamanda, istediğiniz uygulamaya yapılandırarak işleyişi şimdi ayarlayabilirsiniz.

  ![Tek bir tıklatmayla var olan bir uygulama ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Yeni uygulamaları geliştirme

1. Yeni bir uygulama geliştiriyorsanız, uygulama kayıt defterine Galeriden sağ almanız için kolay bir yol vardır:
2. Tıklayın **kendi eklemek** uygulama Galerisi, select seçeneğinden **varolan uygulama geliştirme** seçim ve uygulama Ekle deneyimi sağdan hızlı bir bağlantı göreceksiniz.

  ![Birkaç tıklama ile yeni geliştirilen bir uygulama ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Uygulama kayıt defterini kullanarak bir uygulama ekledikten sonra bunu burada erişim ilkelerini yeni uygulamanız için çoklu oturum açma yapılandırmanıza ve yönetmenize mümkün olacaktır kurumsal uygulamalar listesinde görünmesi görürsünüz.

  ![Kuruluş uygulamaları altında yeni uygulamanıza erişimi yönetme](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a>Hızlı Başlangıç: yeni uygulamanızla birlikte hemen kullanmaya başlamak 

Bir uygulama, önceden tümleştirilmiş veya kendi uygulamanızı ekledikten sonra yeni uygulamalar deneyimi hızla topraklanmış almak için özel bir hızlı başlangıç deneyimi oluşturduk. Her seçenek sistematik olarak izlerseniz, biz, kullanıcı Arabirimi aracılığıyla yol ve yeni uygulamanızın bir pilot mümkün olan en kısa sürede yapmalarını gösterilmektedir. 
 
  ![Deneyimi yeni uygulamalar Hızlı Başlat](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 Tıklayarak herhangi bir zamanda ve herhangi bir uygulama için yeni Bu hızlı başlangıç deneyimi ziyaret edebilirsiniz **Hızlı Başlangıç** uygulama sol gezinti menüsünde.


## <a name="updated-application-proxy-configuration"></a>Güncelleştirilmiş uygulama proxy yapılandırması
Artık, şirket içi ortamınızda say eklediğiniz yeni uygulamalardan biri şimdi çalışıyor ve Azure AD ile tümleştirmek istediğiniz.  Yeni uygulama yapılandırma deneyimi hakkında harika yeni özelliklerinden biri yeni Azure AD portalı, uygulamanın oturum açma modundan uygulama proxy yapılandırmasını bölerek, şimdi kolayca parola SSO açmasıdır veya Federasyon şirket ağınızda birden çok uygulama örneğini oluşturmak zorunda kalmadan buluta sağ çalışan uygulamaları.

Buna ek olarak, şimdi de, yerel Windows kimlik doğrulama deneyimleri destekleyen uygulamalar da dahil olmak üzere yeni portalı, Azure AD uygulama proxy'si sağa ile kullanılmak üzere eklediğiniz yeni uygulamalardan herhangi biri yapılandırabilirsiniz.

  ![Oturum açma tümleşik Windows kimlik doğrulaması seçeneği kullanmak için uygulamayı yapılandırma](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

Uygulama Ara sunucusu ile bir yerel Windows kimlik doğrulama uygulaması yapılandırmaya başlamak için:
1. Üzerinde çoklu oturum açma Gezinti öğesi'ı tıklatın ve seçin **tümleşik Windows kimlik doğrulaması** oturum açma ayarlar dikey penceresinden ve, istediğiniz ayarları yapılandırın.
2. Bu yeni kimlik doğrulama modları destekleyen en üstünde, şimdi de güvenli uç noktaları kuruluşunuz içinde çalışan uygulamaları desteklemek için özel etki alanlarını sertifikaları karşıya yükleyebilirsiniz.  
 
   ![Uygulama Ara sunucusu ile kullanılacak bir sertifika karşıya yükleniyor](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. Sık kullanılan şirket içi uygulamanız için yeni bir sertifika yüklemek için tıklayın **uygulama proxy'si** seçeneği sol gezinti menüsünde, tıklatın **sertifika** Seçici ve şifrelemek için kullanabileceğiniz bir sertifika dosyası istekleri bizim bulut uç noktasından uygulamanızı karşıya yükleme.

## <a name="advanced-federated-single-sign-on-configuration"></a>Federasyon tek oturum açma Gelişmiş Yapılandırma

Bu Federasyon uygulamalarına bugün kullananlar vardır birçok yeni özellik SAML tabanlı oturum açma yapılandırma dikey penceresinde. İle başlamak artık tam olarak özelleştirmek, eklemek, kaldırmak ve SAML belirteçleri Taleplerde olarak verilen varolan kullanıcı öznitelikleri eşlemek kullanabilirsiniz.
 
  ![Geçirilen bir federasyon uygulaması SAML belirteci kullanıcı özniteliklerini özelleştirme](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


Yeni SSO yapılandırma federe olduğunu denetlemek için:
1. Bir Federasyon uygulamanın açmak **çoklu oturum açma** emin olun ve sol gezinti menüsü dikey penceresinde '*SAML tabanlı oturum açma** modu seçilidir. 
2. Kez altındaki onay kutusunu, etkinleştirme **kullanıcı öznitelikleri** başlık tüm SAML belirtecine eklenen özniteliklerini değiştirmek için bu uygulamaya geçirildi.

De oluşturma, aktarma ve Federasyon çoklu oturum açma için kullanılan sertifikaları yönetme yanı Düzenle sertifikanızı dolmak üzere olduğunda kimin bildirim. Bu yeni seçenekleri altında görürsünüz **sertifikaları** aynı tek oturum açma dikey penceresinde başlığı.
 
  ![Süre sonu bildirim e-posta ve seçenekleri imzalama sertifikası özelleştirme yeni bir sertifika oluşturma](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a>Geçiş durumunu paramenter
Son olarak, biz de dahil etmek için desteklediğimiz SAML URL parametrelerinin Genişletilmiş **geçiş durumunu parametresi**, kullanıcılarınızın güden üzerinde bir federasyon uygulaması içinde oturum açma tamamlandıktan sonra sayfa olduğu. Kullanıcılarınızın bunları hızlı bir şekilde giderek almak için uygulama içinde belirli bir yere göndermek istiyorsanız yapılandırmak için çok kullanışlı ayar budur.

  ![SAML geçiş durumunu parametre ayarı](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Geçiş durumu parametresini ayarlama**:

1. Etkinleştirme **Göster Gelişmiş URL ayarları** altındaki onay kutusunu **etki alanı ve URL'leri** çoklu oturum açma yapılandırma dikey penceresi başlığı. 
2. Bunu yaptıktan sonra bu ve diğer SAML URL ayarları ayarlamanıza olanak sağlayacak kutuları görüntülenir yeni URL bir dizi giriş görürsünüz.

## <a name="bring-your-own-password-sso-applications"></a>SSO uygulamaları kendi parolanızı Getir

Her uygulama kutunun sağ dışında Federasyon desteklediğini biliyoruz. Örneğin, belki de eklediğiniz yeni uygulamalardan biri kullanıcılarınız oturum açmak için bir kullanıcı adı ve parola kullanan bir özel oturum açma ekranı sahiptir. Azure AD kullanarak, bu tür uygulamalar hala tümleştirebilirsiniz bizim **kendi uygulamalarınızı Getir** yeni Portalı'nda yapılandırmanız için kullanıma sunulmuştur özelliği.
 
  ![Azure AD ile uygulamaları vaulting özel parola tümleştirme](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**'Kendi uygulamalarınızı Yap' özelliği denetlemek için**:

1. Yeni özel bir uygulama için tek oturum açma modu ayarlandıktan sonra ekledik **parola tabanlı oturum açma**, uygulamanın, oturum açma ekranı burada işler URL'sini girin ve tıklayın **kaydetmek**.  
2. Bunu gerçekleştirdikten sonra biz otomatik olarak bir kullanıcı adı için bu URL'yi scrape ve parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısı kullanarak bu uygulamaya parolaları güvenli bir şekilde iletmek için kullanmanızı sağlar.

## <a name="configure-self-service-application-access"></a>Self Servis uygulama erişimi yapılandırma

Çok sayıda yeni uygulamaları ekledikten sonra belki de gözatın ve yönetici olarak rahatsız gerek kalmadan bu uygulamaları kendi access Panel eklemek, kullanıcılarınızın izin vermek istediğiniz. Şimdi, bu en son sürüm ile yapılandırın ve Self Servis uygulamaya erişim hakkı yeni Portalı'ndan yönetin.

  ![Self Servis uygulama erişimi için bir parola SSO uygulama yapılandırma](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Yapılandırma ve Self Servis uygulama erişimi yönetmek için**:

1. Başlamak için seçebileceğiniz **Self Servis** uygulama seçeneğinden sol gezinti menüsünde ve ayarlama **bu uygulamaya erişmek kullanıcıların?** için seçenek '**Evet**'. 
2. Bu, bu uygulamaya erişim onaylama yetkisi olan kişileri ve hangi grubu Self Servis kullanıcıları eklenecek yapılandırmanıza olanak sağlar. Ayrıca, uygulama parolası çoklu oturum açma için yapılandırılmışsa, isteğe bağlı olarak uygulamaya atanan parolaları yönetmek bu onaylayanlar olanak sağlayan başka bir seçenek de görürsünüz.

## <a name="feedback"></a>Geri Bildirim

Geliştirilmiş kullanarak gibi umuyoruz Azure AD deneyimi. Gelen geri bildirim unutmayın! Geri bildirim ve fikir geliştirme için post **Yönetici portalı** bölümünü bizim [geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Biz, her gün harika yeni hizmetler oluşturma hakkında heyecan ve şekil, kılavuzlar kullanabilir ve sonraki geliştirmemiz ne tanımlayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı için bkz: [Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md).



