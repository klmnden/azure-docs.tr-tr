---
title: 'Öğretici: İş için Dropbox ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve iş için Dropbox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 591d8d28be5fc9322de8cf4886dc5924f53b1deb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103775"
---
# <a name="tutorial-integrate-dropbox-for-business-with-azure-active-directory"></a>Öğretici: Dropbox iş Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iş için Dropbox tümleştirme öğreneceksiniz. Azure AD ile iş için Dropbox'ı tümleştirdiğinizde, şunları yapabilirsiniz:

* İş Dropbox erişimi, Azure AD'de denetler.
* Otomatik olarak iş için Dropbox kendi Azure AD hesapları ile oturum olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, bir aylık ücretsiz deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).
* Dropbox iş çoklu oturum açma (SSO) için abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

* Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. İş desteklediği için Dropbox **SP** tarafından başlatılan

* İş desteklediği için Dropbox **zamanında** kullanıcı sağlama

## <a name="adding-dropbox-for-business-from-the-gallery"></a>İş için Dropbox galeri ekleme

Azure AD'de iş için Dropbox tümleştirmesini yapılandırmak için iş için Dropbox Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **iş için Dropbox** arama kutusuna.
1. Seçin **iş için Dropbox** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO adlı bir test kullanıcı kullanarak iş için Dropbox'ile test etme **Britta Simon**. Çalışmak SSO için dropbox iş için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile iş için Dropbox sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[İş SSO için Dropbox'ı yapılandırma](#configure-dropbox-for-business-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Dropbox için iş test kullanıcısı oluşturma](#create-dropbox-for-business-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı iş için dropbox'ta bir karşılığı Britta simon'un sağlamak için.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **iş için Dropbox** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.dropbox.com/sso/<id>`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir değer yazın: `Dropbox`

    > [!NOTE]
    > Önceki oturum açma URL değeri, gerçek değeri değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek oturum açma URL'si ile değerini güncelleştirir.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Üzerinde **iş için Dropbox ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-dropbox-for-business-sso"></a>İş SSO için Dropbox'ı yapılandırma

1. İş için Dropbox içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![Uygulamaları uzantım](common/install-myappssecure-extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayın **iş için Kurulum Dropbox** dropbox'a iş uygulaması için yönlendirir. Burada, iş için Dropbox oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-8 adımları otomatik hale getirin.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. İş için Dropbox'ı el ile Kurulum istiyorsanız, yeni bir web tarayıcı penceresi açın ve iş Kiracı için Dropbox gidin ve iş Kiracı için Dropbox oturum açma. ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/ic769509.png "çoklu oturum açmayı yapılandırın")

4. Tıklayarak **kullanıcı simgesi** seçip **ayarları** sekmesi.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure1.png "çoklu oturum açmayı yapılandırın")

5. Sol taraftaki gezinti bölmesinde **Yönetici Konsolu**.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure2.png "çoklu oturum açmayı yapılandırın")

6. Üzerinde **Yönetici Konsolu**, tıklayın **ayarları** sol gezinti bölmesinde.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure3.png "çoklu oturum açmayı yapılandırın")

7. Seçin **çoklu oturum açma** altındaki **kimlik doğrulaması** bölümü.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure4.png "çoklu oturum açmayı yapılandırın")

8. İçinde **çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure5.png "çoklu oturum açmayı yapılandırın")

    a. Seçin **gerekli** için açılan listeden bir seçenek olarak **çoklu oturum açma**.

    b. Tıklayarak **oturum açma URL'si ekleme** ve **oturum açma kimlik sağlayıcısı URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portal ve ardından kopyaladığınızdeğeri **Bitti**.

    ![Çoklu oturum açmayı yapılandırma](./media/dropboxforbusiness-tutorial/configure6.png "çoklu oturum açmayı yapılandırın")

    c. Tıklayın **sertifikayı karşıya yükle**ve ardından göz atın, **Base64 olarak kodlanmış sertifika dosyası** Azure portalından indirilen.

    d. Tıklayarak **bağlantıyı Kopyala** kopyalanan değerine yapıştırın **oturum açma URL'si** textbox'ın **iş etki alanı ve URL'ler için Dropbox** bölümü Azure portalı.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı Britta Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `BrittaSimon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, iş için dropbox'a erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **iş için Dropbox**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-dropbox-for-business-test-user"></a>Dropbox için iş test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı, iş için dropbox'ta oluşturulur. İş için Dropbox just-ın-time kullanıcı hazırlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. İş için dropbox'ta bir kullanıcı zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact)

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde iş kutucuk için Dropbox'ı seçtiğinizde, otomatik olarak dropbox'a SSO'yu ayarlama iş için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)