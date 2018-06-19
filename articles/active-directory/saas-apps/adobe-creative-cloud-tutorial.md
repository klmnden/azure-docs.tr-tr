---
title: 'Öğretici: Azure Active Directory Tümleştirme Adobe Creative bulut ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Adobe Creative bulut arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: jeedes
ms.openlocfilehash: fc6661f0c87324fe149fc04ff3d0e162c5470d3f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982184"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Öğretici: Adobe Creative bulut Azure Active Directory Tümleştirme

Bu öğreticide, Adobe Creative bulut Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Adobe Creative bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe Creative bulut erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Adobe Creative buluta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Adobe Creative Bulutla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Adobe Creative bulut çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Adobe Creative bulut ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Galeriden Adobe Creative bulut ekleme
Azure AD Adobe Creative bulut tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe Creative bulut eklemeniz gerekir.

**Adobe Creative bulut Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Adobe Creative bulut**seçin **Adobe Creative bulut** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Adobe Creative bulut](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Adobe Creative bulut "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Adobe Creative bulutta bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Adobe Creative bulutta arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Adobe Creative bulut ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Adobe Creative bulut test kullanıcısı oluşturma](#create-an-adobe-creative-cloud-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Adobe Creative bulut sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Adobe Creative bulut uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Adobe Creative Bulutla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Adobe Creative bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Üzerinde **Adobe Creative bulut etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Adobe Creative bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.okta.com/saml2/service-provider/<token>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Adobe Creative bulut istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) bu değerleri almak için.

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Adobe Creative bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    İçinde **oturum açma URL'si** metin değeri olarak yazın: `https://adobe.com`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. Adobe Creative bulut uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı özniteliği** uygulama sekmesinde. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | FirstName |User.givenName |
    | Soyadı |User.surname |
    | Email |User.Mail |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. **Tamam**’a tıklayın.

    > [!NOTE]
    > Kullanıcılar için talep değeri SAML yanıt olarak doldurulması için geçerli bir Office 365 ExO lisans için e-posta olması gerekir.

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/adobe-creative-cloud-tutorial/tutorial_general_400.png)

9. Üzerinde **Adobe Creative bulut Yapılandırması** 'yi tıklatın **Adobe Creative bulut yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümünde**.

    ![Adobe Creative bulut yapılandırması](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)

10. Farklı web tarayıcısı penceresinde oturum için açma [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak.

11. Git **ayarları** üst gezinti çubuğu ve ardından **kimlik**. Etki alanlarının listesi açılır. Tıklatın **yapılandırma** etki alanınızın karşı bağlantı. Üzerinde aşağıdaki adımları gerçekleştirin **tek oturum yapılandırması gerekli** bölümü. Daha fazla bilgi için bkz: [bir etki alanı Kurulumu](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Ayarları](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "ayarları")

    a. Tıklatın **Gözat** için Azure AD'den indirilen sertifikayı karşıya yüklemek için **IDP sertifika**.

    b. İçinde **IDP veren** metin kutusuna, put değerini **SAML varlık kimliği** öğesinden kopyalanan **yapılandırma oturum açma** Azure portalı bölümünde.

    c. İçinde **IDP oturum açma URL'si** metin kutusuna, put değerini **SAML SSO hizmet URL'si** öğesinden kopyalanan **yapılandırma oturum açma** Azure portalı bölümünde.

    d. Seçin **HTTP - yeniden yönlendirme** olarak **IDP bağlama**.

    e. Seçin **e-posta adresi** olarak **kullanıcı oturum açma ayarı**.

    f. Tıklatın **kaydetmek** düğmesi.

12. Pano artık XML sunacaktır **"Meta veriler indirme"** dosya. Adobe EntityDescriptor URL ve AssertionConsumerService URL'sini içerir. Lütfen dosyayı açın ve Azure AD uygulaması yapılandırın.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Adobe için sağlanan EntityDescriptor değerini kullanmak **tanımlayıcısı** üzerinde **uygulama ayarlarını yapılandır** iletişim.

    b. Adobe için sağlanan AssertionConsumerService değerini kullanmak **yanıt URL'si** üzerinde **uygulama ayarlarını yapılandır** iletişim.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Adobe Creative bulut test kullanıcısı oluşturma

Azure AD kullanıcıların Adobe Creative bulutunu oturum etkinleştirmek için bunlar Adobe Creative bulutunu sağlanmalıdır. Adobe Creative bulut söz konusu olduğunda, sağlama bir el ile bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak site.

2. Federasyon kimliği olarak Adobe Konsolu içinden kullanıcı ekleyin ve bunları bir ürün profili atayın. Kullanıcı ekleme hakkında ayrıntılı bilgi için bkz: [Adobe Yönetici konsolunda kullanıcı ekleme](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Bu noktada, Adobe signın forma SEKME tuşuna basın, e-posta adresi/upn yazın ve Azure AD ile Federasyon:
    * Web erişimi: www.adobe.com > oturum açma
    * Masaüstü uygulaması yardımcı programı içinde > oturum açma
    * Uygulama içinde > Yardım > oturum açma

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe Creative buluta erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Adobe Creative buluta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Adobe Creative bulut**.

    ![Uygulamalar listesinde Adobe Creative bulut bağlantı](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Adobe Creative bulut parçasında tıklattığınızda, otomatik olarak Adobe Creative bulut uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [(Adobe.com) etki alanını ayarlama](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
* [Adobe SSO (adobe.com) ile kullanmak için Azure yapılandırma](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

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
