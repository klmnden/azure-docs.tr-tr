---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RingCentral | Microsoft Docs'
description: Azure Active Directory ve RingCentral arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9cd97bc226ec69441b933a9f7bf3caec17f1478
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877131"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Öğretici: RingCentral ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RingCentral tümleştirme konusunda bilgi edinin.

Azure AD ile RingCentral tümleştirme ile aşağıdaki avantajları sağlar:

- RingCentral erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için RingCentral (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RingCentral yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik RingCentral çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden RingCentral ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ringcentral-from-the-gallery"></a>Galeriden RingCentral ekleme
Azure AD'de RingCentral tümleştirmesini yapılandırmak için RingCentral Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RingCentral eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/ringcentral-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/ringcentral-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/ringcentral-tutorial/a_new_app.png)

4. Arama kutusuna **RingCentral**seçin **RingCentral** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/ringcentral-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RingCentral sınayın.

Tek iş için oturum açma için Azure AD ne RingCentral karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının RingCentral ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma RingCentral ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RingCentral test kullanıcısı oluşturma](#create-a-ringcentral-test-user)**  - kullanıcı Azure AD gösterimini bağlı RingCentral Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve RingCentral uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile RingCentral yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **RingCentral** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/ringcentral-tutorial/b1_b2_select_sso.png)

2. Tıklayın **oturum açma tek Mod Değiştir** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/ringcentral-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/ringcentral-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/ringcentral-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![image](./media/ringcentral-tutorial/b9_saml.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![image](./media/ringcentral-tutorial/b9(1)_saml.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** aşağıda gösterildiği gibi metin bölümü :

    ![image](./media/ringcentral-tutorial/b21-domains_and_urls.png)

    d. İçinde **oturum açma URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    > [!NOTE]
    > Size **hizmet sağlayıcısı meta veri dosyası** RingCentral SSO yapılandırma sayfasında, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

6. Öğeniz yoksa **hizmet sağlayıcısı meta veri dosyası**, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL:

    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. İçinde **tanımlayıcı** metin kutusuna bir URL:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. İçinde **yanıt URL'si** metin kutusuna bir URL:
    
    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![image](./media/ringcentral-tutorial/b2-domains_and_urls.png)

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** olarak başına verilen seçeneklerden sertifikayı indirmek için gereksinim ve bilgisayarınıza kaydedin.

    ![image](./media/ringcentral-tutorial/certificatebase64.png)
    
8. Bir başka web tarayıcı penceresinde RingCentral bir güvenlik yöneticisi olarak oturum açın.

9. En üstte tıklayarak **Araçları**.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

10. Gidin **çoklu oturum açma**.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

11. Üzerinde **çoklu oturum açma** sayfasındaki **SSO yapılandırma** bölümü, gelen **1. adım** tıklayın **Düzenle** ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

12. Üzerinde **tek oturum açma kurulumunu** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Tıklayın **Gözat** Azure portalından indirdiğiniz meta veri dosyasını karşıya yüklemek için.

    b. Meta veriler karşıya yüklendikten sonra otomatik olarak doldurulmuş değerlerini alma **SSO genel bilgileri** bölümü.

    c. Altında **eşleme özniteliği** bölümünden **harita e-posta özniteliği** olarak `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kaydet**’e tıklayın.

    e. Gelen **2. adım** tıklayın **indirme** indirmek için **hizmet sağlayıcısı meta veri dosyası** ve bunu karşıya **temel SAML yapılandırma** bölümü otomatik olarak doldurmak için **tanımlayıcı** ve **yanıt URL'si** Azure portalında değerleri.

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. Aynı sayfaya gidin **SSO etkinleştirme** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    a. Seçin **etkinleştirme SSO hizmet**.
    
    b. Seçin **SSO veya RingCentral kimlik bilgilerinizle oturum açmasına imkan tanıyın**.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/ringcentral-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/ringcentral-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/ringcentral-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-ringcentral-test-user"></a>RingCentral test kullanıcısı oluşturma

Bu bölümde, Britta Simon içinde RingCentral adlı bir kullanıcı oluşturun. Çalışmak [RingCentral istemci Destek ekibine](https://success.ringcentral.com/RCContactSupp) RingCentral platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RingCentral erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![image](./media/ringcentral-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **RingCentral**.

    ![image](./media/ringcentral-tutorial/d_all_proapplications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/ringcentral-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/ringcentral-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde RingCentral kutucuğa tıkladığınızda, otomatik olarak RingCentral uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ringcentral-tutorial/tutorial_general_01.png
[2]: ./media/ringcentral-tutorial/tutorial_general_02.png
[3]: ./media/ringcentral-tutorial/tutorial_general_03.png
[4]: ./media/ringcentral-tutorial/tutorial_general_04.png

[100]: ./media/ringcentral-tutorial/tutorial_general_100.png

[200]: ./media/ringcentral-tutorial/tutorial_general_200.png
[201]: ./media/ringcentral-tutorial/tutorial_general_201.png
[202]: ./media/ringcentral-tutorial/tutorial_general_202.png
[203]: ./media/ringcentral-tutorial/tutorial_general_203.png

