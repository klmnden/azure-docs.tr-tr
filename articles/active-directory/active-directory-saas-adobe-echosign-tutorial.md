---
title: 'Öğretici: Azure Active Directory Tümleştirme Adobe işaretiyle | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Adobe oturum arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: 71aa0af2b3b47c1d9960e72aa36c2d5aae80f140
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Öğretici: Azure Active Directory Tümleştirme Adobe oturum

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe oturum tümleştirmek öğrenin.

Adobe oturum Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe oturum erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Adobe oturum açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Adobe işaretiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Adobe oturum çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Adobe oturum ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adobe-sign-from-the-gallery"></a>Galeriden Adobe oturum ekleme
Azure AD Adobe oturum tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe oturum eklemeniz gerekir.

**Adobe oturum Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Adobe oturum**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Sonuçlar panelinde seçin **Adobe oturum**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Adobe işaretiyle test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Adobe oturum içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Adobe oturum ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Adobe oturum açma değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Adobe işaretiyle sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Adobe oturum test kullanıcısı oluşturma](#creating-an-adobe-sign-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Adobe oturum sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Adobe oturum uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Adobe işaretiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Adobe oturum** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. Üzerinde **Adobe oturum etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.echosign.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. Üzerinde **Adobe oturum yapılandırma** 'yi tıklatın **yapılandırma Adobe oturum** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 

7. Farklı web tarayıcısı penceresinde Adobe oturum şirket sitenize yönetici olarak oturum açın.

8. SAML menüye tıklayın **hesap ayarlarını**ve ardından **SAML ayarları**.
   
    ![Hesap](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "hesabı")

9. İçinde **SAML ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
  
    ![SAML ayarları](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML ayarları")
   
    a. Olarak **SAML modu**seçin **SAML zorunlu**.
   
    b. Seçin **Adobe oturum kimlik bilgilerini kullanarak oturum açması Adobe izin oturum hesap yöneticileri**.
   
    c. Olarak **kullanıcı oluşturma**seçin **otomatik olarak SAML kimliği doğrulanmış kullanıcılar eklemek**.

    d. Yapıştır **SAML varlık kimliği**, Azure portalından kopyalanan **varlık kimliği/sertifika veren URL** metin kutusu.
    
    e. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **oturum açma URL'si/SSO Endpoint** metin kutusu.
   
    f. Yapıştır **Sign-Out URL**, Azure portalından kopyalanan **oturum kapatma URL'si/SLO Endpoint** metin kutusu.

    g. İndirilen açmak **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **IDP sertifika** metin kutusu

    h. Tıklatın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-adobe-sign-test-user"></a>Adobe oturum test kullanıcısı oluşturma

Adobe imzalamak için oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Adobe oturum açın sağlanmalıdır. Adobe söz konusu olduğunda, sağlama el ile bir görev işaretidir.

>[!NOTE]
>API Adobe işaretiyle sağlama AAD kullanıcı hesaplarına sağlanan veya herhangi diğer Adobe oturum kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Adobe oturum** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **hesap**ve ardından, sol taraftaki gezinti bölmesinde, **kullanıcıları ve grupları**ve ardından **yeni bir kullanıcı oluşturmak**.
   
    ![Hesap](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "hesabı")
   
3. İçinde **yeni kullanıcı oluştur** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı oluşturma](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "kullanıcı oluştur")
   
    a. Tür **e-posta adresi**, **ad**, ve **Soyadı** istediğiniz ilgili metin kutularına sağlamayı geçerli bir AAD hesabının.
   
    b. Tıklatın **kullanıcı oluşturma**.

>[!NOTE]
>Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alır. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Adobe imzalamak için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Adobe oturum Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Adobe oturum**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Erişim paneli Adobe oturum parçasında tıklattığınızda, otomatik olarak Adobe oturum uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

