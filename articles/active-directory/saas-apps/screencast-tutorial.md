---
title: 'Öğretici: Azure Active Directory Tümleştirme ile yayını-O-Matic | Microsoft Docs'
description: Azure Active Directory ve yayını-O-Matic arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: 20c0acebde232bd50e6e5befed0facc96ee11b4d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040958"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Öğretici: Azure Active Directory Tümleştirme ile yayını-O-Matic

Bu öğreticide, Azure Active Directory (Azure AD) ile yayını-O-Matic tümleştirme konusunda bilgi edinin.

Yayını-O-Matic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yayını-O-Matic erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan yayını-O-Matic için (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi yayını-O-Matic ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir yayını-O-Matic çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Yayını-O-Matic galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Yayını-O-Matic galeri ekleme
Yayını-O-Matic, Azure AD'de tümleştirmesini yapılandırmak için ekran kaydı-O-Matic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Yayını-O-Matic Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **yayını-O-Matic**seçin **yayını-O-Matic** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Yayını-O-Matic sonuç listesinde](./media/screencast-tutorial/tutorial_screencast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve yayını-O-"Britta Simon" adlı bir test kullanıcı tabanlı Matic ile Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne yayını-O-Matic karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı yayını-O-Matic arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yayını-O-Matic ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yayını-O-Matic test kullanıcısı oluşturma](#create-a-screencast-o-matic-test-user)**  - yayını-O-kullanıcı Azure AD gösterimini bağlı Matic Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve yayını-O-Matic uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma yayını-O-Matic ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **yayını-O-Matic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/screencast-tutorial/tutorial_screencast_samlbase.png)

3. Üzerinde **yayını-O-Matic etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yayını-O-Matic etki alanı ve URL'ler tek oturum açma bilgileri](./media/screencast-tutorial/tutorial_screencast_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [yayını-O-Matic istemci Destek ekibine](mailto:support@screencast-o-matic.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/screencast-tutorial/tutorial_screencast_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/screencast-tutorial/tutorial_general_400.png)

6. Bir başka web tarayıcı penceresinde için yayını-O-Matic yönetici olarak oturum açın.

7. Tıklayarak **abonelik**.

    ![Abonelik](./media/screencast-tutorial/tutorial_screencast_sub.png)

8. Altında **erişim sayfası** bölümü tıklatın **Kurulum**.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_setup.png)

9. Üzerinde **Kurulum erişim sayfası**, aşağıdaki adımları gerçekleştirin:

    * Altında **erişim URL'si** bölümünde, belirtilen metin kutusunda, InstanceName yazın.

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_access.png)

    * Seçin **gerekli etki alanı kullanıcısı** altında **SAML kullanıcı kısıtlama (isteğe bağlı)** bölümü.

    * Altında **IDP meta veri XML dosyasını karşıya yükle**, tıklayın **Dosya Seç** Azure portalından indirdiğiniz metaveri yüklenecek.

    * **Tamam**’a tıklayın. 

    ![Erişim](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/screencast-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/screencast-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/screencast-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/screencast-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-screencast-o-matic-test-user"></a>Yayını-O-Matic test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon yayını-O-Matic içinde adlı bir kullanıcı oluşturmaktır. Yayını-O-Matic tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa yayını-O-Matic erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [yayını-O-Matic istemci Destek ekibine](mailto:support@screencast-o-matic.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, yayını-O-Matic için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon yayını-O-Matic için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **yayını-O-Matic**.

    ![Uygulamalar listesinde yayını-O-Matic bağlantı](./media/screencast-tutorial/tutorial_screencast_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde yayını-O-Matic kutucuğa tıkladığınızda, otomatik olarak yayını-O-Matic uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/screencast-tutorial/tutorial_general_01.png
[2]: ./media/screencast-tutorial/tutorial_general_02.png
[3]: ./media/screencast-tutorial/tutorial_general_03.png
[4]: ./media/screencast-tutorial/tutorial_general_04.png

[100]: ./media/screencast-tutorial/tutorial_general_100.png

[200]: ./media/screencast-tutorial/tutorial_general_200.png
[201]: ./media/screencast-tutorial/tutorial_general_201.png
[202]: ./media/screencast-tutorial/tutorial_general_202.png
[203]: ./media/screencast-tutorial/tutorial_general_203.png

