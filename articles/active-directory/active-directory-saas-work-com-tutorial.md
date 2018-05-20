---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Work.com | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Work.com arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: bfc76d05a52d0283e3367f9c98dc8ed427cbe592
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Öğretici: Azure Active Directory Tümleştirme Work.com ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Work.com tümleştirmek öğrenin.

Work.com Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Work.com erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Work.com (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Work.com ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Work.com çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Work.com Ekle
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-workcom-from-the-gallery"></a>Galeriden Work.com Ekle
Azure AD Work.com tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Work.com eklemeniz gerekir.

**Galeriden Work.com eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Work.com**seçin **Work.com** sonuçları panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Galerisi'nden ekleme](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Work.com sınayın.

Tekli çalışmaya oturum için Azure AD Work.com karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Work.com ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Work.com içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Work.com ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Work.com test kullanıcısı oluşturma](#create-a-workcom-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Work.com sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Work.com uygulamanızda yapılandırın.

>[!NOTE]
>Çoklu oturum açma yapılandırmak için özel bir Work.com etki alanı adı henüz Kurulum gerekir. En az bir etki alanı adı tanımlayın, etki alanı adınızı sınamak ve kuruluşunuzun tümüne dağıtmak gerekir.

**Azure AD çoklu oturum açma ile Work.com yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Work.com** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML Tabanlı Oturum Açma](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. Üzerinde **Work.com etki alanı ve URL'leri** bölümünde, aşağıdaki işlemi gerçekleştirin:

    ![Work.com etki alanı ve URL'ler bölümü](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Work.com istemci destek ekibi](https://help.salesforce.com/articleView?id=000159855&type=3) bu değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet Düğmesi](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. Üzerinde **Work.com yapılandırma** 'yi tıklatın **yapılandırma Work.com** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Work.com yapılandırma bölümü](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Work.com Kiracı yönetici olarak oturum açın.

8. Git **Kurulum**.
   
    ![Kurulum](./media/active-directory-saas-work-com-tutorial/ic794108.png "Kurulumu")

9. Sol gezinti bölmesinde, içinde **Yönet** 'yi tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My etki alanı** açmak için **My etki alanı** sayfası. 
   
    ![Etki alanım](./media/active-directory-saas-work-com-tutorial/ic767825.png "etki alanım")

10. Etki alanınızın doğru şekilde ayarlanmış olması gerektiğini doğrulamak için içinde olduğundan emin olun "**adım 4 dağıtılan kullanıcılara**" ve gözden geçirin, "**My etki alanı ayarları**".
   
    ![Etki alanı kullanıcıya dağıtılan](./media/active-directory-saas-work-com-tutorial/ic784377.png "kullanıcıya dağıtılan etki alanı")

11. Work.com kiracınız için oturum açın.

12. Git **Kurulum**.
    
    ![Kurulum](./media/active-directory-saas-work-com-tutorial/ic794108.png "Kurulumu")

13. Genişletme **güvenlik denetimleri** menüsüne ve ardından **çoklu oturum açma ayarları**.
    
    ![Çoklu oturum açma ayarları](./media/active-directory-saas-work-com-tutorial/ic794113.png "tek oturum açma ayarları")

14. Üzerinde **çoklu oturum açma ayarları** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML etkin](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML etkin")
    
    a. Seçin **SAML etkin**.
    
    b. **Yeni**’ye tıklayın.

15. İçinde **SAML çoklu oturum açma ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Oturum açma SAML tek ayar](./media/active-directory-saas-work-com-tutorial/ic794114.png "oturum açma SAML tek ayar")
    
    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın.  
       
    > [!NOTE]
    > İçin bir değer sağlama **adı** otomatik olarak doldurulması **API adı** metin kutusu.
    
    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
    
    c. Azure Portalı'ndan indirilen sertifikayı karşıya yüklemek için tıklayın **Gözat**.
    
    d. İçinde **varlık kimliği** metin kutusuna, türü `https://salesforce-work.com`.
    
    e. Olarak **SAML kimlik türü**seçin **onaylamayı içeren kullanıcı nesnesinden Federasyon kimliği**.
    
    f. Olarak **SAML kimlik konumu**seçin **kimliktir konu deyimi NameIdentfier öğesinde**.
    
    g. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    h. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
    
    i. Olarak **hizmet sağlayıcısı tarafından başlatılan bağlama isteği**seçin **HTTP Post**.
    
    j. **Kaydet**’e tıklayın.

16. Sol gezinti bölmesinde, Work.com Klasik Portalı'nda tıklatın **etki alanı yönetimi** ilgili bölümü genişletin ve ardından **My etki alanı** açmak için **My etki alanı** Sayfa. 
    
    ![Etki alanım](./media/active-directory-saas-work-com-tutorial/ic794115.png "etki alanım")

17. Üzerinde **My etki alanı** sayfasında **oturum açma sayfası markalama** 'yi tıklatın **Düzenle**.
    
    ![Oturum açma sayfası markalama](./media/active-directory-saas-work-com-tutorial/ic767826.png "oturum açma sayfası markalama")

14. Üzerinde **oturum açma sayfası markalama** sayfasında **kimlik doğrulama hizmeti** bölümünde, adını, **SAML SSO ayarları** görüntülenir. Seçin ve ardından **kaydetmek**.
    
    ![Oturum açma sayfası markalama](./media/active-directory-saas-work-com-tutorial/ic784366.png "oturum açma sayfası markalama")

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar ->](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim sayfası](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-workcom-test-user"></a>Work.com test kullanıcısı oluşturma
Azure Active Directory kullanıcıların oturum açabilmesi için bunlar için Work.com sağlanmalıdır. Work.com söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Work.com şirket sitenize yönetici olarak oturum açma.

2. Git **Kurulum**.
   
    ![Kurulum](./media/active-directory-saas-work-com-tutorial/IC794108.png "Kurulumu")
3. Git **kullanıcıları yönetme \> kullanıcılar**.
   
    ![Kullanıcıları yönetme](./media/active-directory-saas-work-com-tutorial/IC784369.png "kullanıcıları yönetme")

4. Tıklatın **yeni kullanıcı**.
   
    ![Tüm kullanıcılar](./media/active-directory-saas-work-com-tutorial/IC794117.png "tüm kullanıcılar")

5. Kullanıcı düzenleme bölümünde, aşağıdaki adımlarda, geçerli bir Azure özniteliklerini gerçekleştirmek istediğiniz ilgili metin kutularına sağlamayı AD hesabı:
   
    ![Kullanıcı düzenleme](./media/active-directory-saas-work-com-tutorial/ic794118.png "kullanıcı düzenleme")
   
    a. İçinde **ad** metin kutusuna, türü **ad** kullanıcının **Britta**.
    
    b. İçinde **Soyadı** metin kutusuna, türü **Soyadı** kullanıcının **Simon**.
    
    c. İçinde **diğer** metin kutusuna, türü **adı** kullanıcının **BrittaS**.
    
    d. İçinde **e-posta** metin kutusuna, türü **e-posta adresi** kullanıcının **Brittasimon@contoso.com**.
    
    e. İçinde **kullanıcı adı** metin kutusuna, türü kullanıcının kullanıcı adını ister **Brittasimon@contoso.com**.
    
    f. İçinde **takma ad** metin kutusuna, bir **takma ad** kullanıcının **Simon**.
    
    g. Seçin **rol**, **kullanıcı lisansı**, ve **profil**.
    
    h. **Kaydet**’e tıklayın.  
      
    > [!NOTE]
    > Azure AD hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Work.com için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Work.com için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Work.com**.

    ![Uygulamanın listesinde Work.com](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Work.com parçasında tıklattığınızda, otomatik olarak Work.com uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

