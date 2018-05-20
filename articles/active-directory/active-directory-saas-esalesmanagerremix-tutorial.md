---
title: 'Öğretici: Azure Active Directory E satış yöneticisi Remix ile tümleştirin. | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasındaki E satış yöneticisi Remix yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: f681eb91c1e79eb42b572956dfab93e620489e74
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Azure Active Directory E satış yöneticisi Remix ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) E satış yöneticisi Remix ile tümleştirmek öğrenin.

Azure AD, E satış yöneticisi Remix ile tümleştirerek, aşağıdaki faydaları alın:

- E satış yöneticisi Remix erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak E satış yöneticisi Remix için (çoklu oturum açma veya SSO) ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme E satış yöneticisi Remix ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir E satış yöneticisi Remix SSO etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamında kullanın.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

* Galeriden E satış yöneticisi Remix ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>Galeriden E satış yöneticisi Remix Ekle
Azure ad tümleştirme E satış yöneticisi Remix ile yapılandırmak için E satış yöneticisi Remix Galeriden yönetilen SaaS uygulamaları listenize aşağıdakileri yaparak ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** pencerenin üstündeki.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **E satış yöneticisi Remix**seçin **E satış yöneticisi Remix** sonuçları listesi ve ardından **Ekle**.

    ![Sonuçlar listesinde Satış Yöneticisi Remix E](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma E "Britta Simon." olarak adlandırılan bir test kullanıcı dayalı satış yöneticisi Remix ile test etme

Tekli çalışmaya oturum için Azure AD'de E satış yöneticisi Remix kullanıcı ve kendisine karşılık gelen tanımlamak Azure AD gerekiyor. Diğer bir deyişle, bir Azure AD kullanıcısının E satış yöneticisi Remix de aynı kullanıcı arasında bir bağlantı ilişkisi kurulmalıdır.

Yapılandırma ve Azure AD çoklu oturum açma E satış yöneticisi Remix ile test etmek için sonraki beş bölümlerde yapı taşları tamamlayın:

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma E satış yöneticisi Remix uygulamanızda aşağıdakileri yaparak yapılandırın:

1. Azure portalında üzerinde **E satış yöneticisi Remix** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    !["Çoklu oturum açmayı" bağlantı][4]

2. İçinde **çoklu oturum açma** penceresi, **tek oturum açma modu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açmayı" penceresi](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Altında **E satış yöneticisi Remix etki ve URL'leri**, aşağıdakileri yapın:

    ![E satış yöneticisi Remix etki alanı ve URL'leri tek oturum açma bilgilerini](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<Server temel URL > /\<alt etki alanı > / esales pc*.

    b. İçinde **tanımlayıcısı** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<Server temel URL > /\<alt etki alanı > /*.

    c. Not **tanımlayıcısı** Bu öğreticide daha sonra kullanmak için değer.
    
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerleri almak için başvurun [E satış yöneticisi Remix istemci destek ekibi](mailto:esupport@softbrain.co.jp).

4. Altında **SAML imzalama sertifikası**seçin **sertifika (Base64)** ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika (Base64) indirme bağlantısı](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Seçin **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusunu işaretleyin ve ardından **emailaddress** özniteliği.
    
    ![Kullanıcı öznitelikleri penceresi](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

    **Öznitelik Düzenle** penceresi açılır.

6. Kopya **Namespace** ve **adı** değerleri. Düzende değeri üretmek  *\<Namespace > /\<adı >* ve Bu öğreticide daha sonra kullanmak için kaydedin.

    ![Öznitelik Düzenle penceresi](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Altında **E satış yöneticisi Remix Yapılandırması**seçin **yapılandırma E satış yöneticisi Remix**.

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    **Yapılandırma oturum açma** penceresi açılır.

8. İçinde **hızlı başvuru** bölümünde, oturum kapatma URL'sini ve SAML çoklu oturum açma hizmeti URL'sini kopyalayın.

9. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

10. Uygulamanıza E satış yöneticisi Remix yönetici olarak oturum açın.

11. Sağ üst kısmında, seçin **yönetici menüsüne**.

    !["Yönetici menü" komutu](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

12. Sol bölmede seçin **sistem ayarlarını** > **Dış Sistem işbirliği**.

    !["Sistem ayarları" ve "Dış Sistem işbirliği" bağlantılar](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
13. İçinde **Dış Sistem işbirliği** penceresinde, seçin **SAML**.

    !["Dış Sistem işbirliği" penceresi](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

14. Altında **SAML kimlik doğrulama ayarını**, aşağıdakileri yapın:

    !["SAML kimlik doğrulama ayarını" bölümü](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Seçin **PC Sürüm** onay kutusu.
    
    b. İçinde **işbirliği öğesi** bölümünde aşağı açılan listesinde seçin **e-posta**.

    c. İçinde **işbirliği öğesi** kutusunda, Azure portalından önceden kopyaladığınız talep değerini yapıştırın (diğer bir deyişle, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**).

    d. İçinde **veren (varlık kimliği)** kutusunda, daha önce kopyalanan tanımlayıcı değeri yapıştırın **E satış yöneticisi Remix etki ve URL'leri** Azure portalının bölümü.

    e. Azure Portalı'ndan indirilen sertifikanızı karşıya yüklemek için seçin **dosya seçimi**.

    f. İçinde **kimlik sağlayıcısı oturum açma URL'si** kutusunda, Azure portalda daha önce kopyaladığınız SAML çoklu oturum açma hizmeti URL'sini yapıştırın.

    g. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** kutusunda, Azure portalda daha önce kopyaladığınız oturum kapatma URL'si değeri yapıştırabilirsiniz.

    h. Seçin **tam ayarını**.

> [!TIP]
> Uygulaması kuruluyor gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamada ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve ardından erişim belgelerde katıştırılmış **yapılandırma** alt bölüm. Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Üstündeki **tüm kullanıcılar** penceresinde, seçin **Ekle**.

    ![Ekle düğmesi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)
    
    **Kullanıcı** penceresi açılır.

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri not edin **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Bir E satış yöneticisi Remix test kullanıcısı oluşturma

1. Uygulamanıza E satış yöneticisi Remix yönetici olarak oturum açma.

2. Seçin **yönetici menüsünde** sağ üst menüsünde.

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Seçin **şirketinizin ayarları** > **bakım Departmanlar ve çalışan**ve ardından **kaydedilmiş çalışanlar**.

    !["Çalışanlar kayıtlı" sekmesi](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. İçinde **yeni çalışan kayıt** bölümünde, aşağıdakileri yapın:
    
    !["Yeni çalışan kayıt" bölümü](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. İçinde **çalışan adı** kullanıcının adını yazın (örneğin, **Britta**).

    b. Kalan gerekli alanları doldurun.
    
    c. SAML etkinleştirirseniz, yönetici oturum açma sayfasından oturum açamaz. Oturum açma GRANT yönetici ayrıcalıkları seçerek kullanıcıya **yönetici oturum açma** onay kutusu.

    d. Seçin **kayıt**.

5. Gelecekte, yönetici olarak oturum açmak için oturum yönetici izinlerine sahip ve sağ üst kısmında, ardından kullanıcı olarak **yönetici menüsüne**.

    !["Yönetici menü" komutu](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta E satış yöneticisi Remix için erişim izni verme, Azure çoklu oturum açma kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın: 

![Kullanıcı rolü atayın][200] 

1. Azure portalında açmak **uygulamaları** görünümü, Git **dizin** görüntülemek ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamaları" bağlantılar][201] 

2. İçinde **uygulamaları** listesinde **E satış yöneticisi Remix**.

    ![E satış yöneticisi Remix bağlantı](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. Seçin **seçin** düğmesi.

7. İçinde **eklemek atama** penceresinde, seçin **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde E satış yöneticisi Remix döşeme seçtiğinizde, otomatik olarak E satış yöneticisi Remix uygulamanıza oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

