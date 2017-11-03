---
title: "Öğretici: İş için Dropbox Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve iş için Dropbox arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Öğretici: İş için Dropbox Azure Active Directory Tümleştirme

Bu öğreticide, Dropbox iş için Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Dropbox iş için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İş Dropbox erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak iş (çoklu oturum açma) için Dropbox için Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

İş için Dropbox ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Dropbox iş çoklu oturum açma için abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. İş için Dropbox Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-dropbox-for-business-from-the-gallery"></a>İş için Dropbox Galeriden ekleme
Azure AD içinde iş için Dropbox tümleştirmesini yapılandırmak için iş için Dropbox Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**İş için Dropbox Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **iş için Dropbox**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Sonuçlar panelinde seçin **iş için Dropbox**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı iş için Dropbox ile test etme

Tekli çalışmaya oturum için Azure AD iş için Dropbox karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve kurumsal Dropbox ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** iş için Dropbox içinde.

Yapılandırma ve Azure AD çoklu oturum açma iş için Dropbox ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Dropbox iş test kullanıcısı için oluşturma](#creating-a-dropbox-for-business-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı iş için Dropbox sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Dropbox iş uygulaması için yapılandırın.

**İş için Dropbox ile Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Dropbox iş için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Üzerinde **iş etki alanı ve URL'ler için Dropbox** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İş Kiracı için Dropbox oturum açma. 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "çoklu oturum açmayı yapılandırın")
   
    b. Sol taraftaki gezinti bölmesinde tıklayın **Yönetici Konsolu**. 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "çoklu oturum açmayı yapılandırın")
   
    c. Üzerinde **Yönetici Konsolu**, tıklatın **kimlik doğrulaması** sol gezinti bölmesinde. 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "çoklu oturum açmayı yapılandırın")
   
    d. İçinde **çoklu oturum açma** bölümünde, select **çoklu oturum açmayı etkinleştir**ve ardından **daha fazla** bu bölümü genişletin.  
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "çoklu oturum açmayı yapılandırın")
   
    e. URL'yi yanına kopyalayın **kullanıcılar uygulamasında oturum açabilir, e-posta adreslerini girerek veya doğrudan gidebilirsiniz**. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. Azure portalındaki içinde **oturum açma URL'si** metin kutusuna, URL yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Bu değer gerçek değeri değil. Kendi tek oturum açma bölümünden alma gerçek oturum açma URL'si ile değeri güncelleştirin. Kişi [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Üzerinde **iş yapılandırması için Dropbox** 'yi tıklatın **iş için yapılandırma Dropbox** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Çoklu oturum açma yapılandırmak için **iş için Dropbox** tarafı, iş Kiracı için açılan kutu içinde gidin **çoklu oturum açma** bölümünü **kimlik doğrulama** sayfasında, gerçekleştirin Aşağıdaki adımlar: 
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "çoklu oturum açmayı yapılandırın")
   
    a. Tıklatın **gerekli**.
   
    b. Azure portalında üzerinde **yapılandırma oturum açma** penceresinde, kopya **SAML çoklu oturum açma hizmet URL'si** değer ve ardından yapıştırın **oturum açma URL'si** metin kutusu.

    c. Tıklatın **Sertifika Seç**ve ardından göz atın, **Base64 ile kodlanmış sertifika dosyası**.

    d. Tıklatın **değişiklikleri kaydetmek** , DropBox iş Kiracı için üzerindeki yapılandırmayı tamamlamak için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>Bir Dropbox iş test kullanıcısı için oluşturma

Bu bölümde, iş için Dropbox Britta Simon adlı bir kullanıcı oluşturulur. İş için Dropbox tam zamanı sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. İş için Dropbox'ın bir kullanıcı zaten mevcut değilse yeni bir iş için Dropbox erişmeyi denediğinde oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekirse başvurun [iş istemci destek ekibi için açılan kutu](https://www.dropbox.com/business/contact) 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, iş için Dropbox için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**İş için Dropbox Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **iş için Dropbox**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iş parçasında Dropbox'ı tıklattığınızda, Dropbox oturum açma sayfasında iş uygulaması almanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

