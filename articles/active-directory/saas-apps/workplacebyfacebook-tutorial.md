---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Facebook ile çalışma | Microsoft Docs'
description: Azure Active Directory ve Facebook ile iş yeri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: jeedes
ms.openlocfilehash: 2e072a27087f90bddd3f7c416904758e40c2f6b9
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425199"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Öğretici: Azure Active Directory Tümleştirme ile Facebook ile çalışma

Bu öğreticide, Facebook ile çalışma alanına Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Facebook ile çalışma alanına Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Facebook ile çalışma alanına erişimi olan Azure AD'de denetleyebilirsiniz
- Otomatik olarak çalışma alanına göre Facebook (çoklu oturum açma) ile Azure AD hesaplarına açan, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iş yeri tarafından Facebook ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Facebook çoklu oturum açma tarafından bir çalışma alanı aboneliği etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook iki ürün, iş yeri standart (ücretsiz) ve çalışma alanı Premium (Ücretli) sahiptir. Herhangi bir çalışma alanı Premium Kiracı SCIM ve SSO tümleştirme hiçbir diğer uygulamaları maliyet veya gerekli lisanslar ile yapılandırabilirsiniz. SSO ve SCIM standart çalışma alanına durumlarda kullanılamaz.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Facebook ile çalışma alanına ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Galeriden Facebook ile çalışma alanına ekleme
Azure AD çalışma alanına Facebook ile tümleştirilmesi yapılandırmak için çalışma alanına Facebook tarafından Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Facebook ile çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Facebook ile çalışma alanına**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

1. Sonuçlar panelinde seçin **Facebook ile çalışma alanına**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Workplace Facebook "Britta Simon." adlı bir test kullanıcıya bağlı olarak test etme

Tek iş için oturum açma için Azure AD çalışma alanına Facebook tarafından karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve iş yeri Facebook ile ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Facebook ile çalışma.

Yapılandırma ve Azure AD çoklu oturum açma ile çalışma alanına Facebook ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Yeniden kimlik doğrulaması sıklığı yapılandırma](#configuring-reauthentication-frequency)**  - çalışma alanı için bir SAML onay isteyecek şekilde yapılandırmak için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Facebook test kullanıcı tarafından bir çalışma alanı oluşturma](#creating-a-workplace-by-facebook-test-user)**  - çalışma alanına kullanıcı Azure AD gösterimini bağlı Facebook tarafından Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Facebook uygulama tarafından çalışma alanınızda yapılandırın.

**Facebook ile çalışma alanı ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Facebook ile çalışma alanına** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

1. Üzerinde **Facebook etki alanı ve URL'ler ile çalışma alanına** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instancename>.facebook.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kimlik doğrulaması sayfası doğru değerlerin çalışma alanı şirket Pano, çalışma alanına topluluk için bakın. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/tutorial_general_400.png)

1. Üzerinde **Facebook yapılandırmaya göre çalışma alanı** bölümünde **yapılandırma çalışma alanı tarafından Facebook** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/config.png) 

1. Bir başka web tarayıcı penceresinde çalışma alanınızı Facebook şirket site yönetici olarak oturum açın.
  
   > [!NOTE] 
   > SAML kimlik doğrulaması işleminin bir parçası olarak, çalışma alanı, parametreleri Azure AD'ye geçirmek için sorgu dizeleri en fazla boyutu 2,5 kilobayt değerlendirebilir.

1. İçinde **yönetim paneli**Git **güvenlik** sekmesini, ardından **kimlik doğrulaması**.

1. Altında **SAML kimlik doğrulaması**seçin **yalnızca SSO** aşağı açılan listeden.

1. Öğesinden kopyalanan değerleri giriş **Facebook yapılandırmaya göre çalışma alanı** karşılık gelen alanlara Azure portal'ın bölüm:

    *   İçinde **SAML URL** metin değerini yapıştırın **çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    *   İçinde **SAML veren URL'si metin kutusuna**, değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.
    *   İçinde **oturum kapatma SAML yönlendirme** (isteğe bağlı) değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    *   Açık, **base-64 kodlamalı sertifika** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML sertifikası** metin.

1. Alıcı URL'si hedef kitle URL'yi girmeniz gerekebilir ve ACS (onaylama tüketici hizmeti) URL'si altında listelenen **SAML yapılandırma** bölümü.

1. Bölümünün altına ilerleyecek ve **Test SSO** düğmesi. Bu sonuçlar ile Azure AD oturum açma sayfasının görünen açılan pencerede gösterilir. Kimlik bilgilerinizi doğrulamak için normal olarak girin. 

    **Sorun giderme:** iş yeri hesabı ile oturum aynı olup Azure AD'den geri döndürülen e-posta adresi emin olun.

1. Test başarıyla tamamlandıktan sonra sayfanın sonuna kaydırın ve tıklayın **Kaydet** düğmesi.

1. Çalışma alanı kullanan tüm kullanıcıları Azure AD oturum açma sayfasında kimlik doğrulaması için şimdi sunulur.

1. **SAML oturum kapatma (isteğe bağlı) yönlendirme** - 

    İsteğe bağlı olarak Azure AD'nin oturum kapatma sayfasının işaret etmek için kullanılabilecek bir SAML oturum kapatma URL'si yapılandırmayı seçebilirsiniz. Bu ayar etkin ve yapılandırılmış olduğunda, kullanıcı artık çalışma alanına oturum kapatma sayfasına yönlendirilirsiniz. Bunun yerine, kullanıcı, oturum kapatma SAML yeniden yönlendirme ayarı eklendi URL'ye yönlendirilirsiniz.

### <a name="configuring-reauthentication-frequency"></a>Yeniden kimlik doğrulaması sıklığını yapılandırma

Her gün, üç gün, haftalık, iki hafta, ay için SAML onay istemi için çalışma alanına yapılandırabilirsiniz ya da asla.

> [!NOTE] 
>SAML onay mobil uygulamaları için en düşük değer, bir hafta için ayarlanır.

Ayrıca, tüm kullanıcılar için düğmeyi kullanarak sıfırlama SAML zorlayabilirsiniz: artık gerekli SAML kimlik doğrulaması tüm kullanıcılar için.


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Facebook test kullanıcı tarafından bir çalışma alanı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı tarafından Facebook çalışma alanında oluşturulur. Facebook ile çalışma alanına tam zamanında sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde hiçbir şey yoktur. Facebook ile çalışma alanında bir kullanıcı yoksa, yeni bir çalışma alanına erişmeye çalıştığında Facebook tarafından oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa başvurun [Facebook istemcisini destek ekibi ile çalışma](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Facebook ile çalışma alanına erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Facebook tarafından çalışma alanı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Facebook ile çalışma alanına**.

    ![Çoklu oturum açmayı yapılandırın](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/workplacebyfacebook-tutorial/tutorial_general_203.png
