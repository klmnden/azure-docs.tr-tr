---
title: "Öğretici: Azure Active Directory Tümleştirme TigerText güvenli Messenger ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory TigerText güvenli Messenger arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: e101e5fc84b032b66dd0636bab8bff128791f77c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Öğretici: Azure Active Directory Tümleştirme TigerText güvenli Messenger ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TigerText güvenli Messenger tümleştirmek öğrenin.

TigerText güvenli Messenger Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TigerText güvenli Messenger erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak TigerText güvenli Messenger (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme TigerText güvenli Messenger ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TigerText güvenli Messenger çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TigerText güvenli Messenger ekleme
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a>Galeriden TigerText güvenli Messenger ekleme
Azure AD TigerText güvenli Messenger tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TigerText güvenli Messenger eklemeniz gerekir.

**Galeriden TigerText güvenli Messenger eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TigerText güvenli Messenger**seçin **TigerText güvenli Messenger** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Galerisi'nden ekleme](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TigerText güvenli Messenger ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen TigerText güvenli Messenger bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TigerText güvenli Messenger ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Güvenli TigerText Messenger'ın değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TigerText güvenli Messenger ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TigerText güvenli Messenger test kullanıcısı oluşturma](#create-a-tigertext-secure-messenger-test-user)**  - Britta Simon, karşılık gelen TigerText güvenli Messenger kullanıcı Azure AD gösterimini bağlı sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TigerText güvenli Messenger uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma TigerText güvenli Messenger ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TigerText güvenli Messenger** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![SAML tabanlı oturum açma](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. Üzerinde **TigerText güvenli Messenger etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TigerText güvenli Messenger etki alanı ve URL'ler bölümü](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, türü URL'si olarak:`https://home.tigertext.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek tanımlayıcısı ile güncelleştirin. Kişi [TigerText güvenli Messenger istemci destek ekibi](mailTo:prosupport@tigertext.com) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![SAML imzalama sertifikası bölümü](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Kaydet Düğmesi](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma, uygulamanız için yapılandırılmış özelliğini almak için başvurun [TigerText güvenli Messenger destek ekibi](mailTo:prosupport@tigertext.com) ve verin **yüklenen meta veri**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Kullanıcıların ve grupların tüm kullanıcılar ->](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>TigerText güvenli Messenger test kullanıcısı oluşturma

Bu bölümde, TigerText içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen ulaşmak [TigerText güvenli Messenger istemci destek ekibi](mailTo:prosupport@tigertext.com) TigerText platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TigerText Messenger güvenli erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon TigerText güvenli Messenger atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TigerText güvenli Messenger**.

    ![TigerText güvenli Messenger'da uygulama listesi](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TigerText parçasında tıklattığınızda, otomatik olarak TigerText uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

