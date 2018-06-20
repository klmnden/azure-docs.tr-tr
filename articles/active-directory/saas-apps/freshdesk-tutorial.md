---
title: 'Öğretici: Azure Active Directory Tümleştirme ile FreshDesk | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile FreshDesk arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 064f122deb6e53a33048d3159941a8b4dc5d0a9a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228899"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Öğretici: Azure Active Directory Tümleştirme FreshDesk ile

Bu öğreticide, Azure Active Directory (Azure AD) ile FreshDesk tümleştirmek öğrenin.

FreshDesk Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FreshDesk erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için FreshDesk (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme FreshDesk ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir FreshDesk çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden FreshDesk ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-freshdesk-from-the-gallery"></a>Galeriden FreshDesk ekleme
Azure AD FreshDesk tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden FreshDesk eklemeniz gerekir.

**Galeriden FreshDesk eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **FreshDesk**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/tutorial_freshdesk_search.png)

5. Sonuçlar panelinde seçin **FreshDesk**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı FreshDesk sınayın.

Tekli çalışmaya oturum için Azure AD FreshDesk karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının FreshDesk ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** FreshDesk içinde.

Yapılandırma ve Azure AD çoklu oturum açma FreshDesk ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[FreshDesk test kullanıcısı oluşturma](#creating-a-freshdesk-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı FreshDesk sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma FreshDesk uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile FreshDesk yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **FreshDesk** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Üzerinde **FreshDesk etki alanı ve URL'leri** bölümünde, lütfen **oturum açma URL'si** olarak: `https://<tenant-name>.freshdesk.com` veya başka bir değer Freshdesk önerdiği.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Lütfen bu gerçek değer olmadığını unutmayın. Değerin gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Kişi [FreshDesk istemci destek ekibi](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) bu değeri alınamıyor.  

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifikayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_general_400.png)

6. Üzerinde **FreshDesk yapılandırma** 'yi tıklatın **yapılandırma FreshDesk** yapılandırma oturum açma penceresini açın. SAML çoklu oturum açma hizmet URL'si ve Sign-Out URL'den kopyalama **hızlı başvuru** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. Farklı web tarayıcısı penceresinde Freshdesk şirket sitenize yönetici olarak oturum açın.

8. Üstteki menüde tıklatın **yönetici**.
   
   ![Yönetici](./media/freshdesk-tutorial/IC776768.png "yönetici")

9. İçinde **genel ayarları** sekmesini tıklatın, **güvenlik**.
   
   ![Güvenlik](./media/freshdesk-tutorial/IC776769.png "güvenlik")

10. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı](./media/freshdesk-tutorial/IC776770.png "çoklu oturum açmayı")
   
    a. İçin **çoklu oturum açma (SSO)** seçin **üzerinde**.

    b. Seçin **SAML SSO**.

    c. Tür **SAML çoklu oturum açma hizmet URL'si** Azure portalına kopyalanan **SAML oturum açma URL'si** metin kutusu.

    d. Tür **Sign-Out URL** Azure portalına kopyalanan **oturum kapatma URL'si** metin kutusu.

    e. Kopya **parmak izi** değer Azure Portalı'ndan indirilen sertifikasından ve yapıştırın **güvenlik sertifika parmak izi** metin kutusu.  
 
    >[!TIP]
    >Daha fazla ayrıntı için bkz: [bir sertifikanın parmak izi değerini almak nasıl](http://youtu.be/YKQF266SAxI). 
    
    f. **Kaydet**’e tıklayın.


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-freshdesk-test-user"></a>FreshDesk test kullanıcısı oluşturma

Azure AD kullanıcıların FreshDesk oturum etkinleştirmek için bunların FreshDesk sağlanmalıdır.  
FreshDesk söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Freshdesk** Kiracı.
2. Üstteki menüde tıklatın **yönetici**.
   
   ![Yönetici](./media/freshdesk-tutorial/IC776772.png "yönetici")

3. İçinde **genel ayarları** sekmesini tıklatın, **aracıları**.
   
   ![Aracıları](./media/freshdesk-tutorial/IC776773.png "aracıları")

4. Tıklatın **yeni aracı**.
   
    ![Yeni aracı](./media/freshdesk-tutorial/IC776774.png "yeni aracı")

5. Aracı bilgileri iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![Aracı bilgilerini](./media/freshdesk-tutorial/IC776775.png "aracısı bilgileri")
   
   a. İçinde **tam adı** metin kutusuna, sağlamak istediğiniz Azure AD hesabının adını yazın.

   b. İçinde **e-posta** metin kutusuna, sağlamak istediğiniz Azure AD hesabının e-posta adresini yazın Azure AD.

   c. İçinde **başlık** metin kutusuna, sağlamak istediğiniz Azure AD hesabının adını yazın.

   d. Seçin **aracıları rolü**ve ardından **atamak**.
       
   e. **Kaydet**’e tıklayın.     
   
    >[!NOTE]
    >Azure AD hesap sahibi etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. 
    > 
    
    >[!NOTE]
    >API sağlama AAD kullanıcı hesaplarına Freshdesk tarafından sağlanan veya herhangi diğer Freshdesk kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. FreshDesk için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta kutusuna kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**FreshDesk için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **FreshDesk**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli FreshDesk parçasında tıklattığınızda açan FreshDesk uygulamanıza oturum açma sayfasına almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png

