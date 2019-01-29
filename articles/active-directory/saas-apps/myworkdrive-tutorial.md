---
title: 'Öğretici: MyWorkDrive ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve MyWorkDrive arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: f0e2c499619df938bd6f4b05757ba607a9edf244
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183365"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Öğretici: MyWorkDrive ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile MyWorkDrive tümleştirme konusunda bilgi edinin.

MyWorkDrive, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- MyWorkDrive erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan MyWorkDrive'nın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile MyWorkDrive yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- MyWorkDrive çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. MyWorkDrive galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-myworkdrive-from-the-gallery"></a>MyWorkDrive galeri ekleme

Azure AD'de MyWorkDrive'nın tümleştirmesini yapılandırmak için MyWorkDrive Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden MyWorkDrive eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **MyWorkDrive**seçin **MyWorkDrive** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde MyWorkDrive](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı MyWorkDrive sınayın.

Tek çalışmak için oturum açma için Azure AD ne MyWorkDrive karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının MyWorkDrive ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma MyWorkDrive ile'test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[MyWorkDrive test kullanıcısı oluşturma](#creating-a-myworkdrive-test-user)**  - kullanıcı Azure AD gösterimini bağlı MyWorkDrive Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve MyWorkDrive uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile MyWorkDrive yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **MyWorkDrive** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirmek **IDP** başlatılan modu:

    ![MyWorkDrive etki alanı ve URL'ler tek oturum açma bilgileri](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![MyWorkDrive etki alanı ve URL'ler tek oturum açma bilgileri](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SERVER.DOMAIN.COM>/Account/Login-saml` 

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin.  Kendi şirketin MyWorkDrive sunucu konak name:e.g girin.
    > 
    > Yanıt URL'si: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Oturum açma URL'si:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Kendi ana bilgisayar adı ve bu değerler için SSL sertifikası ayarlama değilseniz MyWorkDrive istemci Destek ekibine başvurun.

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde, Kopyala **simgesi** kopyalamak için **uygulama Federasyon meta verileri URL'sini**ve bilgisayarınıza kaydedin...

    ![Sertifika indirme bağlantısı](./media/myworkdrive-tutorial/tutorial_myworkdrive_certificate.png)

7. Bir başka web tarayıcı penceresinde MyWorkDrive bir güvenlik yöneticisi olarak oturum açın.

8. MyWorkDrive sunucuda yönetim paneli tıklayarak **Kurumsal** ve aşağıdaki adımları gerçekleştirin:

    ![Yönetici](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Etkinleştirme **SAML/ADFS SSO**.

    b. Seçin **SAML - Azure AD**

    c. İçinde **Azure uygulama Federasyon meta verileri URL'sini** metin değerini yapıştırın **uygulama Federasyon meta verileri URL'sini** hangi Azure portaldan kopyaladığınız.

    d. **Kaydet**’e tıklayın

    >[!NOTE]
    >Ek bilgileri İnceleme için [MyWorkDrive Azure AD'ye destek makalesi](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

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

### <a name="creating-a-myworkdrive-test-user"></a>MyWorkDrive test kullanıcısı oluşturma

Bu bölümde, Britta Simon MyWorkDrive adlı bir kullanıcı oluşturun. Çalışmak [MyWorkDrive Destek ekibine](mailto:support@myworkdrive.com) MyWorkDrive platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için MyWorkDrive erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **MyWorkDrive**.

    ![Çoklu oturum açmayı yapılandırın](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde MyWorkDrive kutucuğa tıkladığınızda, otomatik olarak MyWorkDrive uygulamanıza açan.
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
