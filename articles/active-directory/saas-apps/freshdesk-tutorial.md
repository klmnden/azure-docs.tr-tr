---
title: 'Öğretici: Azure Active Directory FreshDesk ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve FreshDesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010822"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: Azure Active Directory FreshDesk ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshDesk tümleştirme konusunda bilgi edinin.

FreshDesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FreshDesk erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Freshdesk'e (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi FreshDesk ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik FreshDesk çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FreshDesk ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden FreshDesk ekleme

Azure AD'de FreshDesk tümleştirmesini yapılandırmak için FreshDesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden FreshDesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **FreshDesk**seçin **FreshDesk** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde FreshDesk](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshDesk ile test edin.

Tek iş için oturum açma için Azure AD ne FreshDesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının FreshDesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma FreshDesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FreshDesk test kullanıcısı oluşturma](#creating-a-freshdesk-test-user)**  - kullanıcı Azure AD gösterimini bağlı FreshDesk Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve FreshDesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma FreshDesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **FreshDesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![FreshDesk etki alanı ve URL'ler tek oturum açma bilgileri](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.freshdesk.com` veya başka bir değer Freshdesk önerdi.

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.freshdesk.com` veya başka bir değer Freshdesk önerdi.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [FreshDesk istemci Destek ekibine](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) bu değerleri almak için.

5. FreshDesk uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak **FreshDesk** bu kullanıcının e-posta adresi ile eşlenmesini bekliyor. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın.

    ![image](./media/freshdesk-tutorial/i4-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    a. Tıklayarak **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/freshdesk-tutorial/i2-attribute.png)

    ![image](./media/freshdesk-tutorial/i3-attribute.png)

    b. Gelen **kaynak özniteliği** listesinden **user.mail**.

    c. **Kaydet**’e tıklayın.

7. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. Açık **komut istemi** ve aşağıdaki komutları çalıştırın:

    a. Girin `certutil.exe -dump FreshDesk.cer` değeri komut isteminde.

    > [!NOTE]
    > Burada **FreshDesk.cer** Azure portalından indirilen olan sertifikadır.

    b. Kopyalama **Cert Hash(sha256)** değeri ve Not Defteri'ne yapıştırın. 

9. Üzerinde **FreshDesk kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![FreshDesk yapılandırma](common/configuresection.png)

10. Farklı bir web tarayıcı penceresinde Freshdesk şirket sitenize yönetici olarak oturum.

11. Seçin **ayarlar simgesine** ve **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum](./media/freshdesk-tutorial/IC776770.png "tekli oturum")
  
    a. İçin **çoklu oturum açma (SSO)** seçin **üzerinde**.

    b. Seçin **SAML SSO**.

    c. İçinde **SAML oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    d. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    e. İçinde **güvenlik sertifikası parmak izi** metin kutusu, yapıştırma **Cert Hash(sha256)** daha önce aldığınız değeri.
  
    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-a-freshdesk-test-user"></a>FreshDesk test kullanıcısı oluşturma

FreshDesk açarken Azure AD kullanıcılarının etkinleştirmek için bunların FreshDesk sağlanması gerekir.  
FreshDesk söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Freshdesk** Kiracı.

2. Üstteki menüden **yönetici**.

    ![Yönetici](./media/freshdesk-tutorial/IC776772.png "yönetici")

3. İçinde **genel ayarlar** sekmesinde **aracıları**.
  
    ![Aracıları](./media/freshdesk-tutorial/IC776773.png "aracıları")

4. Tıklayın **yeni aracı**.

    ![Yeni aracı](./media/freshdesk-tutorial/IC776774.png "yeni aracı")

5. Aracı bilgilerini iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Aracı bilgilerini](./media/freshdesk-tutorial/IC776775.png "aracısı bilgileri")

    a. İçinde **e-posta** metin sağlamak istediğiniz Azure AD hesabını Azure AD türü e-posta adresi.

    b. İçinde **tam adı** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

    c. İçinde **başlık** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

    d. **Kaydet**’e tıklayın.

    >[!NOTE]
    >Azure AD hesap sahibinin etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    >
    >[!NOTE]
    >Herhangi diğer Freshdesk kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için FreshDesk AAD kullanıcı hesapları sağlamak üzere Freshdesk tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Freshdesk'e erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **FreshDesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde FreshDesk kutucuğa tıkladığınızda, otomatik olarak FreshDesk uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
