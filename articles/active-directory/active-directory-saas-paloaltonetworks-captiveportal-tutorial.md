---
title: "Öğretici: Azure Active Directory Tümleştirme Palo Alto ağlarla - Captive portalı | Microsoft Docs"
description: "Çoklu oturum açma Palo Alto ağları - Captive portalı ve Azure Active Directory arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: d3126feec3081f71b89d927291c164e261129312
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Öğretici: Azure Active Directory Tümleştirme Palo Alto ağlarla - Captive portalı

Bu öğreticide, Azure Active Directory (Azure AD) ile Captive Portal Palo Alto Networks - tümleştirmek öğrenin.

Palo Alto ağları - Captive Portal Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Palo Alto Networks - Captive Portal erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Palo Alto ağlara - (çoklu oturum açma) Captive Portal ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Palo Alto ağlarla - Captive Portal yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Palo Alto ağları - Captive Portal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeri Captive portalından Palo Alto ağlar - ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Galeri Captive portalından Palo Alto ağlar - ekleme
Palo Alto Networks - Azure AD Captive portalına tümleştirmesini yapılandırma Palo Alto Networks - galeri Captive portalından yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeri Captive portalından Palo Alto Networks - eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Palo Alto Networks - Captive Portal**seçin **Palo Alto Networks - Captive Portal** sonuç panelinden ardından **Ekle** uygulama Ekle düğmesi .

    ![Palo Alto Networks - sonuçlar listesinde Captive portalı](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks - Captive "Britta Simon" adlı bir test kullanıcı tabanlı Portal ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Palo Alto ağlarda - Captive portalı bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Palo Alto Networks - Captive Portal ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Palo Alto ağları - Captive Portal değerini atama **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma Palo Alto ağlarla - sınamak için Captive portalı, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Palo Alto Networks - Captive Portal test kullanıcısı oluşturma](#create-a-palo-alto-networks---captive-portal-test-user)**  - Britta Simon, karşılık gelen Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı Captive Portal sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Palo Alto Networks - Captive Portal uygulaması yapılandırın.

**Azure AD çoklu oturum açma Palo Alto ağlarla - Captive Portal yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Palo Alto Networks - Captive Portal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_samlbase.png)

3. Üzerinde **Palo Alto Networks - Captive Portal etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![-Captive Portal etki alanı ve oturum açma URL'leri tek bilgi Palo Alto ağları](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Customer Firewall Hostname>/SAML20/SP`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Palo Alto Networks - Captive Portal destek ekibi](https://support.paloaltonetworks.com/support) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_400.png)

6. Palo Alto siteyi başka bir tarayıcı penceresinde yönetici olarak açın.

7. Tıklayın **aygıt**.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

8. Seçin **SAML kimlik sağlayıcısı** gelen sol gezinti çubuğu ve "meta veri dosyası al"'i tıklatın.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

9. Aşağıdaki içeri aktarma penceresini eylemleri gerçekleştirme

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. İçinde **profil adı** metin kutusuna, ad örneğin Azure AD yönetim kullanıcı Arabirimi sağlar.
    
    b. İçinde **kimlik sağlayıcısı meta verileri**, tıklatın **Gözat** ve Azure Portalı'ndan indirilen metadata.xml dosyası seçin
    
    c. **Tamam**’a tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-a-palo-alto-networks---captive-portal-test-user"></a>Palo Alto Networks - Captive Portal test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Palo Alto Networks - Captive Portal adlı bir kullanıcı oluşturmaktır. Palo Alto Networks - Captive Portal yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı Palo Alto Networks - Captive henüz yoksa Portal erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [Palo Alto Networks - Captive Portal destek ekibi](https://support.paloaltonetworks.com/support).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Palo Alto Networks - Captive Portal erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Palo Alto Networks - Captive Portal Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Palo Alto Networks - Captive Portal**.

    ![Palo Alto ağlar - uygulamalar listesinde Captive Portal bağlantısı](./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Captive Portal Windows VM Güvenlik duvarının arkasında yapılandırılır.  Çoklu oturum açma Captive Portal'daki RDP kullanarak Windows VM oturum açma test etmek için. Gelen RDP oturumu içinde herhangi bir web sitesi için bir tarayıcı açın, SSO url ve kimlik doğrulaması isteminde otomatik olarak açılmalıdır. Kimlik doğrulaması tamamlandığında, web sitelerine navgiate için görüyor olmalısınız. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltonetworks-captiveportal-tutorial/tutorial_general_203.png

