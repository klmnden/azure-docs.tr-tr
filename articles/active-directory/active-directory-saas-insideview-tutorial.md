---
title: "Öğretici: Azure Active Directory Tümleştirme ile InsideView | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile InsideView arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: f2b0a1d4bc44f8d0cd57c61e2d78950cb6a99854
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Öğretici: Azure Active Directory Tümleştirme InsideView ile

Bu öğreticide, Azure Active Directory (Azure AD) ile InsideView tümleştirmek öğrenin.

InsideView Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- InsideView erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için InsideView (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme InsideView ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir InsideView çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden InsideView ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-insideview-from-the-gallery"></a>Galeriden InsideView ekleme
Azure ad içinde InsideView tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden InsideView eklemeniz gerekir.

**Galeriden InsideView eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **InsideView**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. Sonuçlar panelinde seçin **InsideView**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı InsideView ile test etme

Tekli çalışmaya oturum için Azure AD InsideView karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının InsideView ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

InsideView içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma InsideView ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[InsideView test kullanıcısı oluşturma](#creating-a-insideview-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı InsideView sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma InsideView uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile InsideView yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **InsideView** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. Üzerinde **InsideView etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [InsideView destek ekibi ](mailto:support@insideview.com) bu değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (ham)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. Üzerinde **InsideView yapılandırma** 'yi tıklatın **yapılandırma InsideView** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. Farklı web tarayıcısı penceresinde InsideView şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **yönetici**, **SingleSignOn ayarları**ve ardından **eklemek SAML**.
   
   ![SAML çoklu oturum açma ayarları](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML çoklu oturum açma ayarları")

9. İçinde **yeni bir SAML eklemek** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni bir SAML eklemek](./media/active-directory-saas-insideview-tutorial/ic794136.png "yeni bir SAML ekleme")
   
    a. İçinde **STS adını** metin kutusuna, yapılandırmanız için bir ad yazın.

    b. İçinde **SamlP/WS-Fed istenmeyen EndPoint** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    
    c. Azure portalından indirmiş, base-64 kodlanmış sertifika açmak içeriğini, panoya kopyalayın ve yapıştırın kendisine **STS sertifikası** metin kutusu.

    d. İçinde **Crm kullanıcı kimliği eşleme** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. İçinde **Crm e-posta eşleme** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. İçinde **ilk adı Crm eşleme** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. İçinde **Crm lastName eşleme** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-insideview-test-user"></a>InsideView test kullanıcısı oluşturma

InsideView için oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar içinde InsideView için hazırlanması gerekir. InsideView söz konusu olduğunda, sağlama bir el ile bir görevdir.

Kullanıcılar veya InsideView içinde oluşturulan kişileri almak için başvurun [InsideView destek ekibi](mailto:support@insideview.com).

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için InsideView tarafından sağlanan veya herhangi diğer InsideView kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta InsideView için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**InsideView için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **InsideView**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli InsideView parçasında tıklattığınızda, otomatik olarak InsideView uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

