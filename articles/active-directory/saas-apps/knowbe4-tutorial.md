---
title: 'Öğretici: Azure Active Directory Tümleştirme KnowBe4 güvenlik tanıma eğitimi | Microsoft Docs'
description: Azure Active Directory ve KnowBe4 güvenlik tanıma eğitim arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: c27af4e7bc88f24b76310336859740d8795f7cbe
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449282"
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Öğretici: Azure Active Directory Tümleştirme KnowBe4 güvenlik tanıma eğitimi

Bu öğreticide, Azure Active Directory (Azure AD) ile KnowBe4 güvenlik tanıma eğitim tümleştirme konusunda bilgi edinin.

Azure AD ile KnowBe4 güvenlik tanıma eğitim tümleştirme ile aşağıdaki avantajları sağlar:

- KnowBe4 güvenlik tanıma eğitim erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan KnowBe4 güvenlik tanıma eğitimi (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi KnowBe4 güvenlik tanıma eğitimle yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik KnowBe4 güvenlik tanıma eğitim çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden KnowBe4 güvenlik tanıma eğitim ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>Galeriden KnowBe4 güvenlik tanıma eğitim ekleme
Azure AD'de KnowBe4 güvenlik tanıma eğitim tümleştirmesini yapılandırmak için KnowBe4 güvenlik tanıma eğitim Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden KnowBe4 güvenlik tanıma eğitim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **KnowBe4 güvenlik tanıma eğitim**seçin **KnowBe4 güvenlik tanıma eğitim** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde KnowBe4 güvenlik tanıma eğitim](./media/knowbe4-tutorial/tutorial_knowbe4_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma "Britta Simon" adlı bir test kullanıcı tabanlı eğitim test.

Tek iş için oturum açma için Azure AD ne KnowBe4 güvenlik tanıma eğitim karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı KnowBe4 güvenlik tanıma eğitim arasında bir bağlantı ilişki kurulması gerekir.

Değerini KnowBe4 güvenlik tanıma eğitim atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma eğitim ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[KnowBe4 güvenlik tanıma eğitim test kullanıcısı oluşturma](#create-a-knowbe4-security-awareness-training-test-user)**  - kullanıcı Azure AD gösterimini bağlı KnowBe4 güvenlik tanıma eğitim Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve KnowBe4 güvenlik tanıma eğitim uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma eğitimlerle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **KnowBe4 güvenlik tanıma eğitim** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/knowbe4-tutorial/tutorial_knowbe4_samlbase.png)

1. Üzerinde **KnowBe4 güvenlik tanıma eğitim etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![KnowBe4 güvenlik tanıma eğitim etki alanı ve URL'ler tek oturum açma bilgileri](./media/knowbe4-tutorial/tutorial_knowbe4_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [KnowBe4 güvenlik tanıma eğitim istemci Destek ekibine](mailto:support@KnowBe4.com) bu değeri alınamıyor. 

    b. İçinde **tanımlayıcı** metin dizesi değeri yazın: `KnowBe4`

    > [!NOTE]
    >Bu büyük küçük harfe duyarlıdır

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/knowbe4-tutorial/tutorial_knowbe4_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/knowbe4-tutorial/tutorial_general_400.png)

1. Üzerinde **KnowBe4 güvenlik tanıma eğitim Yapılandırması** bölümünde **yapılandırma KnowBe4 güvenlik tanıma eğitim** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![KnowBe4 güvenlik tanıma eğitim yapılandırması](./media/knowbe4-tutorial/tutorial_knowbe4_configure.png) 

1. Çoklu oturum açmayı yapılandırma **KnowBe4 güvenlik tanıma eğitim** tarafı, indirilen göndermek için ihtiyacınız **sertifika (ham)**, **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma Hizmet URL'si** için [KnowBe4 güvenlik tanıma eğitim istemci Destek ekibine](mailto:support@KnowBe4.com).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/knowbe4-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/knowbe4-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/knowbe4-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/knowbe4-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-knowbe4-security-awareness-training-test-user"></a>KnowBe4 güvenlik tanıma eğitim test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon KnowBe4 güvenlik tanıma eğitim adlı bir kullanıcı oluşturmaktır. KnowBe4 güvenlik tanıma eğitim tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa KnowBe4 güvenlik tanıma eğitim erişme denemesi sırasında oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [KnowBe4 güvenlik tanıma eğitimi destek ekibi](mailto:support@KnowBe4.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, KnowBe4 güvenlik tanıma eğitim için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon KnowBe4 güvenlik tanıma eğitimine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **KnowBe4 güvenlik tanıma eğitim**.

    ![Uygulamalar listesinde KnowBe4 güvenlik tanıma eğitim bağlantı](./media/knowbe4-tutorial/tutorial_knowbe4_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.
  
Erişim panelinde KnowBe4 güvenlik tanıma eğitim kutucuğa tıkladığınızda, otomatik olarak KnowBe4 güvenlik tanıma eğitim uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/knowbe4-tutorial/tutorial_general_04.png

[100]: ./media/knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/knowbe4-tutorial/tutorial_general_201.png
[202]: ./media/knowbe4-tutorial/tutorial_general_202.png
[203]: ./media/knowbe4-tutorial/tutorial_general_203.png

