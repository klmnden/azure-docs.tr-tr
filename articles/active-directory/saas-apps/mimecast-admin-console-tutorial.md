---
title: 'Öğretici: Azure Active Directory Mimecast Yönetici Konsolu ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Mimecast Yönetici Konsolu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 27c9606357d9599fa56e4045606f8d9046722e7f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041730"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Öğretici: Azure Active Directory Mimecast Yönetici Konsolu ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Mimecast Yönetici Konsolu tümleştirme konusunda bilgi edinin.

Mimecast Yönetim Konsolu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mimecast yönetici Konsolu'na erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Mimecast yönetim konsoluna açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Yönetici Konsolu Mimecast çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Mimecast Yönetici Konsolu'nu ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Galeriden Mimecast Yönetici Konsolu'nu ekleme
Azure AD Yönetici Konsolu Mimecast tümleştirilmesi yapılandırmak için Mimecast Yönetici Konsolu Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mimecast Yönetici Konsolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Mimecast Yönetici Konsolu**seçin **Mimecast Yönetici Konsolu** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Mimecast Yönetici Konsolu](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Mimecast Yönetici Konsolu ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Mimecast Yönetici konsolunda bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Mimecast Yönetici konsolunda ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Mimecast yönetim konsolunda, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yönetici Konsolu Mimecast test kullanıcısı oluşturma](#create-a-mimecast-admin-console-test-user)**  - Mimecast Yönetici konsolunda, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mimecast yönetici Konsol uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Mimecast Yönetici Konsolu ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mimecast Yönetici Konsolu** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Üzerinde **Mimecast Yönetici Konsolu etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast Yönetici Konsolu etki alanı ve URL'ler tek oturum açma bilgileri](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Oturum açma URL'si belirli bölgedir.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Üzerinde **Mimecast Yönetici Konsolu Yapılandırması** bölümünde **Mimecast Yönetici konsolunda yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Mimecast Yönetici Konsolu yapılandırması](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Farklı bir web tarayıcı penceresinde Mimecast yönetici konsolunuza yönetici olarak oturum.

8. Git **Hizmetleri \> uygulama**.

    ![Hizmetleri](./media/mimecast-admin-console-tutorial/ic794998.png "Hizmetleri")

9. Tıklayın **kimlik doğrulaması profilleri**.

    ![Kimlik doğrulaması profilleri](./media/mimecast-admin-console-tutorial/ic794999.png "kimlik doğrulaması profilleri")
    
10. Tıklayın **yeni kimlik doğrulama profili**.

    ![Yeni kimlik doğrulama profilleri](./media/mimecast-admin-console-tutorial/ic795000.png "yeni kimlik doğrulama profilleri")

11. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama profili](./media/mimecast-admin-console-tutorial/ic795015.png "kimlik doğrulama profili")
    
    a. İçinde **açıklama** metin yapılandırmanız için bir ad yazın.
    
    b. Seçin **SAML kimlik doğrulaması Mimecast Yönetici Konsolu için zorunlu**.
    
    c. Olarak **sağlayıcısı**seçin **Azure Active Directory**.
    
    d. Yapıştırma **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız **veren URL'si** metin.
    
    e. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız **oturum açma URL'si** metin.

    f. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız **oturum kapatma URL'si** metin.
    
    >[!NOTE]
    >Oturum açma URL'si ve oturum kapatma URL'si değerleri Mimecast Yönetici Konsolu için aynıdır.
    
    g. Açık base-64 sertifikanızı indirilen Not Defteri'nde, Azure portalından ilk satırı Kaldır ("*--*") ve son satırı ("*--*"), kalan içeriği içine kopyalayın, Pano, kendisine yapıştırın **kimlik sağlayıcısı sertifikası (meta veriler)** metin.
    
    h. Seçin **çoklu oturum açmaya izin ver**.
    
    i. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/mimecast-admin-console-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/mimecast-admin-console-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/mimecast-admin-console-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Yönetici Konsolu Mimecast test kullanıcısı oluşturma

Azure AD kullanıcılarının Mimecast Yönetici Konsolu'nda sizin oturum etkinleştirmek için bunlar Mimecast yönetici konsoluna sağlanması gerekir. Mimecast yönetim söz konusu olduğunda, sağlama elle bir görevin konsoludur.

* Kullanıcılar oluşturabilmeniz için önce bir etki alanı kayıt olmanız gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mimecast Yönetici Konsolu** yönetici olarak.
2. Git **dizinleri \> iç**.
   
   ![Dizinleri](./media/mimecast-admin-console-tutorial/ic795003.png "dizinleri")
3. Tıklayın **yeni etki alanı kayıt**.
   
   ![Yeni etki alanı kayıt](./media/mimecast-admin-console-tutorial/ic795004.png "kaydetme yeni etki alanı")
4. Yeni etki alanınız oluşturulduktan sonra tıklayın **yeni adresi**.
   
   ![Yeni adresi](./media/mimecast-admin-console-tutorial/ic795005.png "yeni adresi")
5. Yeni adresi iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![Kaydet](./media/mimecast-admin-console-tutorial/ic795006.png "Kaydet")
   
   a. Tür **e-posta adresi**, **genel adı**, **parola**, ve **parolayı onayla** öznitelikleri geçerli bir Azure ad hesabı istiyorsunuz ilgili metin kutularına sağlayın.

   b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Mimecast Yönetim Konsolu kullanıcı hesabı oluşturma araçları veya Mimecast Yönetim Konsolu tarafından sağlanan API'leri kullanabilirsiniz. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Mimecast yönetim konsoluna erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Mimecast yönetim konsoluna atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Mimecast Yönetici Konsolu**.

    ![Uygulamalar listesinde Mimecast Yönetici konsolunda bağlantı](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Mimecast Yönetici Konsolu kutucuğa tıkladığınızda, otomatik olarak Mimecast Yönetici Konsolu uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-admin-console-tutorial/tutorial_general_203.png

