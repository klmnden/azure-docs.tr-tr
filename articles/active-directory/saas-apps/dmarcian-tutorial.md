---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle dmarcian | Microsoft Docs'
description: Azure Active Directory ve dmarcian arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2018
ms.author: jeedes
ms.openlocfilehash: 677c40932a557b8a15a51b947794b4281801f65a
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637656"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Öğretici: Azure Active Directory dmarcian ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile dmarcian tümleştirme konusunda bilgi edinin.

Azure AD ile dmarcian tümleştirme ile aşağıdaki avantajları sağlar:

- Dmarcian erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için dmarcian (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile dmarcian yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik dmarcian çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden dmarcian ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-dmarcian-from-the-gallery"></a>Galeriden dmarcian ekleme
Azure AD'de dmarcian tümleştirmesini yapılandırmak için dmarcian Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden dmarcian eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **dmarcian**seçin **dmarcian** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde dmarcian](./media/dmarcian-tutorial/tutorial_dmarcian_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı dmarcian sınayın.

Tek iş için oturum açma için Azure AD ne dmarcian karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının dmarcian ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma dmarcian ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Dmarcian test kullanıcısı oluşturma](#create-a-dmarcian-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı dmarcian içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve dmarcian uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile dmarcian yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **dmarcian** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dmarcian-tutorial/tutorial_dmarcian_samlbase.png)

3. Üzerinde **dmarcian etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![dmarcian etki alanı ve URL'ler tek oturum açma bilgileri](./media/dmarcian-tutorial/tutorial_dmarcian_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian.com-eu/sso/saml/<ACCOUNT_ID>/sp.xml ` |
    | `https://dmarcian.com-ap/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian.com-eu/login/<ACCOUNT_ID>/handle/ `|
    | `https://dmarcian.com-ap/login/<ACCOUNT_ID>/handle/`|

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![dmarcian etki alanı ve URL'ler tek oturum açma bilgileri](./media/dmarcian-tutorial/tutorial_dmarcian_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian.com-eu/login/<ACCOUNT_ID>` |
    | `https://dmarcian.com-ap/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL'si ile güncelleştirir. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/dmarcian-tutorial/tutorial_dmarcian_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/dmarcian-tutorial/tutorial_general_400.png)
    
7. Farklı bir web tarayıcı penceresinde dmarcian için bir güvenlik yöneticisi olarak oturum açın.

8. Tıklayarak **profili** sağ üst köşe ve gidin **tercihleri**.

    ![Tercihleri ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

9. Ekranı aşağı kaydırın ve tıklayarak **çoklu oturum açma** bölümüne ve ardından tıklayarak **yapılandırma**.

    ![Tek ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

10. Üzerinde **SAML çoklu oturum açma** sayfasında kümesi **durumu** olarak **etkin** ve aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulaması ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * Altında **kimlik sağlayıcınız dmarcian ekleme** bölümünde **kopyalama** kopyalamak için **onay belgesi tüketici hizmeti URL'si** örneğinizin yapıştırın  **Yanıt URL'si** metin kutusunda **dmarcian etki alanı ve URL'ler bölüm** Azure portalında.

    * Altında **kimlik sağlayıcınız dmarcian ekleme** bölümünde **kopyalama** kopyalamak için **varlık kimliği** örneğinizin yapıştırın **tanımlayıcı**metin kutusunda **dmarcian etki alanı ve URL'ler bölüm** Azure portalında.

    * Altında **kimlik doğrulamasını ayarlama** bölümünde **kimlik sağlayıcısı meta verileri** textbox Yapıştır **uygulama Federasyon meta verileri URL'sini**, hangi Azure Portalı'ndan kopyaladığınız.

    * Altında **kimlik doğrulamasını ayarlama** bölümünde **özniteliği deyimleri** metin URL'sini yapıştırın `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    * Altında **oturum açma URL'sini ayarlayın** bölümünde, kopya **oturum açma URL'si** örneğinizin yapıştırın **oturum açma URL'si** metin kutusunda **dmarcian etki alanı ve URL'ler bölüm** Azure portalında.

        > [!Note]
        > Değiştirebileceğiniz **oturum açma URL'si** kuruluşunuza göre.

    * **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dmarcian-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/dmarcian-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dmarcian-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dmarcian-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dmarcian-test-user"></a>Dmarcian test kullanıcısı oluşturma

Dmarcian için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların dmarcian sağlanması gerekir. Dmarcian içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Dmarcian için bir güvenlik yöneticisi olarak oturum açın.

2. Tıklayarak **profili** sağ üst köşe ve gidin **Kullanıcıları Yönet**.

    ![Kullanıcı ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. Sağ alt tarafında **SSO kullanıcıların** bölümünde, tıklayarak **yeni kullanıcı Ekle**.

    ![Kullanıcı Ekle ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Üzerinde **yeni kullanıcı Ekle** açılan, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. İçinde **yeni kullanıcı e-posta** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

    b. Kullanıcısına yönetim hakları vermek isteyip istemediğinizi seçin **olun kullanıcı yönetici**.

    c. Tıklayın **kullanıcı ekleme**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma dmarcian erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon dmarcian için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **dmarcian**.

    ![Uygulamalar listesinde dmarcian bağlantı](./media/dmarcian-tutorial/tutorial_dmarcian_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde dmarcian kutucuğa tıkladığınızda, otomatik olarak dmarcian uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dmarcian-tutorial/tutorial_general_01.png
[2]: ./media/dmarcian-tutorial/tutorial_general_02.png
[3]: ./media/dmarcian-tutorial/tutorial_general_03.png
[4]: ./media/dmarcian-tutorial/tutorial_general_04.png

[100]: ./media/dmarcian-tutorial/tutorial_general_100.png

[200]: ./media/dmarcian-tutorial/tutorial_general_200.png
[201]: ./media/dmarcian-tutorial/tutorial_general_201.png
[202]: ./media/dmarcian-tutorial/tutorial_general_202.png
[203]: ./media/dmarcian-tutorial/tutorial_general_203.png

