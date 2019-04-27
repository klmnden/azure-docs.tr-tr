---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Spotinst | Microsoft Docs'
description: Azure Active Directory ve Spotinst arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f6dbd70-c2db-4ae9-99ee-976c3090d214
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0bffdf439a192fb10fe695fbfa18e8c7abf8077
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542092"
---
# <a name="tutorial-azure-active-directory-integration-with-spotinst"></a>Öğretici: Spotinst ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Spotinst tümleştirme konusunda bilgi edinin.

Azure AD ile Spotinst tümleştirme ile aşağıdaki avantajları sağlar:

- Spotinst erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Spotinst (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Spotinst yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Spotinst çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Spotinst ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-spotinst-from-the-gallery"></a>Galeriden Spotinst ekleme
Azure AD'de Spotinst tümleştirmesini yapılandırmak için Spotinst Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Spotinst eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Spotinst**seçin **Spotinst** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Spotinst](./media/spotinst-tutorial/tutorial_spotinst_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Spotinst sınayın.

Tek iş için oturum açma için Azure AD ne Spotinst karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Spotinst ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Spotinst ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Spotinst test kullanıcısı oluşturma](#create-a-spotinst-test-user)**  - kullanıcı Azure AD gösterimini bağlı Spotinst Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Spotinst uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Spotinst yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Spotinst** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/spotinst-tutorial/tutorial_spotinst_samlbase.png)

3. Üzerinde **Spotinst etki alanı ve URL'ler** bölümünde, IDP tarafından başlatılan modunda uygulama yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![Spotinst etki alanı ve URL'ler tek oturum açma bilgileri](./media/spotinst-tutorial/tutorial_spotinst_url1.png)

    a. Denetleme **Gelişmiş URL ayarlarını göster**.

    b. İçinde **geçiş durumu** metin kutusuna bir değer girin: `<ID>`

    c. Uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://console.spotinst.com`

    > [!NOTE]
    > Geçiş durumu değeri gerçek değil. Geçiş durumu değeri, öğreticinin ilerleyen bölümlerinde açıklanan gerçek geçiş durumu değeri ile güncelleştirir.

4. Spotinst uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran için bir örnek gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/spotinst-tutorial/tutorial_Spotinst_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |
    | Email | User.Mail |
    | FirstName | User.givenName |
    | LastName | User.surname |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/spotinst-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/spotinst-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.

    e. Tıklayın **Tamam**

6. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/spotinst-tutorial/tutorial_spotinst_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/spotinst-tutorial/tutorial_general_400.png)

8. Bir başka web tarayıcı penceresinde Spotinst bir güvenlik yöneticisi olarak oturum açın.

9. Tıklayarak **kullanıcı simgesi** tıklayın ve ekranın üst sağ tarafında **ayarları**.

    ![Spotinst ayarları](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

10. Tıklayarak **güvenlik** üstte sekmesini seçip **kimlik sağlayıcıları** ve aşağıdaki adımları gerçekleştirin:

    ![Spotinst güvenlik](./media/spotinst-tutorial/tutorial_spotinst_security.png)

    a. Kopyalama **geçiş durumu** yapıştırın ve değer Örneğiniz için **geçiş durumu** metin kutusunda **Spotinst etki alanı ve URL'ler** bölümü Azure portalı.

    b. Tıklayın **Gözat** Azure portalından indirdiğiniz meta veri xml dosyasını karşıya yüklemek için

    c. **KAYDET**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/spotinst-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/spotinst-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/spotinst-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/spotinst-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-spotinst-test-user"></a>Spotinst test kullanıcısı oluşturma

Bu bölümün amacı Spotinst Britta Simon adlı bir kullanıcı oluşturmaktır.

1. Uygulamada yapılandırdıysanız **SP** intiated modu, aşağıdaki adımları gerçekleştirin:

   a. Bir başka web tarayıcı penceresinde Spotinst bir güvenlik yöneticisi olarak oturum açın.

   b. Tıklayarak **kullanıcı simgesi** tıklayın ve ekranın üst sağ tarafında **ayarları**.

    ![Spotinst ayarları](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

    c. Tıklayın **kullanıcılar** seçip **Kullanıcı Ekle**.

    ![Spotinst ayarları](./media/spotinst-tutorial/adduser1.png)

    d. Kullanıcı Ekle bölümünün üzerinde aşağıdaki adımları gerçekleştirin:

    ![Spotinst ayarları](./media/spotinst-tutorial/adduser2.png)

    * İçinde **tam adı** metin gibi kullanıcının tam adını girin **BrittaSimon**.

    * İçinde **e-posta** metin gibi kullanıcı e-posta adresini girin **brittasimon\@contoso.com**.

    * Kuruluşa özgü ayrıntılar için **kuruluş rolü, hesabı rolü ve hesapları**.

2. Uygulamada yapılandırdıysanız **IDP** intiated modu, burada, bu bölümde, hiçbir eylem öğesini. Spotinst tam zamanında sağlama, varsayılan olarak etkin olan destekler. Yeni bir kullanıcı, henüz yoksa Spotinst erişme denemesi sırasında oluşturulur.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Spotinst erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Spotinst için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Spotinst**.

    ![Uygulamalar listesinde Spotinst bağlantı](./media/spotinst-tutorial/tutorial_spotinst_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Spotinst kutucuğa tıkladığınızda, otomatik olarak Spotinst uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spotinst-tutorial/tutorial_general_01.png
[2]: ./media/spotinst-tutorial/tutorial_general_02.png
[3]: ./media/spotinst-tutorial/tutorial_general_03.png
[4]: ./media/spotinst-tutorial/tutorial_general_04.png

[100]: ./media/spotinst-tutorial/tutorial_general_100.png

[200]: ./media/spotinst-tutorial/tutorial_general_200.png
[201]: ./media/spotinst-tutorial/tutorial_general_201.png
[202]: ./media/spotinst-tutorial/tutorial_general_202.png
[203]: ./media/spotinst-tutorial/tutorial_general_203.png

