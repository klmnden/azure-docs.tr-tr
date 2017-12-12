---
title: "Öğretici: Azure Active Directory Tümleştirme ile LogicMonitor | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile LogicMonitor arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d3ef333fcb0c517c44d2d5d83c7f7a51872f1490
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Öğretici: Azure Active Directory Tümleştirme LogicMonitor ile

Bu öğreticide, Azure Active Directory (Azure AD) ile LogicMonitor tümleştirmek öğrenin.

LogicMonitor Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- LogicMonitor erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için LogicMonitor (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme LogicMonitor ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir LogicMonitor çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden LogicMonitor ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-logicmonitor-from-the-gallery"></a>Galeriden LogicMonitor ekleme
Azure AD LogicMonitor tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden LogicMonitor eklemeniz gerekir.

**Galeriden LogicMonitor eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **LogicMonitor**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. Sonuçlar panelinde seçin **LogicMonitor**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı LogicMonitor sınayın.

Tekli çalışmaya oturum için Azure AD LogicMonitor karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının LogicMonitor ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

LogicMonitor içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma LogicMonitor ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[LogicMonitor test kullanıcısı oluşturma](#creating-a-logicmonitor-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı LogicMonitor sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma LogicMonitor uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile LogicMonitor yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **LogicMonitor** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. Üzerinde **LogicMonitor etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.logicmonitor.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<companyname>.logicmonitor.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [LogicMonitor istemci destek ekibi](https://www.logicmonitor.com/contact/) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. Oturum, **LogicMonitor** yönetici olarak şirket site.

7. Üstteki menüde tıklatın **ayarları**.
   
   ![Ayarları](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "ayarları")

8. Sol taraftaki gezinti bat tıklatın **çoklu oturum açma**
   
   ![Çoklu oturum açma](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "çoklu oturum açma")

9. İçinde **çoklu oturum açma (SSO) ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Çoklu oturum açma ayarları](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "tek oturum açma ayarları")
   
   a. Seçin **çoklu oturum açmayı etkinleştir**.

   b. Olarak **varsayılan rol ataması**seçin **salt okunur**.
   
   c. İndirilen meta veri dosyasını Not Defteri'nde açın ve içeriği dosyaya yapıştırın **kimlik sağlayıcısı meta verileri** metin kutusu.
   
   d. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-logicmonitor-test-user"></a>LogicMonitor test kullanıcısı oluşturma

AAD kullanıcıların oturum açabilmesi için bunlar Azure Active Directory kullanıcı adlarını kullanarak LogicMonitor uygulamaya sağlanmalıdır.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. LogicMonitor şirket sitenize yönetici olarak oturum açın.

2. Üstteki menüde tıklatın **ayarları**ve ardından **rol ve kullanıcı**.
   
   ![Rol ve kullanıcı](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "rolleri ve kullanıcılar")

3. **Ekle**'ye tıklayın.

4. İçinde **Hesap Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Hesap Ekle](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Hesap Ekle")
   
   a. Türü **kullanıcıadı**, **e-posta**, **parola**, ve **parolayı yeniden yazın parola** ilgili metin kutularına sağlamayı istediğiniz Azure Active Directory kullanıcı değerleri.
   
   b. Seçin **rolleri**, **izinleri görüntüle**ve **durum**.
   
   c. Tıklatın **gönderme**.

>[!NOTE]
>API tarafından LogicMonitor sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer LogicMonitor kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta LogicMonitor için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**LogicMonitor için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **LogicMonitor**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Erişim paneli LogicMonitor parçasında tıklattığınızda, otomatik olarak LogicMonitor uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

