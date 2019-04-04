---
title: 'Öğretici: Jamf Pro ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Jamf Pro ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18b575b74c80499f2ddd6648bf051b5245077d2f
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58906149"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Öğretici: Jamf Pro ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jamf Pro tümleştirme konusunda bilgi edinin.
Jamf Pro Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Jamf Pro erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Jamf Pro için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Jamf Pro ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Jamf Pro çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Jamf Pro destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro galeri ekleme

Azure AD'de Jamf Pro tümleştirmesini yapılandırmak için Jamf Pro Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Jamf Pro Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Jamf Pro**seçin **Jamf Pro** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Jamf Pro'da sonuçları listesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Jamf Pro ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Jamf Pro'da arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Jamf Pro ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Jamf Pro çoklu oturum açmayı yapılandırma](#configure-jamf-pro-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Jamf Pro test kullanıcısı oluşturma](#create-jamf-pro-test-user)**  - Jamf Pro'da kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Jamf Pro ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Jamf Pro** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** yapılandırmak istiyorsanız, bölüm **IDP** intiated modu, aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.jamfcloud.com/saml/SSO`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<subdomain>.jamfcloud.com`

    ![Jamf Pro etki alanı ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Gerçek tanımlayıcı değerini alırsınız **çoklu oturum açma** bölümünde Jamf Pro portal, öğreticinin ilerleyen bölümlerinde açıklanmıştır. Gerçek ayıklayabilir **alt etki alanı** değer tanımlayıcısı değeri ve kullanan **alt etki alanı** bilgileri oturum açma URL'si ve yanıt URL'si. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-jamf-pro-single-sign-on"></a>Jamf Pro çoklu oturum açmayı yapılandırın

1. Jamf Pro içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)

2. Uzantı tarayıcıya ekledikten sonra tıklayarak **Jamf Pro Kurulum** Jamf Pro uygulamaya yönlendirir. Burada, Jamf Pro ile oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve 3-7 arası adımlar otomatik hale getirin.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

3. Jamf Pro el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve günlük Jamf Pro şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Tıklayarak **tekli oturum**.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Üzerinde **çoklu oturum açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro tek](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Seçin **Jamf Pro sunucu** çoklu oturum açma erişimi sağlamak için.

    b. Seçerek **tüm kullanıcılar için izin atlama** kullanıcıların kimlik doğrulaması için kimlik sağlayıcısı oturum açma sayfasına yönlendirilmeyecek ancak Jamf Pro doğrudan bunun yerine oturum açabilirsiniz. Bir kullanıcı, kimlik sağlayıcısı ile Jamf Pro erişmeye çalıştığında, IDP tarafından başlatılan SSO'yu kimlik doğrulaması ve yetkilendirme gerçekleşir.

    c. Seçin **Nameıd** seçeneğini **kullanıcı eşleme: SAML**. Varsayılan olarak, bu ayar **Nameıd** ancak özel bir öznitelik tanımlayın.

    d. Seçin **e-posta** için **kullanıcı eşlemesi: JAMF PRO**. Jamf Pro aşağıdaki yollarla IDP tarafından gönderilen SAML öznitelikleri eşler: kullanıcılar ve gruplar. Bir kullanıcının erişmeye çalıştığında Jamf Pro, varsayılan Jamf Pro tarafından kimlik sağlayıcısından kullanıcıyla ilgili bilgileri alır ve Jamf Pro kullanıcı hesaplarına yönelik eşleşir. Jamf Pro'da gelen kullanıcı hesabı yoksa, grup adıyla eşleşen gerçekleşir.

    e. Değer yapıştırın `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` içinde **grubu ÖZNİTELİK adı** metin.

7. Aynı sayfayı kaydırın üzerinde **kimlik SAĞLAYICISI** altında **çoklu oturum açma** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Seçin **diğer** bir seçenek olarak **kimlik SAĞLAYICISI** açılır.

    b. İçinde **diğer SAĞLAYICISI** metin girin **Azure AD'ye**.

    c. Seçin **meta veri URL'si** bir seçenek olarak **kimlik SAĞLAYICISI meta veri kaynağı** açılır ve aşağıdaki metin kutusuna yapıştırın **uygulama Federasyon meta verileri URL'sini** değeri hangi Azure portaldan kopyaladığınız.

    d. Kopyalama **varlık kimliği** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** metin kutusunda **Jamf Pro etki alanı ve URL'ler** bölümü Azure portalı.

    > [!NOTE]
    > Burada bulanık değeri alt etki alanı parçasıdır. Oturum açma URL'si ve yanıt URL'si olarak tamamlamak için bu değeri kullanın **Jamf Pro etki alanı ve URL'ler** bölümü Azure portalı.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Jamf Pro için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Jamf Pro**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Jamf Pro**.

    ![Uygulamalar listesinde Jamf Pro bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-jamf-pro-test-user"></a>Jamf Pro test kullanıcısı oluşturma

Jamf Pro oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jamf Pro ile sağlanması gerekir. Jamf Pro söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jamf Pro şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Tıklayarak **Jamf Pro kullanıcı hesaplarını ve grupları**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user1.png)

4. **Yeni**’ye tıklayın.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user2.png)

5. Seçin **standart hesabı oluşturma**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user3.png)

6. Üzerinde **yeni hesabı** dailog, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user4.png)

    a. İçinde **kullanıcıadı** metin BrittaSimon tam adını yazın.

    b. Kuruluşunuz için göre uygun seçenekleri belirleyin **erişim düzeyi**, **ayrıcalık**ve **erişim durumu**.

    c. İçinde **tam adı** metin Britta Simon tam adını yazın.

    d. İçinde **e-posta adresi** metin Britta Simon hesabı e-posta adresini yazın.

    e. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    f. İçinde **PAROLAYI doğrula** metin kutusu, kullanıcı parolasını yazın.

    g. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jamf Pro kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Jamf Pro için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
