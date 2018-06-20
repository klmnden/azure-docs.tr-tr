---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Grovo | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Grovo arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 399cecc3-aa62-4914-8b6c-5a35289820c1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: f43a0270a1c9211c71f92311c3c024b14bd4e77a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230497"
---
# <a name="tutorial-azure-active-directory-integration-with-grovo"></a>Öğretici: Azure Active Directory Tümleştirme Grovo ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Grovo tümleştirmek öğrenin.

Grovo Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Grovo erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Grovo (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Grovo ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Grovo çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Grovo ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-grovo-from-the-gallery"></a>Galeriden Grovo ekleme
Azure AD Grovo tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Grovo eklemeniz gerekir.

**Galeriden Grovo eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Grovo**seçin **Grovo** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Grovo](./media/grovo-tutorial/tutorial_grovo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Grovo sınayın.

Tekli çalışmaya oturum için Azure AD Grovo karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Grovo ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Grovo içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Grovo ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Grovo test kullanıcısı oluşturma](#create-a-grovo-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Grovo sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Grovo uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Grovo yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Grovo** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/grovo-tutorial/tutorial_grovo_samlbase.png)

3. Üzerinde **Grovo etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Grovo etki alanı ve URL'leri tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.grovo.com/sso/saml2/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

4. Denetleme **Göster Gelişmiş URL ayarları**, aşağıdaki adımı gerçekleştirin:

    ![Grovo etki alanı ve URL'leri tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url1.png)

    a. İçinde **geçiş durumu** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.grovo.com`

    b. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan aşağıdaki adımları gerçekleştirin:

    ![Grovo etki alanı ve URL'leri tek oturum açma bilgileri](./media/grovo-tutorial/tutorial_grovo_url2.png)
    
    İçinde **URL üzerinde oturum** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcısı, yanıt URL'si, oturum açma URL'si ve geçiş durumu ile güncelleştirin. Kişi [Grovo destek ekibi](https://www.grovo.com/contact-us) bu değerleri almak için.
 
5. Grovo uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirmesi sayfasında bölüm. Lütfen eşleyin **kullanıcı tanımlayıcısı** ile **user.mail** ve ekran görüntüsü gösterildiği gibi diğer öznitelikleri yapılandırabilirsiniz.
    
    ![Çoklu oturum açma attb yapılandırın](./media/grovo-tutorial/tutorial_grovo_attribute.png)
    
6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | Ad          | User.givenName |
    | Soyadı           | User.surname |
    | E-posta adresi       | User.Mail    |
    | EmployeeID          | User.employeeid |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/grovo-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/grovo-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.


7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/grovo-tutorial/tutorial_grovo_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/grovo-tutorial/tutorial_general_400.png)

9. Üzerinde **Grovo yapılandırma** 'yi tıklatın **yapılandırma Grovo** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_configure.png) 

10. Bir farklı web tarayıcısı penceresinde, Grovo yönetici olarak oturum açın.

11. Git **yönetici** > **tümleştirmeler**.
 
    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_admin.png) 

12. Tıklatın **Ayarla** altında **SP tarafından başlatılan SAML 2.0** bölümü.

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_setup.png)

13. İçinde **SP tarafından başlatılan SAML 2.0** açılan penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Grovo yapılandırma](./media/grovo-tutorial/tutorial_grovo_saml.png)

    a. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    b. İçinde **tek oturum açma Hizmeti uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. Seçin **tek oturum açma hizmet uç noktası bağlama** olarak `urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`.
    
    d. İndirilen açmak **Base64 ile kodlanmış sertifika** Not Defteri'nde Azure portalından yapıştırın **ortak anahtar** metin kutusu.

    e. **İleri**’ye tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/grovo-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/grovo-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/grovo-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/grovo-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-grovo-test-user"></a>Grovo test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Grovo adlı bir kullanıcı oluşturmaktır. Grovo yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Grovo erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekirse başvurun [Grovo destek ekibi](https://www.grovo.com/contact-us).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Grovo için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Grovo için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Grovo**.

    ![Uygulamalar listesinde Grovo bağlantı](./media/grovo-tutorial/tutorial_grovo_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Grovo parçasında tıklattığınızda, otomatik olarak Grovo uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/grovo-tutorial/tutorial_general_01.png
[2]: ./media/grovo-tutorial/tutorial_general_02.png
[3]: ./media/grovo-tutorial/tutorial_general_03.png
[4]: ./media/grovo-tutorial/tutorial_general_04.png

[100]: ./media/grovo-tutorial/tutorial_general_100.png

[200]: ./media/grovo-tutorial/tutorial_general_200.png
[201]: ./media/grovo-tutorial/tutorial_general_201.png
[202]: ./media/grovo-tutorial/tutorial_general_202.png
[203]: ./media/grovo-tutorial/tutorial_general_203.png

