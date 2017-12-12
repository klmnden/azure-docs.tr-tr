---
title: "Öğretici: Azure Active Directory Tümleştirme Cezanne ik yazılımıyla | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Cezanne ik yazılımı arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 3934f814a9060adf275a4bdcc83403da4b2a4075
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Öğretici: Azure Active Directory Tümleştirme Cezanne ik yazılımıyla

Bu öğreticide, Azure Active Directory (Azure AD) ile Cezanne ik yazılım tümleştirmek öğrenin.

Cezanne HR yazılım Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cezanne HR yazılım erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Cezanne ik yazılımı açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Cezanne ik yazılımıyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Cezanne ik yazılım çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Cezanne ik yazılım ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Galeriden Cezanne ik yazılım ekleme
Azure AD Cezanne ik yazılım tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Cezanne ik yazılım eklemeniz gerekir.

**Galeriden Cezanne ik yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cezanne ik yazılım**seçin **Cezanne ik yazılım** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Cezanne ik yazılım](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Cezanne ik yazılım "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Cezanne ik yazılım bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Cezanne ik yazılım ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Cezanne HR yazılımda değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Cezanne ik yazılımıyla sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cezanne HR yazılım test kullanıcısı oluşturma](#create-a-cezannehrsoftware-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cezanne ik yazılım sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cezanne ik yazılım uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Cezanne ik yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Cezanne ik yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Üzerinde **Cezanne ik yazılım etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cezanne HR yazılım etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://w3.cezanneondemand.com/CezanneOnDemand/`

    c. İçinde **yanıt URL'si** metin kutusuna, URL'yi yazın:`https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Kişi [Cezanne ik yazılım istemci destek ekibi](https://cezannehr.com/services/support/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)

6. Üzerinde **Cezanne ik yazılım yapılandırma** 'yi tıklatın **Cezanne ik yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi.

    ![Cezanne HR yazılım yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png)

7. Ekranı aşağı kaydırarak **hızlı başvuru** bölümü. Kopya **SAML çoklu oturum açma hizmet URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Cezanne HR yazılım yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure1.png)

8. Farklı web tarayıcısı penceresinde Cezanne ik yazılım kiracınız yönetici olarak oturum.

9. Sol gezinti bölmesinde tıklatın **sistem kurulumu**. Git **güvenlik ayarlarını**. Ardından gidin **tek oturum açma yapılandırması**.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

10. İçinde **kullanıcıların aşağıdaki çoklu oturum açma (SSO) hizmet kullanarak oturum açmasına izin** paneli, onay **SAML 2.0** kutusunda ve seçin **Gelişmiş Yapılandırma** seçeneği.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

11. Tıklatın **yeni Ekle** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

12. Aşağıdaki adımları gerçekleştirin **SAML 2.0 kimlik SAĞLAYICISI** bölümü.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. Kimlik sağlayıcınız olarak adını girin **görünen adı**.

    b. İçinde **varlık tanımlayıcısı** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan. 

    c. Değişiklik **SAML bağlama** 'Gönderisine'.

    d. İçinde **güvenlik belirteci Hizmeti uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    e. Kullanıcı Kimliği öznitelik adı metin kutusuna girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Tıklatın **karşıya** Azure Portalı'ndan indirilen sertifikayı karşıya yüklemek için simge.
    
    g. **Tamam** düğmesine tıklayın. 

13. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Cezanne HR yazılım test kullanıcısı oluşturma

Azure AD kullanıcıların Cezanne ik yazılımına oturum etkinleştirmek için bunlar Cezanne ik yazılımına sağlanmalıdır. Cezanne HR söz konusu olduğunda, sağlama el ile bir görev yazılımdır.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1.  Cezanne HR yazılım şirket sitenize yönetici olarak oturum açın.

2.  Sol gezinti bölmesinde tıklatın **sistem kurulumu**. Git **kullanıcıları yönetme**. Ardından gidin **yeni kullanıcı Ekle**.

    ![Yeni kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "yeni kullanıcı")

3.  Üzerinde **kişi ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "yeni kullanıcı")
    
    a. Ayarlama **iç kullanıcı** OFF olarak.
    
    b. İçinde **ad** kullanıcı türü adı metin kutusuna, ister **Britta**.  
 
    c. İçinde **Soyadı** metin kutusuna, kullanıcının soyadını türü ister **Simon**.
    
    d. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

4.  Üzerinde **hesap bilgilerini** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "yeni kullanıcı")
    
    a. İçinde **kullanıcıadı** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.
    
    b. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.
    
    c. Seçin **ik Professional** olarak **güvenlik rolü**.
    
    d. **Tamam** düğmesine tıklayın.

5. Gidin **çoklu oturum açma** sekmesinde ve seçin **yeni Ekle** içinde **SAML 2.0 tanımlayıcıları** alanı.

    ![Kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "kullanıcı")

6. Kimlik sağlayıcınızı seçin **kimlik sağlayıcısı** ve metin kutusunda **kullanıcı tanımlayıcısı**, Britta Simon hesabının e-posta adresi girin.

    ![Kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "kullanıcı")
    
7. Tıklatın **kaydetmek** düğmesi.

    ![Kullanıcı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "kullanıcı")

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Cezanne ik yazılıma erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Cezanne ik yazılım atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Cezanne ik yazılım**.

    ![Uygulamalar listesinde Cezanne ik yazılım bağlantısı](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Cezanne ik yazılım parçasında tıklattığınızda, otomatik olarak Cezanne ik yazılım uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

