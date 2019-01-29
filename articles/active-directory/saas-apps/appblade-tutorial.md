---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AppBlade | Microsoft Docs'
description: Azure Active Directory ve AppBlade arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2efe0cd50e894ad76ab3b40b17970091870f9711
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178061"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Öğretici: AppBlade ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AppBlade tümleştirme konusunda bilgi edinin.

Azure AD ile AppBlade tümleştirme ile aşağıdaki avantajları sağlar:

- AppBlade erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için AppBlade (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AppBlade yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir AppBlade çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden AppBlade ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-appblade-from-the-gallery"></a>Galeriden AppBlade ekleme
Azure AD'de AppBlade tümleştirmesini yapılandırmak için AppBlade Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AppBlade eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AppBlade**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/tutorial_appblade_search.png)

5. Sonuçlar panelinde seçin **AppBlade**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı AppBlade ile test etme

Tek iş için oturum açma için Azure AD ne AppBlade karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının AppBlade ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

AppBlade içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma AppBlade ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AppBlade test kullanıcısı oluşturma](#creating-an-appblade-test-user)**  - kullanıcı Azure AD gösterimini bağlı AppBlade Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve AppBlade uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile AppBlade yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **AppBlade** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_samlbase.png)

3. Üzerinde **AppBlade etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AppBlade istemci Destek ekibine](mailto:support@appblade.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **AppBlade** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [AppBlade Destek ekibine](mailto:support@appblade.com). Ayrıca, Lütfen bunları yapılandırmasını isteyin **SSO veren URL'si** olarak `https://appblade.com/saml`. Bu ayar tek çalışmak için oturum açma için gereklidir.


> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/appblade-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-appblade-test-user"></a>Bir AppBlade test kullanıcısı oluşturma

Bu bölümün amacı AppBlade Britta Simon adlı bir kullanıcı oluşturmaktır. AppBlade tam zamanında sağlama, varsayılan olarak etkin olan destekler. **Etki alanı adınızı AppBlade ile kullanıcı sağlama için yapılandırıldığından emin olun. Yalnızca tam zamanında kullanıcı sonra çalışır sağlama.**

Kullanıcı tarafından AppBlade hesabınız için yapılandırılmış etki alanı kullanıcı otomatik olarak belirlediğiniz izin düzeyine sahip bir üye olarak hesaba katılır sonra biten bir e-posta adresi varsa, "Temel" (yalnızca uygulama yükleyebilmek için temel bir kullanıcı) biri mi , "Üyesini takım" (yeni uygulama sürümleri yükleyebilir ve projeleri yönetmek bir kullanıcı), veya "Yönetici" (hesap tam yönetim ayrıcalıklarını). Normalde bir temel seçin ve sonra kullanıcı bir yönetici oturum açma (Yönetici e-posta tabanlı oturum önceden yapılandırmak veya bir kullanıcı oturum açtıktan sonra müşteri adına yükseltmek için AppBlade gerekir) kullanarak el ile yükseltmeniz.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa AppBlade erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [AppBlade Destek ekibine](mailto:support@appblade.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için AppBlade erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon AppBlade için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **AppBlade**.

    ![Çoklu oturum açmayı yapılandırın](./media/appblade-tutorial/tutorial_appblade_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  
Erişim panelinde AppBlade kutucuğa tıkladığınızda, otomatik olarak AppBlade uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/appblade-tutorial/tutorial_general_01.png
[2]: ./media/appblade-tutorial/tutorial_general_02.png
[3]: ./media/appblade-tutorial/tutorial_general_03.png
[4]: ./media/appblade-tutorial/tutorial_general_04.png

[100]: ./media/appblade-tutorial/tutorial_general_100.png

[200]: ./media/appblade-tutorial/tutorial_general_200.png
[201]: ./media/appblade-tutorial/tutorial_general_201.png
[202]: ./media/appblade-tutorial/tutorial_general_202.png
[203]: ./media/appblade-tutorial/tutorial_general_203.png

