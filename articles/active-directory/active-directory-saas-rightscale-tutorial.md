---
title: "Öğretici: Azure Active Directory Tümleştirme ile Rightscale | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Rightscale arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 222c4414a9f736a3589b4cdd0ed934696f6c31ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Öğretici: Azure Active Directory Tümleştirme Rightscale ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Rightscale tümleştirmek öğrenin.

Rightscale Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Rightscale erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Rightscale (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Rightscale ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Rightscale çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Rightscale ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-rightscale-from-the-gallery"></a>Galeriden Rightscale ekleme
Azure AD Rightscale tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Rightscale eklemeniz gerekir.

**Galeriden Rightscale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Rightscale**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. Sonuçlar panelinde seçin **Rightscale**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Rightscale sınayın.

Tekli çalışmaya oturum için Azure AD Rightscale karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Rightscale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Rightscale içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rightscale ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Rightscale test kullanıcısı oluşturma](#creating-a-rightscale-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Rightscale sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Rightscale uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Rightscale yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Rightscale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. Üzerinde **Rightscale etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP başlatılan modu** uygulama zaten Azure ile önceden tümleştirilmiş gibi herhangi bir adım gerçekleştirmeniz gerekmez.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. Üzerinde **Rightscale etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **SP tarafından başlatılan modu**, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Tıklayın **Göster Gelişmiş URL ayarları**.

    b. İçinde **oturum üzerinde URL'si** metin kutusuna, URL'yi yazın:`https://login.rightscale.com/`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. Üzerinde **Rightscale yapılandırma** 'yi tıklatın **yapılandırma Rightscale** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS>
8. Uygulamanız için yapılandırılmış SSO almak için RightScale kiracınız yönetici olarak oturum gerekir.

    a. Üstteki menüde tıklatın **ayarları** sekmesinde ve seçin **çoklu oturum açma**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Tıklayın "**yeni**" eklemek için düğmeyi **bilgisayarınızı SAML kimlik sağlayıcısı**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. Metin kutusuna **görünen adı**, şirket adınızı girin.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Seçin **izin RightScale tarafından başlatılan SSO'yu bir bulma ipucu kullanarak** ve giriş, **etki alanı adı** içinde metin kutusu altında.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    e. Değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan **SAML SSO Endpoint** RightScale içinde.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    f. Değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan **SAML Entityıd** RightScale içinde.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    g. Tıklatın **tarayıcı** Azure portalından indirdiğiniz ve sertifikayı karşıya yüklemek için düğmeyi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    h. **Kaydet** düğmesine tıklayın.
<CE>
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-rightscale-test-user"></a>Rightscale test kullanıcısı oluşturma

Bu bölümde, RightScale içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Rightscale istemci destek ekibi](mailto:support@rightscale.com) RightScale platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Rightscale için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Rightscale için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Rightscale**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.  

Erişim paneli RightScale parçasında tıklattığınızda, otomatik olarak RightScale uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

