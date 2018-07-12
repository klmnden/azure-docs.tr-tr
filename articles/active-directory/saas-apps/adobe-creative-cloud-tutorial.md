---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Adobe Creative Cloud | Microsoft Docs'
description: Azure Active Directory ve Adobe Creative Cloud arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2018
ms.author: jeedes
ms.openlocfilehash: e1788de7c2372797b2034eb1753ab435c1299889
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38548295"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Öğretici: Azure Active Directory Adobe Creative Cloud ile tümleştirme

Bu öğreticide, Adobe Creative Cloud, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Adobe Creative Cloud, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe Creative Cloud erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan-Adobe Creative Cloud'a (çoklu oturum açma), kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Adobe Creative Cloud ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Adobe Creative Cloud çoklu oturum açma abonelik etkin.
- Gerekli bir Adobe Creative Cloud Kurumsal sürüm

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Adobe Creative Cloud galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Adobe Creative Cloud galeri ekleme

Azure AD'de Adobe Creative Cloud tümleştirmesini yapılandırmak için Adobe Creative Cloud Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Adobe Creative Cloud galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Adobe Creative Cloud**seçin **Adobe Creative Cloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Adobe Creative Cloud](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Adobe Creative Cloud ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Adobe Creative cloud'da bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Adobe Creative Cloud ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Adobe Creative Cloud ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Adobe Creative Cloud test kullanıcısı oluşturma](#create-an-adobe-creative-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Adobe Creative cloud'da Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Adobe Creative Cloud uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Adobe Creative Cloud ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Adobe Creative Cloud** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Üzerinde **Adobe Creative Cloud etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Adobe Creative Cloud etki alanı ve URL'ler tek oturum açma bilgileri](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.okta.com/saml2/service-provider/<token>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Adobe Creative Cloud Kurumsal](https://www.adobe.com/au/creativecloud/business/teams/plans.html) bu değerleri almak için.

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Adobe Creative Cloud etki alanı ve URL'ler tek oturum açma bilgileri](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    İçinde **oturum açma URL'si** metin değeri olarak yazın: `https://adobe.com`

5. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. Adobe Creative Cloud'a uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı özniteliği** uygulama sekmesinde. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | FirstName |User.givenName |
    | LastName |User.surname |
    | Email |User.Mail |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. **Tamam**’a tıklayın.

    > [!NOTE]
    > Kullanıcıların talep SAML yanıtta doldurulacak değeri geçerli bir Office 365 ExO lisans için e-posta olması gerekir.

8. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_general_400.png)

9. Üzerinde **Adobe Creative Cloud Yapılandırması** bölümünde **Adobe Creative Cloud yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümüne**.

    ![Adobe Creative Cloud yapılandırması](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)

10. Farklı bir web tarayıcı penceresinde için oturum [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak.

11. Git **ayarları** üst gezinti çubuğunda ve ardından **kimlik**. Etki alanlarının listesi açılır. Tıklayın **yapılandırma** etki alanınıza yönelik bağlantı. Ardından üzerinde aşağıdaki adımları gerçekleştirin **tek oturum yapılandırması gerekli** bölümü. Daha fazla bilgi için [bir etki alanı Kurulumu](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Ayarları](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "ayarları")

    a. Tıklayın **Gözat** için Azure AD'den yüklenen sertifikayı karşıya yüklemek için **IDP sertifika**.

    b. İçinde **IDP veren** metin değerini put **SAML varlık kimliği** bölümünden kopyaladığınız **yapılandırma oturum açma** bölümü Azure Portalı'nda.

    c. İçinde **IDP'nin oturum açma URL'si** metin değerini put **SAML SSO hizmet URL'si** bölümünden kopyaladığınız **yapılandırma oturum açma** bölümü Azure Portalı'nda.

    d. Seçin **HTTP - Redirect** olarak **IDP bağlama**.

    e. Seçin **e-posta adresi** olarak **kullanıcı oturum açma ayarı**.

    f. Tıklayın **Kaydet** düğmesi.

12. Pano artık XML sunacaktır **"Meta verileri indirme"** dosya. Bu, Adobe EntityDescriptor URL ve AssertionConsumerService URL'sini içerir. Lütfen dosyayı açın ve bunları Azure AD'ye yapılandırın.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Adobe için sağlanan EntityDescriptor değeri kullanın **tanımlayıcı** üzerinde **uygulama ayarlarını yapılandırma** iletişim.

    b. Adobe için sağlanan AssertionConsumerService değeri kullanın **yanıt URL'si** üzerinde **uygulama ayarlarını yapılandırma** iletişim.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Bir Adobe Creative Cloud test kullanıcısı oluşturma

Azure AD kullanıcılarının Adobe Creative Cloud oturum etkinleştirmek için Adobe Creative Cloud sağlanması gerekir. Adobe Creative Cloud söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak site.

2. Adobe Konsolu içinden kullanıcı Federasyon kimliği ekleyin ve bunları bir ürün profiline atayın. Kullanıcı ekleme ile ilgili ayrıntılı bilgi için bkz [Adobe Yönetici konsolunda kullanıcı ekleme](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Bu noktada, e-posta adresi/upn Adobe signın forma SEKME tuşuna basın, yazın ve Azure AD ile federasyona eklenmesi:
    * Web erişimi: www.adobe.com > oturum açma
    * Masaüstü uygulaması yardımcı program içinde > oturum açma
    * Uygulama içinde > Yardım > oturum açma

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe Creative Cloud erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Adobe Creative Cloud atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Adobe Creative Cloud**.

    ![Uygulamalar listesini Adobe Creative Cloud bağlantıdaki](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Adobe Creative Cloud kutucuğa tıkladığınızda, otomatik olarak Adobe Creative Cloud uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
* [(Adobe.com) etki alanını ayarlama](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
* [Azure, Adobe SSO (adobe.com) ile kullanım için yapılandırın](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

<!--Image references-->

[1]: ./media/adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/adobe-creative-cloud-tutorial/tutorial_general_203.png
