---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Egnyte | Microsoft Docs'
description: Azure Active Directory ve Egnyte arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2018
ms.author: jeedes
ms.openlocfilehash: e33fc71e0e43864d7d70495fc5056a8acaf4ad56
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159021"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Öğretici: Egnyte ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Egnyte tümleştirme konusunda bilgi edinin.

Azure AD ile Egnyte tümleştirme ile aşağıdaki avantajları sağlar:

- Egnyte erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Egnyte (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Egnyte yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Egnyte çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Egnyte ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-egnyte-from-the-gallery"></a>Galeriden Egnyte ekleme

Azure AD'de Egnyte tümleştirmesini yapılandırmak için Egnyte Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Egnyte eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Egnyte**seçin **Egnyte** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Egnyte](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Egnyte sınayın.

Tek iş için oturum açma için Azure AD ne Egnyte karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Egnyte ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Egnyte ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Egnyte test kullanıcısı oluşturma](#creating-an-egnyte-test-user)**  - kullanıcı Azure AD gösterimini bağlı Egnyte Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Egnyte uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Egnyte yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Egnyte** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Egnyte etki alanı ve URL'ler tek oturum açma bilgileri](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Egnyte istemci Destek ekibine](https://www.egnyte.com/corp/contact_egnyte.html) bu değeri alınamıyor. 

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

6. Üzerinde **Egnyte kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Egnyte yapılandırma](common/configuresection.png)

7. Farklı bir web tarayıcı penceresinde Egnyte şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Tıklayın **ayarları**.
   
    ![Ayarları](./media/egnyte-tutorial/ic787819.png "ayarları")

9. Menüde **ayarları**.

    ![Ayarları](./media/egnyte-tutorial/ic787820.png "ayarları")

10. Tıklayın **yapılandırma** sekmesine ve ardından **güvenlik**.

    ![Güvenlik](./media/egnyte-tutorial/ic787821.png "güvenlik")

11. İçinde **çoklu oturum açma kimlik doğrulaması** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma kimlik doğrulaması](./media/egnyte-tutorial/ic787822.png "çoklu oturum açma kimlik doğrulaması")   
    
    a. Olarak **çoklu oturum açma kimlik doğrulaması**seçin **SAML 2.0**.
   
    b. Olarak **kimlik sağlayıcısı**seçin **AzureAD**.
   
    c. Yapıştırma **oturum açma URL'si** Azure portalına kopyalama kaynağı **kimlik sağlayıcısı oturum açma URL'si** metin.
   
    d. Yapıştırma **Azure AD tanımlayıcısı** Azure portalından kopyalanan **kimlik sağlayıcısı varlık kimliği** metin.
      
    e. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.
   
    f. Olarak **kullanıcı eşleme varsayılan**seçin **e-posta adresi**.
   
    g. Olarak **etki alanına özgü veren değerini kullanmak**seçin **devre dışı**.
   
    h. **Kaydet**’e tıklayın.

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

### <a name="creating-an-egnyte-test-user"></a>Bir Egnyte test kullanıcısı oluşturma

Egnyte için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Egnyte sağlanması gerekir. Egnyte söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Egnyte** şirketinizin sitesi yöneticisi olarak.

2. Git **ayarları \> kullanıcıları ve grupları**.

3. Tıklayın **yeni kullanıcı Ekle**ve ardından eklemek istediğiniz kullanıcı türünü seçin.
   
    ![Kullanıcılar](./media/egnyte-tutorial/ic787824.png "kullanıcılar")

4. İçinde **yeni güç kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Yeni standart kullanıcı](./media/egnyte-tutorial/ic787825.png "yeni standart kullanıcı")   

    a. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon@contoso.com**.

    b. İçinde **kullanıcıadı** metin kutusunda, gibi kullanıcının kullanıcı adı girin **Brittasimon**.

    c. Seçin **çoklu oturum açma** olarak **kimlik doğrulama türü**.
   
    d. **Kaydet**’e tıklayın.
    
    >[!NOTE]
    >Azure Active Directory hesap sahibi bir bildirim e-posta alacaksınız.
    >

>[!NOTE]
>Herhangi diğer Egnyte kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Egnyte tarafından sağlanan.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Egnyte erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Egnyte**.

    ![Çoklu oturum açmayı yapılandırın](./media/egnyte-tutorial/tutorial_egnyte_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Egnyte kutucuğa tıkladığınızda, otomatik olarak Egnyte uygulamanıza açan.
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
