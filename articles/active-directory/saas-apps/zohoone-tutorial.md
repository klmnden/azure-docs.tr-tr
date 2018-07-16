---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zoho bir | Microsoft Docs'
description: Azure Active Directory ve Zoho bir arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 9f1db693ac75d7ab4b9776d60f63a0ca7524f760
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050193"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Öğretici: Azure Active Directory Tümleştirme ile Zoho bir

Bu öğreticide, Zoho bir Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Zoho bir Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Zoho bir Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Zoho One (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zoho bir ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Zoho bir çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Zoho bir galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zoho-one-from-the-gallery"></a>Zoho bir galeri ekleme
Azure AD'de, Zoho bir tümleştirmesini yapılandırmak için Zoho bir Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden bir Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zoho bir**seçin **Zoho bir** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde bir Zoho](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zoho "Britta Simon" adlı bir test kullanıcı tabanlı bir test.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Zoho tek bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Zoho bir arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile Zoho bir test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zoho bir test kullanıcısı oluşturma](#create-a-zoho-one-test-user)**  - Zoho kullanıcı Azure AD gösterimini bağlantılı bir Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zoho bir uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Zoho bir ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zoho bir** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

3. Üzerinde **Zoho bir etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Zoho bir etki alanı ve URL'ler tek oturum açma bilgileri](./media/zohoone-tutorial/tutorial_zohoone_url.png)

    a. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL: `one.zoho.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Denetleme **Gelişmiş URL ayarlarını göster**.

    d. İçinde **geçiş durumu** metin kutusuna bir URL:`https://one.zoho.com`

4. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu aşağıdaki adımı uygulayın:

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`
     
    > [!NOTE] 
    > Önceki **yanıt URL'si** ve **oturum açma URL'si** değeri gerçek değil. Değer, gerçek yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL'si ile güncelleştirir. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/zohoone-tutorial/tutorial_general_400.png)
    
7. Üzerinde **bir yapılandırmasına Zoho** bölümünde **Zoho bir yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Zoho bir yapılandırma](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

8. Farklı bir web tarayıcı penceresinde Zoho bir şirketinizin sitesi için bir yönetici olarak oturum açın.

9. Üzerinde **kuruluş** sekmesine tıklatın **Kurulum** altında **SAML kimlik doğrulaması**.

    ![Zoho bir kuruluş](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

10. Açılan sayfada, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir imza](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **Gözat** yüklenecek **sertifika (Base64)** Azure portalından indirilen.

    d. **Kaydet**’e tıklayın.

11. SAML kimlik doğrulaması kurulumu kaydettikten sonra kopyalama **SAML Identfier** değeri ve bu değeri kullanın **yanıt URL'si** Azure portalında altında **Zoho bir etki alanı ve URL'ler** bölümü.

    ![Zoho bir saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

12. Git **etki alanları** sekmesine ve ardından **etki alanı Ekle**.

    ![Zoho bir etki alanı](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

13. Üzerinde **etki alanı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir etki alanı Ekle](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. İçinde **etki alanı adı** metin türü etki alanı gibi **contoso.com**.

    b. **Ekle**'ye tıklayın.

    >[!Note]
    >Etki alanı izleme ekledikten sonra [bunlar](https://www.zoho.com/one/help/admin-guide/domain-verification.html) adımları etki alanınızı doğrulayın. Etki alanı verfified olduğunda, etki alanı adınızı kullanmak **oturum açma URL'si** içinde **Zoho bir etki alanı ve URL'ler** bölümü Azure Portalı'nda.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zohoone-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/zohoone-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zohoone-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zohoone-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoho-one-test-user"></a>Zoho bir test kullanıcısı oluşturma

One Zoho oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Zoho bir sağlanması gerekir. One Zoho, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Zoho tek bir güvenlik yöneticisi olarak oturum açın.

2. Üzerinde **kullanıcılar** sekmesine tıklatın üzerinde **kullanıcı logosu**.

    ![Zoho bir kullanıcı](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir kullanıcı ekleyin](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **Britta simon**.
    
    b. İçinde **e-posta adresi** metin kutusuna, kullanıcının gibi e-posta girin **brittasimon@contoso.com**.

    >[!Note]
    >Doğrulanmış etki alanınızın etki alanı listeden seçin.

    c. **Ekle**'ye tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Zoho bir erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Zoho One atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Zoho bir**.

    ![Uygulamalar listesini Zoho bir bağlantıya](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zoho bir kutucuğa tıkladığınızda, size otomatik olarak Zoho bir uygulamanız açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

