---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Dome9 yay | Microsoft Docs'
description: Azure Active Directory ve Dome9 yay arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: c84f98da4d179aaee198fc489b9fc18650220b33
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048275"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Öğretici: Azure Active Directory tümleştirmesiyle Dome9 yay

Bu öğreticide, Azure Active Directory (Azure AD) ile Dome9 yay tümleştirme konusunda bilgi edinin.

Azure AD ile Dome9 yay tümleştirme ile aşağıdaki avantajları sağlar:

- Dome9 yay erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Dome9 Yaya (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Dome9 yay yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Dome9 yay çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Dome9 yay ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-dome9-arc-from-the-gallery"></a>Galeriden Dome9 yay ekleme
Azure AD'de Dome9 yay tümleştirmesini yapılandırmak için Dome9 yay Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Dome9 yay eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Dome9 yay**seçin **Dome9 yay** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Dome9 yay](./media/dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Dome9 yay "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Dome9 Yayı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Dome9 yay ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Dome9 yay içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Dome9 yay ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dome9 yay test kullanıcısı oluşturma](#create-a-dome9-arc-test-user)**  - kullanıcı Azure AD gösterimini bağlı Dome9 yay Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Dome9 yay uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Dome9 yay yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Dome9 yay** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. Üzerinde **Dome9 yay etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Dome9 yay etki alanı ve URL'ler tek oturum açma bilgileri](./media/dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://secure.dome9.com/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Öğreticinin ilerleyen bölümlerinde açıklanan dome9 Yönetim Portalı'nda, şirket adı değeri seçer.

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Dome9 yay etki alanı ve URL'ler tek oturum açma bilgileri](./media/dome9arc-tutorial/tutorial_dome9arc_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://secure.dome9.com/sso/saml/<yourcompanyname>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Dome9 yay istemci Destek ekibine](https://dome9.com/about/contact-us/) bu değerleri almak için. 

5. Dome9 yay yazılım uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açma attb yapılandırın](./media/dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri | 
    | --------------- | --------------- | 
    | İz | User.assignedroles | 
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma attb Ekle](./media/dome9arc-tutorial/tutorial_dome9_04.png)

    ![Düzenleme attb çoklu oturum açmayı yapılandırın](./media/dome9arc-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/dome9arc-tutorial/tutorial_general_400.png)
    
9. Üzerinde **Dome9 yay yapılandırma** bölümünde **yapılandırma Dome9 yay** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Dome9 yay yapılandırma](./media/dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. Farklı bir web tarayıcı penceresinde Dome9 yay şirket sitenize yönetici olarak oturum.

11. Tıklayarak **profil ayarları** 'a tıklayın ve sağ üst köşedeki **hesap ayarları**. 

    ![Dome9 yay yapılandırma](./media/dome9arc-tutorial/configure1.png)

12. Gidin **SSO** ve ardından **etkinleştirme**.

    ![Dome9 yay yapılandırma](./media/dome9arc-tutorial/configure2.png)

13. SSO Yapılandırması bölümünde aşağıdaki adımları gerçekleştirin:

    ![Dome9 yay yapılandırma](./media/dome9arc-tutorial/configure3.png)

    a. Şirket adı girmeniz **hesap kimliği** metin. Yanıt URL'si Azure portal URL bölümünde belirtildiği şekilde değerdir.

    b. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalında form kopyaladığınız.

    c. İçinde **IDP uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, Azure portalında form kopyaladığınız.

    d. İndirilen, Base64 olarak kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.

    e. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dome9arc-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/dome9arc-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dome9arc-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dome9arc-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dome9-arc-test-user"></a>Dome9 yay test kullanıcısı oluşturma

Dome9 Yaya oturum açmak Azure AD kullanıcılarının etkinleştirmek için kullanıcılar uygulamaya sağlanması gerekir. Dome9 yay, just-ın-time sağlamayı destekler ancak kullanıcı sahip belirli seçmek olan düzgün çalışması **rol** ve aynı kullanıcıya atayın.

   >[!Note] 
   >İçin **rol** oluşturma ve diğer kişi ayrıntıları [Dome9 yay istemci Destek ekibine](https://dome9.com/about/contact-us/).

**Bir kullanıcı hesabını el ile sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Dome9 yay şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **kullanıcıları ve rolleri** ve ardından **kullanıcılar**.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user1.png)

3. Tıklayın **Kullanıcı Ekle**.

    ![Çalışan Ekle](./media/dome9arc-tutorial/user2.png)

4. İçinde **Create User** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çalışan Ekle](./media/dome9arc-tutorial/user3.png)

    a. İçinde **e-posta** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    b. İçinde **ad** metin adı Britta gibi kullanıcı türü.

    c. İçinde **Soyadı** metin son Simon gibi kullanıcının adını yazın.

    d. Olun **SSO kullanıcı** olarak **üzerinde**.

    e. Tıklayın **Oluştur**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Dome9 Yaya erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Dome9 Yaya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Dome9 yay**.

    ![Uygulamalar listesinde Dome9 yay bağlantı](./media/dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Dome9 yay kutucuğa tıkladığınızda, otomatik olarak Dome9 yay uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/dome9arc-tutorial/tutorial_general_203.png

