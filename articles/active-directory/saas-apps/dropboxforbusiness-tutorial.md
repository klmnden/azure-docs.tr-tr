---
title: 'Öğretici: İş için Dropbox Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve iş için Dropbox arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: a0481b2eb688b70d5e56b2b6793b026d6b4c96a2
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220008"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Öğretici: İş için Dropbox Azure Active Directory Tümleştirme

Bu öğreticide, Dropbox iş için Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Dropbox iş için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İş Dropbox erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak iş (çoklu oturum açma) için Dropbox için Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

İş için Dropbox ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Dropbox iş çoklu oturum açma için abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. İş için Dropbox Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-dropbox-for-business-from-the-gallery"></a>İş için Dropbox Galeriden ekleme
Azure AD içinde iş için Dropbox tümleştirmesini yapılandırmak için iş için Dropbox Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**İş için Dropbox Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **iş için Dropbox**seçin **iş için Dropbox** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde iş için açılan kutu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iş için Dropbox ile test etme.

Tekli çalışmaya oturum için Azure AD iş için Dropbox karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve kurumsal Dropbox ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

İş için Dropbox'değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma iş için Dropbox ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Dropbox iş test kullanıcısı için oluşturma](#create-a-dropbox-for-business-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı iş için Dropbox sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Dropbox iş uygulaması için yapılandırın.

**İş için Dropbox ile Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Dropbox iş için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Üzerinde **iş etki alanı ve URL'ler için Dropbox** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Dropbox iş etki alanı ve URL'leri tek oturum açma bilgileri](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.dropbox.com/sso/<id>`

    b. İçinde **tanımlayıcısı** metin kutusuna, bir değer yazın: `Dropbox`

    > [!NOTE] 
    > Önceki oturum açma URL'si değerin gerçek değeri değil. Değer, daha sonra öğreticide açıklandığı gerçek oturum açma URL ile güncelleştirir. Kişi [iş istemci destek ekibi için Dropbox](https://www.dropbox.com/business/contact) değeri alınamıyor. 
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Üzerinde **iş yapılandırması için Dropbox** 'yi tıklatın **iş için yapılandırma Dropbox** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Dropbox iş yapılandırması için](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Çoklu oturum açma yapılandırmak için **iş için Dropbox** tarafı, iş Kiracı için Dropbox gidin.

    a. İş Kiracı için Dropbox oturum açma. 
   
    ![Çoklu oturum açma yapılandırma](./media/dropboxforbusiness-tutorial/ic769509.png "çoklu oturum açmayı yapılandırın")
   
    b. Sol taraftaki gezinti bölmesinde tıklayın **Yönetici Konsolu**. 
   
    ![Çoklu oturum açma yapılandırma](./media/dropboxforbusiness-tutorial/ic769510.png "çoklu oturum açmayı yapılandırın")
   
    c. Üzerinde **Yönetici Konsolu**, tıklatın **kimlik doğrulaması** sol gezinti bölmesinde. 
   
    ![Çoklu oturum açma yapılandırma](./media/dropboxforbusiness-tutorial/ic769511.png "çoklu oturum açmayı yapılandırın")
   
    d. İçinde **çoklu oturum açma** bölümünde, select **çoklu oturum açmayı etkinleştir**ve ardından **daha fazla** bu bölümü genişletin.  
   
    ![Çoklu oturum açma yapılandırma](./media/dropboxforbusiness-tutorial/ic769512.png "çoklu oturum açmayı yapılandırın")
   
    e. URL'yi yanına kopyalayın **kullanıcılar uygulamasında oturum açabilir, e-posta adreslerini girerek veya doğrudan gidebilirsiniz** ve yapıştırın **oturum açma URL'si** , metin kutusuna **iş etki alanı ve URL'leriçinDropbox** Azure Portal'daki bölümü. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/dropboxforbusiness-tutorial/ic769513.png)
    
8. İçinde **çoklu oturum açma** bölümünü **kimlik doğrulaması** sayfasında, aşağıdaki adımları gerçekleştirin: 
   
    ![Çoklu oturum açma yapılandırma](./media/dropboxforbusiness-tutorial/IC769516.png "çoklu oturum açmayı yapılandırın")
   
    a. Tıklatın **gerekli**.
   
    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. Tıklatın **Sertifika Seç**ve ardından göz atın, **Base64 ile kodlanmış sertifika dosyası**.

    d. Tıklatın **değişiklikleri kaydetmek** , DropBox iş Kiracı için üzerindeki yapılandırmayı tamamlamak için.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Bir Dropbox iş test kullanıcısı için oluşturma

Bu bölümde, iş için Dropbox Britta Simon adlı bir kullanıcı oluşturulur. İş için Dropbox tam zamanı sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. İş için Dropbox'ın bir kullanıcı zaten mevcut değilse yeni bir iş için Dropbox erişmeyi denediğinde oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekirse başvurun [iş istemci destek ekibi için açılan kutu](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, iş için Dropbox için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İş için Dropbox Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **iş için Dropbox**.

    ![Uygulamalar listesini iş bağlantısı için açılan kutu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iş parçasında Dropbox'ı tıklattığınızda, Dropbox oturum açma sayfasında iş uygulaması almanız gerekir.
 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

