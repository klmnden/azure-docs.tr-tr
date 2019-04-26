---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Shmoop için okullar | Microsoft Docs'
description: Azure Active Directory ve Shmoop için okullar arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4091b20e97ca76629260a7420beecb77412b0d39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60310867"
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Öğretici: Azure Active Directory için Shmoop okullar ile tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Shmoop için okullar tümleştirme konusunda bilgi edinin.

Azure AD ile Shmoop için okullar tümleştirme ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Shmoop okullara için Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Shmoop okullara için kendi Azure AD hesapları ile oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için Shmoop okullar ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik için Shmoop okullar çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için önerilir:

- Yalnızca gerekli olduğunda, üretim ortamınızın kullanma.
- Başlarken bir [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) bir Azure AD deneme ortam zaten yoksa.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Galeriden Shmoop için okullar ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Galeriden Shmoop için okullar Ekle
Azure AD'de Shmoop için okullar tümleştirmesini yapılandırmak için Shmoop için okullar Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Shmoop için okullar eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Shmoop için okullar**. Seçin **Shmoop için okullar** sonuçlardan seçip **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Shmoop için okullar](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Shmoop için "Britta Simon." adlı bir test kullanıcı tabanlı okullar ile test etme

Tek iş için oturum açma için Azure AD için Shmoop okullar karşılık gelen kullanıcı Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı için Shmoop okullar ilgili kullanıcı arasında bir bağlantı kurmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma için Shmoop okullar ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Shmoop için okullar test kullanıcısı oluşturma](#create-a-shmoop-for-schools-test-user) bir karşılığı Britta simon'un Shmoop için kullanıcının Azure AD gösterimini bağlı okullar sağlamak için.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma için Shmoop okullar uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma için Shmoop okullar ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Shmoop için okullar** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda aşağı açılan menüsünün altında **çoklu oturum açma modunu**seçin **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. İçinde **Shmoop için okullar etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. İçinde **oturum açma URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. İçinde **tanımlayıcı** kutusuna aşağıdaki desene sahip bir URL yazın: `https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [okullar için Shmoop istemci Destek ekibine](mailto:support@shmoop.com) bu değerleri almak için. 
 
4. Shmoop için okullar uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsünde, onaylamalar yapılandırma işlemi gösterilmektedir:

    ![Çoklu oturum açmayı yapılandırma](./media/shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Okul için Shmoop kullanıcılar için iki rollerini destekler: **Öğretmen** ve **Öğrenci**. Böylece kullanıcılar uygun roller atanabilir Azure AD'de bu rolleri ayarlama. Azure AD'de rolleri yapılandırmak nasıl anlamak için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).
    
5. İçinde **kullanıcı öznitelikleri** konusundaki **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırabilirsiniz.  Ardından aşağıdaki adımları uygulayın:

    | Öznitelik adı | Öznitelik değeri |
    | -------------- | --------------- |
    | rol           | User.assignedroles |

    a. Açmak için **öznitelik Ekle** iletişim kutusunda **eklemek agentconfigutil**.
    
    ![Çoklu oturum açmayı yapılandırma](./media/shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırma](./media/shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.

    d. Bırakın **Namespace** kutusunu boş.
    
    e. **Tamam**’ı seçin.

6. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/shmoopforschools-tutorial/tutorial_general_400.png)

7. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_certificate.png)

8. Çoklu oturum açmayı yapılandırma **Shmoop için okullar** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [Shmoop için okullar Destek ekibine](mailto:support@shmoop.com).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/shmoopforschools-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/shmoopforschools-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/shmoopforschools-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/shmoopforschools-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Shmoop için okullar test kullanıcısı oluşturma

Bu bölümün amacı, okullar için Shmoop Britta Simon adlı bir kullanıcı oluşturmaktır. Shmoop için okullar tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı henüz mevcut değilse Shmoop için okullar erişim girişimi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Shmoop için okullar Destek ekibine](mailto:support@shmoop.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Shmoop okullar için erişim izni verdiğinizde Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için Shmoop okullar atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümünü açın. Ardından **kurumsal uygulamalar** görünümündeki directory.  Ardından, **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Shmoop için okullar**.

    ![Uygulamalar listesinde Shmoop için okullar bağlantı](./media/shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Seçin **Ekle** düğmesi. Ardından **atama Ekle** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi. 

7. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Shmoop için okullar** oturumunuz otomatik olarak Shmoop için okullar uygulamanıza erişim Paneli'nde; kutucuk.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme öğrenmek için öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/shmoopforschools-tutorial/tutorial_general_203.png

