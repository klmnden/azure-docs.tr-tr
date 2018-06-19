---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı G paketi yapılandırma | Microsoft Docs'
description: Otomatik olarak sağlamak ve Azure AD kullanıcı hesaplarından G paketine sağlanmasını öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 7bd2cc871300ca9aa9e7cccb5222bb003d199780
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35981999"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Öğretici: G Suite otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı, size nasıl otomatik olarak sağlamak ve Azure Active Directory (Azure AD) kullanıcı hesaplarından G paketine sağlanmasını göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme G paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir G Suite çoklu oturum açma abonelik etkin
- Bir Google Apps aboneliği veya Google Cloud Platform'un abonelik.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-g-suite"></a>Kullanıcılar G paketine atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırıp sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de uygulamanızı erişmeniz karar vermeniz gerekir. Bu karara yaptıktan sonra bu kullanıcılar, uygulamanızın'ndaki yönergeleri izleyerek atayabilirsiniz [bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> Tek bir öneririz, Azure AD kullanıcısının sağlama yapılandırmayı test etmek için G Suite atanabilir. Daha sonra ek kullanıcılar ve grupları atayabilirsiniz.

> Bir kullanıcı G paketine atadığınızda, seçin **kullanıcı** veya **grup** rol ataması iletişim kutusunda. **Varsayılan erişim** rol sağlamak için çalışmıyor.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD G Suite API sağlama kullanıcı hesabına bağlanma sürecinde size kılavuzluk eder. Ayrıca oluşturmak, güncelleştirmek ve kullanıcı ve grup atama Azure AD'de göre G paketindeki atanan kullanıcı hesapları devre dışı bırakmak için sağlama hizmeti yapılandırmanıza yardımcı olur.

>[!TIP]
>Yönergeleri izleyerek, SAML tabanlı çoklu oturum açma G paketleri için etkinleştirmeyi tercih edebilirsiniz [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı Yapılandır

> [!NOTE]
> Kullanıcı G paketine sağlama otomatikleştirmek için başka bir uygulanabilir seçenek kullanmaktır [Google Apps dizin eşitleme (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS şirket içi Active Directory kimliklerinizi G paketine sağlar. Buna karşılık, çözüm Bu öğreticide, Azure Active Directory (bulut) kullanıcıları ve e-posta özellikli gruplar G paketine sağlamasını yapar. 

1. Oturum [Google Apps Yönetici Konsolu](http://admin.google.com/) yönetici hesabı ve ardından **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **daha fazla denetim** ekranın altındaki menüsü.
   
    ![Güvenlik seçin.][10]

2. Üzerinde **güvenlik** sayfasında, **API Başvurusu**.
   
    ![API Başvurusu seçin.][15]

3. Seçin **etkinleştirmek API erişimini**.
   
    ![API Başvurusu seçin.][16]

    > [!IMPORTANT]
    > G paketine sağlamak istediğiniz her kullanıcı için kendi kullanıcı adı Azure Active Directory'de *gerekir* özel bir etki alanına bağlı. Örneğin, görünüm gibi kullanıcı adları bob@contoso.onmicrosoft.com G paketi tarafından kabul edilmedi. Diğer taraftan, bob@contoso.com kabul edilir. Mevcut bir kullanıcının etki alanı Azure AD'de özelliklerini düzenleyerek değiştirebilirsiniz. Biz, aşağıdaki adımlarda Azure Active Directory ve G paketi için özel bir etki alanı ayarlama hakkında yönergeler dahil ettiğiniz.
      
4. Ardından, bir özel etki alanı adı, Azure Active Directory'ye henüz eklemediyseniz, aşağıdaki adımları uygulayın:
  
    a. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde seçin **Active Directory**. Dizin listesinde dizininizi seçin. 

    b. Seçin **etki alanı adı** sol gezinti bölmesinde ve ardından **Ekle**.
     
     ![Etki alanı](./media/google-apps-provisioning-tutorial/domain_1.png)

     ![etki alanı ekleme](./media/google-apps-provisioning-tutorial/domain_2.png)

    c. Etki alanı adınızı yazın **etki alanı adı** alan. Bu etki alanı adı için G Suite kullanmayı düşündüğünüz aynı etki alanı adı olmalıdır. Ardından **etki alanı Ekle** düğmesi.
     
     ![Etki alanı adı](./media/google-apps-provisioning-tutorial/domain_3.png)

    d. Seçin **sonraki** doğrulama sayfasına gidin. Bu etki alanına ait olduğunu doğrulamak için bu sayfada sağlanan değerlere göre etki alanının DNS kayıtlarını düzenleyin. Kullanarak doğrulamak seçebilirsiniz **MX kayıtları** veya **TXT kayıtlarının**için seçtiğiniz bağlı olarak **kayıt türü** seçeneği. 
    
    Daha kapsamlı yönergeler için Azure AD ile etki alanı adları doğrulamak, bkz: [kendi etki alanı adını Azure AD'ye ekleme](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Etki alanı](./media/google-apps-provisioning-tutorial/domain_4.png)

    e. Dizininize eklemek istediğiniz tüm etki alanları için önceki adımları yineleyin.

    > [!NOTE]
    Özel etki alanı kullanıcı sağlamak için Azure AD kaynak etki alanı adı eşleşmelidir. Bunlar eşleşmiyorsa öznitelik eşleme özelleştirme uygulayarak sorunu çözmek mümkün olabilir.


5. Azure AD ile etki alanları doğruladıktan, bunları Google Apps ile yeniden doğrulamalısınız. Google ile zaten kayıtlı değil her etki alanı için aşağıdaki adımları uygulayın:
   
    a. İçinde [Google Apps Yönetici Konsolu](http://admin.google.com/)seçin **etki alanları**.
     
     ![Etki alanları seçin][20]

    b. Seçin **bir etki alanı veya bir etki alanı diğer adı eklemek**.
     
     ![Yeni bir etki alanı Ekle][21]

    c. Seçin **başka bir etki alanı ekleme**, eklemek istediğiniz etki alanı adını yazın.
     
     ![Etki alanı adınızı yazın][22]

    d. Seçin **devam ve etki alanı sahipliği doğrulama**. Sonra etki alanı adının size ait olduğunu doğrulamak için adımları izleyin. Kapsamlı yönergeler için etki alanı Google ile doğrulamak, bkz: [Google Apps ile site sahipliği doğrulamak](https://support.google.com/webmasters/answer/35179).

    e. Google Apps için eklemek istediğiniz her ek etki alanları için önceki adımları yineleyin.
     
     > [!WARNING]
     > G Suite kiracınız için birincil etki alanı değiştirmek ve zaten var, yapılandırılan çoklu oturum açma Azure AD ile durumunda #3. adım altında yinelemek zorunda [2. adım: çoklu oturum açmayı etkinleştir](#step-two-enable-single-sign-on).
       
6. İçinde [Google Apps Yönetici Konsolu](http://admin.google.com/)seçin **yönetici rollerine**.
   
     ![Google Apps seçin][26]

7. Kullanıcı sağlamayı yönetmek için kullanmak istediğiniz hangi yönetici hesabı belirleyin. İçin **Yönetici rolü** bu hesabı, düzenleme **ayrıcalıkları** bu rol için. Tüm etkinleştirdiğinizden emin olun **yönetici API ayrıcalıkları** böylece sağlamak için bu hesabı kullanılabilir.
   
     ![Google Apps seçin][27]
   
    > [!NOTE]
    > Bir üretim ortamında yapılandırıyorsanız, en iyi uygulama olarak bir yönetici hesabı G paketindeki özellikle bu adım için oluşturmaktır. Bu hesaplar gerekli API ayrıcalıklara sahip bir yönetici rolü bunlarla ilişkilendirilmiş olması gerekir.
     
8. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tüm uygulamaları** bölümü.

9. Çoklu oturum açma için zaten G Suite yapılandırdıysanız, G Suite Örneğiniz için arama alanı kullanarak arayın. Aksi takdirde seçin **Ekle**, arayın ve sonra **G Suite** veya **Google Apps** uygulama galerisinde. Arama sonuçlarından uygulamanızı seçin ve ardından, uygulamalar listesine ekleyin.

10. Örneğiniz G paketi seçin ve ardından **sağlama** sekmesi.

11. Ayarlama **sağlama modunda** için **otomatik**. 

     ![Sağlama](./media/google-apps-provisioning-tutorial/provisioning.png)

12. Altında **yönetici kimlik bilgileri** bölümünde, select **Authorize**. Bu, yeni bir tarayıcı penceresinde bir Google yetkilendirme iletişim kutusunu açar.

13. Azure Active Directory G Suite kiracınız değişiklik izin vermek istediğinizi onaylayın. Seçin **kabul**.
    
     ![İzinleri doğrulayın.][28]

14. Azure portalında seçin **Bağlantıyı Sına** Azure AD, uygulamanızın bağlandığından emin olmak için. Bağlantı başarısız olursa G Suite hesabınızın Team yönetici izinleri olduğundan emin olun. Daha sonra deneyin **Authorize** adım yeniden uygulayın.

15. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan. Ardından onay kutusunu seçin.

16. Seçin **kaydedin.**

17. Altında **eşlemeleri** bölümünde, select **eşitleme Azure Active Directory Kullanıcıları Google Apps için**.

18. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den G paketine eşitlenen kullanıcı öznitelikleri gözden geçirin. Öznitelikleri **eşleşen** özellikleri güncelleştirme işlemleri için kullanıcı hesapları G paketindeki eşleştirmek için kullanılır. Seçin **kaydetmek** değişiklikleri kaydedilemedi.

19. Azure AD hizmeti G paketi için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları**.

20. **Kaydet**’i seçin.

Bu işlem, herhangi bir kullanıcı veya kullanıcılar ve Gruplar bölümünde G Suite atanan grupları ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada hizmet çalışırken oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlükleri sağlamak için bağlantıları izleyin. Bu günlükler uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
