---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Aha! | Microsoft Docs'
description: Azure Active Directory ve Aha arasında çoklu oturum açmayı yapılandırmayı öğrenin!
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 669932df7504d71da9b8c109672772a6371c5490
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171210"
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Aha!

Bu öğreticide, Aha tümleştirmeyi öğrenin! Azure Active Directory (Azure AD).

AHA tümleştirme! Azure AD ile aşağıdaki faydaları sağlar:

- Aha erişimi, Azure AD'de kontrol edebilirsiniz!
- Otomatik olarak imzalanan Aha için açma, kullanıcılarınızın etkinleştirebilirsiniz! (Çoklu oturum açma) ile Azure AD hesaplarına
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Aha yapılandırılamadı!, aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Aha! Çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. AHA ekleme! Galeriden
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-aha-from-the-gallery"></a>AHA ekleme! Galeriden
Aha tümleştirmesini yapılandırmak için! Azure AD ile Aha eklemeniz gerekir! Galeriden listenizi yönetilen SaaS uygulamaları için.

**AHA eklemek için! Galeriden, aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Aha!**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/tutorial_aha_search.png)

5. Sonuçlar panelinde seçin **Aha!** ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Aha ile test etme! "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD içinde Aha karşılığı kullanıcının bilmesi gerekir! bir kullanıcının Azure AD'de ' dir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Aha ilgili kullanıcı arasında bağlantı ilişki! kurulması gerekir.

Aha içinde!, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Aha ile test etmek için!, şu yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Oluşturma bir Aha! Kullanıcı test](#creating-an-aha-test-user)**  - Aha içinde bir karşılığı Britta simon'un sağlamak için! Bu kullanıcı Azure AD gösterimini bağlıdır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Aha içinde yapılandırma! uygulama.

**Azure AD çoklu oturum açma ile Aha yapılandırmak için!, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Aha!** Uygulama Tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/aha-tutorial/tutorial_aha_samlbase.png)

3. Üzerinde **Aha! Etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/aha-tutorial/tutorial_aha_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.aha.io/session/new`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.aha.io`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Aha! İstemci Destek ekibine](https://www.aha.io/company/contact) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/aha-tutorial/tutorial_aha_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/aha-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde, Aha için oturum açın! Yönetici olarak şirketin sitesi.

7. Üstteki menüden **ayarları**.

    ![Ayarları](./media/aha-tutorial/IC798950.png "ayarları")

8. Tıklayın **hesabı**.
   
    ![Profili](./media/aha-tutorial/IC798951.png "profili")

9. Tıklayın **güvenlik ve çoklu oturum açma**.
   
    ![Güvenlik ve çoklu oturum açma](./media/aha-tutorial/IC798952.png "güvenlik ve çoklu oturum açma")

10. İçinde **çoklu oturum açma** bölümünde olarak **kimlik sağlayıcısı**seçin **SAML2.0**.
   
    ![Güvenlik ve çoklu oturum açma](./media/aha-tutorial/IC798953.png "güvenlik ve çoklu oturum açma")

11. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açma](./media/aha-tutorial/IC798954.png "çoklu oturum açma")
    
       a. İçinde **adı** metin yapılandırmanız için bir ad yazın.

       b. İçin **kullanarak yapılandırma**seçin **meta veri dosyası**.
   
       c. İndirilen meta verileri dosyanızı karşıya yüklemek için tıklayın **Gözat**.
   
       d. Tıklayın **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/aha-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-aha-test-user"></a>Oluşturma bir Aha! Test kullanıcısı

Azure AD kullanıcıların oturum açmak için AHA sağlamak için!, Aha sağlanması gerekir!  

Aha söz konusu olduğunda!, sağlama, otomatik bir görev olduğundan. Sizin için hiçbir eylem öğesini yoktur.

Kullanıcıları otomatik olarak ilk çoklu oturum açma girişimi sırasında gerekirse oluşturulur.

>[!NOTE]
>Diğer bir Aha kullanabilirsiniz! kullanıcı hesabı oluşturma araçları veya Aha tarafından sağlanan API'leri! AAD kullanıcı hesapları sağlamak için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Aha erişim vererek Britta Simon etkinleştirin!.

![Kullanıcı Ata][200] 

**Britta Simon Aha için atanacak!, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Aha!**.

    ![Çoklu oturum açmayı yapılandırın](./media/aha-tutorial/tutorial_aha_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/aha-tutorial/tutorial_general_01.png
[2]: ./media/aha-tutorial/tutorial_general_02.png
[3]: ./media/aha-tutorial/tutorial_general_03.png
[4]: ./media/aha-tutorial/tutorial_general_04.png

[100]: ./media/aha-tutorial/tutorial_general_100.png

[200]: ./media/aha-tutorial/tutorial_general_200.png
[201]: ./media/aha-tutorial/tutorial_general_201.png
[202]: ./media/aha-tutorial/tutorial_general_202.png
[203]: ./media/aha-tutorial/tutorial_general_203.png

