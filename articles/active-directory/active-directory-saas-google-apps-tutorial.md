---
title: "Öğretici: Google Apps Azure Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Google Apps arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Öğretici: Google Apps Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Google Apps tümleştirmek öğrenin.

Google Apps Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Google Apps erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Google uygulamalara (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Google Apps ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Google Apps çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Video öğretici
Çoklu oturum açma Google Apps için 2 dakika içinde etkinleştirmek nasıl:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular
1. **S: Azure AD çoklu oturum açma ile uyumlu Chromebooks ve diğer Chrome aygıtları misiniz?**
   
    A: Evet kullanıcıları Azure AD kimlik bilgilerini kullanarak Chromebook cihazlarını imzalayabilirsiniz. Bu bkz [Google Apps destek makalesi](https://support.google.com/chrome/a/answer/6060880) neden hakkında bilgi için kimlik bilgilerini iki kez kullanıcılardan.

2. **S: ı çoklu oturum açma etkinleştirirseniz, kullanıcıların Google sınıf, GMail, Google sürücü, YouTube vb. gibi herhangi bir Google ürünü oturumu açmak için Azure AD kimlik bilgilerini kullanmak erişebilecek mi?**
   
    A: Evet hangisini seçtiğinize bağlı [hangi Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) etkinleştirmek veya kuruluşunuz için devre dışı bırakmak seçin.

3. **S: Google Apps Kullanıcılarım yalnızca bir kısmı için çoklu oturum açmayı etkinleştir?**
   
    A: Hayır çoklu oturum açma üzerinde hemen kapatma kendi Azure AD kimlik bilgileriyle kimlik doğrulaması tüm Google Apps kullanıcılarınızın gerektirir. Google Apps birden çok kimlik sağlayıcısı sahip desteklemediğinden, Google Apps ortamınız için kimlik sağlayıcısı ya da Azure AD olabilir veya Google--ancak ikisini aynı anda.

4. **S: bir kullanıcının Windows oturum açtığı, bunlar otomatik olarak Google Apps için bir parola girmesi istenir alma olmadan kimlik doğrulaması olur mu?**
   
    Y: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Windows 10 cihazlara üzerinden oturum [Azure Active Directory katılım](active-directory-azureadjoin-overview.md). Alternatif olarak, kullanıcıların etki alanına katılmış bir şirket içi Active Directory'ye Azure ad çoklu oturum açma için etkinleştirilen Windows cihazları oturum bir [Active Directory Federasyon Hizmetleri (AD FS)](active-directory-aadconnect-user-signin.md) dağıtım. Her iki seçenek Azure AD arasında çoklu oturum açmayı etkinleştirmek için aşağıdaki öğreticide adımları gerçekleştirmek ihtiyaç duyduğunuz ve Google Apps.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Google Apps Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-google-apps-from-the-gallery"></a>Google Apps Galeriden ekleme
Azure AD Google Apps tümleştirilmesi yapılandırmak için Google Apps Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Google Apps Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Google Apps**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. Sonuçlar panelinde seçin **Google Apps**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Google "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı uygulamalar ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Google Apps içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Google Apps ilgili kullanıcı arasındaki bağlantıyı ilişki kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Google Apps içinde.

Yapılandırma ve Azure AD çoklu oturum açma Google Apps ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Google Apps test kullanıcısı oluşturma](#creating-a-google-apps-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Google Apps sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Google Apps uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Google Apps ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Google Apps** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Üzerinde **Google Apps etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Bu değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. kişi [Google destek ekibi](https://www.google.com/contact/).
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifikayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. Üzerinde **Google Apps Yapılandırması** 'yi tıklatın **yapılandırma Google Apps** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML çoklu oturum açma hizmet URL'si ve değişiklik parola URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. Tarayıcınızda yeni bir sekme açın ve oturum [Google Apps Yönetici Konsolu](http://admin.google.com/) yönetici hesabını kullanarak.

8. Tıklatın **güvenlik**. Bağlantıyı görmüyorsanız, altında gizli olabilir **daha fazla denetim** ekranın altındaki menüsü.
   
    ![Güvenlik'e tıklayın.][10]

9. Üzerinde **güvenlik** sayfasında, **çoklu oturum açmayı kurduğunuzda (SSO).**
   
    ![SSO'ı tıklatın.][11]

10. Aşağıdaki yapılandırma değişikliklerini gerçekleştirin:
   
    ![SSO yapılandırın][12]
   
    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan Google Apps, değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. İçinde **oturum kapatma sayfası URL'si** alan Google Apps, değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan. 

    d. İçinde **değiştirmek parola URL'si** alan Google Apps, değerini yapıştırın **değiştirmek parola URL'si**, Azure portalından kopyalanan. 

    e. Google Apps içinde için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-google-apps-test-user"></a>Google Apps test kullanıcısı oluşturma

Bu bölümün amacı, Google Apps yazılımda Britta Simon adlı bir kullanıcı oluşturmaktır. Google Apps otomatik sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümdeki hiçbir şey yoktur. Bir kullanıcı zaten Google Apps yazılımda yoksa, Google Apps yazılım erişmeyi denediğinde yeni bir tane oluşturulur.

>[!NOTE] 
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google destek ekibi](https://www.google.com/contact/).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Google Apps erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Google Apps Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Google Apps**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Adresinden erişim Paneli'nde tek oturum açma ayarlarınızı sınamak için bu bölümde, açık [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), test hesaba oturum ve tıklatın **Google Apps** döşeme erişim panelinde.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png