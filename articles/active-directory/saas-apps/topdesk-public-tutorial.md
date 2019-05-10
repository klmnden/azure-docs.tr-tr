---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle TOPdesk - Public | Microsoft Docs'
description: Azure Active Directory ve TOPdesk - genel arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: d5ecfcd249dd07dc94b3b17ea0a7a7de3559c681
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407946"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Öğretici: Azure Active Directory Tümleştirmesi ile TOPdesk - genel

Bu öğreticide, Azure Active Directory (Azure AD) ile genel TOPdesk - tümleştirme konusunda bilgi edinin.
TOPdesk - genel Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* TOPdesk - genel erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak TOPdesk - Azure AD hesaplarına (çoklu oturum açma) genel oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için genel, aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* TOPdesk - aboneliği etkin ortak çoklu oturum açma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Ortak TOPdesk - destekleyen **SP** tarafından başlatılan

## <a name="adding-topdesk---public-from-the-gallery"></a>Genel Galeriden TOPdesk - ekleme

TOPdesk - genel Azure AD'ye tümleştirmesini yapılandırmak için genel Galeriden listenize yönetilen SaaS uygulamaları - TOPdesk eklemeniz gerekir.

**TOPdesk - genel Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **TOPdesk - genel**seçin **TOPdesk - genel** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![TOPdesk - genel sonuçları listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - ortak adlı bir test kullanıcı tabanlı **Britta Simon**.
İş, bir Azure AD kullanıcısının TOPdesk - ilgili kullanıcı arasında bir bağlantı ilişki için çoklu oturum açma için ortak kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için genel, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Genel çoklu oturum açma TOPdesk - yapılandırma](#configure-topdesk---public-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[TOPdesk - genel bir test kullanıcısı oluşturma](#create-topdesk---public-test-user)**  - TOPdesk - kullanıcı Azure AD gösterimini bağlı olduğu ortak bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma - TOPdesk ile yapılandırmak için genel, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **TOPdesk - genel** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4.  Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Erişmenizi sağlayacak **hizmet sağlayıcısı meta veri dosyası** gelen **- genel çoklu oturum açmayı yapılandırma TOPdesk** bölümünde öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.
    
    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik temel SAML yapılandırma bölümünde doldurulur.

    ![Çoklu oturum açma bilgileri TOPdesk - ortak etki alanı ve URL'ler](common/sp-identifier-reply.png)

    d. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.topdesk.net`

    e. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net/tas/public/login/verify`

    > [!NOTE] 
    > Varsa **tanımlayıcı** ve **yanıt URL'si** değerleri alınamadı otomatik doldurulur, bunları el ile girmeniz gerekir. Tanımlayıcı, yukarıda belirtildiği gibi desenini izler ve yanıt URL'si değerini alma **- genel çoklu oturum açmayı yapılandırma TOPdesk** öğreticinin ilerleyen bölümlerinde açıklanan bölümü. **Oturum açma URL'si** değeri değil gerçek, değerin gerçek oturum açma URL'si ile güncelleştirilmesi gerekmez. İlgili kişi [TOPdesk - genel istemci Destek ekibine](https://help.topdesk.com/saas/enterprise/user/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **TOPdesk - genel ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-topdesk---public-single-sign-on"></a>TOPdesk - genel çoklu oturum açmayı yapılandırın

1. Oturum açın, **TOPdesk - genel** yönetici olarak şirketin site.

2. İçinde **TOPdesk** menüsünü tıklatın **ayarları**.
   
    ![Ayarları](./media/topdesk-public-tutorial/ic790598.png "ayarları")

3. Tıklayın **oturum açma ayarları**.
   
    ![Oturum açma ayarları](./media/topdesk-public-tutorial/ic790599.png "oturum açma ayarları")

4. Genişletin **oturum açma ayarları** menüsüne ve ardından **genel**.
   
    ![Genel](./media/topdesk-public-tutorial/ic790600.png "genel")

5. İçinde **genel** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Teknik ayarlarla](./media/topdesk-public-tutorial/ic790601.png "teknik ayarları")
   
    a. Tıklayın **indirme** ortak meta veri dosyası indirin ve bilgisayarınıza yerel olarak kaydedin.
   
    b. İndirilen meta veri dosyası açın ve ardından bulun **AssertionConsumerService** düğümü.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopyalama **AssertionConsumerService** değeri, bu değeri yapıştırın **yanıt URL'si** metin kutusunda **temel SAML yapılandırma** bölümü.      
   
6. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/topdesk-public-tutorial/ic790606.png "sertifika")
    
    a. Azure Portalı'ndan indirilen meta veri dosyası açın.
    
    b. Genişletin **verilerde Securitytokenservicetype** sahip düğüm bir **xsi: type** , **beslenir: ApplicationServiceType**.
    
    c. Değerini kopyalayın **X509Certificate** düğümü.
    
    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyadaki değeri.

7. İçinde **genel** bölümünde **Ekle**.
    
    ![SAML oturum açma](./media/topdesk-public-tutorial/ic790625.png "SAML oturum açma")

8. Üzerinde **SAML yapılandırma Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML yapılandırma Yardımcısı](./media/topdesk-public-tutorial/ic790608.png "SAML yapılandırma Yardımcısı")
    
    a. Azure portalından indirilen meta verileri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklayın **Gözat**.

    b. Altında sertifika dosyası karşıya **sertifika (RSA)**, tıklayın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **logosu simgesi**, tıklayın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. İçinde **görünen adı** metin yapılandırmanız için bir ad yazın.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma TOPdesk - genel erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **TOPdesk - genel**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **TOPdesk - genel**.

    ![Genel bağlantı TOPdesk - uygulamalar listesinde](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-topdesk---public-test-user"></a>TOPdesk - genel bir test kullanıcısı oluşturma

Azure AD TOPdesk - oturum açmasına olanak tanımak ortak, bunlar sağlanmalıdır TOPdesk - genel. TOPdesk - söz konusu olduğunda genel, el ile bir görev olduğundan sağlama.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **TOPdesk - genel** şirketinizin sitesi yöneticisi olarak.

2. Üstteki menüden **TOPdesk \> yeni \> destek dosyalarını \> kişi**.
   
    ![Kişi](./media/topdesk-public-tutorial/ic790628.png "kişi")

3. Yeni bir kişiye iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Yeni bir kişiye](./media/topdesk-public-tutorial/ic790629.png "yeni kişi")
   
    a. Genel sekmesini tıklatın.

    b. İçinde **Soyadı** metin Simon gibi kullanıcının soyadı yazın
 
    c. Seçin bir **Site** hesabı.
 
    d. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer TOPdesk - genel bir kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri TOPdesk - Azure AD kullanıcı hesapları sağlamak için ortak tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

TOPdesk tıklayın - genel erişim Paneli'nde döşeme sonra otomatik olarak TOPdesk - genel SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
