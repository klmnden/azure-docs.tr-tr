---
title: "Öğretici: Azure Active Directory Tümleştirme ile Pluralsight | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Pluralsight arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 62429643a108665544e42001d264046b5db1ec97
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Öğretici: Azure Active Directory Tümleştirme Pluralsight ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Pluralsight tümleştirmek öğrenin.

Pluralsight Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pluralsight erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Pluralsight (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Pluralsight ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Pluralsight çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Pluralsight ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-pluralsight-from-the-gallery"></a>Galeriden Pluralsight ekleme
Azure AD Pluralsight tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Pluralsight eklemeniz gerekir.

**Galeriden Pluralsight eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Pluralsight**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. Sonuçlar panelinde seçin **Pluralsight**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Pluralsight sınayın.

Tekli çalışmaya oturum için Azure AD Pluralsight karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Pluralsight ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Pluralsight içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pluralsight ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Pluralsight test kullanıcısı oluşturma](#creating-a-pluralsight-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Pluralsight sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Pluralsight uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Pluralsight yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Pluralsight** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. Üzerinde **Pluralsight etki alanı ve URL'leri** bölümünde, aşağıdaki işlemi gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<instance name>.pluralsight.com/sso/<company name>`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Pluralsight istemci destek ekibi](mailto:support@pluralsight.com) bu değeri alınamıyor. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. Azure AD çoklu oturum açma Azure portalında etkinleştirmek ve Pluralsight uygulamada SSO yapılandırmak için bu bölümün amacı olur.

    Pluralsight uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    >Ayrıca ekleyebileceğiniz **"Benzersiz kimliği"** , kuruluşunuz için uygun uygun değeri EmployeeID veya başka bir şey gibi özniteliğiyle. Ayrıca bu gerekli öznitelik olmadığını unutmayın; Ancak, benzersiz bir kullanıcıyı tanımlamak için ekleyebilirsiniz. 

6. Gerekli eklemek için **SAML belirteci öznitelikleri**, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
   
   | Öznitelik adı | Öznitelik değeri |
   | ---| --- |
   | Ad |User.givenName |
   | Soyadı |User.surname |
   | E-posta |User.Mail |
   
   a. Tıklatın **kullanıcı özniteliği eklemek** açmak için **ekleme kullanıcı Attribure** iletişim.
    
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   b. İçinde **öznitelik adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
  
   c. Gelen **öznitelik değeri** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.
  
   d. **Tamam**’a tıklayın.    

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. Uygulamanız için yapılandırılmış SSO almak için başvurun [Pluralsight Profesyonel Hizmetler](mailTo:professionalservices@pluralsight.com) ekip ve indirilen meta veri dosyası sağlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-pluralsight-test-user"></a>Pluralsight test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Pluralsight adlı bir kullanıcı oluşturmaktır. Lütfen çalışmak [Pluralsight istemci destek ekibi](mailto:support@pluralsight.com) Pluralsight hesabında kullanıcıları eklemek için. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Pluralsight için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Pluralsight için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Pluralsight**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Pluralsight parçasında tıklattığınızda, otomatik olarak Pluralsight uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

