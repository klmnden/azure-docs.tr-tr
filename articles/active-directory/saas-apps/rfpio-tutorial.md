---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RFPIO | Microsoft Docs'
description: Azure Active Directory ve RFPIO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6b8109c8d3834f932ba492eddb8d6332acc1707
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61081051"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: RFPIO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RFPIO tümleştirme konusunda bilgi edinin.

Azure AD ile RFPIO tümleştirme ile aşağıdaki avantajları sağlar:

- Denetleyebilirsiniz kimin RFPIO erişimi, Azure AD'de.
- Otomatik olarak imzalanan için RFPIO (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RFPIO yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz.
- RFPIO tek bir oturum üzerinde etkin olmayan abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için bir üretim ortamında kullanmanızı önermemekteyiz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Galeriden RFPIO ekleniyor.
1. Yapılandırma ve test Azure AD çoklu oturum açma.

## <a name="add-rfpio-from-the-gallery"></a>Galeriden RFPIO Ekle
Azure AD'de RFPIO tümleştirmesini yapılandırmak için RFPIO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

### <a name="to-add-rfpio-from-the-gallery"></a>Galeriden RFPIO eklemek için

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **RFPIO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/tutorial_rfpio_search.png)

1. Sonuçlar panelinde seçin **RFPIO**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RFPIO sınayın.

Tek iş için oturum açma için Azure AD RFPIO karşılığı kullanıcının Azure AD'de kullanıcı arasındaki ilişki nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının RFPIO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RFPIO içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma RFPIO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **Azure AD çoklu oturum açmayı yapılandırmayı**--bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **Bir Azure AD test kullanıcısı oluşturma**--Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **RFPIO test kullanıcısı oluşturma** --bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı RFPIO sağlamak için.
1. **Azure AD test kullanıcı atama**--Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. **Çoklu oturum açmayı test** --yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve RFPIO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile RFPIO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **RFPIO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_samlbase.png)

1. Üzerinde **RFPIO etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://www.rfpio.com`

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Denetleme **Gelişmiş URL ayarlarını göster**.

    c. İçinde **geçiş durumu** metin kutusuna bir dize değeri. İlgili kişi [RFPIO Destek ekibine](https://www.rfpio.com/contact/) bu değeri alınamıyor. 

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu: 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://www.app.rfpio.com`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_general_400.png)

1. Bir oturum açma için farklı bir web tarayıcı penceresinde **RFPIO** yönetici olarak Web sitesi.

1. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

1. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

1. Tıklayarak **özellikler ve tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app4.png)

1. İçinde **SAML SSO yapılandırma** tıklayın **Düzenle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app3.png)

1. Bu bölümde, eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app5.png)
    
    a. İçeriğini kopyalayın **indirilen meta veri XML** yapıştırın **kimlik Yapılandırması** alan.

    > [!NOTE]
    >İçeriği kopyalamak için indirilen **meta veri XML** kullanım **not defteri ++** veya uygun **XML Düzenleyicisi**. 

    b. Tıklayın **doğrulama**.

    c. ' I tıklattıktan sonra **doğrulama**Çevir, **SAML(Enabled)** açık.

    d. **Gönder**'e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rfpio-test-user"></a>RFPIO test kullanıcısı oluşturma

RFPIO için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların RFPIO sağlanması gerekir.  
RFPIO söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RFPIO şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

1. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

1. Tıklayın **TAKIM ÜYELERİ**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app6.png)

1. Tıklayarak **ÜYELER Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app7.png)

1. İçinde **eklediğiniz yeni üyeler** bölümü. Eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app8.png)

    a. ENTER **e-posta adresi** içinde **her satırda bir e-posta girin** alan.

    b. Lütfen seçin **rol** gereksinimlerinize göre.

    c. Tıklayın **ÜYELER Ekle**.
        
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RFPIO erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon RFPIO için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **RFPIO**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde RFPIO kutucuğa tıkladığınızda, otomatik olarak RFPIO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/rfpio-tutorial/tutorial_general_01.png
[2]: ./media/rfpio-tutorial/tutorial_general_02.png
[3]: ./media/rfpio-tutorial/tutorial_general_03.png
[4]: ./media/rfpio-tutorial/tutorial_general_04.png

[100]: ./media/rfpio-tutorial/tutorial_general_100.png

[200]: ./media/rfpio-tutorial/tutorial_general_200.png
[201]: ./media/rfpio-tutorial/tutorial_general_201.png
[202]: ./media/rfpio-tutorial/tutorial_general_202.png
[203]: ./media/rfpio-tutorial/tutorial_general_203.png

