---
title: 'Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Arcgıs Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 3284202ffaa6767a8dd4a6a5050dbdc928075237
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846131"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: Arcgıs Online ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Arcgıs Online tümleştirme konusunda bilgi edinin.

Arcgıs Online Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Arcgıs Online erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Arcgıs Online için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Arcgıs Online ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Arcgıs Online çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Arcgıs Online galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-arcgis-online-from-the-gallery"></a>Arcgıs Online galeri ekleme

Azure AD'de Arcgıs Online'nın tümleştirmesini yapılandırmak için Arcgıs Online Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Arcgıs Online Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/arcgis-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/arcgis-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/arcgis-tutorial/a_new_app.png)

4. Arama kutusuna **Arcgıs Online**seçin **Arcgıs Online** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Arcgıs Online ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Arcgıs Online bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Arcgıs Online arasında bir bağlantı ilişki kurulması gerekir.

Arcgıs Online içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Arcgıs Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Arcgıs Online bir test kullanıcısı oluşturma](#create-an-arcgis-online-test-user)**  - Arcgıs kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Arcgıs Online uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Arcgıs Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Arcgıs Online** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.maps.arcgis.com`.

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `<companyname>.maps.arcgis.com`.

    ![image](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Arcgıs Online istemci Destek ekibine](https://support.esri.com/en/) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** bölümü tıklatın üzerinde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda xml dosyasını kaydedin.

    ![image](./media/arcgis-tutorial/federationxml.png)

7. İçinde yapılandırmasını otomatik hale getirmenizi **Arcgıs Online**, yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![image](./media/arcgis-tutorial/install_extension.png)

8. Uzantı tarayıcıya ekledikten sonra tıklayarak **Arcgıs Online Kurulum** Arcgıs Online uygulamaya yönlendirir. Burada, Arcgıs Online imzalamak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 9-13 arasındaki adımları otomatik hale getirin.

9. Arcgıs Online el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi açın ve Arcgıs şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

10. Tıklayın **ayarları düzenleme**.

    ![Ayarları Düzenle](./media/arcgis-tutorial/ic784742.png "ayarlarını Düzenle")

11. Tıklayın **güvenlik**.

    ![Güvenlik](./media/arcgis-tutorial/ic784743.png "güvenlik")

12. Altında **Kurumsal oturum açma bilgileri**, tıklayın **AYARLANMIŞ bir kimlik sağlayıcı**.

    ![Kurumsal oturum açma bilgileri](./media/arcgis-tutorial/ic784744.png "Kurumsal oturum açma bilgileri")

13. Üzerinde **ayarlanmış bir kimlik sağlayıcı** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısını Ayarla](./media/arcgis-tutorial/ic784745.png "kimlik sağlayıcısını Ayarla")

    a. İçinde **adı** metin kutusuna kuruluşunuzun adını yazın.

    b. İçin **Kurumsal kimlik sağlayıcısı meta verileri kullanarak belirtilmelidir**seçin **dosya**.

    c. İndirilen meta verileri dosyanızı karşıya yüklemek için tıklayın **dosya**.

    d. Tıklayın **KÜMESİ kimlik SAĞLAYICISI**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/arcgis-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/arcgis-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/arcgis-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-an-arcgis-online-test-user"></a>Arcgıs Online bir test kullanıcısı oluşturma

Arcgıs Online'da oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Arcgıs Online sağlanması gerekir.  
Arcgıs Online söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Arcgıs** Kiracı.

2. Tıklayın **davet ÜYELERİ**.
   
    ![Üyeler davet](./media/arcgis-tutorial/ic784747.png "üyeler davet")

3. Seçin **üyeleri otomatik olarak bir e-posta göndermeden ekleme**ve ardından **sonraki**.
   
    ![Üyeleri otomatik olarak Ekle](./media/arcgis-tutorial/ic784748.png "üyeleri otomatik olarak Ekle")

4. Üzerinde **üyeleri** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
     ![Ekleme ve gözden geçirme](./media/arcgis-tutorial/ic784749.png "ekleme ve gözden geçirme")
    
     a. Girin **e-posta**, **ad**, ve **Soyadı** sağlamak istediğiniz geçerli bir AAD hesabı.
  
     b. Tıklayın **ekleme ve gözden geçirme**.
5. Girdiğiniz ve ardından verileri gözden **Üye Ekle**.
   
    ![Üye Ekle](./media/arcgis-tutorial/ic784750.png "Üye Ekle")
        
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Arcgıs Online erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/arcgis-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Arcgıs Online**.

    ![image](./media/arcgis-tutorial/d_all_application.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/arcgis-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Arcgıs Online kutucuğa tıkladığınızda, size otomatik olarak Arcgıs Online uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



