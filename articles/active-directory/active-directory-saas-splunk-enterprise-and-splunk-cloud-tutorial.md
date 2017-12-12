---
title: "Öğretici: Azure Active Directory Tümleştirme Splunk Kurumsal ve Splunk bulut ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Splunk Kurumsal Splunk bulut arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 82a7f8ec5be62848b45b23fce1e012aaad3b1798
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Splunk Kurumsal ve Splunk bulut ile

Bu öğreticide, Splunk Kurumsal ve Splunk bulut Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Splunk Kurumsal ve Splunk bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Splunk Kurumsal ve Splunk bulut erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Splunk Kurumsal ve Splunk bulut çoklu oturum açma (SSO) ile Azure AD hesaplarına için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Klasik Azure portalı Yönet

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Splunk Enterprise ve Splunk bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik Splunk Enterprise veya Splunk bulut SSO etkin


>[!NOTE]
>Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.
>

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Splunk Kurumsal ve Splunk bulut ekleme
2. Yapılandırma ve Azure AD SSO test etme


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Galeriden Splunk Kurumsal ve Splunk bulut ekleme
Azure AD Splunk Enterprise ve Splunk bulut tümleştirilmesi yapılandırmak için Splunk Kurumsal ve Splunk bulut Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Splunk Kurumsal ve Splunk bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.

    ![Active Directory][1]

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.

    ![Uygulamalar][2]

4. Tıklatın **Ekle** sayfanın sonundaki.

    ![Uygulamalar][3]

5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.

    ![Uygulamalar][4]

6. Arama kutusuna **Splunk Enterprise veya Splunk bulut**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. Sonuçlar bölmesinde seçin **Splunk Kurumsal ve Splunk bulut**ve ardından **tam** uygulama eklemek için.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Splunk kuruluş ile test etme ve "Britta Simon" adlı bir test kullanıcı Splunk bulut tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Splunk Kurumsal ve Splunk bulut bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Splunk Kurumsal ve Splunk bulut arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Splunk Kurumsal ve Splunk bulut.

Yapılandırma ve Azure AD çoklu oturum açma Splunk Kurumsal ve Splunk bulut ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Splunk Kurumsal ve Splunk bulut test kullanıcısı oluşturma](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  - karşılık gelen Britta Simon Splunk kuruluştaki ve Azure AD gösterimini her için bağlantılı Splunk bulut sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Klasik portalda Azure AD SSO'yu etkinleştirmek ve SSO Splunk Kurumsal ve Splunk bulut uygulamanızda yapılandırın.


**Azure AD çoklu oturum açma Splunk Kurumsal ve Splunk bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Portalı'ndaki üzerinde **Splunk Kurumsal ve Splunk bulut** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **yapılandırma çoklu oturum açma** iletişim.
     
    ![Çoklu oturum açmayı yapılandırın][6] 

2. Üzerinde **Splunk Kurumsal ve Splunk bulut oturum açmaya kullanıcıların nasıl istiyorsunuz** sayfasında, **Azure AD çoklu oturum açma**ve ardından **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Üzerinde **uygulama ayarlarını yapılandır** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. İçinde **oturum üzerinde URL'si** metin kutusuna, türü URL'yi, kullanıcılarınıza oturum açma uygulamanıza şu biçimi kullanarak Splunk Kurumsal ve Splunk bulut tarafından kullanılan:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. İçinde **tanımlayıcısı** metin kutusuna, Splunk sunucunuzun URL'sini yazın.
  3. İçinde **yanıt URL'si** metin kutusuna, aşağıdaki desende URL'yi yazın:`https://<splunkserver>/saml/acs`
  4. **İleri**’ye tıklayın.
 
4. Üzerinde **çoklu oturum açma Splunk Enterprise ve Splunk bulut yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Tıklatın **karşıdan meta veri**ve ardından dosyayı bilgisayarınıza kaydedin.
  2. **İleri**’ye tıklayın.

5. Uygulamanız için yapılandırılmış SSO almak için Splunk Enterprise ve Splunk bulut Destek ekibine başvurun ve aşağıdaki verin:

    * İndirilen **federaton meta verileri**
6. Klasik Portalı'ndaki tek oturum açma yapılandırması onay seçin ve ardından **sonraki**.
    
    ![Azure AD çoklu oturum açma][10]

7. Üzerinde **tek oturum açma onay** sayfasında, **tam**.  
 
    ![Azure AD çoklu oturum açma][11]

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, bir test kullanıcı Britta Simon adlı Klasik portalda oluşturun.

![Azure AD Kullanıcı oluşturma][20]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Klasik Azure portalı**, sol gezinti bölmesinde tıklatın **Active Directory**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.

3. Üstteki menüde kullanıcıların listesini görüntülemek için tıklatın **kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. Açmak için **Kullanıcı Ekle** iletişim kutusunda, araç çubuğunda alt tıklatın **Kullanıcı Ekle**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Üzerinde **bu kullanıcı hakkında bize** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Kullanıcı türü olarak, kuruluşunuzdaki yeni kullanıcı seçin.
  2. Kullanıcı adı **textbox**, türü **BrittaSimon**.
  3. **İleri**’ye tıklayın.

6.  Üzerinde **kullanıcı profili** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. İçinde **ad** metin kutusuna, türü **Britta**.  
  2. İçinde **Soyadı** metin kutusuna, türü, **Simon**.
  3. İçinde **görünen adı** metin kutusuna, türü **Britta Simon**.
  4. İçinde **rol** listesinde **kullanıcı**.
  5. **İleri**’ye tıklayın.

7. Üzerinde **Get geçici parola** iletişim sayfasında, tıklatın **oluşturma**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Üzerinde **Get geçici parola** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Değerini yazmak **yeni parola**.
  2. **Tamamla**’ya tıklayın.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Splunk Enterprise ve Splunk bulut test kullanıcısı oluşturma

Bu bölümde, Britta Simon Splunk kuruluştaki ve Splunk bulut adlı bir kullanıcı oluşturun. Lütfen Splunk Kurumsal ve Splunk bulut platform kullanıcıları eklemek için Splunk Enterprise ve Splunk bulut destek ekibi ile çalışın.


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure Splunk Kurumsal ve Splunk bulut kendi erişim verilmesi SSOy kullanmak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Splunk Enterprise ve Splunk bulut atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik portalı üzerinde dizin görünümünde uygulamaları görünümü açmak için tıklatın **uygulamaları** üst menüde.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Splunk Kurumsal ve Splunk bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. Üstteki menüde tıklatın **kullanıcılar**.

    ![Kullanıcı atama][203]

4. Kullanıcılar listesinden seçin **Britta Simon**.

5. Araç çubuğunda alt tıklatın **atamak**.

    ![Kullanıcı atama][205]

### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, Azure AD erişim paneli kullanarak SSOonfiguration test edin.

Splunk Enterprise'ı tıklatın ve Splunk bulut döşeme erişim panelinde, otomatik olarak Splunk Kurumsal ve Splunk bulut uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
