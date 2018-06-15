---
title: 'Öğretici: Azure Active Directory Tümleştirme Mimecast Yönetici Konsolu ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Mimecast Yönetici Konsolu arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: bef0312a3ee44a441f44eb2ae7e4292b966cec3f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344754"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Öğretici: Azure Active Directory Tümleştirme Mimecast Yönetici Konsolu ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Mimecast Yönetici Konsolu tümleştirmek öğrenin.

Mimecast Yönetici Konsolu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mimecast Yönetici Konsolu erişebilir, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Mimecast yönetici konsoluna açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Mimecast Yönetici Konsolu çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Mimecast Yönetici Konsolu'nu ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Galeriden Mimecast Yönetici Konsolu'nu ekleme
Azure AD Mimecast Yönetim Konsolu tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Mimecast Yönetici Konsolu eklemeniz gerekir.

**Galeriden Mimecast Yönetici Konsolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Mimecast Yönetici Konsolu**seçin **Mimecast Yönetici Konsolu** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Mimecast Yönetim Konsolu](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Mimecast Yönetici Konsolu ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Mimecast Yönetici konsolunda bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Mimecast Yönetici konsolunda ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Mimecast yönetim konsolunda, Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mimecast Yönetici Konsolu test kullanıcısı oluşturma](#create-a-mimecast-admin-console-test-user)**  - Mimecast Yönetici konsolunda, kullanıcının Azure AD gösterimini bağlı Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mimecast Yönetici Konsolu uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mimecast Yönetici Konsolu** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Üzerinde **Mimecast Yönetici Konsolu etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast Yönetici Konsolu etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Oturum açma URL'si belirli bölgedir.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Üzerinde **Mimecast Yönetici Konsolu Yapılandırması** 'yi tıklatın **Mimecast Yönetim Konsolu** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Mimecast Yönetici Konsolu yapılandırması](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Farklı web tarayıcısı penceresinde Mimecast Yönetici konsolunda bir yönetici olarak oturum açın.

8. Git **Hizmetleri \> uygulama**.

    ![Hizmetleri](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Hizmetleri")

9. Tıklatın **kimlik doğrulaması profilleri**.

    ![Kimlik doğrulama profilleri](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "kimlik doğrulaması profilleri")
    
10. Tıklatın **yeni kimlik doğrulama profili**.

    ![Yeni kimlik doğrulama profilleri](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "yeni kimlik doğrulama profilleri")

11. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama profili](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "kimlik doğrulama profili")
    
    a. İçinde **açıklama** metin kutusuna, yapılandırmanız için bir ad yazın.
    
    b. Seçin **Mimecast Yönetici Konsolu için SAML kimlik doğrulaması zorunlu**.
    
    c. Olarak **sağlayıcı**seçin **Azure Active Directory**.
    
    d. Yapıştır **SAML varlık kimliği**, Azure portalından kopyalanan **veren URL'si** metin kutusu.
    
    e. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **oturum açma URL'si** metin kutusu.

    f. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **oturum kapatma URL'si** metin kutusu.
    
    >[!NOTE]
    >Oturum açma URL'si ve oturum kapatma URL'si değerleri Mimecast Yönetim Konsolu için aynıdır.
    
    g. Base-64 sertifikanızı Not Defteri'nde, Azure Portalı'ndan indirilen açık ilk satırı Kaldır ("*--*") ve son satırında ("*--*"), kalan içeriği, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası (meta verileri)** metin kutusu.
    
    h. Seçin **çoklu oturum açmaya izin verme**.
    
    i. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Mimecast Yönetici Konsolu test kullanıcısı oluşturma

Azure AD kullanıcıların Mimecast Yönetici konsolunda oturum etkinleştirmek için bunlar Mimecast Yönetici konsolunda sağlanmalıdır. Mimecast yönetim söz konusu olduğunda, sağlama el ile bir görev konsoludur.

* Bir etki alanı kullanıcıları oluşturabilmeniz için önce kaydetmeniz gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Mimecast Yönetici Konsolu** yönetici olarak.
2. Git **dizinleri \> iç**.
   
   ![Dizinleri](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "dizinleri")
3. Tıklatın **kayıt yeni bir etki alanı**.
   
   ![Yeni etki alanı kayıt](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "kayıt yeni bir etki alanı")
4. Yeni etki alanınızın oluşturulduktan sonra tıklatın **yeni adresi**.
   
   ![Yeni bir adres](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "yeni adresi")
5. Yeni adres iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![Kaydet](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Kaydet")
   
   a. Tür **e-posta adresi**, **genel adı**, **parola**, ve **parolayı onayla** öznitelikleri geçerli bir Azure ad hesabına ilgili metin kutularına içine sağlamak istiyorsunuz.

   b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Mimecast Yönetim Konsolu kullanıcı hesabı oluşturma araçlarını veya Mimecast Yönetici Konsolu tarafından sağlanan API'leri kullanabilirsiniz. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Mimecast yönetim konsoluna erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Mimecast yönetici konsoluna atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Mimecast Yönetici Konsolu**.

    ![Uygulamalar listesinde Mimecast yönetici konsolunu bağlantısı](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mimecast Yönetici Konsolu parçasında tıklattığınızda, otomatik olarak Mimecast Yönetici Konsolu uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

