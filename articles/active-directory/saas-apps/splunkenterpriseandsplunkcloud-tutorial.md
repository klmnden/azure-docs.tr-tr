---
title: 'Öğretici: Azure Active Directory Tümleştirme Splunk Kurumsal ve Splunk bulut ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Splunk Kurumsal Splunk bulut arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 445d891e767bd25eb885abcb3bd3955645ff8145
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36222238"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Splunk Kurumsal ve Splunk bulut ile

Bu öğreticide, Splunk Kurumsal ve Splunk bulut Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Splunk Kurumsal ve Splunk bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Splunk Kurumsal ve Splunk bulut erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Splunk Enterprise ve Splunk bulut (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Splunk Enterprise ve Splunk bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Splunk Kurumsal ve Splunk bulut çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Splunk Kurumsal ve Splunk bulut ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Galeriden Splunk Kurumsal ve Splunk bulut ekleme
Azure AD Splunk Enterprise ve Splunk bulut tümleştirilmesi yapılandırmak için Splunk Kurumsal ve Splunk bulut Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Splunk Kurumsal ve Splunk bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Splunk Kurumsal ve Splunk bulut**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

5. Sonuçlar panelinde seçin **Splunk Kurumsal ve Splunk bulut**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Splunk kuruluş ile test etme ve "Britta Simon." olarak adlandırılan bir test kullanıcı Splunk bulut tabanlı

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Splunk Kurumsal ve Splunk bulut bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Splunk Kurumsal ve Splunk bulut arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Splunk Enterprise ve Splunk bulut Ata **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Splunk Kurumsal ve Splunk bulut ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Splunk Kurumsal ve Splunk bulut test kullanıcısı oluşturma](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  - karşılık gelen Britta Simon Splunk kuruluştaki ve kullanıcı Azure AD gösterimini bağlı Splunk bulut sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Splunk Kurumsal ve Splunk bulut uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Splunk Kurumsal ve Splunk bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Splunk Kurumsal ve Splunk bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

3. Üzerinde **Splunk Enterprise ve Splunk bulut etki alanı ve URL'leri** uygulamada yapılandırmak istiyorsanız, bölüm **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<splunkserverUrl>/en-US/app/launcher/home`
    
    b. İçinde **tanımlayıcısı** metin kutusuna, Splunk sunucunuzun URL'sini yazın.

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<splunkserver>/saml/acs`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Burada dizesinin benzersiz değeri tanımlayıcıda kullanmanızı öneririz. Kişi [Splunk Enterprise ve Splunk bulut istemci destek ekibi](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

6. Çoklu oturum açma yapılandırmak için **Splunk Kurumsal ve Splunk bulut** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [Splunk Kurumsal ve Splunk bulut destek ekibi](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Splunk Kurumsal ve Splunk bulut test kullanıcısı oluşturma

Bu bölümde, Britta Simon Splunk kuruluştaki ve Splunk bulut adlı bir kullanıcı oluşturun. Çalışmak [Splunk Kurumsal ve Splunk bulut destek ekibi](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) Splunk Kurumsal ve Splunk bulut platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Splunk Kurumsal ve Splunk bulut erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Splunk Enterprise ve Splunk bulut atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Splunk Kurumsal ve Splunk bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, Azure AD erişim paneli kullanarak SSOonfiguration test edin.

Splunk Enterprise'ı tıklatın ve Splunk bulut döşeme erişim panelinde, otomatik olarak Splunk Kurumsal ve Splunk bulut uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

