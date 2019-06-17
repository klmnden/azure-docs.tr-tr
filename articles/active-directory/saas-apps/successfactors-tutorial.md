---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile SuccessFactors | Microsoft Docs'
description: Azure Active Directory ve SuccessFactors arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/3/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9526cde92fe2f504207de188fa8f0567ffa580d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089819"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Öğretici: SuccessFactors ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SuccessFactors tümleştirme konusunda bilgi edinin.
Azure AD ile SuccessFactors tümleştirme ile aşağıdaki avantajları sağlar:

* SuccessFactors erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) SuccessFactors için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SuccessFactors yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SuccessFactors tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SuccessFactors destekler **SP** tarafından başlatılan

## <a name="adding-successfactors-from-the-gallery"></a>Galeriden SuccessFactors ekleme

Azure AD'de SuccessFactors tümleştirmesini yapılandırmak için SuccessFactors Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SuccessFactors eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SuccessFactors**seçin **SuccessFactors** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde SuccessFactors](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SuccessFactors adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının SuccessFactors ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SuccessFactors ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SuccessFactors çoklu oturum açmayı yapılandırma](#configure-successfactors-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SuccessFactors test kullanıcısı oluşturma](#create-successfactors-test-user)**  - kullanıcı Azure AD gösterimini bağlı SuccessFactors Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile SuccessFactors yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SuccessFactors** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SuccessFactors etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SuccessFactors istemci Destek ekibine](https://www.successfactors.com/content/ssf-site/en/support.html) bu değerleri almak için.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **SuccessFactors kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-successfactors-single-sign-on"></a>SuccessFactors tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **SuccessFactors Yönetici portalı** yönetici olarak.

2. Ziyaret **uygulama güvenliği** ve yerel **tek oturum açma özelliğini**.

3. Herhangi bir değer yerleştirin **sıfırlama belirteci** tıklatıp **Kaydet belirteci** SAML SSO etkinleştirme.

    ![Çoklu oturum açma uygulama tarafında yapılandırma][11]

    > [!NOTE]
    > Bu değer, açma/kapatma düğmesi kullanılır. Herhangi bir değer kaydedilirse, SAML SSO açık'tır. SAML SSO OFF ise boş bir değer kaydedilir.

4. Yerel ekran görüntüsü aşağıda ve aşağıdaki eylemleri gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma][12]
  
    a. Seçin **SAML v2 SSO** radyo düğmesi
  
    b. Ayarlama **SAML sunduğundan taraf adı**(örneğin, SAML veren + şirket adı).

    c. İçinde **veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    d. Seçin **onaylama** olarak **zorunlu imzası iste**.

    e. Seçin **etkin** olarak **etkinleştirme SAML bayrağı**.

    f. Seçin **Hayır** olarak **oturum açma isteği imza (SF oluşturulan/SP/RP)** .

    g. Seçin **tarayıcı/Post profili** olarak **SAML profili**.

    h. Seçin **Hayır** olarak **sertifika geçerli süresi zorunlu**.

    i. Azure Portalı'ndan indirilen sertifika dosyasının içeriğini kopyalayın ve ardından yapıştırın **SAML sertifikası doğrulama** metin.

    > [!NOTE] 
    > Sertifika içeriği sahip başlamalıdır sertifika ve bitiş sertifika etiketleri.

5. SAML V2'ye gidin ve ardından aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma][13]

    a. Seçin **Evet** olarak **destek SP tarafından başlatılan genel oturum kapatma**.

    b. İçinde **genel oturum kapatma hizmeti URL'si (LogoutRequest hedef)** metin kutusu, yapıştırma **oturum kapatma URL'si** kopyaladığınız değeri form Azure portalı.

    c. Seçin **Hayır** olarak **sp gereken tüm Nameıd öğesi şifreleme gerektiren**.

    d. Seçin **belirtilmeyen** olarak **Nameıd biçimi**.

    e. Seçin **Evet** olarak **etkinleştir sp tarafından başlatılan oturum açma (AuthnRequest)** .

    f. İçinde **şirket çapında veren olarak gönderme isteği** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

6. Oturum açma kullanıcı adlarını hale getirmek isterseniz bu adımları uygulamadan büyük küçük harfe duyarlı.

    ![Çoklu oturum açmayı yapılandırın][29]

    a. Ziyaret **şirket ayarları**(ekranın altındaki).

    b. yanında onay kutusunu işaretleyin **etkinleştirme durumu hassas olmayan kullanıcı adı**.

    c. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Bunu etkinleştirmek çalışırsanız, yinelenen bir SAML oturum açma adı oluşturur sistem denetler. Örneğin, müşteri Kullanıcı1 hem Kullanıcı1 kullanıcı adları varsa. Büyük/küçük harfe duyarlılık hemen almak, bu yinelemeler yapar. Sistem, bir hata iletisi verir ve özellik etkinleştirmez. Müşteri, farklı yazıldığından kullanıcı adlarını birini değiştirmek gerekiyor.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için SuccessFactors erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SuccessFactors**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **SuccessFactors**.

    ![Uygulamalar listesinde SuccessFactors bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-successfactors-test-user"></a>SuccessFactors test kullanıcısı oluşturma

SuccessFactors için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların SuccessFactors sağlanması gerekir. SuccessFactors söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

İletişime geçmeniz SuccessFactors içinde oluşturulan kullanıcıları almak için [SuccessFactors Destek ekibine](https://www.successfactors.com/content/ssf-site/en/support.html).

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SuccessFactors kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama SuccessFactors için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png
