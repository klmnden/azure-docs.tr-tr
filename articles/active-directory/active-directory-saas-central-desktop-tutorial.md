---
title: 'Öğretici: Azure Active Directory Tümleştirme Merkezi Masaüstü ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Merkezi Masaüstü arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 92c24688cf3d9baefcedcf22c915752b2d29b53c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Azure Active Directory Tümleştirme ile Merkezi Masaüstü

Bu öğreticide, Azure Active Directory (Azure AD) ile Merkezi Masaüstü tümleştirmek öğrenin.

Merkezi Masaüstü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Merkezi Masaüstü erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Merkezi Masaüstü kendi Azure AD hesaplarıyla oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Merkezi Masaüstü ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Merkezi Masaüstü tek oturum üzerindeki etkin olmayan abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam zaten yoksa [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Merkezi Masaüstü ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-central-desktop-from-the-gallery"></a>Merkezi Masaüstü Galerisi'nden ekleme
Azure AD Merkezi Masaüstü tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Merkezi Masaüstü eklemeniz gerekir.

**Merkezi Masaüstü Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Merkezi Masaüstü**. Seçin **Merkezi Masaüstü** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Merkezi Masaüstü](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Merkezi "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı masaüstü ile test etme

Tekli çalışmaya oturum için Azure AD için bir kullanıcı Azure AD'de Merkezi Masaüstü karşılık gelen kullanıcı olan bilmek ister. Diğer bir deyişle, Merkezi Masaüstü bir Azure AD kullanıcısının ve ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Merkezi Masaüstü vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcılar arasında bağlantı kurulduktan.

Yapılandırma ve Azure AD çoklu oturum açma Merkezi Masaüstü ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Merkezi Masaüstü test kullanıcısı oluşturma](#create-a-central-desktop-test-user) Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı olduğu Merkezi Masaüstü sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Merkezi Masaüstü uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Merkezi Masaüstü ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında üzerinde **Merkezi Masaüstü** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **modu** aşağı açılan listesinden, **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. İçinde **Merkezi Masaüstü etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri tek bir merkezi Masaüstü etki alanı ve URL'leri](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<companyname>.centraldesktop.com`

    b. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desende bir URL yazın:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. İçinde **yanıt URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek tanımlayıcısı ile bu değerleri güncelleştirmek URL'si ve oturum açma URL'si yanıtlayın. Kişi [Merkezi Masaüstü istemci destek ekibi](https://imeetcentral.com/contact-us) bu değerleri almak için. 

4. İçinde **SAML imzalama sertifikası** bölümünde, select **sertifika**. Ardından, bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/active-directory-saas-central-desktop-tutorial/tutorial_general_400.png)
    
6. İçinde **merkezi masaüstü yapılandırması** bölümünde, select **yapılandırma Merkezi Masaüstü** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru** bölümü.

    ![Merkezi masaüstü yapılandırması](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Oturum açın, **Merkezi Masaüstü** Kiracı.

8. Git **ayarları**. Seçin **Gelişmiş**ve ardından **çoklu oturum açma**.

    ![Kurulum - Gelişmiş](./media/active-directory-saas-central-desktop-tutorial/ic769563.png "Kurulum - Gelişmiş")

9. Üzerinde **tek oturum açma ayarları** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma ayarları](./media/active-directory-saas-central-desktop-tutorial/ic769564.png "tek oturum açma ayarları")
    
    a. Seçin **etkinleştir SAML v2 çoklu oturum açma**.
    
    b. İçinde **SSO URL** kutusunda, yapıştırma **SAML varlık kimliği** Azure portalından kopyaladığınız değeri.
    
    c. İçinde **SSO oturum açma URL'si** kutusunda, yapıştırma **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri.
    
    d. İçinde **SSO oturum kapatma URL'si** kutusunda, yapıştırma **Sign-Out URL** Azure portalından kopyaladığınız değeri.

10. İçinde **ileti imzası doğrulama yöntemi** bölümünde, aşağıdaki adımları uygulayın:

    ![İleti imzası doğrulama yöntemi](./media/active-directory-saas-central-desktop-tutorial/ic769565.png "ileti imzası doğrulama yöntemi") bir. **Sertifika**’yı seçin.
    
    b. İçinde **SSO sertifika** listesinde **RSH SHA256**.
    
    c. İndirilen sertifikanızı Not Defteri'nde açın. Sonra sertifika içeriğini kopyalayın ve yapıştırın **SSO sertifika** alan.
        
    d. Seçin **SAMLv2 oturum açma sayfanız bir bağlantı görüntüler**.
    
    e. Seçin **güncelleştirme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve sonra katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-central-desktop-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-central-desktop-test-user"></a>Merkezi Masaüstü test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açabilmesi için bunların merkezi masaüstü uygulamasında sağlanmalıdır. Bu bölümde Azure AD kullanıcı hesaplarının Merkezi Masaüstü nasıl oluşturulacağı açıklanmaktadır.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için herhangi bir merkezi Masaüstü kullanıcı hesabı oluşturma araçlarını veya Merkezi Masaüstü tarafından sağlanan API'ları kullanabilirsiniz.

**Merkezi Masaüstü kullanıcı hesaplarına sağlamak için:**

1. Merkezi Masaüstü kiracınız için oturum açın.

2. Git **kişiler** > **iç üyeleri**.

3. Seçin **iç üye eklemek**.

    ![Kişiler](./media/active-directory-saas-central-desktop-tutorial/ic781051.png "kişiler")
    
4. İçinde **yeni üyeler e-posta adresi** sağlamak ve ardından istediğiniz bir Azure AD hesabının yazın **sonraki**.

    ![E-posta adreslerini yeni üyeler](./media/active-directory-saas-central-desktop-tutorial/ic781052.png "e-posta adreslerini yeni üyeler")

5. Seçin **Ekle iç üyeleri**.

    ![İç Üye Ekle](./media/active-directory-saas-central-desktop-tutorial/ic781053.png "iç Üye Ekle")
   
   >[!NOTE]
   >Eklediğiniz kullanıcı hesaplarını etkinleştirmek için onay bağlantısı içeren bir e-posta alırsınız.
   
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta merkezi masaüstüne erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Merkezi Masaüstü Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümünü açın. Dizin görünümüne gidin ve gidin **kurumsal uygulamalar**.

2. Seçin **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Merkezi Masaüstü**.

    ![Uygulamalar listesinde Merkezi Masaüstü Bağlantısı](./media/active-directory-saas-central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **eklemek atama** iletişim kutusu.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı erişim paneli kullanarak sınayın.

Erişim panelinde Merkezi Masaüstü döşeme seçtiğinizde, otomatik olarak Merkezi Masaüstü uygulamanıza oturum.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-central-desktop-tutorial/tutorial_general_203.png

