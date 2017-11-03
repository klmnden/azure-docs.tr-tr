---
title: "Self Servis uygulamaya erişim ve Azure Active Directory ile yetkilendirilmiş Yönetimi | Microsoft Docs"
description: "Bu makalede, Self Servis uygulamaya erişim ve Azure Active Directory ile temsil edilen yönetimini etkinleştirmek açıklar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 39c62461c9659b0cb4422de88686283ba462c53b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Self Servis uygulamaya erişim ve Azure Active Directory ile yetkilendirilmiş Yönetimi
Son kullanıcılarınız için Self Servis özellikleri etkinleştirme, kurumsal BT için ortak bir senaryodur. Kullanıcılar, çok sayıda uygulamaları ve erişimi verme kararlar best-informed olan kişi çok sayıda dizin yönetici olmayabilir. Genellikle uygulamanın erişebilecek kişileri karar vermek için en iyi bir takım lideri veya diğer yönetici temsilcisi kişidir. Ancak, uygulamayı kullanan kullanıcı olduğunu ve işlerini yapmak istedikleri kullanıcının bildiği.

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor. 

Self Servis uygulamaya erişim özelliğidir [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 ve P2 directory yöneticileri izin veren lisans:

* "Daha fazla uygulama Al" bir parçasında kullanan uygulamalar için erişim istemek kullanıcıların [Azure AD erişim paneli](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Hangi uygulamaların kullanıcıların erişim istemek ayarlama
* Bir onay kullanıcıların bir uygulamaya erişim otomatik olarak atamak için gerekli olup olmadığını ayarlayın
* İsteği onaylayın ve her uygulama için erişim yöneten ayarlama

Bu özellik önceden tümleştirilmiş tüm bugün desteklenmez ve bir Federasyon ya da parola tabanlı tek destekleyen özel uygulamalar oturum açma [Azure Active Directory Uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/)Salesforce, Dropbox, Google Apps gibi uygulamalar ve daha fazlası dahil olmak üzere.
Bu makalede nasıl yapılır:

* İsteğe bağlı onay iş akışı yapılandırma dahil olmak üzere son kullanıcılar için Self Servis uygulama erişimi yapılandırma 
* Erişim yönetimi, kuruluşunuzda en uygun kişilere belirli uygulamalar için temsilci ve bunları erişim isteklerini onaylama, doğrudan erişim Seçilen kullanıcılara atamak için Azure AD erişim paneli kullanmayı etkinleştirmek ya da (isteğe bağlı) parola tabanlı çoklu oturum açma yapılandırıldığında, uygulama erişimi için kimlik bilgilerini ayarlayın.

## <a name="configuring-self-service-application-access"></a>Self Servis uygulama erişimi yapılandırma
Self Servis uygulamaya erişim sağlamak için hangi uygulamaların eklenebilir veya, son kullanıcılarınız tarafından istenen bu yönergeleri izleyin ve yapılandırılır.

1. Oturum [Klasik Azure portalı](https://manage.windowsazure.com/).

2.   Altında **Active Directory** bölümünde, dizininizi seçin ve sonra seçin **uygulamaları** sekmesi. 

3. Seçin **Ekle** düğmesine tıklayın ve seçin ve bir uygulama eklemek için Galeri seçeneğini kullanın.

4. Uygulamanızı eklendikten sonra uygulama hızlı başlangıç sayfası alırsınız. Tıklatın **yapılandırma çoklu oturum açma**, istenen tek oturum açma modunu seçin ve yapılandırmayı kaydedin. 

5. Ardından, **yapılandırma** sekmesi. Azure AD erişim Masası'ndan bu uygulamaya erişmek için kullanıcıları etkinleştirmek üzere ayarlamak **Self Servis uygulamaya erişim izin** için **Evet**.
  
  ![][1]

6. İsteğe bağlı olarak erişim istekleri için bir onay iş akışını yapılandırmak için ayarlayın **erişim vermeden önce onay gerektiren** için **Evet**. Bir veya daha fazla onaylayanlar kullanarak seçilebilir sonra **onaylayanlar** düğmesi.

  Onaylayan bir Azure AD hesapla kuruluştaki herhangi bir kullanıcı olabilir ve sağlama, lisanslama hesabından sorumlu olabilir veya bir uygulamaya erişim vermeden önce kuruluşunuzun başka bir iş sürecini gerektirir. Onaylayan bir grup sahibi bile olabilir veya daha fazla hesap grupları paylaşılan ve kullanıcıların bir paylaşılan bir hesap üzerinden erişim vermek için bu grupları atayabilirsiniz. 

  Ardından onay gerekiyorsa, kullanıcılar kendi Azure AD erişim paneli eklenen uygulama anında alır. Uygulama için ayarlarsanız, [otomatik kullanıcı sağlamayı](active-directory-saas-app-provisioning.md), veya ayarlandığına ["kullanıcı tarafından yönetilen" parola SSO modu](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), kullanıcının bir kullanıcı hesabı ve parolayı biliyor olmanız gerekir.

7. Uygulama her kullanıcı adına SSO kimlik bilgilerini ayarlamak onaylayan izin vermek için parola tabanlı çoklu oturum açma ve ardından bir seçenek kullanacak şekilde yapılandırılmış olsa da mevcuttur. Daha fazla bilgi için bölümüne bakarak [erişim yönetimi için temsilci](#delegated-application-access-management).

8. Son olarak, **Self-Assigned kullanıcılar için grubu** izin vermiş veya uygulamaya erişim atanan kullanıcılar depolamak için kullanılan grup adını gösterir. Erişim onaylayan bu grubun sahibi olur. Gösterilen grup adı yoksa, otomatik olarak oluşturulur. İsteğe bağlı olarak grup adı, varolan bir grup adı için ayarlanabilir.

9. Yapılandırmayı kaydetmek için tıklatın **kaydetmek** ekranın altındaki. Şimdi isteği erişimden bu uygulama için erişim paneli için kullanıcılar yapabilirsiniz.

10. Son kullanıcı deneyimi denemek için kuruluşunuzun Azure AD erişim paneli adresindeki https://myapps.microsoft.com, tercihen uygulama onaylayan değil farklı bir hesabı kullanarak oturum açın. 

11. Altında **uygulamaları** sekmesini tıklatın, **daha fazla uygulama alın** döşeme. Bu kutucuğu tüm için Self Servis uygulamaya erişim dizinde arama ve filtreleme soldaki uygulama kategoriye göre imkanına sahip etkinleştirilmiş uygulamalar Galerisi görüntüler. 

12. Bir uygulama üzerinde tıklatarak istek işlem başlatır. Onay işlemi gereklidir sonra uygulamayı hemen altında eklenecek **uygulamaları** sekmesinden kısa bir onay sonra. Onay gerekiyorsa, bunu belirten bir iletişim kutusu görürsünüz ve bir e-posta onaylayanlara gönderilir. Erişim paneline bir olmayan-onaylayan olarak bu istek işlemi görmek için imzalanması gerekir.

13. E-posta Azure AD erişim paneline imzalamak ve isteği onaylamak için onaylayan yönlendirir. İstek onaylandıktan (ve tanımladığınız herhangi bir özel işlem onaylayan tarafından gerçekleştirilmiş sonra), kullanıcı altında görüntülenen uygulama görür kendi **uygulamaları** burada kullanıcılar oturum açabilir içine sekmesi.

## <a name="delegated-application-access-management"></a>Temsilci uygulamaya erişim yönetimi
Uygulama erişimi onaylayıcı onaylamak veya söz konusu uygulama erişimini engellemek için en uygun kişi, kuruluşunuzda herhangi bir kullanıcı olabilir. Bu kullanıcının lisans, hesap sağlama için sorumlu olabilir veya bir uygulamaya erişim vermeden önce kuruluşunuzun başka bir iş sürecini gerektirir.

Yukarıda açıklanan Self Servis uygulamaya erişim yapılandırırken herhangi onaylayanlar bkz ek atanan uygulama **uygulamalarını yönet** erişim yöneticisi oldukları uygulamaları gösterir Azure AD erişim paneli parçasında. Bir uygulamaya tıklamak çeşitli seçeneklere sahip bir ekran gösterir.

![][2]

### <a name="approve-requests"></a>İsteği onaylama
**İsteklerini onaylama** döşeme herhangi bekleyen onayları bu uygulama için belirli görmek onaylayanlar sağlar ve burada istekleri onaylanabilir veya reddedildi onayları sekmesine yönlendirir. Her bir isteği ne yönlendiren oluşturulduğunda onaylayan de otomatik e-postaları alır.

### <a name="add-users"></a>Kullanıcıları ekleme
**Kullanıcı Ekle** döşeme onaylayanlar doğrudan seçili kullanıcıların uygulamaya erişim sağlar. Bu kutucuğu tıklatıldığında, onaylayan bir iletişim kutusu görüntülemek ve kullanıcılar kendi dizinde aramak için veren görür. Bu kullanıcının Azure AD erişim panelleri veya Office 365 gösterildikten uygulamada bir kullanıcı sonuçları ekleniyor. Kullanıcı oturum açmadan önce sağlama işlemi herhangi bir el ile kullanıcı uygulamaya gerekiyorsa, ardından onaylayan bu işlem erişim vermeden önce gerçekleştirmeniz gerekir.  

### <a name="manage-users"></a>Kullanıcıları yönetme
**Kullanıcıları yönetme** döşeme doğrudan güncelleştirmek veya hangi kullanıcıların uygulama erişimi kaldırmak onaylayanlar sağlar. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Parola SSO kimlik bilgilerini (varsa) yapılandırın
**Yapılandırma** döşeme uygulama parola tabanlı çoklu oturum açma kullanmak için BT yöneticisi tarafından yapılandırılan ve parola SSO kimlik daha önce açıklandığı şekilde ayarlamanıza olanak onaylayan yönetici izni yalnızca gösterilir. Seçili olduğunda, onaylayan atanan kullanıcıların kimlik bilgilerini nasıl yayılır için çeşitli seçenekler sunulur:

![][3]

* **Kullanıcıların kendi parolalarını oturum oturum** – bu modda, atanan kullanıcılar ne kendi kullanıcı adları ve parolalar uygulamaya ve bunları kendi ilk oturum açma sırasında uygulamaya girmesi gereken bilgilendirin. Senaryo parola SSO çalışması için karşılık gelen nerede [kullanıcıların kimlik bilgilerini yönetme](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Kullanıcılar, yönetebilirim ayrı hesaplar kullanarak otomatik olarak oturumunuz** – bu modda, atanan kullanıcılar girin veya uygulamaya özgü kimlik bilgilerini uygulamasına imzalarken biliyorsanız gerekli değildir. Bunun yerine, onaylayan kimlik bilgilerini her kullanıcı için erişim kullanarak atadıktan sonra Ayarlar **Kullanıcı Ekle** döşeme. Kullanıcı kendi erişim paneli veya Office 365 uygulamasında tıkladığında, bunlar otomatik olarak onaylayan tarafından ayarlanmış kimlik bilgilerini kullanarak oturum açtınız. Senaryo parola SSO çalışması için karşılık gelen nerede [yöneticileri kimlik bilgilerini yönetir](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Kullanıcılar, yönetebilirim tek bir hesap kullanarak otomatik olarak oturumunuz** -özel bir durum bu tüm atanan kullanıcılar tek bir paylaşılan hesabı kullanarak erişim verilmesi gerektiğinde kullanmak uygun bir durumdur. Bu özellik için en yaygın kullanım örneği sosyal medya uygulamalarla burada bir kuruluşun tek "Şirket" hesabı varsa ve birden çok kullanıcı bu hesap için güncelleştirmeleri yapmanız ' dir. Ayrıca karşılık gelen parola SSO çalışması senaryo nerede [yöneticileri kimlik bilgilerini yönetir](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Ancak, bu seçeneği belirledikten sonra onaylayan tek bir paylaşılan hesap için kullanıcı adı ve parola girmeniz istenir. Tamamlandığında, tüm atanan kullanıcıların kendi Azure AD erişim panelleri veya Office 365 uygulamasında tıklayarak, bu hesabı kullanarak oturum açtınız.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
