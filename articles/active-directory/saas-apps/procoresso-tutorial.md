---
title: 'Öğretici: Procore SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Procore SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36e1c8d6cae79147c5cd4b5a46f5e1c330811ab8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57877658"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Öğretici: Procore SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Procore SSO tümleştirme konusunda bilgi edinin.

Procore SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Procore SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Procore SSO (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Procore SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Procore SSO çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Procore SSO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-procore-sso-from-the-gallery"></a>Galeriden Procore SSO ekleme

Azure AD'de Procore SSO tümleştirmesini yapılandırmak için Procore SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Procore SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Procore SSO**seçin **Procore SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde procore SSO](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Procore "Britta Simon" adlı bir test kullanıcı tabanlı SSO ile test edin.

Tek iş için oturum açma için Azure AD ne Procore SSO karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Procore SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Procore SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Procore SSO test kullanıcısı oluşturma](#creating-a-procore-sso-test-user)**  - kullanıcı Azure AD gösterimini bağlı Procore SSO Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Procore SSO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Procore SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Procore SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açma bilgileri procore SSO etki alanı ve URL'ler](./media/procoresso-tutorial/tutorial_procoresso_url.png)

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

6. Üzerinde **Procore SSO'yu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Procore SSO yapılandırma](common/configuresection.png)

7. Çoklu oturum açmayı yapılandırma **Procore SSO** tarafı, procore şirketinizin sitesi yönetici olarak oturum açın.

8. Araç kutusu açılan listeden, aşağı, tıklayarak **yönetici** SSO ayarlar sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/procore_tool_admin.png)

9. Aşağıdaki - açıklandığı kutularında değerleri yapıştırın

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. İçinde **tek oturum üzerinde veren URL'si** metin kutusunda, değerini yapıştırın **Azure AD tanımlayıcısı** hangi Azure portaldan kopyaladığınız.

    b. İçinde **hedef URL üzerinde SAML oturum** kutusunda, değerini yapıştırın **oturum açma URL'si** hangi Azure portaldan kopyaladığınız.

    c. Artık **Federasyon meta verileri XML** yukarıda Azure portalından indirilen ve etiketin adlı sertifikayı kopyalamanız **X509Certificate**. Kopyalanan değer içine yapıştırın **çoklu oturum açma x509 sertifika** kutusu.

10. **Değişiklikleri Kaydet**’e tıklayın.

11. Bu ayarlar sonra göndermesi gerekiyor. **etki alanı adı** (ör. **contoso.com**) için Procore içine, oturum açtığınız aracılığıyla [Procore Destek ekibine](https://support.procore.com/) ve bunlar etki alanı Federasyon SSO'yu etkinleştirin.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->

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

### <a name="creating-a-procore-sso-test-user"></a>Procore SSO test kullanıcısı oluşturma

Lütfen izleyin Procore SSOc tarafında Procore test kullanıcısı oluşturmak için aşağıdaki adımları.

1. Procore şirketinizin sitesi yönetici olarak oturum açın.  

2. Araç kutusu açılan listeden, aşağı, tıklayarak **dizin** şirket dizin sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Tıklayarak **bir kişi ekleyin** seçeneği formu açın ve girmek için seçenekler - gerçekleştirin

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_add.png)

    a. İçinde **ad** metin türü kullanıcının adı gibi **Britta**.

    b. İçinde **Soyadı** metin türü kullanıcının soyadını gibi **Simon**.

    c. İçinde **e-posta adresi** metin türü kullanıcının e-posta adresi ister **BrittaSimon\@contoso.com**.

    d. Seçin **izin şablonu** olarak **daha sonra izin şablonu Uygula**.

    e. **Oluştur**’a tıklayın.

4. Kontrol edin ve yeni eklenen kişi ayrıntılarını güncelleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_check.png)

5. Tıklayarak **Kaydet ve Davet Gönder** (davet e-posta aracılığıyla gerekliyse) veya **Kaydet** (doğrudan kullanıcı kaydı tamamlamak için Kaydet).
    
    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_save.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Procore SSO erişimi vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Procore SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Procore SSO kutucuğa tıkladığınızda, otomatik olarak Procore SSO uygulamanıza açan.
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
