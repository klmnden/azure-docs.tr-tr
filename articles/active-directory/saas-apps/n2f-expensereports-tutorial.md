---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle N2F - gider raporları | Microsoft Docs'
description: Azure Active Directory ve N2F - arasında çoklu oturum açmayı yapılandırma, Gider raporlarını öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 27fb299bc3bbbbf75bdf40ae02eac627763ce6d4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006836"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Öğretici: Azure Active Directory tümleştirmesiyle N2F - gider bildirir.

Bu öğreticide, Azure Active Directory (Azure AD) ile gider raporlarını N2F - tümleştirmek nasıl öğrenin.

Tümleştirme N2F - Azure AD ile gider raporlarını ile aşağıdaki avantajları sağlar:

- N2F - erişimi, Azure AD'de denetleyebilirsiniz gider bildirir.
- Otomatik olarak imzalanan N2F - gider raporlarını (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi N2F - gider raporlarını yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir N2F - gider raporlarını tek oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. N2F - gider raporlarını galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>N2F - gider raporlarını galeri ekleme

N2F - eklemenize gerek N2F - Azure AD'de, Gider raporlarını tümleştirmesini yapılandırmak için yönetilen SaaS listenizden galerisinden raporlar gider.

**N2F - galerisinden, Gider raporlarını eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde ** [Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **N2F - gider raporlarını**seçin **N2F - gider raporlarını** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![N2F - gider raporlarını sonuçları listesi](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve test Azure AD çoklu oturum açma ile N2F - gider raporlarını bir test kullanıcı tabanlı "Britta Simon" denir.

Tek çalışmak, oturum için Azure AD içinde N2F karşılığı kullanıcının bilmesi gerekir - gider raporlarını, Azure AD'de bir kullanıcı için olan. Diğer bir deyişle, bir Azure AD kullanıcısının N2F - ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekiyorsa gider bildirir.

Yapılandırma ve Azure AD çoklu oturum açma ile N2F - gider raporlarını, test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on) ** - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) ** - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir N2F Oluştur - gider raporlarını kullanıcı test](#create-a-n2f---expense-reports-test-use) ** - N2F içinde bir karşılığı Britta simon'un sağlamak için - gider kullanıcı Azure AD gösterimini bağlantılı raporlar.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) ** - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on) ** - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, N2F içinde yapılandırma - gider uygulama bildirir.

**Azure AD yapılandırmak için çoklu oturum açma ile N2F - gider raporlarını, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **N2F - gider raporlarını** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_samlbase.png)

3. Üzerinde **N2F - gider raporlarını etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, kullanıcı uygulamaya zaten ile önceden tümleştirilmiş olduğu gibi tüm adımları gerçekleştirmek sahip değil Azure.

    ![Çoklu oturum açma bilgileri N2F - gider raporlarını etki alanı ve URL'ler](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url1.png)

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açma bilgileri N2F - gider raporlarını etki alanı ve URL'ler](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://www.n2f.com/app/`

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_certificate.png)

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/n2f-expensereports-tutorial/tutorial_general_400.png)

7. Üzerinde **N2F - gider raporlarını yapılandırma** bölümünde **yapılandırma N2F - gider raporlarını** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_configure.png)

8. Farklı bir web tarayıcı penceresinde N2F - gider raporlarını şirket site yönetici olarak oturum açın.

9. Tıklayarak **ayarları** seçip **Gelişmiş ayarlar** açılır listeden.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure1.png)

10. Seçin **hesap ayarları** sekmesi.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure2.png)

11. Seçin **kimlik doğrulaması** seçip **+ kimlik doğrulama yöntemi Ekle** sekmesi.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure3.png)

12. Seçin **SAML Microsoft Office 365** kimlik doğrulama yöntemi olarak.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure4.png)

13. Üzerinde **kimlik doğrulama yöntemi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure5.png)

    a. İçinde **varlık kimliği** metin kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.

    b. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/n2f-expensereports-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/n2f-expensereports-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/n2f-expensereports-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/n2f-expensereports-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-n2f---expense-reports-test-user"></a>Bir N2F oluşturma - kullanıcı gider raporlarını test

Azure AD kullanıcılarının N2F - gider raporlarını, oturum açmayı etkinleştirmek için bunların N2F - gider raporlarını sağlanması gerekir. N2F - gider raporlarını, söz konusu olduğunda sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. N2F - gider raporlarını şirket site yönetici olarak oturum açın.

2. Tıklayarak **ayarları** seçip **Gelişmiş ayarlar** açılır listeden.

   ![N2F - kullanıcı gider Ekle](./media/n2f-expensereports-tutorial/configure1.png)

3. Seçin **kullanıcılar** sol gezinti bölmesi sekmesinden.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user1.png)

4. Seçin **+ yeni kullanıcı** sekmesi.

   ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user2.png)

5. Üzerinde **kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user3.png)

    a. İçinde **e-posta adresi** metin gibi kullanıcı e-posta adresini girin ** brittasimon@contoso.com **.

    b. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    c. İçinde **adı** metin gibi kullanıcı adını girin **BrittaSimon**.

    d. Seçin **doğrudan Yöneticisi (N + 1) rolü,**, ve **bölme** kuruluş ihtiyacınıza göre.

    e. Tıklayın **doğrula ve gönderme davet**.

    > [!NOTE]
    > Kullanıcı eklenirken herhangi bir sorunla karşılaşıyorsanız Lütfen başvurun [N2F - gider raporlarını Destek ekibine](mailto:support@n2f.com)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için N2F erişim vererek Britta Simon enable - gider bildirir.

![Kullanıcı rolü atayın][200]

**Britta Simon N2F - gider raporlarını atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **N2F - gider raporlarını**.

    ![-N2F gider bağlantıyı uygulamalar listesini bildirir.](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

N2F tıklayın - erişim Paneli'nde gider raporlarını kutucuğuna yaptığınızda, otomatik olarak imzalanan, N2F için açma - uygulama gider raporlarını.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/n2f-expensereports-tutorial/tutorial_general_01.png
[2]: ./media/n2f-expensereports-tutorial/tutorial_general_02.png
[3]: ./media/n2f-expensereports-tutorial/tutorial_general_03.png
[4]: ./media/n2f-expensereports-tutorial/tutorial_general_04.png

[100]: ./media/n2f-expensereports-tutorial/tutorial_general_100.png

[200]: ./media/n2f-expensereports-tutorial/tutorial_general_200.png
[201]: ./media/n2f-expensereports-tutorial/tutorial_general_201.png
[202]: ./media/n2f-expensereports-tutorial/tutorial_general_202.png
[203]: ./media/n2f-expensereports-tutorial/tutorial_general_203.png

