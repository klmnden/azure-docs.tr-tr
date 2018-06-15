---
title: 'Öğretici: Azure Active Directory Tümleştirme ile TalentLMS | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TalentLMS arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1c81d3e01170b2d134f83551911f9fcbd6e5f6e9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34351724"
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Öğretici: Azure Active Directory Tümleştirme TalentLMS ile

Bu öğreticide, Azure Active Directory (Azure AD) ile TalentLMS tümleştirmek öğrenin.

TalentLMS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TalentLMS erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için TalentLMS (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme TalentLMS ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir TalentLMS çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, burada bir aylık deneme elde edebilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden TalentLMS ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-talentlms-from-the-gallery"></a>Galeriden TalentLMS ekleme
Azure AD TalentLMS tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden TalentLMS eklemeniz gerekir.

**Galeriden TalentLMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **TalentLMS**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. Sonuçlar panelinde seçin **TalentLMS**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı TalentLMS ile test etme

Tekli çalışmaya oturum için Azure AD TalentLMS karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TalentLMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TalentLMS içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TalentLMS ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TalentLMS test kullanıcısı oluşturma](#creating-a-talentlms-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı TalentLMS sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma TalentLMS uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile TalentLMS yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TalentLMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. Üzerinde **TalentLMS etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenant-name>.TalentLMSapp.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `http://<tenant-name>.talentlms.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [TalentLMS istemci destek ekibi](https://www.talentlms.com/contact) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. Üzerinde **TalentLMS yapılandırma** 'yi tıklatın **yapılandırma TalentLMS** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. Farklı web tarayıcısı penceresinde TalentLMS şirket sitenize yönetici olarak oturum açın.

8. İçinde **hesap & ayarları** 'yi tıklatın **kullanıcılar** sekmesi.
   
    ![Hesap & ayarları](./media/active-directory-saas-talentlms-tutorial/IC777296.png "hesap & ayarları")

9. Tıklatın **çoklu oturum açma (SSO)**,

10. Çoklu oturum açma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/active-directory-saas-talentlms-tutorial/IC777297.png "çoklu oturum açma")   

    a. Gelen **SSO tümleştirme türü** listesinde **SAML 2.0**.

    b. İçinde **kimlik sağlayıcıyı (IDP)** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.
 
    c. Yapıştır **parmak izi** Azure portalına değerinden **sertifika parmak izi** metin kutusu.    

    d.  İçinde **uzaktan oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
 
    e. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    f. Aşağıdakileri doldurun: 

    * İçinde **TargetedID** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
     
    * İçinde **ad** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    
    * İçinde **Soyadı** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    
    * İçinde **e-posta** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    
11. **Kaydet**’e tıklayın.
 
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-talentlms-test-user"></a>TalentLMS test kullanıcısı oluşturma

Azure AD kullanıcıları için TalentLMS oturum açmak etkinleştirmek için bunların TalentLMS sağlanmalıdır. TalentLMS söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **TalentLMS** Kiracı.

2. Tıklatın **kullanıcılar**ve ardından **Kullanıcı Ekle**.

3. Üzerinde **Kullanıcı Ekle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/active-directory-saas-talentlms-tutorial/IC777299.png "kullanıcı ekleme")  

    a. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusuna, son kullanıcı gibi adını **Simon**.
 
    c. İçinde **e-posta adresi** metin kutusuna, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    d. Tıklatın **kullanıcı ekleme**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına TalentLMS tarafından sağlanan veya herhangi diğer TalentLMS kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta TalentLMS için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**TalentLMS için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TalentLMS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli TalentLMS parçasında tıklattığınızda, otomatik olarak TalentLMS uygulamanıza açan

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

