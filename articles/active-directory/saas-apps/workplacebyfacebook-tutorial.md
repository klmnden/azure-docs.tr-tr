---
title: 'Öğretici: Facebook ile çalışma alanına Azure Active Directory tümleştirmesiyle | Microsoft Docs'
description: Azure Active Directory ve Facebook ile iş yeri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27173c8beeecf2be43e80f59df8907952734c06b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65905259"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Öğretici: Facebook ile çalışma alanı ile Azure Active Directory Tümleştirme

Bu öğreticide, Facebook ile çalışma alanına Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Facebook ile çalışma alanına Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Facebook ile çalışma alanına erişimi olan Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak çalışma alanına (çoklu oturum açma) tarafından Facebook kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iş yeri tarafından Facebook ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Facebook çoklu oturum açma ile Workplace abonelik etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook iki ürün, iş yeri standart (ücretsiz) ve çalışma alanı Premium (Ücretli) sahiptir. Herhangi bir çalışma alanı Premium Kiracı SCIM ve SSO tümleştirme hiçbir diğer uygulamaları maliyet veya gerekli lisanslar ile yapılandırabilirsiniz. SSO ve SCIM standart çalışma alanına durumlarda kullanılamaz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Facebook ile çalışma alanına destekler **SP** tarafından başlatılan
* Facebook ile çalışma alanına destekler **tam zamanında sağlama**
* Facebook ile çalışma alanına destekler  **[otomatik kullanıcı hazırlama](workplacebyfacebook-provisioning-tutorial.md)**

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Galeriden Facebook ile çalışma alanına ekleme

Azure AD çalışma alanına Facebook ile tümleştirilmesi yapılandırmak için çalışma alanına Facebook tarafından Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Facebook ile çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Facebook ile çalışma alanına**seçin **Facebook ile çalışma alanına** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Facebook ile çalışma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Workplace Facebook adlı bir test kullanıcı tabanlı olarak test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve iş yeri Facebook ile ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile çalışma alanına Facebook ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Çalışma alanı tarafından Facebook çoklu oturum açmayı yapılandırma](#configure-workplace-by-facebook-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Facebook test kullanıcısı tarafından çalışma alanı oluşturma](#create-workplace-by-facebook-test-user)**  - çalışma alanına kullanıcı Azure AD gösterimini bağlı Facebook tarafından Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Facebook ile çalışma alanı ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Facebook ile çalışma alanına** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Facebook etki alanı ve URL'ler tek oturum açma bilgileri ile çalışma](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<instancename>.facebook.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kimlik doğrulaması sayfası doğru değerlerin çalışma alanı şirket Pano, çalışma alanına topluluk için bakın.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Facebook tarafından çalışma alanı ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-workplace-by-facebook-single-sign-on"></a>Facebook çoklu oturum açma ile Workplace yapılandırın

1. Bir başka web tarayıcı penceresinde çalışma alanınızı Facebook şirket site yönetici olarak oturum açın.
  
    > [!NOTE]
    > SAML kimlik doğrulaması işleminin bir parçası olarak, çalışma alanı, parametreleri Azure AD'ye geçirmek için sorgu dizeleri en fazla boyutu 2,5 kilobayt değerlendirebilir.

2. İçinde **yönetim paneli**Git **güvenlik** sekmesi.

    ![Yönetim paneli](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure01.png)

3. Altında **kimlik doğrulaması** sekmesinde **çoklu oturum açma (SSO)** ve aşağıdaki adımları gerçekleştirin:

    ![Kimlik doğrulama sekmesi](./media/workplacebyfacebook-tutorial/tutorial-workplace-by-facebook-configure02.png)

    a. İçinde **SAML URL** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **SAML veren URI textbox**, değerini yapıştırın **Azure Ad tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **oturum kapatma SAML yönlendirme** (isteğe bağlı) değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. Açık, **base-64 kodlamalı sertifika** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML sertifikası** metin.

    e. Kopyalama **İzleyici URL** örneğinizin yapıştırın **tanımlayıcı (varlık kimliği)** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    f. Kopyalama **alıcı URL** örneğinizin yapıştırın **oturum açma URL'si** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalı.

    g. Bölümünün altına ilerleyecek ve **Test SSO** düğmesi. Bu sonuçlar ile Azure AD oturum açma sayfasının görünen açılan pencerede gösterilir. Kimlik bilgilerinizi doğrulamak için normal olarak girin.

    **Sorun giderme:** Azure AD'den geri döndürülen e-posta adresi, oturum açmış olduğunuz çalışma alanı hesabı ile aynı olduğundan emin olun.

    h. Test başarıyla tamamlandıktan sonra sayfanın sonuna kaydırın ve tıklayın **Kaydet** düğmesi.

    i. Çalışma alanı kullanan tüm kullanıcıları Azure AD oturum açma sayfasında kimlik doğrulaması için şimdi sunulur.

4. **SAML oturum kapatma (isteğe bağlı) yönlendirme** -

    İsteğe bağlı olarak Azure AD'nin oturum kapatma sayfasının işaret etmek için kullanılabilecek bir SAML oturum kapatma URL'si yapılandırmayı seçebilirsiniz. Bu ayar etkin ve yapılandırılmış olduğunda, kullanıcı artık çalışma alanına oturum kapatma sayfasına yönlendirilirsiniz. Bunun yerine, kullanıcı, oturum kapatma SAML yeniden yönlendirme ayarı eklendi URL'ye yönlendirilirsiniz.

### <a name="configuring-reauthentication-frequency"></a>Yeniden kimlik doğrulaması sıklığını yapılandırma

Her gün, üç gün, haftalık, iki hafta, ay için SAML onay istemi için çalışma alanına yapılandırabilirsiniz ya da asla.

> [!NOTE]
> SAML onay mobil uygulamaları için en düşük değer, bir hafta için ayarlanır.

Ayrıca, tüm kullanıcılar için düğmeyi kullanarak sıfırlama SAML zorlayabilirsiniz: SAML kimlik doğrulaması, tüm kullanıcılar için artık gerektirir.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Facebook ile çalışma alanına erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Facebook ile çalışma alanına**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Facebook ile çalışma alanına**.

    ![Facebook bağlantı uygulamalar listesinde çalışma](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-workplace-by-facebook-test-user"></a>Facebook test kullanıcısı tarafından çalışma alanı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı tarafından Facebook çalışma alanında oluşturulur. Facebook ile çalışma alanına tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde hiçbir şey yoktur. Facebook ile çalışma alanında bir kullanıcı yoksa, yeni bir çalışma alanına erişmeye çalıştığında Facebook tarafından oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [Facebook istemcisini destek ekibi ile çalışma](https://workplace.fb.com/faq/)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çalışma alanı tarafından erişim panelinde Facebook kutucuğa tıkladığınızda, size otomatik olarak çalışma alanına SSO'yu ayarlama Facebook ile oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](workplacebyfacebook-provisioning-tutorial.md)
