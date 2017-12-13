---
title: "Öğretici: Azure Active Directory Tümleştirme ile Cisco Webex | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Cisco Webex arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 835b7c139fc466719489c6fd1986dcbf16de549f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Öğretici: Cisco Webex Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cisco Webex tümleştirmek öğrenin.

Cisco Webex Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Webex erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Cisco Webex açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Cisco Webex ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Cisco Webex çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Cisco Webex Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex Galeriden ekleme
Azure AD Cisco Webex tümleştirilmesi yapılandırmak için Cisco Webex Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Cisco Webex Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cisco Webex**seçin **Cisco Webex** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Cisco Webex sonuçlar listesinde](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon" adlı bir test kullanıcı tabanlı Webex ile test etme.

Tekli çalışmaya oturum için Azure AD Cisco Webex karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Cisco Webex ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Cisco Webex içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma ile Cisco Webex sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cisco Webex test kullanıcısı oluşturma](#create-a-cisco-webex-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cisco Webex sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cisco Webex uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Cisco Webex yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Cisco Webex** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. Üzerinde **Cisco Webex etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cisco Webex etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.webex.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`http://www.webex.com`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Cisco Webex istemci destek ekibi](https://www.webex.co.in/support/support-overview.html) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_400.png)
    
6. Üzerinde **Cisco Webex yapılandırma** 'yi tıklatın **yapılandırma Cisco Webex** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. Farklı web tarayıcısı penceresinde Cisco Webex şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **Site Yönetimi**.

    ![Site Yönetimi](./media/active-directory-saas-cisco-webex-tutorial/ic777621.png "Site Yönetimi")

9. İçinde **sitesi yönetme** 'yi tıklatın **SSO yapılandırma**.
   
    ![SSO yapılandırma](./media/active-directory-saas-cisco-webex-tutorial/ic777622.png "SSO yapılandırma")

10. İçinde **Federe Web SSO Yapılandırması** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SSO yapılandırma Federasyon](./media/active-directory-saas-cisco-webex-tutorial/ic777623.png "Federasyon SSO yapılandırma")  

    a. Gelen **Federasyon Protokolü** listesinde **SAML 2.0**.

    b. Olarak **SSO profili**seçin **SP Intiated**.

    c. İndirilen sertifikanızı Not Defteri'nde açın ve içeriğini kopyalayın.

    d. Tıklatın **SAML meta verileri içeri aktarma**ve ardından sertifika kopyalanan içeriğini yapıştırın.

    e. İçinde **veren SAML (IDP kimliği) için** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    f. İçinde **müşteri SSO hizmeti oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    g. Gelen **NameID biçimi** listesinde **e-posta adresi**.

    h. İçinde **AuthnContextClassRef** metin kutusuna, türü **urn: OASIS: adları: tc: SAML:2.0:ac:classes:Password**.

    ı. İçinde **müşteri SSO hizmet oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyalanan.
   
    j. Tıklatın **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-cisco-webex-test-user"></a>Cisco Webex test kullanıcısı oluşturma

Cisco Webex oturum açmak Azure AD kullanıcıları etkinleştirmek için bunların Cisco Webex sağlanmalıdır. Cisco Webex söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Cisco Webex** Kiracı.

2. Git **kullanıcıları yönetme \> kullanıcı ekleme**.
   
    ![Kullanıcıları ekleme](./media/active-directory-saas-cisco-webex-tutorial/ic777625.png "kullanıcı ekleme")

3. Kullanıcı Ekle bölüm üzerinde aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı Ekle](./media/active-directory-saas-cisco-webex-tutorial/ic777626.png "Kullanıcı Ekle")   

    a. Olarak **hesap türü**seçin **konak**.

    b. İçinde **ad** metin kutusu, kullanıcı Britta gibi ilk türünün adı.

    c. İçinde **Soyadı** metin kutusuna, son kullanıcı Simon gibi yazın.

    d. İçinde **kullanıcıadı** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    e. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    f. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.

    g. İçinde **Onayla** parola metin kutusu, kullanıcı parolasını yeniden girin.

    h. **Ekle**'ye tıklayın.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Cisco Webex tarafından sağlanan veya herhangi diğer Cisco Webex kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Cisco Webex erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Cisco Webex Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Cisco Webex**.

    ![Uygulamalar listesinde Cisco Webex bağlantı](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Cisco Webex parçasında tıklattığınızda, otomatik olarak Cisco Webex uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_203.png

