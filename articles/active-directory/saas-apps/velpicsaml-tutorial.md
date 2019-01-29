---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Velpic SAML | Microsoft Docs'
description: Azure Active Directory arasındaki Velpic SAML çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 80b1495153d77599a91713aa492688ce9f29d0f0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160896"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Öğretici: Velpic SAML ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Velpic SAML tümleştirme konusunda bilgi edinin.

Azure AD ile Velpic SAML tümleştirme ile aşağıdaki avantajları sağlar:

- Velpic SAML erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Velpic SAML açma, kullanıcılarınızın etkinleştirebilirsiniz
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Velpic SAML ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Velpic SAML çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Velpic SAML ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-velpic-saml-from-the-gallery"></a>Galeriden Velpic SAML ekleme
Azure AD'de SAML Velpic tümleştirmesini yapılandırmak için Velpic SAML Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Velpic SAML eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Velpic SAML**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/tutorial_velpicsaml_search.png)

1. Sonuçlar panelinde seçin **Velpic SAML**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Velpic SAML ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Velpic SAML karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Velpic SAML ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Velpic SAML içinde.

Yapılandırma ve Azure AD çoklu oturum açma Velpic SAML ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Velpic SAML test kullanıcısı oluşturma](#creating-a-velpic-saml-test-user)**  - Azure AD gösterimini her için bağlı Velpic SAML Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve Velpic SAML uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Velpic SAML ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Velpic SAML** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

1. Ayrıntıları girin **Velpic SAML etki alanı ve URL'ler** bölümü -

    ![Çoklu oturum açmayı yapılandırın](./media/velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. İçinde **oturum açma URL'si** metin değeri olarak yazın: `https://<sub-domain>.velpicsaml.net`

    b. İçinde **tanımlayıcı** metin kutusu, yapıştırma **'Çoklu oturum açma URL'si'** değeri `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Oturum açma URL'si Velpic SAML ekibi tarafından sağlanır ve tanımlayıcı değeri Velpic SAML tarafında SSO eklentisi yapılandırırken kullanılabilecek lütfen unutmayın. Bu değer Velpic SAML uygulama sayfasından kopyalayın ve kopyalayıp buraya yapıştırın gerekir.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/velpicsaml-tutorial/tutorial_general_400.png)

1. Velpic SAML yapılandırma bölümünü Velpic SAML yapılandırma oturum açmak için Yapılandır'ı tıklatın. SAML varlık kimliği için hızlı başvuru bölümünden kopyalayın.

1. Farklı bir web tarayıcı penceresinde Velpic SAML şirket sitenize yönetici olarak oturum.

1. Tıklayarak **Yönet** sekmesini ve Git **tümleştirme** bölümüne tıklayarak gereken **eklentileri** oturum açmak için yeni bir eklenti oluşturmak için.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_1.png)

1. Tıklayarak **'Eklentisi Ekle'** düğmesi.
    
    ![Eklentisi](./media/velpicsaml-tutorial/velpic_2.png)

1. Tıklayarak **SAML** kutucuğuna eklentisini ekleyin sayfasında.
    
    ![Eklentisi](./media/velpicsaml-tutorial/velpic_3.png)

1. Yeni SAML eklentisini adını girin ve tıklayın **'Ekle'** düğmesi.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_4.png)

1. Ayrıntılar aşağıdaki gibi girin:

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_5.png)

    a. İçinde **adı** metin SAML eklentisini adını yazın.

    b. İçinde **veren URL'si** metin kutusu, yapıştırma **SAML varlık kimliği** , kopyalamanın **yapılandırma oturum açma** Azure portal'ın penceresi.

    c. İçinde **sağlayıcısı meta verileri yapılandırma** Azure portalından indirilen meta veri XML dosyasını karşıya yükleyin.

    d. Ayrıca yalnızca zaman etkinleştirerek sağlama SAML etkinleştirmeyi seçebilirsiniz **'Otomatik olarak yeni kullanıcılar oluşturma'** onay kutusu. Bir kullanıcı Velpic yok ve bu bayrağı etkin değil, Azure oturum açma işlemi başarısız olur. Bayrak etkinleştirilirse kullanıcının otomatik olarak Velpic oturum açma sırasında sağlanacaktır. 

    e. Kopyalama **çoklu oturum açma URL'si** metin kutusu ve Azure portalında yapıştırın.
    
    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/velpicsaml-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-velpic-saml-test-user"></a>Velpic SAML test kullanıcısı oluşturma

Bu adım yalnızca zaman sağlama kullanıcı uygulamanın desteklediği gibi genellikle gerekli değildir. Otomatik kullanıcı hazırlama etkin değilse el ile kullanıcı oluşturma aşağıda açıklandığı gibi yapılabilir.

Velpic SAML şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları uygulayın:
    
1. Yönet sekmesini tıklatın ve kullanıcılar bölümüne gidin ve ardından kullanıcı eklemek için yeni düğmesine tıklayın.

    ![Kullanıcı Ekle](./media/velpicsaml-tutorial/velpic_7.png)

1. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin.

    ![kullanıcı](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. İçinde **ad** metin Britta simon'un tür ilk adı.

    b. İçinde **Soyadı** metin Britta Simon son adını yazın.

    c. İçinde **kullanıcı adı** metin Britta simon'un kullanıcı adını yazın.

    d. İçinde **e-posta** metin Britta Simon hesabı e-posta adresini yazın.

    e. Geri kalan bilgileri isteğe bağlı olduğundan, gerekirse doldurabilirsiniz.
    
    f. **KAYDET**'e tıklayın.  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma için SAML Velpic erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Velpic SAML için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Velpic SAML**.

    ![Çoklu oturum açmayı yapılandırın](./media/velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim paneli Velpic SAML kutucuğa tıkladığınızda, oturum açma sayfası Velpic SAML uygulamanın almanız gerekir. Görmelisiniz **'Oturumu, Azure AD'de oturum'** oturum açma sayfasında düğme.

    ![Eklentisi](./media/velpicsaml-tutorial/velpic_6.png)

1. Tıklayarak **'Oturumu, Azure AD'de oturum'** düğmesini için Velpic Azure AD'ye hesabınızı kullanarak oturum açın.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/velpicsaml-tutorial/tutorial_general_203.png

