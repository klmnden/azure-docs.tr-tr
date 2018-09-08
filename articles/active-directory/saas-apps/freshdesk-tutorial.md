---
title: 'Öğretici: Azure Active Directory FreshDesk ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve FreshDesk arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2018
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: db4750e01b62835cf08fd52e3288e94aea539b26
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161331"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: Azure Active Directory FreshDesk ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshDesk tümleştirme konusunda bilgi edinin.

FreshDesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FreshDesk erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan Freshdesk'e (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi FreshDesk ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir FreshDesk çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FreshDesk ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden FreshDesk ekleme

Azure AD'de FreshDesk tümleştirmesini yapılandırmak için FreshDesk Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden FreshDesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FreshDesk**. Seçin **FreshDesk** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshDesk ile test edin.

Tek iş için oturum açma için Azure AD ne FreshDesk karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının FreshDesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** FreshDesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma FreshDesk ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FreshDesk test kullanıcısı oluşturma](#creating-a-freshdesk-test-user)**  - Azure AD gösterimini her için bağlı olan FreshDesk Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve FreshDesk uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma FreshDesk ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **FreshDesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Üzerinde **FreshDesk etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.freshdesk.com` veya başka bir değer Freshdesk önerdi.

    > [!NOTE]
    > Bu gerçek değer olmadığını unutmayın. Değerini gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. İlgili kişi [FreshDesk istemci Destek ekibine](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) bu değeri alınamıyor.  

4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

    > [!NOTE]
    > Herhangi bir sorun varsa, lütfen bakın [bağlantı](https://support.freshdesk.com/support/discussions/topics/317543).

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_general_400.png)

6. Yükleme **OpenSSL** sisteminizde, sisteminizde yüklü değilse.

7. Açık **komut istemi** ve aşağıdaki komutları çalıştırın:

    a. Girin `openssl x509 -inform DER -in FreshDesk.cer -out certificate.crt` değeri komut isteminde.

    > [!NOTE]
    > Burada **FreshDesk.cer** Azure portalından indirilen olan sertifikadır.

    b. Girin `openssl x509 -noout -fingerprint -sha256 -inform pem -in certificate.crt` değeri komut isteminde. Burada **certificate.crt** önceki adımda oluşturulan çıktı sertifikadır.

    c. Kopyalama **parmak izi** değeri ve Not Defteri'ne yapıştırın. İki nokta üst üste alınan parmak izini kaldırın ve son parmak izi değerini alın.

8. Üzerinde **FreshDesk yapılandırma** bölümünde **yapılandırma FreshDesk** yapılandırma oturum açma penceresini açın. SAML çoklu oturum açma hizmet URL'si ve oturum kapatma URL'si kopyalama **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

9. Farklı bir web tarayıcı penceresinde Freshdesk şirket sitenize yönetici olarak oturum.

10. Üstteki menüden **yönetici**.

    ![Yönetici](./media/freshdesk-tutorial/IC776768.png "yönetici")

11. İçinde **genel ayarlar** sekmesinde **güvenlik**.
  
    ![Güvenlik](./media/freshdesk-tutorial/IC776769.png "güvenlik")

12. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum](./media/freshdesk-tutorial/IC776770.png "tekli oturum")
  
    a. İçin **çoklu oturum açma (SSO)** seçin **üzerinde**.

    b. Seçin **SAML SSO**.

    c. İçinde **SAML oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    d. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    e. İçinde **güvenlik sertifikası parmak izi** metin kutusu, yapıştırma **parmak izi** iki nokta üst üste kaldırdıktan sonra daha önce almış olan değer.
  
    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

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

   a. İçinde **tam adı** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

   b. İçinde **e-posta** metin sağlamak istediğiniz Azure AD hesabını Azure AD türü e-posta adresi.

   c. İçinde **başlık** metin sağlamak istediğiniz Azure AD hesabı adını yazın.

   d. Seçin **aracıları rol**ve ardından **atama**.

   e. **Kaydet**’e tıklayın.

    >[!NOTE]
    >Azure AD hesap sahibinin etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
    >
    >[!NOTE]
    >Herhangi diğer Freshdesk kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Freshdesk tarafından sağlanan.
    Freshdesk'e.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kutusuna erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200]

**Britta Simon Freshdesk'e atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **FreshDesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FreshDesk kutucuğa tıkladığınızda, oturum açma sayfası açan FreshDesk uygulamanız için almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png