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
ms.openlocfilehash: fefc508679a309262d07a582fc8f5bdf9f67cfe5
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310129"
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal Uygulama Yönetimi'ndeki yenilikler 

Azure Active Directory (Azure AD) ile yeni özellikler ve yetenekler uygulamaları yönetme daha kolay ve verimli hale getirmek için kurumsal uygulama yönetimi araçları Gelişmiş.

Bazıları şunlardır iyileştirmeler için Azure AD'de [Azure portalında](https://portal.azure.com).

- Geliştirilmiş uygulama Galerisi, Basitleştirilmiş uygulama oluşturma modeliyle deneyimi ve için kullanılan tüm uygulama türleri için destek. 
- Yardımcı olabilecek yeni hızlı deneyimi elde, uygulamanızın bir pilot ile devam edebiliriz. 
- Self Servis ilkeleri, yalnızca birkaç tıklama ile yapılandırın. 
- Uygulama proxy'si geliştirmeleri, çoklu oturum açma yapılandırması ve daha fazlasını daha önce elde etmenize imkan sağlar, kendi uygulama deneyimleri getirin.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Azure Active Directory Uygulama Galerisi geliştirmeleri

İçinden olup olmadığını sık kullanılan uygulamalarınıza ekleyin [uygulama Galerisi](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery), buluta genişletme özel uygulamalar veya yeni uygulamalar geliştirme.  Bu yeni deneyim tıklayarak oluşturabileceğinize dair **Ekle** altında **kurumsal uygulamalar** veya **tüm uygulamaları**.
 
  ![Bir uygulama eklendiğinde](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Bir kez galeride ön merkezi kullanıcı sağlamayı destekleyen tüm öne çıkan uygulamalar görüntülendiğini göreceksiniz. Verdiğiniz uygulamaları incelemek için farklı kategorilerdeki her türlü göz atabilir veya arama deneyimini hızla tümleştirmek istediğiniz uygulamaları bulmak için kullanabilirsiniz.

  ![Uygulama Galerisi](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Tek bir yerden özel uygulamalar ekleme

Önceden tümleştirilmiş uygulamalar Galeriden eklemenin yanı sıra, tüm özel uygulama yapılandırması için Klasik Yönetim Portalı'nda kullandığınız deneyimleri artık yeni portalda kullanılabilir. Olup kendi parolasını ya da Federasyon SSO uygulama, tümleştirme uygulama proxy'sini kullanarak şirket içi bir uygulamadan genişletme veya uygulama kayıt defterini kullanarak yeni bir uygulama oluşturma, için tüm bu tek tek bir yerden ulaşabilirsiniz.

  ![Kendi uygulamanızı ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Kendi uygulamanızı eklemeye başlamak için**:

1. Tıklayın **kendi bağlantı eklemek** üst uygulama Galerisi. 
1. Önünde iki seçenek görürsünüz: **mevcut bir uygulamayı dağıtmak** veya **yeni bir uygulama geliştirin**. İçin okumaya devam bunları nasıl kullanacağınızı iki seçenek arasındaki farkı öğrenin.

### <a name="deploying-existing-applications"></a>Mevcut uygulamaları dağıtma

1. Zaten çalışan bir uygulama var ve bu Azure AD çoklu oturum üzerinde tümleştirin veya sağlama, seçmek yalnızca istediğiniz **mevcut bir uygulamayı dağıtmak** seçeneği. Uygulamanız için bir ad seçin, tıklayın **Ekle**.
1. İşte bu kadar! Önden uygulamanızla ilgili tüm ayrıntıları bilmek zorunda kalmadan, yeni uygulamanızı sol menüsü üzerinden giderek ve dilediğiniz zaman dilediğiniz gibi uygulamayı yapılandırma şeklini artık ayarlayabilirsiniz.

  ![Tek bir tıklamayla mevcut bir uygulama eklendiğinde](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Yeni uygulamaları geliştirme

1. Yeni bir uygulama geliştiriyorsanız, uygulama kayıt defterine Galeriden doğru hale getirmek kolay bir yol vardır:
1. Tıklayın **kendi uygulamanızı ekleyin** seçeneği uygulama galerisinden seçim **mevcut uygulama geliştirme**, uygulama ekleme deneyimi için şu hızlı bağlantı göreceksiniz.

  ![Birkaç tıklamayla yeni geliştirilen bir uygulama ekleme](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Uygulama kayıt defterini kullanarak bir uygulama ekledikten sonra bunu burada erişim ilkelerini yeni uygulamanız için çoklu oturum açma yapılandırmanızı ve yönetmenizi mümkün olacaktır kurumsal uygulamalar listesinde görünmesi görürsünüz.

  ![Yeni uygulamanızı Kurumsal uygulamalara erişimi yönetme](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quickstart-get-going-with-your-new-application-right-away"></a>Hızlı Başlangıç: yeni uygulamanızla hemen harekete geçin 

Bir uygulama, önceden tümleştirilmiş veya kendi uygulamanızı ekledikten sonra yeni uygulamalar deneyiminde hızla üzerindeki olan bağlılığımızı temel almak için özel olarak uyarlanmış bir hızlı başlangıç deneyimi oluşturduk. Her seçeneği sistematik olarak izlerseniz, biz kullanıcı Arabirimi aracılığıyla izlemek ve uygulamanızın yeni bir pilot ile mümkün olan en kısa sürede kullanmaya başlamak nasıl göster. 
 
  ![Yeni uygulamaları hızlı başlangıç deneyimi](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 Tıklayarak dilediğiniz zaman ve herhangi bir uygulama için yeni Bu hızlı başlangıç deneyiminin ziyaret edebilirsiniz **hızlı** uygulama sol gezinti menüsünde.


## <a name="updated-application-proxy-configuration"></a>Güncelleştirilmiş uygulama ara sunucusu yapılandırması

Şimdi, say eklediğiniz yeni uygulamalardan biri, şirket içi ortamınızda şimdi çalışıyor ve Azure AD ile tümleştirmek istediğiniz.  Yeni uygulama yapılandırma deneyimi hakkında yeni harika şeylerden biri yeni Azure AD'de uygulamanın oturum açma modundan uygulama proxy yapılandırmasını ayırarak, artık kolayca parola SSO veya Federasyon uygulamalarına getirebilir, portal olduğu uygulama birden çok örneğini oluşturmak zorunda kalmadan, bulutta, şirket ağına sağ çalıştıran.

Yerel Windows kimlik doğrulama deneyimleri destekleyen uygulamalar da dahil olmak üzere yeni portalı, Azure AD uygulama proxy'si sağ ile kullanmak için eklediğiniz yeni uygulamalardan herhangi biri de yapılandırabilirsiniz.

  ![Tümleşik Windows kimlik doğrulaması oturum açma seçeneğini kullanmak için uygulamayı yapılandırma](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 
Uygulama proxy'si ile yerel bir Windows kimlik doğrulaması uygulama yapılandırmaya başlamak için:
1. Çoklu oturum açma Gezinti öğeye tıklayıp seçin **tümleşik Windows kimlik doğrulaması** oturum açma ayarları altında ve dilediğiniz gibi ayarları yapılandırın.
1. Bu yeni kimlik doğrulama modlarını destekleme üzerinde şimdi de kuruluşunuzdaki güvenli uç üzerinde çalışan uygulamaları desteklemek için özel etki alanları sertifikaları karşıya yükleyebilirsiniz.  
 
   ![Uygulama Ara sunucusu ile kullanılacak bir sertifika karşıya yükleniyor](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

1. Sık kullanılan şirket içi uygulamanız için yeni bir sertifikayı karşıya yüklemek için tıklayın **uygulama proxy'si** seçeneği sol gezinti menüsünden, tıklayın **sertifika** Seçici ve bir sertifika yükleyin dosyayı şifrelemek için kullanabilirsiniz, müşterilerimize bulut uç noktasına ait uygulamanızı ister.

## <a name="advanced-federated-single-sign-on-configuration"></a>Gelişmiş Federasyon çoklu oturum açma yapılandırması

Bu, Federasyon uygulamalarına bugün kullanarak vardır birçok yeni özellik SAML tabanlı oturum açma yapılandırmasında. İle başlamak artık tam olarak özelleştirme, ekleme, kaldırabileceğiniz ve harita SAML belirteçlere talep olarak verilen mevcut kullanıcı öznitelikleri.
 
  ![Bir federasyon uygulaması için geçirilen SAML belirteci kullanıcı öznitelikleri özelleştirme](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)

Yeni SSO yapılandırma Federal denetlemek için:
1. Bir Federasyon uygulamanın açın **çoklu oturum açma** emin olun ve sol gezinti menüsünde '*SAML tabanlı oturum açma** modu seçili. 
1. Bir kez, onay kutusunun etkinleştirme **kullanıcı öznitelikleri** başlık tüm SAML belirtecinde bulunan özniteliklerini değiştirmek için bu uygulamaya geçirilen.

Ayrıca oluşturma, geçiş ve Federasyon çoklu oturum açma için kullanılan sertifikaları yönetme, yapabilir Düzenle sertifikanızın süresi dolmak üzere olduğunda kimin bildirim. Bu yeni seçenekler altında görürsünüz **sertifikaları** aynı tek oturum açma bölmenin başlık.
 
  ![Süre sonu bildirimi e-posta ve sertifika imzalama seçenekleri özelleştirme, yeni bir sertifika oluşturma](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-parameter"></a>Geçiş durumu parametresi
Son olarak, biz de içerecek şekilde destekliyoruz SAML URL parametreleri kümesini Genişletilmiş **geçiş durumu parametresi**, kullanıcılarınızın kavuşmak üzerinde bir federasyon uygulaması içinde oturum açma işlemi tamamlandıktan sonra sayfa olduğu. Bu, kullanıcılarınızın bunları hızlı bir şekilde giderek almak için uygulama içinde belirli bir yere göndermek istiyorsanız yapılandırmak için yararlı bir ayardır.

  ![SAML geçiş durumu parametre ayarı](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Geçiş durumu parametresini ayarlamak için**:

1. Etkinleştirme **Gelişmiş URL ayarlarını göster** altındaki onay kutusunu **etki alanı ve URL'ler** üzerinde çoklu oturum açma yapılandırma bölmesi başlığı. 
1. Yeni URL kümesi kutuları, bu parametre ve diğer SAML URL ayarları ayarlamanıza izin görünen girin.

## <a name="bring-your-own-password-sso-applications"></a>Kendi parola SSO uygulamalarınızı getirin

Her uygulamanın çıktığı Federasyon desteklediğini biliyoruz. Örneğin, belki de eklediğiniz yeni uygulamalarından birini kullanıcılarınızın oturum açmak için bir kullanıcı adı ve parola kullanan bir özel oturum açma ekranı sahiptir. Azure AD kullanarak bu tür uygulamalar yine de tümleştirebilirsiniz bizim **kendi uygulamalarınızı getirin** özellik, yeni portalda yapılandırmanız için kullanıma sunulmuştur.
 
  ![Özel parola vaulting Azure AD ile uygulamaları tümleştirme](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**'Kendi uygulamalarınızı getirin' özelliği denetlenecek**:

1. Yeni özel bir uygulama için tek oturum açma modu ayarlandıktan sonra için eklediğiniz **parola tabanlı oturum açma**, uygulamanın, oturum açma ekranı burada işler URL'sini girin ve 
1. **Kaydet**’e tıklayın.  
1. Bunu yaptığınızda, biz otomatik olarak bir kullanıcı adı için bu URL'yi scrape ve parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısını kullanırken, uygulama parolaları güvenli bir şekilde aktarmaya kullanmanıza izin verir.

## <a name="configure-self-service-application-access"></a>Self Servis uygulama erişimi yapılandırma

Çok sayıda yeni uygulamaları ekledikten sonra belki de göz atın ve bir yönetici olarak rahatsız gerek kalmadan kendi erişim panelleri üzerinden bu uygulamaları eklemek, kullanıcılarınızın izin vermek istediğiniz. Şimdi, bu en son sürüm ile yapılandırın ve sağa yeni Portalı'ndan Self Servis uygulama erişimini yönetme.

  ![Self Servis uygulama erişimi için bir parola SSO uygulaması yapılandırma](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Self Servis uygulama erişimi yapılandıracağınız ve yöneteceğiniz için**:

1. Başlamak için seçebileceğiniz **Self Servis** uygulamadan seçeneği sol gezinti menüsünde ve ayarlama **kullanıcıların bu uygulamaya erişim istemesine izin?** seçeneğini '**Evet**'. 
1. Bu, bu uygulamaya erişimi onaylama yetkisi olan kişileri ve hangi grubu Self Servis kullanıcıları eklenecek yapılandırmanıza olanak sağlar. Ayrıca, uygulama parolası çoklu oturum açma için yapılandırılmışsa, isteğe bağlı olarak uygulamaya atanmış parolaları yönetmek bu onaylayanlar olanak sağlayan başka bir seçenek de görürsünüz.

## <a name="feedback"></a>Geri Bildirim

Geliştirilmiş kullanma gibi umuyoruz Azure AD deneyimi. Lütfen geri bildirim yakında tutun! Geri bildirim ve geliştirme için fikir gönderin **Yönetici portalı** bölümünü bizim [geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Size her gün harika yeni öğeler oluşturma hakkında çok heyecanlıyız ve kılavuzunuzu şekle kullanın ve sonraki geliştirmemiz ne tanımlayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı için [Azure Active Directory ile uygulamaları yönetme](manage-apps/what-is-application-management.md).



