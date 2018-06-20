---
title: 'Öğretici: Azure Active Directory Tümleştirme yayımlama yay - SSO ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve yayımlama yay - SSO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: bf811d789c0c6effd6f8940ad433092ea9ba04cb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36210082"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Öğretici: Azure Active Directory Tümleştirme yayımlama yay - SSO ile

Bu öğreticide, Arc yayımlama - SSO Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Yay yayımlama - SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yayımlama yay - SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak yayımlama yay - Azure AD hesaplarına ile SSO (çoklu oturum açma) için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme yay yayımlama ile - SSO yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir yay yayımlama - abonelik etkin SSO çoklu oturum açma

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Yay yayımlama - galerisinden SSO ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Yay yayımlama - galerisinden SSO ekleme
Yayımlama yay - Azure AD'ye SSO tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize galerisinden SSO yay yayımlama - eklemeniz gerekir.

**SSO Galerisi'nden yay yayımlama - eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **yayımlama yay - SSO**seçin **yayımlama yay - SSO** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Yay yayımlama - sonuçlar listesinde SSO](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yay yayımlama ile test etme - SSO "Britta Simon" adlı bir test kullanıcı tabanlı.

Çoklu oturum açma çalışmak özelliğini için Azure AD ne karşılık gelen kullanıcı yay yayımlama bilmek ister - SSO kullanıcıya Azure AD'de. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı yay yayımlama - arasında bir bağlantı ilişkisi SSO kurulması gerekir.

Yapılandırmak ve Azure AD çoklu oturum açma ile yay yayımlama - sınamak için SSO, aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir yay yayımlama oluşturma - SSO test kullanıcı](#create-an-arc-publishing---sso-test-user)**  - yayımlama yay - kullanıcı Azure AD gösterimini bağlı SSO Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Arc yayımlama içinde - SSO uygulaması yapılandırın.

**Azure AD çoklu oturum açma yay yayımlama ile - SSO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **yayımlama yay - SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/arc-tutorial/tutorial_arc_samlbase.png)

3. Üzerinde **yayımlama yay - SSO etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Yay yayımlama - SSO etki alanı ve oturum açma URL'leri tek bilgi](./media/arc-tutorial/tutorial_arc_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Yay yayımlama - SSO etki alanı ve oturum açma URL'leri tek bilgi](./media/arc-tutorial/tutorial_arc_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [yayımlama yay - SSO istemci destek ekibi](mailto:inf@washpost.com) bu değerleri almak için. 

5. Yayımlama yay - SSO uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_arc_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | FirstName | User.givenName |
    | Soyadı | User.surname |
    | e-posta | User.Mail |
    | gruplar | User.assignedroles |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

     ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklatın **Tamam**

    > [!NOTE]
    > Burada **grupları** özniteliği ile eşlendiği **user.assignedroles**. Bu grup adlarını geri uygulama eşlemek için Azure AD'de oluşturulan özel rolleridir. Daha fazla yönergeler bulabilirsiniz [burada](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de özel roller oluşturma. 

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/arc-tutorial/tutorial_arc_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/arc-tutorial/tutorial_general_400.png)
    
9. Üzerinde **yayımlama yay - SSO yapılandırma** 'yi tıklatın **yapılandırma yay yayımlama - SSO** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Yay yayımlama - SSO yapılandırma](./media/arc-tutorial/tutorial_arc_configure.png) 

10. Çoklu oturum açma yapılandırmak için **yayımlama yay - SSO** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [yay Yayımlama - SSO destek ekibi](mailto:inf@washpost.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/arc-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/arc-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/arc-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/arc-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Bir yay yayımlama - SSO test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon yay yayımlama içinde - SSO adlı bir kullanıcı oluşturmaktır. Yayımlama yay - SSO'yu yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı yayımlama yay - henüz yoksa SSO erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [yayımlama yay - SSO destek ekibi](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta erişim yay yayımlama için - SSO vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon yay yayımlama için - SSO atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **yayımlama yay - SSO**.

    ![Yay yayımlama - uygulamalar listesinde SSO bağlantı](./media/arc-tutorial/tutorial_arc_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Yay yayımlama - SSO kutucuğu erişim Paneli'nde tıklattığınızda, otomatik olarak, Arc yayımlama için - SSO uygulaması açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

