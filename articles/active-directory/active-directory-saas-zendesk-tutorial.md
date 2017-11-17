---
title: "Öğretici: Azure Active Directory Tümleştirme ile Zendesk | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Zendesk arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: ebf07218a6b356d71af51383ac85933ec63b543b
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Öğretici: Zendesk Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zendesk tümleştirmek öğrenin.

Zendesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zendesk erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Zendesk (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Zendesk ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zendesk çoklu oturum açma abonelik etkin


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zendesk ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-zendesk-from-the-gallery"></a>Galeriden Zendesk ekleme
Azure AD Zendesk tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zendesk eklemeniz gerekir.

**Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zendesk**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Sonuçlar panelinde seçin **Zendesk**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zendesk sınayın.

Tekli çalışmaya oturum için Azure AD Zendesk karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Zendesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Zendesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma Zendesk ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zendesk test kullanıcısı oluşturma](#creating-a-zendesk-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Zendesk sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zendesk uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Zendesk yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zendesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Üzerinde **Zendesk etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak değeri yazın:`https://<subdomain>.zendesk.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, şu biçimi kullanarak değeri yazın:`<subdomain>.zendesk.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcı URL'sini ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [Zendesk destek ekibi](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) bu değerleri almak için. 

4. Zendesk SAML onaylar belirli bir biçimde bekliyor. Zorunlu SAML özniteliklere vardır ancak özniteliği isteğe bağlı olarak ekleyebileceğiniz **kullanıcı öznitelikleri** izleyerek bölümü aşağıdaki adımları: 

     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Tıklatın **görünümü ve tüm diğer özniteliklerle düzenleme** onay kutusu.
     
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Tıklatın **özniteliği eklemek** açmak için **Ekle özniteliği** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. İçinde **adı** metin kutusuna, öznitelik adı yazın (örneğin **emailaddress**).
    
    d. Gelen **değeri** listesinde, öznitelik değeri seçin (olarak **user.mail**).
    
    e. Tıklatın **Tamam**
 
    > [!NOTE] 
    > Uzantı öznitelikleri, varsayılan olarak Azure AD'de olmayan öznitelikler eklemek için kullanın. Tıklatın [SAML ayarlanabilir kullanıcı öznitelikleri](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tam listesini almak için SAML öznitelikleri **Zendesk** kabul eder. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Üzerinde **Zendesk yapılandırma** 'yi tıklatın **yapılandırma Zendesk** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. Farklı web tarayıcısı penceresinde Zendesk şirket sitenize yönetici olarak oturum açın.

8. Tıklatın **yönetici**.

9. Sol gezinti bölmesinde **ayarları**ve ardından **güvenlik**.

10. Üzerinde **güvenlik** sayfasında, aşağıdaki adımları gerçekleştirin: 
   
     ![Güvenlik](./media/active-directory-saas-zendesk-tutorial/ic773089.png "güvenlik")

    ![Çoklu oturum açma](./media/active-directory-saas-zendesk-tutorial/ic773090.png "çoklu oturum açma")

     a. Tıklatın **yönetici & aracıları** sekmesi.

     b. Seçin **çoklu oturum açma (SSO) ve SAML**ve ardından **SAML**.

     c. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan. 

     d. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
        
     e. İçinde **sertifika parmak izi** metin kutusuna, Yapıştır **parmak izi** Azure portalından kopyaladığınız sertifika değeri.
     
     f. **Kaydet** düğmesine tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 

### <a name="creating-a-zendesk-test-user"></a>Zendesk test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açmanız **Zendesk**, içine sağlanmalıdır **Zendesk**.  
Uygulamalar atanan role bağlı olarak, bu beklenen bir davranıştır:

 1. **Son kullanıcı** hesapları otomatik olarak oturum açarken sağlandı.
 2. **Aracı** ve **yönetici** hesapları el ile olarak sağlanması gerekir **Zendesk** oturum açmadan önce.
 
**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Zendesk** Kiracı.

2. Seçin **müşteri listesi** sekmesi.

3. Seçin **kullanıcı** sekmesine ve tıklayın **Ekle**.
   
    ![Kullanıcı Ekle](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Kullanıcı Ekle")
4. Sağlamak istediğiniz ve ardından bir var olan bir Azure AD hesabının e-posta adresini yazın **kaydetmek**.
   
    ![Yeni kullanıcı](./media/active-directory-saas-zendesk-tutorial/ic773633.png "yeni kullanıcı")

> [!NOTE]
> API sağlama AAD kullanıcı hesaplarına Zendesk tarafından sağlanan veya herhangi diğer Zendesk kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Zendesk için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Zendesk için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zendesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zendesk parçasında tıklattığınızda, otomatik olarak Zendesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
