---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Vodeclic | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Vodeclic arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.openlocfilehash: bc889919f2d869478843881cc8eae06fc9cb232c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34348215"
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Öğretici: Azure Active Directory Tümleştirme Vodeclic ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Vodeclic tümleştirmek öğrenin.

Vodeclic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Vodeclic erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Vodeclic (çoklu oturum açma veya SSO) ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Vodeclic ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Vodeclic SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Vodeclic ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-vodeclic-from-the-gallery"></a>Galeriden Vodeclic Ekle
Azure AD Vodeclic tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Vodeclic eklemeniz gerekir.

**Galeriden Vodeclic eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vodeclic**. Seçin **Vodeclic** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Vodeclic](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Vodeclic ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de Vodeclic karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, Vodeclic içinde bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Vodeclic içinde değere vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan.

Yapılandırma ve Azure AD çoklu oturum açma Vodeclic ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Vodeclic test kullanıcısı oluşturma](#create-a-vodeclic-test-user) karşılık gelen Britta Simon, kullanıcının Azure AD gösterimini bağlı Vodeclic sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Vodeclic uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Vodeclic yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Vodeclic** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **çoklu oturum açma modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Uygulamada yapılandırmak istiyorsanız, **IDP** modunda başlatılan **Vodeclic etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Vodeclic etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desende bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml`

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Uygulamada yapılandırmak istiyorsanız, **SP** başlatılan modu, select **Göster Gelişmiş URL ayarları** onay kutusunu işaretleyin ve aşağıdaki adımları uygulamanız:

    ![Vodeclic etki alanı ve oturum açma URL'leri tek bilgi](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    İçinde **oturum açma URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Bu değerleri gerçek değil. Gerçek tanımlayıcısı ile bu değerleri güncelleştirmek URL'si ve oturum açma URL'si yanıtlayın. Kişi [Vodeclic istemci destek ekibi](mailto:hotline@vodeclic.com) bu değerleri almak için.

5. İçinde **SAML imzalama sertifikası** bölümünde, select **meta veri XML**. Ardından meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. **Kaydet**’i seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Çoklu oturum açma yapılandırmak için **Vodeclic** tarafı, indirilen Gönder **meta veri XML** için [Vodeclic destek ekibi](mailto:hotline@vodeclic.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-vodeclic-test-user"></a>Vodeclic test kullanıcısı oluşturma

Bu bölümde, Vodeclic içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Vodeclic destek ekibi](mailto:hotline@vodeclic.com) Vodeclic platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

> [!NOTE]
> Uygulama gereksinimlerine göre makine Güvenilenler listesine al gerekebilir. Ortak IP adresi ile paylaşmak, gerçekleşmesi gerekir [Vodeclic destek ekibi](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Vodeclic için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Vodeclic için Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve dizin görünümüne gidin. Ardından, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Vodeclic**.

    ![Uygulamalar listesinde Vodeclic bağlantı](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** iletişim kutusu.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde Vodeclic döşeme seçtiğinizde, otomatik olarak Vodeclic uygulamanıza oturum.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_203.png

