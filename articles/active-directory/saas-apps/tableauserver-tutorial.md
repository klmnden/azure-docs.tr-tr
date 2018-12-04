---
title: 'Öğretici: Azure Active Directory Tableau Server ile tümleştirme | Microsoft Docs'
description: Tableau Server ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: 84ea1d999a26ce0ce1d548da92549c6a718d5978
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850372"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Öğretici: Azure Active Directory Tableau Server ile tümleştirme

Bu öğreticide, Tableau Server, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Tableau Server, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tableau Server erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Tableau Server açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Tableau Server ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Tableau Server Çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Tableau Server ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau Server ekleme

Azure AD'de Tableau Server tümleştirmesini yapılandırmak için Tableau Server Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Tableau Server eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Tableau Server**seçin **Tableau Server** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde tableau Server](./media/tableauserver-tutorial/tutorial-tableauserver-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Tableau Server'ın "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Tableau Server karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Tableau Server ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Server ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tableau Server test kullanıcısı oluşturma](#creating-a-tableau-server-test-user)**  - kullanıcı Azure AD gösterimini bağlı Tableau Server Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Tableau Server uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Tableau Server ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Tableau Server** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial-general-301.png)

3. Tableau Server uygulaması bir özel talep bekliyor **kullanıcıadı** aşağıda olarak tanımlanması gerekir. Benzersiz kullanıcı tanımlayıcısı talebi yerine kullanıcı tanımlayıcısı olarak kullanılıyor. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Tıklayın **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-attribute.png)

4. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | kullanıcı adı | User.userPrincipalName |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-add-attribute.png)

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-manage-attribute.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Girin **Namespace** değeri.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. **Kaydet**’e tıklayın.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

6. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link`
    
    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    ![image](./media/tableauserver-tutorial/tutorial-tableauserver-url.png)
     
    > [!NOTE] 
    > Yukarıdaki değerlerden gerçek değerlerin değil. Gerçek URL ve tanımlayıcıdır öğreticinin ilerleyen bölümlerinde açıklanan Tableau Server yapılandırma sayfasından ile güncelleştirin.

7. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/tableauserver-tutorial/tutorial-tableauserver-certificate.png) 

8. Uygulamanız için yapılandırılmış SSO elde etmek için Tableau Server Kiracı yönetici olarak oturum açma gerekir.

9. Üzerinde **Tableau Server Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-001.png)

    a. Tableau Server yapılandırmasında tıklayın **SAML** sekmesi. 
  
    b. Onay kutusunu işaretleyin, **kullanım SAML çoklu oturum açma için**.
   
    c. Tableau Server dönüş URL'si — Tableau Server kullanıcıları, gibi erişecek URL http://tableau_server. Kullanarak http://localhost önerilmez. Bir URL kullanarak bir eğik çizgiyle (örneğin, http://tableau_server/) desteklenmiyor. Kopyalama **Tableau Server dönüş URL'si** ve Azure AD'ye yapıştırın **işareti bulunan URL'si** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
   
    d. SAML varlık kimliği — varlık kimliği için IDP Tableau Server yüklemenizin benzersiz olarak tanımlar. Tableau Server URL'nizi yeniden burada isterseniz girebilirsiniz, ancak Tableau Server URL'nizi olması gerekmez. Kopyalama **SAML varlık kimliği** ve Azure AD'ye yapıştırın **tanımlayıcı** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
     
    e. Tıklayın **meta veri dosyası dışarı** ve metin düzenleyicisi uygulamayı açın. Onay belgesi tüketici hizmeti URL'si Http Post ile bulup 0 dizini ve URL'yi kopyalayın. Artık Azure AD'ye yapıştırın **yanıt URL'si** metin kutusunda **Tableau Server etki alanı ve URL'ler** bölümü.
   
    f. Azure portalından indirdiğiniz, Federasyon meta verileri dosyasını bulun ve bunu karşıya **IDP SAML meta veri dosyası**.
   
    g. Tıklayın **Tamam** Tableau Server yapılandırma sayfasında düğme.
   
    >[!NOTE] 
    >Müşteriniz varsa Tableau Server SAML SSO yapılandırma herhangi bir sertifikayı karşıya yüklemek ve SSO akışta dikkate.
    >Tableau Server üzerinde SAML yapılandırmasına yardımcı olması sonra lütfen bu makaleyi okuyun [SAML yapılandırma](https://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).

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
  
### <a name="creating-a-tableau-server-test-user"></a>Tableau Server test kullanıcısı oluşturma

Bu bölümün amacı, Tableau Server Britta Simon adlı bir kullanıcı oluşturmaktır. Tableau server içindeki tüm kullanıcılar sağlamanız gerekir. 

Bu kullanıcının kullanıcı adı Azure AD'ye özel öznitelik içinde yapılandırılmış değer ile eşleşmelidir **username**. Doğru eşleme içeren tümleştirme çalışmalıdır [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kuruluşunuzdaki Tableau Server yöneticinize başvurun gerekir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Tableau Server için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Tableau Server**.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauserver-tutorial/tutorial-tableauserver-app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Tableau Server kutucuğa tıkladığınızda, size otomatik olarak Tableau Server uygulamanıza açan.
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
