---
title: "Öğretici: Azure Active Directory Tümleştirme Confluence Kantega SSO | Microsoft Docs"
description: "Çoklu oturum açma Kantega SSO Confluence için Azure Active Directory arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ec11decbff4cf2f6c39b40228e349312fd86da00
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Öğretici: Azure Active Directory Tümleştirme Confluence Kantega SSO

Bu öğreticide, Azure Active Directory (Azure AD) ile Kantega SSO Confluence için tümleştirme öğrenin.

Kantega SSO Confluence için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Confluence Kantega SSO erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Kantega SSO (çoklu oturum açma) Confluence için için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Confluence Kantega SSO ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik Kantega SSO Confluence çoklu oturum açma için etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Kantega SSO Confluence için ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Galeriden Kantega SSO Confluence için ekleme
Azure AD'ye Confluence Kantega SSO tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Confluence Kantega SSO eklemeniz gerekir.

**Galeriden Confluence Kantega SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Kantega SSO Confluence için**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. Sonuçlar panelinde seçin **Kantega SSO Confluence için**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Confluence Kantega SSO ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Kantega SSO Confluence için bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Kantega SSO Confluence için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Confluence için Kantega SSO içinde atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Confluence Kantega SSO sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kantega SSO Confluence test kullanıcısı için oluşturma](#creating-a-kantega-sso-for-confluence-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Confluence Kantega SSO sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve, Kantega SSO Confluence uygulama için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Kantega SSO Confluence için yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Kantega SSO Confluence için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. İçinde **IDP** üzerinde modunda başlatılan **Kantega SSO Confluence etki alanı ve URL'ler için** bölümü aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. İçinde **SP** başlatılan modu, onay **Göster Gelişmiş URL ayarları** ve aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Öğreticide daha sonra açıklanan Confluence eklentisi yapılandırma sırasında bu değerleri alma.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. Farklı web tarayıcısı penceresinde oturum açın, **Confluence Yönetici portalı** yönetici olarak.

8. Dişlisine üzerine gelin ve tıklatın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon1.png)

9. Altında **ATLASSIAN Market** sekmesini tıklatın, **Bul yeni eklentileri**. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon.png)

10. Arama **Kantega SSO Confluence SAML Kerberos için** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğmeyi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon2.png)

11. Eklentisi yüklemeyi başlatır.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon3.png)

12. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon33.png)

13. **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon34.png)
    
14. Tıklatın **yapılandırma** yeni eklenti yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon35.png)

15. Bu yeni eklenti ayrıca altında bulunabilir **kullanıcılar ve güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon36.png)
    
16. İçinde **SAML** bölümü. Seçin **Azure Active Directory (Azure AD)** gelen **Ekle kimlik sağlayıcısı** açılır.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon4.png)

17. Abonelik düzeyi olarak **temel**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon5.png)       

18. Üzerinde **uygulama özellikleri** bölümünde, şu adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon6.png)

    a. Kopya **uygulama kimliği URI'si** değer ve olarak kullanmak **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** üzerinde **Kantega SSO Confluence etki alanı ve URL'ler için** Azure portalı bölümünde.

    b. **İleri**’ye tıklayın.

19. Üzerinde **meta veri içeri aktarma** bölümünde, şu adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon7.png)

    a. Seçin **meta veri dosyası bilgisayarımda**ve Azure portalından karşıdan yükleme meta veri dosyası.

    b. **İleri**’ye tıklayın.

20. Üzerinde **adı ve SSO konumunu** bölümünde, şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon8.png)
    
    a. Kimlik sağlayıcı adını eklemek **kimlik sağlayıcı adı** textbox (örneğin Azure AD).

    b. **İleri**’ye tıklayın.

21. İmzalama sertifikası doğrulayın ve tıklatın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon9.png)

22. Üzerinde **Confluence kullanıcı hesapları** bölümünde, şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon10.png)

    a. Seçin **gerekirse Confluence'nın iç dizinde kullanıcılar oluşturma** ve kullanıcılar uygun Grup adını girin (olabilir birden çok yok. virgülle ayrılmış grupları).

    b. **İleri**’ye tıklayın.

23. **Son**'a tıklayın.   

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon11.png)

24. Üzerinde **için Azure AD etki alanları bilinen** bölümünde, şu adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon12.png)

    a. Seçin **etki alanları bilinen** sayfanın sol panelindeki.

    b. Etki alanı adı girin **etki alanları bilinen** metin kutusu.

    c. **Kaydet** düğmesine tıklayın. 

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Kantega SSO Confluence test kullanıcısı için oluşturma

Azure AD kullanıcıları için Confluence oturum açmak etkinleştirmek için bunların Confluence sağlanmalıdır. Söz konusu olduğunda Kantega SSO Confluence için sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Kantega SSO Confluence şirket site için bir yönetici olarak oturum açın.

2. Dişlisine üzerine gelin ve tıklatın **kullanıcı yönetimi**.

    ![Çalışanı ekleyin](./media/active-directory-saas-kantegassoforconfluence-tutorial/user1.png) 

3. Kullanıcılar bölümü altında tıklatın **Kullanıcı Ekle** sekmesi. Üzerinde **"Bir kullanıcı Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/active-directory-saas-kantegassoforconfluence-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin kutusu, kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklatın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklatın **Ekle** düğmesi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Confluence Kantega SSO için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Kantega SSO Confluence için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Kantega SSO Confluence için**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Confluence parçasında Kantega SSO tıklattığınızda, otomatik olarak, Kantega SSO Confluence uygulama için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_203.png

