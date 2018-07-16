---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Boomi | Microsoft Docs'
description: Azure Active Directory ve Boomi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: e0128d4422c462d4424583306af0b30174178bac
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049261"
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Öğretici: Azure Active Directory Boomi ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Boomi tümleştirme konusunda bilgi edinin.

Azure AD ile Boomi tümleştirme ile aşağıdaki avantajları sağlar:

- Boomi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Boomi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Boomi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Boomi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Boomi ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-boomi-from-the-gallery"></a>Galeriden Boomi ekleme
Azure AD'de Boomi tümleştirmesini yapılandırmak için Boomi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Boomi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Boomi**seçin **Boomi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Boomi](./media/boomi-tutorial/tutorial_boomi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Boomi sınayın.

Tek iş için oturum açma için Azure AD ne Boomi karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Boomi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Boomi içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Boomi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Boomi test kullanıcısı oluşturma](#create-a-boomi-test-user)**  - kullanıcı Azure AD gösterimini bağlı Boomi Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Boomi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Boomi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Boomi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/boomi-tutorial/tutorial_boomi_samlbase.png)

3. Üzerinde **Boomi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Boomi etki alanı ve URL'ler tek oturum açma bilgileri](./media/boomi-tutorial/tutorial_boomi_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://platform.boomi.com/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://platform.boomi.com/sso/<boomi-tenant>/saml`

    > [!NOTE] 
    > Yanıt URL'si değeri gerçek değil. Değerini gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [Boomi Destek ekibine](https://boomi.com/company/contact/) değeri alınamıyor.
 
4. Boomi uygulama belirli bir biçimde SAML onaylamalarını bekler. Lütfen bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/boomi-tutorial/tutorial_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/boomi-tutorial/tutorial_officespace_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/boomi-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/boomi-tutorial/tutorial_boomi_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/boomi-tutorial/tutorial_general_400.png)

8. Üzerinde **Boomi yapılandırma** bölümünde **yapılandırma Boomi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Boomi yapılandırma](./media/boomi-tutorial/tutorial_boomi_configure.png) 

9. Farklı bir web tarayıcı penceresinde Boomi şirket sitenize yönetici olarak oturum. 

10. Gidin **şirket adı** gidin **ayarlanan**.

11. Tıklayın **SSO seçenekleri** sekmesinde ve aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/boomi-tutorial/tutorial_boomi_11.png)

    a. Denetleme **etkinleştirme SAML çoklu oturum açma** onay kutusu.

    b. Tıklayın **alma** için Azure AD'den yüklenen sertifikayı karşıya yüklemek için **kimlik sağlayıcısı sertifikası**.
    
    c. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini put **SAML çoklu oturum açma hizmeti URL'si** Azure AD uygulama yapılandırma penceresinden.

    d. Olarak **Federasyon kimliği konumu**seçin **Federasyon kimliğidir FEDERATION_ID özniteliği öğesinde** radyo düğmesi. 

    e. Tıklayın **Kaydet** düğmesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/boomi-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/boomi-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/boomi-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/boomi-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-boomi-test-user"></a>Boomi test kullanıcısı oluşturma

Boomi için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Boomi sağlanması gerekir. Boomi söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Boomi şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Oturum açtıktan sonra gitmek **kullanıcı yönetimi** gidin **kullanıcılar**.

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_001.png "kullanıcılar")

3. Tıklayın **+** simgesi ve **kullanıcı rolleri Ekle/koru** iletişim kutusu açılır.

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_002.png "kullanıcılar")

    ![Kullanıcılar](./media/boomi-tutorial/tutorial_boomi_003.png "kullanıcılar")

    a. İçinde **kullanıcı e-posta adresi** metin kutusuna kullanıcı e-posta türünü ister BrittaSimon@contoso.com.
    
    b. İçinde **ad** metin kutusu, kullanıcının Britta gibi ilk tür adı.

    c. İçinde **Soyadı** metin son Simon gibi kullanıcı adını yazın.
    
    d. Kullanıcının girin **Federasyon kimliği**. Her kullanıcının kullanıcı hesabı içinde benzersiz olarak tanımlayan bir Federasyon kimliği olması gerekir.
    
    e. Ata **standart kullanıcı** kullanıcı rolü. İlişkiyi çoklu oturum açma erişimi yanı sıra normal Atmosfer erişim vereceği için Yönetici rolü atamayın.
    
    f. **Tamam**’a tıklayın.
    
    > [!NOTE]
    > Kullanıcı parolasını kimlik sağlayıcısı olarak yönetildiğinden AtomSphere hesabında oturum açma için kullanılan bir parola içeren bir Hoş Geldiniz bildirim e-posta alırsınız. Herhangi diğer Boomi kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Boomi tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Boomi erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Boomi için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Boomi**.

    ![Uygulamalar listesinde Boomi bağlantı](./media/boomi-tutorial/tutorial_boomi_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Boomi kutucuğa tıkladığınızda, otomatik olarak Boomi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/boomi-tutorial/tutorial_general_01.png
[2]: ./media/boomi-tutorial/tutorial_general_02.png
[3]: ./media/boomi-tutorial/tutorial_general_03.png
[4]: ./media/boomi-tutorial/tutorial_general_04.png

[100]: ./media/boomi-tutorial/tutorial_general_100.png

[200]: ./media/boomi-tutorial/tutorial_general_200.png
[201]: ./media/boomi-tutorial/tutorial_general_201.png
[202]: ./media/boomi-tutorial/tutorial_general_202.png
[203]: ./media/boomi-tutorial/tutorial_general_203.png

