---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Inkling | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Inkling arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: b0277fbdc599ad700c5ee7ea8de94a63e02a3290
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982890"
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a>Öğretici: Azure Active Directory Tümleştirme Inkling ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Inkling tümleştirmek öğrenin.

Inkling Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Inkling erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Inkling (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Inkling ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Inkling çoklu oturum açma etkin abonelik


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Inkling ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-inkling-from-the-gallery"></a>Galeriden Inkling ekleme
Azure AD Inkling tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Inkling eklemeniz gerekir.

**Galeriden Inkling eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Inkling**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/tutorial_inkling_001.png)

5. Sonuçlar panelinde seçin **Inkling**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Inkling sınayın.

Tekli çalışmaya oturum için Azure AD Inkling karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Inkling ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Inkling içinde.

Yapılandırma ve Azure AD çoklu oturum açma Inkling ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Inkling test kullanıcısı oluşturma](#creating-an-inkling-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Inkling sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma Inkling uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Inkling yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Inkling** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_300.png)
    
3. Üzerinde **Inkling etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_01.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api.inkling.com/saml/v2/metadata/<user-id>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api.inkling.com/saml/v2/acs/<user-id>`

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirmeniz gerekir. Kişi [Inkling destek ekibi](mailto:press@inkling.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_400.png)  

5. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayın ve bir **sona erme tarihi**. Ardından **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_500.png)

6. Üzerinde **SAML imzalama sertifikası** bölümünde, select **yeni sertifika etkin hale getirin** tıklatıp **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_02.png)

7. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklatın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_600.png)

8. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_03.png) 

9. Uygulamanız için yapılandırılmış SSO almak için başvurun [Inkling destek ekibi](mailto:press@inkling.com) ve verin ile indirilen **meta verileri**. 


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 



### <a name="creating-an-inkling-test-user"></a>Bir Inkling test kullanıcısı oluşturma

Bu bölümde, Inkling içinde Britta Simon adlı bir kullanıcı oluşturun. Lütfen çalışmak [Inkling destek ekibi](mailto:press@inkling.com) Inkling platform kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Inkling için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Inkling için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Inkling**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_50.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Inkling parçasında tıklattığınızda, otomatik olarak Inkling uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/inkling-tutorial/tutorial_general_01.png
[2]: ./media/inkling-tutorial/tutorial_general_02.png
[3]: ./media/inkling-tutorial/tutorial_general_03.png
[4]: ./media/inkling-tutorial/tutorial_general_04.png

[100]: ./media/inkling-tutorial/tutorial_general_100.png

[200]: ./media/inkling-tutorial/tutorial_general_200.png
[201]: ./media/inkling-tutorial/tutorial_general_201.png
[202]: ./media/inkling-tutorial/tutorial_general_202.png
[203]: ./media/inkling-tutorial/tutorial_general_203.png