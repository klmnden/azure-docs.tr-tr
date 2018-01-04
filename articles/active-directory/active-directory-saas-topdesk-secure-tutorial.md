---
title: "Öğretici: Azure Active Directory Tümleştirme TOPdesk - güvenli ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile TOPdesk - güvenli arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: ca3362bc3f966adaf9940f6eb4bec5235c6ea7d8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Öğretici: Azure Active Directory Tümleştirme TOPdesk - güvenli ile

Bu öğreticide, TOPdesk - tümleştirmek öğrenin Azure Active Directory (Azure AD) ile güvenli.

TOPdesk - güvenli tümleştirme Azure AD ile aşağıdaki faydaları sağlar:

- TOPdesk - güvenli erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak TOPdesk için - güvenli (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için güvenli, aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- TOPdesk - bir abonelik etkin güvenli çoklu oturum açma

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. TOPdesk - ekleme Galeriden güvenli
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-topdesk---secure-from-the-gallery"></a>TOPdesk - ekleme Galeriden güvenli
TOPdesk - tümleştirmesini yapılandırmak için güvenli Azure AD ile TOPdesk add - Galeriden yönetilen SaaS listenize uygulamaları güvenli vermeniz gerekir.

**TOPdesk - eklemek için Galeriden güvenli, aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TOPdesk - güvenli**seçin **TOPdesk - güvenli** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![TOPdesk - sonuçlar listesinde güvenli](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - güvenli "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD TOPdesk - güvenli karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının TOPdesk - güvenli ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TOPdesk - güvenli, değeri atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için güvenli, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TOPdesk - güvenli test kullanıcısı oluşturma](#create-a-topdesk---secure-test-user)**  - Britta Simon TOPdesk - güvenli içinde kullanıcı Azure AD gösterimini bağlı, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, TOPdesk - güvenli uygulama yapılandırın.

**Azure AD çoklu oturum açma ile TOPdesk - yapılandırmak için güvenli, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TOPdesk - güvenli** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. Üzerinde **TOPdesk - güvenli etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TOPdesk - güvenli etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.topdesk.net`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Yanıt URL'si öğreticinin ilerleyen bölümlerinde açıklanmıştır. Kişi [TOPdesk - güvenli istemci destek ekibi](http://www.topdesk.com/us/support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_400.png)

6. Üzerinde **TOPdesk - güvenli yapılandırma** 'yi tıklatın **yapılandırma TOPdesk - güvenli** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TOPdesk - güvenli yapılandırma](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
7. Oturum, **TOPdesk - güvenli** yönetici olarak şirket site.

8. İçinde **TOPdesk** menüsünde tıklatın **ayarları**.

    ![Ayarları](./media/active-directory-saas-topdesk-secure-tutorial/ic790598.png "ayarları")

9. Tıklatın **oturum açma ayarları**.

    ![Oturum açma ayarları](./media/active-directory-saas-topdesk-secure-tutorial/ic790599.png "oturum açma ayarları")

10. Genişletme **oturum açma ayarları** menüsüne ve ardından **genel**.

    ![Genel](./media/active-directory-saas-topdesk-secure-tutorial/ic790600.png "genel")

11. İçinde **güvenli** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Teknik ayarları](./media/active-directory-saas-topdesk-secure-tutorial/ic790855.png "teknik ayarları")
   
    a. Tıklatın **karşıdan** ortak meta veri dosyası indirip bilgisayarınıza yerel olarak kaydedin.
   
    b. Meta veri dosyasını açın ve ardından bulun **AssertionConsumerService** düğümü.
    
    ![Onaylama işlemi tüketici hizmeti](./media/active-directory-saas-topdesk-secure-tutorial/ic790856.png "onaylama tüketici hizmeti")
   
    c. Kopya **AssertionConsumerService** değeri, bu değer yanıt URL'si metin kutusuna yapıştırın **TOPdesk - güvenli etki alanı ve URL'leri** bölümü.

12. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/active-directory-saas-topdesk-secure-tutorial/ic790606.png "sertifika")
    
    a. Azure Portalı'ndan indirilen meta veri dosyası açın.

    b. Genişletme **RoleDescriptor** sahip düğümü bir **xsi: type** , **ssas'nin: ApplicationServiceType**.

    c. Değerini kopyalayın **X509Certificate** düğümü.

    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyada değeri.

13. İçinde **ortak** 'yi tıklatın **Ekle**.
    
    ![Ekleme](./media/active-directory-saas-topdesk-secure-tutorial/ic790607.png "ekleme")

14. Üzerinde **SAML Yapılandırması Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML Yapılandırması Yardımcısı](./media/active-directory-saas-topdesk-secure-tutorial/ic790608.png "SAML Yapılandırması Yardımcısı")
    
    a. Azure Portalı'ndan indirilen meta veri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklatın **Gözat**.

    b. Sertifika dosyanızın altında karşıya yüklemek için **sertifika (RSA)**, tıklatın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **Logo simgesini**, tıklatın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. İçinde **görünen adı** metin kutusuna, yapılandırmanız için bir ad yazın.

    f. **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-topdesk---secure-test-user"></a>TOPdesk - güvenli test kullanıcısı oluşturma

Azure AD kullanıcıların TOPdesk - oturum etkinleştirmek için güvenli, bunların TOPdesk - güvenli sağlanmalıdır.  
Söz konusu olduğunda TOPdesk - sağlama el ile bir görev güvenlidir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum, **TOPdesk - güvenli** yönetici olarak şirket site.
2. Üstteki menüde tıklatın **TOPdesk \> yeni \> destek dosyalarını \> işleci**.
   
    ![İşleç](./media/active-directory-saas-topdesk-secure-tutorial/ic790610.png "işleci")

3. Üzerinde **New işleci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![New işleci](./media/active-directory-saas-topdesk-secure-tutorial/ic790611.png "New işleci")
   
    a. Tıklatın **genel** sekmesi.
   
    b. İçinde **Soyadı** metin kutusuna, kullanıcının soyadını türü ister **Simon**.
   
    c. Seçin bir **Site** hesap için **konumu** bölümü.
   
    d. İçinde **oturum açma adı** , metin kutusuna **TOPdesk oturum açma** bölümünde, kullanıcı oturum açma adını yazın.
   
    e. **Kaydet** düğmesine tıklayın.

> [!NOTE]
> Tüm diğer TOPdesk - güvenli kullanıcı hesabı oluşturma araçlarını veya tarafından TOPdesk - AAD kullanıcı hesaplarını sağlamak için güvenli sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta için TOPdesk - güvenli erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**TOPdesk için - Britta Simon atamak için güvenli, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TOPdesk - güvenli**.

    ![TOPdesk - uygulamalar listesinde güvenli bağlantı](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

TOPdesk - güvenli erişim paneli parçasında tıklattığınızda, otomatik olarak, TOPdesk - güvenli uygulama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_203.png

