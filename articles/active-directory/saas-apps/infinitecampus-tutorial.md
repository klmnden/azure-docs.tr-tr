---
title: 'Öğretici: Azure Active Directory Tümleştirmesi, kampüs sonsuz ile | Microsoft Docs'
description: Azure Active Directory ve sonsuz kampüs arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d7d194d810e0fd3b9fb57b0876bee12447f65c6
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519876"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Öğretici: Sonsuz kampüs ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile sonsuz kampüs tümleştirme konusunda bilgi edinin.

Sonsuz kampüs Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Sonsuz kampüs erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için sonsuz kampüs açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile sonsuz kampüs yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir sonsuz kampüs çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).
- En azından, Azure Active Directory yönetici olmanız ve kampüs ürün güvenlik rolü, "Öğrenci bilgi sistemi (yapılandırmasını tamamlamak için SIS)" olması gerekir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden sonsuz kampüs ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-infinite-campus-from-the-gallery"></a>Galeriden sonsuz kampüs ekleme

Azure AD'de sonsuz kampüs tümleştirmesini yapılandırmak için sonsuz kampüs Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden sonsuz kampüs eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **sonsuz kampüs**seçin **sonsuz kampüs** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde sonsuz kampüs](./media/infinitecampus-tutorial/tutorial_infinitecampus_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma sonsuz "Britta Simon" adlı bir test kullanıcı tabanlı kampüs sınayın.

Tek iş için oturum açma için Azure AD ne sonsuz kampüs karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve sonsuz kampüs ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma sonsuz kampüs ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir sonsuz kampüs test kullanıcısı oluşturma](#creating-an-infinite-campus-test-user)**  - sonsuz kullanıcı Azure AD gösterimini bağlı olduğu kampüs Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve sonsuz kampüs uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile sonsuz kampüs yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **sonsuz kampüs** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm bir **hizmet sağlayıcısı meta veri dosyası** sonsuz kampüs dışarı aktarılan, tamamlamak 4.a 4.d aracılığıyla adımları ve 11.c adımı atlayın. Bir hizmet sağlayıcısı meta veri dosyası yoksa, 5. adıma atlayın.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

        ![image](common/b9_saml.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![image](common/b9(1)_saml.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** aşağıda gösterildiği gibi metin bölümü :

    ![image](./media/infinitecampus-tutorial/tutorial_infinitecampus_url.png)

    d. İçinde **oturum açma URL'si** metin kutusuna bir URL (etki alanı barındırma modeliyle göre değişir) aşağıdaki düzeni kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

5. Yoksa **hizmet sağlayıcısı meta veri dosyası**, (etki alanı barındırma modeliyle değişir unutmayın) aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![Çoklu oturum açma bilgileri sonsuz kampüs etki alanı ve URL'ler](./media/infinitecampus-tutorial/tutorial_infinitecampus_url1.png)

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde, Kopyala **simgesi** kopyalamak için **uygulama Federasyon meta verileri URL'sini**  kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/infinitecampus-tutorial/tutorial_infinitecampus_certificate.png) 

7. Üzerinde **sonsuz kampüs kümesi** bölümünde, karşıya yükleme veya Azure meta veri dosyası/URL'sini kullanarak doğrulamak için aşağıdaki değerleri kullanın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Sonsuz kampüs yapılandırma](common/configuresection.png)

8. Bir başka web tarayıcı penceresinde sonsuz kampüs bir güvenlik yöneticisi olarak oturum açın.

9. Menü sol tarafında tıklayın **Sistem Yönetimi**.

    ![Yönetici](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

10. Gidin **kullanıcı güvenlik** > **SAML Yönetim** > **SSO Servis Sağlayıcı Yapılandırması**.

    ![Saml](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

11. Üzerinde **SSO Servis Sağlayıcı Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Sso](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Seçin **etkinleştir SAML çoklu oturum açma**.
    
    b. Düzen **isteğe bağlı öznitelik adı** içerecek şekilde **adı**
    
    c. Üzerinde **kimlik sağlayıcısı (IDP) sunucu verilerini almak için bir seçenek belirleyin** bölümünde, seçin **meta veri URL'si**, Yapıştır **uygulama Federasyon meta verileri URL'sini** (Başlangıç, yukarıdaki adım 6) içinde kutusunu ve ardından **eşitleme**.

    d. Tıklayın **hizmet sağlayıcısı meta verileri** kaydetmek için bağlantı **hizmet sağlayıcısı meta veri dosyası** bilgisayarınızda ve bunu karşıya **temel SAML yapılandırma** otomatik olarak bölümü doldurma **tanımlayıcı** ve **yanıt URL'si** değerleri Azure portalında (4. adım karşıya yükleme ve değerlerinin otomatik popülasyonu bakın veya el ile girişi için 5. adım).

    e. ' I tıklattıktan sonra **eşitleme** içinde otomatik olarak doldurulan değerleri almak **SSO Servis Sağlayıcı Yapılandırması** sayfası.

    f. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Oluşturmak için bu bölümün amacı olan bir _tek_ Britta Simon adlı Azure portalında test kullanıcısı.

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

### <a name="creating-an-infinite-campus-test-user"></a>Bir sonsuz kampüs test kullanıcısı oluşturma

Sonsuz kampüs, demografik bilgileri ortalanmış bir mimariye sahiptir. Lütfen başvurun [sonsuz kampüs Destek ekibine](mailto:sales@infinitecampus.com) sonsuz kampüs platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için sonsuz kampüs erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **sonsuz kampüs**.

    ![Çoklu oturum açmayı yapılandırın](./media/infinitecampus-tutorial/tutorial_infinitecampus_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde sonsuz kampüs kutucuğa tıkladığınızda, otomatik olarak sonsuz kampüs uygulamanıza açan. Azure AD yönetmekte olduğunuz tarayıcıda sonsuz kampüs uygulamasına oturum açıyorsanız, Azure AD test kullanıcısı olarak oturum açtığınız emin olun. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

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
