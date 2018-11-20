---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Peakon | Microsoft Docs'
description: Azure Active Directory ve Peakon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a944c397-ed3f-4d45-b9b2-6d4bcb6b0a09
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: af3402aab6e4a3a1b0401d66d42e82e449552867
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977741"
---
# <a name="tutorial-azure-active-directory-integration-with-peakon"></a>Öğretici: Azure Active Directory Peakon ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Peakon tümleştirme konusunda bilgi edinin.

Azure AD ile Peakon tümleştirme ile aşağıdaki avantajları sağlar:

- Peakon erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Peakon (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Peakon yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Peakon çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Peakon ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-peakon-from-the-gallery"></a>Galeriden Peakon ekleme

Azure AD'de Peakon tümleştirmesini yapılandırmak için Peakon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Peakon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Peakon**seçin **Peakon** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Peakon](./media/peakon-tutorial/tutorial_peakon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Peakon sınayın.

Tek iş için oturum açma için Azure AD ne Peakon karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Peakon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Peakon ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Peakon test kullanıcısı oluşturma](#creating-a-peakon-test-user)**  - kullanıcı Azure AD gösterimini bağlı Peakon Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Peakon uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Peakon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Peakon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **Kurulum çoklu oturum açma SAML ile** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirmek **IDP** başlatılan modu:

    ![Peakon etki alanı ve URL'ler tek oturum açma bilgileri](./media/peakon-tutorial/tutorial_peakon_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://app.peakon.com/saml/<companyid>/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://app.peakon.com/saml/<companyid>/assert`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Peakon etki alanı ve URL'ler tek oturum açma bilgileri](./media/peakon-tutorial/tutorial_peakon_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://app.peakon.com/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve öğreticisinde açıklandığı latert olan yanıt URL'si ile güncelleştirin.

6. İçinde **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/peakon-tutorial/tutorial_peakon_certificate.png) 

7. Üzerinde **Peakon kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Peakon yapılandırma](common/configuresection.png)

8. Farklı bir web tarayıcı penceresinde Peakon için yönetici olarak oturum açın.

9. Sayfanın sol tarafındaki menü çubuğundaki **yapılandırma**, ardından gidin **tümleştirmeler**.

    ![Yapılandırma](./media/peakon-tutorial/tutorial_peakon_config.png)

10. Üzerinde **tümleştirmeler** sayfasında, tıklayarak **çoklu oturum açma**.

    ![Tek](./media/peakon-tutorial/tutorial_peakon_single.png)

11. Altında **çoklu oturum açma** bölümünde, tıklayarak **etkinleştirme**.

    ![Etkinleştir](./media/peakon-tutorial/tutorial_peakon_enable.png)

12. Üzerinde **çoklu oturum açma için SAML kullanarak çalışanlara** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Saml](./media/peakon-tutorial/tutorial_peakon_saml.png)

    a. İçinde **SSO oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure portaldan kopyaladığınız.

    b. İçinde **SSO oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    c. Tıklayın **dosya** sertifika kutusuna Azure portalından indirilen sertifikayı karşıya yüklemek için.

    d. Tıklayın **simgesi** kopyalamak için **varlık kimliği** ve yapıştırın **tanımlayıcı** metin kutusunda **temel SAML yapılandırma** bölümü Azure portalında.

    e. Tıklayın **simgesi** kopyalamak için **yanıt URL'si (ACS)** ve yapıştırın **yanıt URL'si** metin kutusunda **temel SAML yapılandırma**   Azure portalında bölümü.

    f. **Kaydet**’e tıklayın

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

### <a name="creating-a-peakon-test-user"></a>Peakon test kullanıcısı oluşturma

Peakon için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Peakon sağlanması gerekir.  
Peakon söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Peakon şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Sayfanın sol tarafındaki menü çubuğundaki **yapılandırma**, ardından gidin **çalışanlar**.

    ![Çalışan](./media/peakon-tutorial/tutorial_peakon_employee.png)

3. Sayfanın üst sağ tarafında tıklayın **Ekle çalışan**.

      ![Çalışanı Ekle](./media/peakon-tutorial/tutorial_peakon_addemployee.png)

3. Üzerinde **yeni çalışan** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

     ![Yeni çalışan](./media/peakon-tutorial/tutorial_peakon_create.png)

    a. İçinde **adı** metin türü adı olarak **Britta** ve Soyadı olarak **simon**.

    b. İçinde **e-posta** metin kutusu, türü e-posta adresi ister **Brittasimon@contoso.com**.

    c. Tıklayın **Oluştur çalışan**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Peakon erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Peakon**.

    ![Çoklu oturum açmayı yapılandırın](./media/peakon-tutorial/tutorial_peakon_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Peakon kutucuğa tıkladığınızda, otomatik olarak Peakon uygulamanıza açan.
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
