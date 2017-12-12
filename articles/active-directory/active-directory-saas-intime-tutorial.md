---
title: "Öğretici: Azure Active Directory Tümleştirme ile InTime | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile InTime arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 5a260338354799e7076f7475a98aa648bf73a45a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a>Öğretici: Azure Active Directory Tümleştirme InTime ile

Bu öğreticide, Azure Active Directory (Azure AD) ile InTime tümleştirmek öğrenin.

InTime Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- InTime erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için InTime (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme InTime ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir InTime çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden InTime ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-intime-from-the-gallery"></a>Galeriden InTime ekleme
Azure AD InTime tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden InTime eklemeniz gerekir.

**Galeriden InTime eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **InTime**seçin **InTime** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde inTime](./media/active-directory-saas-intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı InTime sınayın.

Tekli çalışmaya oturum için Azure AD InTime karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının InTime ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

InTime içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma InTime ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[InTime test kullanıcısı oluşturma](#create-a-intime-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı InTime sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma InTime uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile InTime yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **InTime** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-intime-tutorial/tutorial_intime_samlbase.png)

3. Üzerinde **InTime etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri inTime etki alanı ve URL'leri tek](./media/active-directory-saas-intime-tutorial/tutorial_intime_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://intime6.intimesoft.com/mytime/login/login.xhtml`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://auth.intimesoft.com/auth/realms/master`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-intime-tutorial/tutorial_intime_certificate.png) 

5. SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde SAML onaylar InTime uygulamanızı bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olan **user.userprincipalname** ancak InTime bu kullanıcının e-posta adresi ile eşlenecek bekliyor. Bunun için kullanabileceğiniz **user.mail** özniteliği listeden veya kuruluş yapılandırmanızı temel alarak uygun öznitelik değeri kullanın 

    ![Öznitelik yapılandırma](./media/active-directory-saas-intime-tutorial/tutorial_intime_attribute.png)

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-intime-tutorial/tutorial_general_400.png)

7. Üzerinde **InTime yapılandırma** 'yi tıklatın **yapılandırma InTime** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![İnTime yapılandırma](./media/active-directory-saas-intime-tutorial/tutorial_intime_configure.png) 

8. Çoklu oturum açma yapılandırmak için **InTime** yan, indirilen göndermek için ihtiyacınız **meta veri XML**, **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** için [ İnTime destek ekibi](mailto:hdollard@intimesoft.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-intime-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-intime-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-intime-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-intime-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-intime-test-user"></a>InTime test kullanıcısı oluşturma

Bu bölümde, InTime içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [InTime destek ekibi](mailto:hdollard@intimesoft.com) InTime platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta InTime için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**InTime için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **InTime**.

    ![InTime bağlantı uygulamalar listesinde](./media/active-directory-saas-intime-tutorial/tutorial_intime_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

InTime kutucuğa tıkladığınızda erişim panelinde oturum açma sayfasına InTime uygulamanızın almanız gerekir. Tıklatın **oturum açma** düğmesi, sonra da IdPs bir dizi düğmeleri listede görüntülenir. tıklatın **IDP adı** tarafından verilen [InTime destek ekibi](mailto:hdollard@intimesoft.com) InTime uygulamanıza oturum açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intime-tutorial/tutorial_general_203.png

