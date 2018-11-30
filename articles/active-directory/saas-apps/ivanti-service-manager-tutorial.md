---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Ivanti Service Manager (ISM) | Microsoft Docs'
description: Azure Active Directory ve Ivanti Service Manager'ı (ISM) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 14297c74-0d57-4146-97fa-7a055fb73057
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: jeedes
ms.openlocfilehash: 3b394ff8e3638a9663e756fd6db866b0c3e5d2ef
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450035"
---
# <a name="tutorial-azure-active-directory-integration-with-ivanti-service-manager-ism"></a>Öğretici: Azure Active Directory Tümleştirme ile Ivanti Service Manager (ISM)

Bu öğreticide, Azure Active Directory (Azure AD) ile Ivanti Service Manager'ı (ISM) tümleştirme konusunda bilgi edinin.

Ivanti Service Manager'ı (ISM) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Ivanti Service Manager (ISM) için Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Ivanti Service Manager (ISM için) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Ivanti Service Manager (ISM) yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Ivanti Service Manager'ı (ISM) çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Ivanti Service Manager'ı (ISM) ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ivanti-service-manager-ism-from-the-gallery"></a>Galeriden Ivanti Service Manager'ı (ISM) ekleme

Azure AD'de Ivanti Service Manager (ISM) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden Ivanti Service Manager'ı (ISM) eklemeniz gerekir.

**Galeriden Ivanti Service Manager'ı (ISM) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Ivanti Service Manager'ı (ISM)** seçin **Ivanti Service Manager'ı (ISM)** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Ivanti Service Manager (ISM)](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Ivanti Service Manager ("Britta Simon" adlı bir test kullanıcı tabanlı ISM) test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Ivanti Service Manager (ISM) bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Ivanti Service Manager (ISM) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Ivanti Service Manager (ISM) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ivanti Service Manager'ı (ISM) bir test kullanıcısı oluşturma](#creating-an-ivanti-service-manager-ism-test-user)**  - Ivanti Service Manager (ISM) kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Ivanti Service Manager'ı (ISM) uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Ivanti Service Manager (ISM) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Ivanti Service Manager'ı (ISM)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial-general-301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirmek **IDP** başlatılan modu:

    ![Ivanti Service Manager'ı (ISM) etki alanı ve URL'ler tek oturum açma bilgileri](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<customer>.saasit.com/` |
    | `https://<customer>.saasiteu.com/` |
    | `https://<customer>.saasitau.com/` |

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer>/handlers/sso/SamlAssertionConsumerHandler.ashx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Ivanti Service Manager'ı (ISM) etki alanı ve URL'ler tek oturum açma bilgileri](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer>.saasit.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Ivanti Service Manager'ı (ISM) istemci Destek ekibine](https://www.ivanti.com/support/contact) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-certificate.png) 

7. Üzerinde **Ivanti Service Manager (ISM) kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Ivanti Service Manager (ISM) yapılandırma](common/configuresection.png)

8. Çoklu oturum açmayı yapılandırma **Ivanti Service Manager'ı (ISM)** tarafı, indirilen göndermek için ihtiyacınız **sertifika (ham)** ve kopyalanan **oturum açma URL'si**,  **Azure AD tanımlayıcısı**, **oturum kapatma URL'si** için [Ivanti Service Manager'ı (ISM) destek ekibi](https://www.ivanti.com/support/contact). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create-aaduser-01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create-aaduser-02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-an-ivanti-service-manager-ism-test-user"></a>Ivanti Service Manager'ı (ISM) bir test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Ivanti Service Manager (ISM) adlı bir kullanıcı oluşturmaktır. Ivanti Service Manager'ı (ISM) tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Ivanti Service Manager'ı (ISM) erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Ivanti Service Manager'ı (ISM) destek ekibi](https://www.ivanti.com/support/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Ivanti Service Manager (ISM) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Ivanti Service Manager'ı (ISM)**.

    ![Çoklu oturum açmayı yapılandırın](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Ivanti Service Manager'ı (ISM) kutucuğa tıkladığınızda, otomatik olarak Ivanti Service Manager'ı (ISM) uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
