---
title: "Öğretici: Azure Active Directory Tümleştirme ile Zendesk | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Zendesk arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jeedes
ms.openlocfilehash: 095fdd68cafbb1bf7d753dd82821bdc2fd089ef0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Öğretici: Zendesk Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Zendesk tümleştirmek öğrenin.

Zendesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zendesk erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Zendesk (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Zendesk ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zendesk çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zendesk ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zendesk-from-the-gallery"></a>Galeriden Zendesk ekleme
Azure AD Zendesk tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zendesk eklemeniz gerekir.

**Galeriden Zendesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zendesk**seçin **Zendesk** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Zendesk](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zendesk sınayın.

Tekli çalışmaya oturum için Azure AD Zendesk karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Zendesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Zendesk içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zendesk ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zendesk test kullanıcısı oluşturma](#create-a-zendesk-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Zendesk sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zendesk uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Zendesk yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zendesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Üzerinde **Zendesk etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zendesk etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.zendesk.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, şu biçimi kullanarak değeri yazın:`<subdomain>.zendesk.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Zendesk istemci destek ekibi](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png)

5. Zendesk SAML onaylar belirli bir biçimde bekliyor. Zorunlu SAML özniteliklere vardır ancak özniteliği isteğe bağlı olarak ekleyebileceğiniz **kullanıcı öznitelikleri** izleyerek bölümü aşağıdaki adımları: 

     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma addattb yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.
 
    > [!NOTE] 
    > Uzantı öznitelikleri, varsayılan olarak Azure AD'de olmayan öznitelikler eklemek için kullanın. Tıklatın [SAML ayarlanabilir kullanıcı öznitelikleri](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tam listesini almak için SAML öznitelikleri **Zendesk** kabul eder.  

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-zendesk-tutorial/tutorial_general_400.png)

7. Üzerinde **Zendesk yapılandırma** 'yi tıklatın **yapılandırma Zendesk** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Zendesk yapılandırma](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

8. Farklı web tarayıcısı penceresinde Zendesk şirket sitenize yönetici olarak oturum açın.

9. Tıklatın **yönetici**.

10. Sol gezinti bölmesinde **ayarları**ve ardından **güvenlik**.

11. Üzerinde **güvenlik** sayfasında, aşağıdaki adımları gerçekleştirin: 
   
     ![Güvenlik](./media/active-directory-saas-zendesk-tutorial/ic773089.png "güvenlik")

    ![Çoklu oturum açma](./media/active-directory-saas-zendesk-tutorial/ic773090.png "çoklu oturum açma")

     a. Tıklatın **yönetici & aracıları** sekmesi.

     b. Seçin **çoklu oturum açma (SSO) ve SAML**ve ardından **SAML**.

     c. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan. 

     d. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.
        
     e. İçinde **sertifika parmak izi** metin kutusuna, Yapıştır **parmak izi** Azure portalından kopyaladığınız sertifika değeri.
     
     f. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-zendesk-test-user"></a>Zendesk test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açmanız **Zendesk**, içine sağlanmalıdır **Zendesk**.  
Uygulamalar atanan role bağlı olarak, bu beklenen bir davranıştır:

 1. **Son kullanıcı** hesapları otomatik olarak oturum açarken sağlandı.
 2. **Aracı** ve **yönetici** hesapları el ile olarak sağlanması gerekir **Zendesk** oturum açmadan önce.
 
**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Zendesk** Kiracı.

2. Seçin **müşteri listesi** sekmesi.

3. Seçin **kullanıcı** sekmesine ve tıklayın **Ekle**.
   
    ![Kullanıcı Ekle](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Kullanıcı Ekle")
4. Tür **adı** ve **e-posta** sağlamak istediğiniz ve ardından bir var olan bir Azure AD hesabının **kaydetmek**.
   
    ![Yeni kullanıcı](./media/active-directory-saas-zendesk-tutorial/ic773633.png "yeni kullanıcı")

> [!NOTE]
> API sağlama AAD kullanıcı hesaplarına Zendesk tarafından sağlanan veya herhangi diğer Zendesk kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Zendesk için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Zendesk için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zendesk**.

    ![Uygulamalar listesinde Zendesk bağlantı](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

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

