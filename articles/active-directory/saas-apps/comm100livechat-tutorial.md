---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Comm100 canlı sohbet | Microsoft Docs'
description: Azure Active Directory ve Comm100 canlı sohbet arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: b85162c8392e8ecdb08a3ed04ff5b9de835808a1
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42055354"
---
# <a name="tutorial-azure-active-directory-integration-with-comm100-live-chat"></a>Öğretici: Azure Active Directory Tümleştirme ile Comm100 canlı sohbet

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Comm100 canlı sohbet öğrenin.

Azure AD ile tümleştirme Comm100 canlı sohbet ile aşağıdaki avantajları sağlar:

- Comm100 canlı sohbet erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Comm100 canlı sohbet için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Comm100 canlı sohbet ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Comm100 canlı sohbet çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Comm100 canlı sohbet ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Galeriden Comm100 canlı sohbet ekleme
Azure AD'de Comm100 canlı sohbet, tümleştirmesini yapılandırmak için Comm100 canlı sohbet Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Comm100 canlı sohbet eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Comm100 canlı sohbet**seçin **Comm100 canlı sohbet** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Canlı sohbet Comm100 sonuç listesinde](./media/comm100livechat-tutorial/tutorial_comm100livechat_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Comm100 canlı sohbet ile test edin.

Tek iş için oturum açma için Azure AD ne Comm100 canlı sohbet karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Comm100 canlı sohbet arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Comm100 canlı sohbet ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Comm100 canlı sohbet test kullanıcısı oluşturma](#create-a-comm100-live-chat-test-user)**  - Comm100 Canlı kullanıcı Azure AD gösterimini bağlı sohbet Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Comm100 canlı sohbet uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Comm100 canlı sohbet ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Comm100 canlı sohbet** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/comm100livechat-tutorial/tutorial_comm100livechat_samlbase.png)

3. Üzerinde **Comm100 canlı sohbet etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Comm100 canlı sohbet etki alanı ve URL'ler tek oturum açma bilgileri](./media/comm100livechat-tutorial/tutorial_comm100livechat_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`
    
    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Oturum açma URL değeri, bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek oturum açma URL ile güncelleştirir.

4. Özel öznitelikler içeren SAML onaylamalarını Comm100 canlı sohbet uygulaması bekliyor. Aşağıdaki öznitelikler bu uygulama için yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/comm100livechat-tutorial/tutorial_attribute_03.png)
    
5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    |  Öznitelik Adı  | Öznitelik Değeri |
    | --------------- | -------------------- |    
    |   e-posta    | User.Mail |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/comm100livechat-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/comm100livechat-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/comm100livechat-tutorial/tutorial_comm100livechat_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/comm100livechat-tutorial/tutorial_general_400.png)

8. Üzerinde **Comm100 canlı sohbet yapılandırma** bölümünde **yapılandırma Comm100 canlı sohbet** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Comm100 canlı sohbet yapılandırma](./media/comm100livechat-tutorial/tutorial_comm100livechat_configure.png)

9. Bir başka web tarayıcı penceresinde Comm100 canlı sohbet bir güvenlik yöneticisi olarak oturum açın.

10. Sayfanın üst sağ tarafında tıklayın **hesabım**.

    ![Myaccount Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

11. Menü Sol taraftan tıklayın **güvenlik** ve ardından **aracı çoklu oturum açma**.

    ![Güvenlik Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

12. Üzerinde **aracı çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Güvenlik Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

    a. İlk vurgulanan bağlantıyı kopyalayın ve yapıştırın **oturum açma URL'si** metin kutusunda **Comm100 canlı sohbet etki alanı ve URL'ler** bölümü Azure portalı.

    b. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    c. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    d. Tıklayın **bir dosya seçin** base-64 karşıya yüklemek için Azure portalından içine yüklediğiniz sertifika kodlanmış **sertifika**.

    e. Tıklayın **Değişiklikleri Kaydet**

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/comm100livechat-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/comm100livechat-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/comm100livechat-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/comm100livechat-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-comm100-live-chat-test-user"></a>Comm100 canlı sohbet test kullanıcısı oluşturma

Comm100 canlı sohbet oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Comm100 canlı sohbet sağlanması gerekir. Comm100 canlı sohbet sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Comm100 canlı sohbet bir güvenlik yöneticisi olarak oturum açın.

2. Sayfanın üst sağ tarafında tıklayın **hesabım**.

    ![Myaccount Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. Menü Sol taraftan tıklayın **aracıları** ve ardından **yeni aracı**.

    ![Aracı Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. Üzerinde **yeni aracı** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yeni aracı Comm100 canlı sohbet](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon@contoso.com**.

    b. İçinde **ad** metin kutusunda, gibi kullanıcı adını girin **Britta**.

    c. İçinde **Soyadı** metin kutusunda, son kullanıcı gibi adını **simon**.

    d. İçinde **görünen adı** metin gibi kullanıcının görünen adını girin **Britta simon**

    e. İçinde **parola** metin kutusuna bir parola yazın.

    f. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Comm100 canlı sohbet için erişim izni verme kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Comm100 canlı sohbet atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Comm100 canlı sohbet**.

    ![Uygulamalar listesinde Comm100 canlı sohbet bağlantı](./media/comm100livechat-tutorial/tutorial_comm100livechat_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Comm100 canlı sohbet kutucuğa tıkladığınızda, otomatik olarak Comm100 canlı sohbet uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/comm100livechat-tutorial/tutorial_general_01.png
[2]: ./media/comm100livechat-tutorial/tutorial_general_02.png
[3]: ./media/comm100livechat-tutorial/tutorial_general_03.png
[4]: ./media/comm100livechat-tutorial/tutorial_general_04.png

[100]: ./media/comm100livechat-tutorial/tutorial_general_100.png

[200]: ./media/comm100livechat-tutorial/tutorial_general_200.png
[201]: ./media/comm100livechat-tutorial/tutorial_general_201.png
[202]: ./media/comm100livechat-tutorial/tutorial_general_202.png
[203]: ./media/comm100livechat-tutorial/tutorial_general_203.png

