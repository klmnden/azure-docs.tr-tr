---
title: 'Öğretici: DocuSign ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fda9df8e7781a9e0c45fb1aead9f8167f89a833
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850879"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Öğretici: DocuSign ile Azure Active Directory Tümleştirme

Bu öğreticide, DocuSign, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

DocuSign'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- DocuSign erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan DocuSign'ın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

DocuSign ve Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir DocuSign çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. DocuSign galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-docusign-from-the-gallery"></a>DocuSign galeri ekleme

Azure AD'ye DocuSign tümleştirmesini yapılandırmak için DocuSign galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden DocuSign eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **DocuSign**seçin **DocuSign** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde DocuSign](./media/docusign-tutorial/tutorial_docusign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı DocuSign sınayın.

Tek çalışmak için oturum açma için Azure AD ne DocuSign karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının DocuSign ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma DocuSign ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[DocuSign test kullanıcısı oluşturma](#creating-a-docusign-test-user)**  - kullanıcı Azure AD gösterimini bağlı DocuSign Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve DocuSign uygulamanızda çoklu oturum açmayı yapılandırın.

**DocuSign ve Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **DocuSign** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![DocuSign etki alanı ve URL'ler tek oturum açma bilgileri](./media/docusign-tutorial/tutorial_docusign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve daha sonra açıklanan tanımlayıcısı güncelleştirme **uç noktalarını görüntüle SAML 2.0** öğretici bölümünde.

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/docusign-tutorial/tutorial_docusign_certificate.png) 

6. Üzerinde **DocuSign ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![DocuSign yapılandırma](common/configuresection.png)

7. Bir oturum açma için farklı bir web tarayıcı penceresinde, **DocuSign Yönetici portalı** yönetici olarak.

8. Üst sayfanın sağ tıklayın, profili **logosu** ve ardından **Admin Git**.
  
    ![Çoklu oturum açma yapılandırılıyor][51]

9. Etki alanı çözümleri sayfanızda tıklayarak **etki alanları**

    ![Çoklu oturum açma yapılandırılıyor][50]

10. Altında **etki alanları** bölümünde **talep etki alanı**.

    ![Çoklu oturum açma yapılandırılıyor][52]

11. Üzerinde **bir etki alanı talep** iletişim, **etki alanı adı** metin şirket etki alanınızı girin ve ardından **talep**. Etki alanını doğrulayın ve durumun etkin olduğundan emin olun.

    ![Çoklu oturum açma yapılandırılıyor][53]

12. Etki alanı çözümleri sayfanızda tıklayın **kimlik sağlayıcıları**.
  
    ![Çoklu oturum açma yapılandırılıyor][54]

13. Altında **kimlik sağlayıcıları** bölümünde **kimlik SAĞLAYICISI Ekle**. 

    ![Çoklu oturum açma yapılandırılıyor][55]

14. Üzerinde **kimlik sağlayıcı ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırılıyor][56]

    a. İçinde **adı** metin yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. İçinde **kimlik sağlayıcısını veren textbox**, değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. Seçin **oturum AuthN isteği**.

    f. Olarak **AuthN gönderme isteği tarafından**seçin **POST**.

    g. Olarak **gönderme oturum kapatma isteği tarafından**seçin **alma**.

    h. İçinde **özel öznitelik eşlemesi** bölümünde, tıklayarak **yeni eşleme Ekle**.

    ![Çoklu oturum açma yapılandırılıyor][62]

    i. Azure AD talep ile eşlemek istediğiniz alanı seçin. Bu örnekte, **emailaddress** talep değeriyle eşleşen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Azure ad için e-posta talebi ve ardından varsayılan talep adı olan **Kaydet**.

    ![Çoklu oturum açma yapılandırılıyor][57]

    > [!NOTE]
    > Uygun kullanın **kullanıcı tanımlayıcısı** kullanıcının Azure AD'den DocuSign kullanıcı eşleme eşlemek için. Uygun alanı seçmek ve kuruluş ayarlarınıza göre uygun değeri girin.

    j. İçinde **kimlik sağlayıcısı sertifikaları** bölümünde **sertifika Ekle**ve ardından Azure AD Portalı'ndan yüklemiş ve tıklayın sertifikasını yükleme **Kaydet**.

    ![Çoklu oturum açma yapılandırılıyor][58]

    k. İçinde **kimlik sağlayıcıları** bölümünde **eylemleri**ve ardından **uç noktaları**.

    ![Çoklu oturum açma yapılandırılıyor][59]

    m. İçinde **uç noktalarını görüntüle SAML 2.0** bölümünde **DocuSign Yönetici portalı**, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma yapılandırılıyor][60]

    * Kopyalama **hizmet sağlayıcısı veren URL'si**, ardından yapıştırın **tanımlayıcı** metin kutusunda **DocuSign etki alanı ve URL'ler** bölümünde Azure portalında.

    * Kopyalama **hizmet sağlayıcısı oturum açma URL'si**, ardından yapıştırın **işareti bulunan URL'si** metin kutusunda **DocuSign etki alanı ve URL'ler** bölümünde Azure portalında.

    * Tıklayın **Kapat**

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-a-docusign-test-user"></a>DocuSign test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Docusign'da adlı bir kullanıcı oluşturmaktır. DocuSign tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse, DocuSign erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [DocuSign Destek ekibine](https://support.docusign.com/).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için DocuSign erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **DocuSign**.

    ![Çoklu oturum açmayı yapılandırın](./media/docusign-tutorial/tutorial_docusign_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde DocuSign kutucuğa tıkladığınızda, otomatik olarak DocuSign uygulamanıza açan.
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
[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
