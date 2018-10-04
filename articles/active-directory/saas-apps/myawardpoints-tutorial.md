---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle ödül noktaları üst alt/üst Ekibim | Microsoft Docs'
description: Azure Active Directory ve ödül noktaları üst alt/üst Ekibim arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a7a08eed-7a6b-4a83-8f8e-0add6d2fb8cf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: jeedes
ms.openlocfilehash: 479fcc0408021ff63dbcabe3734f60a4ad6d542f
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247763"
---
# <a name="tutorial-azure-active-directory-integration-with-my-award-points-top-subtop-team"></a>Öğretici: Azure Active Directory My ödül noktaları üst alt/üst ekibi ile tümleştirme

Bu öğreticide, My ödül noktaları üst alt/üst ekibi Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

My ödül noktaları üst alt/üst ekibi, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişebilir ödül noktaları üst alt/üst Ekibim için Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan ödül noktaları üst alt/üst Ekibim için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi My ödül noktaları üst alt/üst ekibi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir ödül noktaları üst alt/üst Ekibim çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Ödül noktaları üst alt/üst Ekibim galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-my-award-points-top-subtop-team-from-the-gallery"></a>Ödül noktaları üst alt/üst Ekibim galeri ekleme

Azure AD'de ödül noktaları üst alt/üst Ekibim tümleştirmesini yapılandırmak için ödül noktaları üst alt/üst Ekibim Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ödül noktaları üst alt/üst Ekibim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ödül noktaları üst alt/üst Ekibim**seçin **ödül noktaları üst alt/üst Ekibim** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Ödül noktaları üst alt/üst Takımım sonuç listesinde](./media/myawardpoints-tutorial/tutorial_myawardpoints_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve ödül noktaları üst alt/üst "Britta Simon" adlı bir test kullanıcı tabanlı Ekibim Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne ödül noktaları üst alt/üst Ekibim karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının ödül noktaları üst alt/üst Ekibim ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma My ödül noktaları üst alt/üst ekibi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[My ödül noktaları üst alt/üst takım test kullanıcısı oluşturma](#create-a-my-award-points-top-subtop-team-test-user)**  - ödül noktaları üst alt/üst kullanıcı Azure AD gösterimini bağlı Ekibim Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ödül noktaları üst alt/üst Ekibim uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma My ödül noktaları üst alt/üst ekibi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ödül noktaları üst alt/üst Ekibim** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/myawardpoints-tutorial/tutorial_myawardpoints_samlbase.png)

3. Üzerinde **My ödül noktaları üst alt/üst takım etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Ödül noktaları üst alt/üst takım etki alanı ve URL'ler tek oturum açma Bilgilerim](./media/myawardpoints-tutorial/tutorial_myawardpoints_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://microsoftrr.performnet.com/biwv1auth/Shibboleth.sso/Login?providerId=<SAMLENTITYID>`

    > [!NOTE]
    > Erişmenizi sağlayacak `<SAMLENTITYID>` Bu öğreticide sonraki adımlarda değeri.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/myawardpoints-tutorial/tutorial_myawardpoints_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/myawardpoints-tutorial/tutorial_general_400.png)

6. İçinde **My ödül noktaları üst alt/üst Team yapılandırma** bölümünden **yapılandırma ödül noktaları üst alt/üst Ekibim** oturum açmayı Yapılandır penceresini açın. SAML varlık kimliği kopyalayın **hızlı başvuru** bölüm ve oturum açma URL'si yerine, SAML varlık kimliği değeriyle ekleme `<SAMLENTITYID>` içinde **My ödül noktaları üst alt/üst takım etki alanı ve URL'ler** Azure portalında bölümü.

7. Çoklu oturum açmayı yapılandırma **ödül noktaları üst alt/üst Ekibim** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [ödül noktaları üst alt/üst Ekibim Destek ekibine](mailto:myawardpoints@biworldwide.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/myawardpoints-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/myawardpoints-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/myawardpoints-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/myawardpoints-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-my-award-points-top-subtop-team-test-user"></a>My ödül noktaları üst alt/üst takım test kullanıcısı oluşturma

Bu bölümde, My ödül noktaları üst alt/üst ekibi Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [ödül noktaları üst alt/üst Ekibim Destek ekibine](mailto:myawardpoints@biworldwide.com) ödül noktaları üst alt/üst Ekibim platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, ödül noktaları üst alt/üst Ekibim için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon ödül noktaları üst alt/üst Ekibim için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **ödül noktaları üst alt/üst Ekibim**.

    ![Uygulamalar listesinde ödül noktaları üst alt/üst Ekibim bağlantı](./media/myawardpoints-tutorial/tutorial_myawardpoints_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ödül noktaları üst alt/üst Ekibim kutucuğa tıkladığınızda, otomatik olarak ödül noktaları üst alt/üst Ekibim uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/myawardpoints-tutorial/tutorial_general_01.png
[2]: ./media/myawardpoints-tutorial/tutorial_general_02.png
[3]: ./media/myawardpoints-tutorial/tutorial_general_03.png
[4]: ./media/myawardpoints-tutorial/tutorial_general_04.png

[100]: ./media/myawardpoints-tutorial/tutorial_general_100.png

[200]: ./media/myawardpoints-tutorial/tutorial_general_200.png
[201]: ./media/myawardpoints-tutorial/tutorial_general_201.png
[202]: ./media/myawardpoints-tutorial/tutorial_general_202.png
[203]: ./media/myawardpoints-tutorial/tutorial_general_203.png