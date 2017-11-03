---
title: "Öğretici: Azure Active Directory Tümleştirme ile Wdesk | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Wdesk arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 37660b80cfb01d6a3105aea5ce248f1e03c46695
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Öğretici: Azure Active Directory Tümleştirme Wdesk ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Wdesk tümleştirmek öğrenin.

Wdesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Wdesk erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Wdesk (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [Uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Wdesk ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Wdesk çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Wdesk ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme
Azure AD Wdesk tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Wdesk eklemeniz gerekir.

**Galeriden Wdesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Wdesk**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. Sonuçlar panelinde seçin **Wdesk**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Wdesk ile test etme

Tekli çalışmaya oturum için Azure AD Wdesk karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Wdesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Wdesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma Wdesk ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Wdesk test kullanıcısı oluşturma](#creating-a-wdesk-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Wdesk sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Wdesk uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Wdesk yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Wdesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. Üzerinde **Wdesk etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** başlatılan modu aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

4. Denetleme **Göster Gelişmiş URL ayarları**. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan aşağıdaki adımı gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. SSO yapılandırdığınızda WDesk Portalı'ndan bu değerleri alır. 
  
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. Bir farklı web tarayıcısı penceresinde, Wdesk bir güvenlik yöneticisi olarak oturum açın.

8. Alt sol tıklayın **yönetici** ve **Hesap Yöneticisi**:
 
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. Wdesk Yöneticisi gidin **güvenlik**, ardından **SAML** > **SAML ayarları**:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. Altında **genel ayarları**, denetleme **SAML çoklu oturum açmayı etkinleştir**:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. Altında **hizmet sağlayıcı ayrıntıları**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopya **oturum açma URL'si** ve yapıştırın **oturum açma URL'si** Azure Portal'daki metin kutusu.
   
      b. Kopya **meta veri URL'sini** ve yapıştırın **tanımlayıcısı** Azure Portal'daki metin kutusu.
       
      c. Kopya **tüketici url** ve yapıştırın **yanıt URL'si** Azure Portal'daki metin kutusu.
   
      d. Tıklatın **kaydetmek** değişiklikleri kaydetmek için Azure Portal.      

12. Tıklatın **IDP ayarlarını yapılandır** açmak için **IDP ayarlarını Düzenle** iletişim. Tıklatın **Dosya Seç** bulmak için **Metadata.xml** Azure portalından, kaydettiğiniz dosyayı karşıya yükleyin.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. Tıklatın **değişiklikleri kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Azure AD kullanıcıları için Wdesk oturum açmak etkinleştirmek için bunların Wdesk sağlanmalıdır. Wdesk içinde sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Wdesk bir güvenlik yöneticisi olarak oturum açın.
2. Gidin **yönetici** > **yönetici hesabı**.

     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Tıklatın **üyeleri** altında **kişiler**.

4. Şimdi **Üye Ekle** açmak için **Üye Ekle** iletişim kutusu. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. İçinde **kullanıcı** metin kutusuna, gibi kullanıcının kullanıcı adı girin  **brittasimon@contoso.com**  tıklatıp **devam** düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  Aşağıda gösterildiği gibi ayrıntıları girin:
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    a. İçinde **e-posta** metin kutusunda, bir kullanıcı gibi e-posta girin  **brittasimon@contoso.com** .

    b. İçinde **ad** metin kutusuna, gibi kullanıcının ilk adını girin **Britta**.

    c. İçinde **Soyadı** metin kutusuna, gibi kullanıcının soyadını girin **Simon**.

7. Tıklatın **Save Member** düğmesi.  

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Wdesk için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Wdesk için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Wdesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Wdesk parçasında tıklattığınızda, otomatik olarak Wdesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

