---
title: 'Öğretici: Azure Active Directory Tümleştirme ile SpringCM | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SpringCM arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 70d3ef14875ef7af730b41e02b5f04e3930797ac
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34351588"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Öğretici: Azure Active Directory Tümleştirme SpringCM ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SpringCM tümleştirmek öğrenin.

SpringCM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SpringCM erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için SpringCM (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SpringCM ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SpringCM çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SpringCM ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-springcm-from-the-gallery"></a>Galeriden SpringCM ekleme
Azure AD SpringCM tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SpringCM eklemeniz gerekir.

**Galeriden SpringCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SpringCM**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. Sonuçlar panelinde seçin **SpringCM**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı SpringCM ile test etme

Tekli çalışmaya oturum için Azure AD SpringCM karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SpringCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SpringCM içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SpringCM ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SpringCM test kullanıcısı oluşturma](#creating-a-springcm-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SpringCM sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SpringCM uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SpringCM yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SpringCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. Üzerinde **SpringCM etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [SpringCM istemci destek ekibi](https://knowledge.springcm.com/support) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Raw)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. Üzerinde **SpringCM yapılandırma** 'yi tıklatın **yapılandırma SpringCM** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. Farklı web tarayıcısı penceresinde oturum açın, **SpringCM** yönetici olarak şirket site.

8. Üstteki menüde'ı tıklatın **GİTMEK için**, tıklatın **Tercihler**ve ardından **hesap tercihleri** 'yi tıklatın **SAML SSO**.
   
    ![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")

9. Kimlik sağlayıcısı Yapılandırması bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı Yapılandırması](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "kimlik sağlayıcı yapılandırması")
    
    a. İndirilen Azure Active Directory sertifikanızı karşıya yüklemek için tıklayın **veren sertifika Seç** veya **değişiklik sertifikayı**.
    
    b. Yapıştır **SAML varlık kimliği** Azure portalından kopyaladığınız değeri **veren** metin kutusu.
    
    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **hizmet sağlayıcısı (SP) başlatılan Endpoint** metin kutusu.
            
    d. Seçin **SAML etkin** olarak **etkinleştirmek**.

    e. **Kaydet**’e tıklayın.
 
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-springcm-test-user"></a>SpringCM test kullanıcısı oluşturma

Azure Active Directory Kullanıcıları için SpringCM oturum açmayı etkinleştirmek için bunların SpringCM sağlanmalıdır. SpringCM söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>Daha fazla bilgi için bkz: [oluşturma ve SpringCM kullanıcı düzenleme](http://knowledge.springcm.com/create-and-edit-a-springcm-user). 

**Bir kullanıcı hesabına SpringCM sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **SpringCM** yönetici olarak şirket site.

2. Tıklatın **GOTO**ve ardından **adres defteri**.
   
    ![Kullanıcı oluşturma](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "kullanıcı oluştur")

3. Tıklatın **kullanıcı oluşturma**.

4. Seçin bir **kullanıcı rolü**.

5. Seçin **etkinleştirme e-posta Gönder**.

6. Ad, Soyadı ve ilgili metin kutularına sağlamayı istediğiniz geçerli bir Azure Active Directory kullanıcı hesabının e-posta adresini yazın.

7. Kullanıcıyı eklemek bir **güvenlik grubu**.

8. **Kaydet**’e tıklayın.

  >[!NOTE]
  >API sağlama AAD kullanıcı hesaplarına SpringCM tarafından sağlanan veya herhangi diğer SpringCM kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.  
  > 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta SpringCM için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**SpringCM için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SpringCM**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Erişim paneli SpringCM parçasında tıklattığınızda, otomatik olarak SpringCM uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

