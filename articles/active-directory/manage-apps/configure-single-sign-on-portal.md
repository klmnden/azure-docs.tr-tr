---
title: Çoklu oturum açmayı yapılandırma - Azure Active Directory | Microsoft Docs
description: Bu öğreticide Azure portal kullanılarak bir uygulama için Azure Active Directory (Azure AD) ile SAML tabanlı çoklu oturum açma yapılandırması yapılmaktadır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 634c1d05847f3d4d7b7168d484cd16bf8e351b27
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723930"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Öğretici: SAML tabanlı çoklu oturum açma bir uygulama için Azure Active Directory ile yapılandırın.

Bu öğreticide [Azure portal](https://portal.azure.com) kullanılarak bir uygulama için Azure Active Directory (Azure AD) ile SAML tabanlı çoklu oturum açma yapılandırması yapılmaktadır. Bu öğreticiyi kullanın, bir [uygulamaya özgü öğretici](../saas-apps/tutorial-list.md) kullanılamaz.

Bu öğreticide Azure portalda aşağıdaki işlemler gerçekleştirilmektedir:

> [!div class="checklist"]
> * SAML tabanlı çoklu oturum açma modunu seçme
> * Uygulamaya özgü etki alanını ve URL'leri yapılandırma
> * Kullanıcı özniteliklerini yapılandırma
> * SAML imzalama sertifikası oluşturma
> * Uygulamaya kullanıcı atama
> * Uygulamada SAML tabanlı oturum açma yapılandırması gerçekleştirme
> * SAML ayarlarını test etme

## <a name="before-you-begin"></a>Başlamadan önce

1. Azure AD kiracınız için uygulama eklenmemişse, bkz. [hızlı başlangıç: Azure AD kiracınız için uygulama ekleme](add-application-portal.md).
1. Bilgi açıklanan için uygulama satıcınıza başvurun [temel SAML seçenekleri](#configure-basic-saml-options).
1. Bu öğreticideki adımları test etmek için üretim dışı ortamda kullanın. Üretim ortamı dışında bir Azure AD ortamınız yoksa [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilirsiniz.
1. Oturum [Azure portalında](https://portal.azure.com) bir bulut uygulaması Yöneticisi veya Azure AD kiracınız için uygulama yönetici olarak.

## <a name="select-a-single-sign-on-mode"></a>Çoklu oturum açma modunu seçme

Azure AD kiracınızla bir uygulamayı ekledikten sonra uygulama için çoklu oturum açmayı yapılandırmaya hazırsınız.

Çoklu oturum açma ayarlarını açmak için:

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti panelinde seçin **Azure Active Directory**.
1. Altında **Yönet** içinde **Azure Active Directory** görüntülenen gezinti panelini seçin **kurumsal uygulamalar**. Uygulamaları Azure AD kiracınızda rastgele oluşturulmuş bir örnek görünür.
1. İçinde **uygulama türü** menüsünde **tüm uygulamaları**ve ardından **Uygula**.
1. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamanın adını seçin. Örneğin, girdiğiniz **GitHub test** eklediğiniz uygulama yapılandırmak için [uygulama ekleme](add-application-portal.md) hızlı başlangıç.  

   ![Uygulama arama çubuğunu gösteren ekran görüntüsü](media/configure-single-sign-on-portal/azure-portal-application-search.png)

1. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.
1. Altında **Yönet** bölümünden **çoklu oturum açma**.
1. Seçin **SAML** çoklu oturum açmayı yapılandırmak için. **Yukarı çoklu oturum açma SAML - Preview ile ayarlanmış** sayfası görüntülenir.

## <a name="configure-basic-saml-options"></a>Temel SAML seçeneklerini yapılandırın

Etki alanını ve URL'leri yapılandırmak için:

1. Uygulama satıcısıyla iletişime geçerek aşağıdaki ayarlar için doğru bilgileri alın:

    | Yapılandırma ayarı | SP ile başlatılan | idP ile başlatılan | Açıklama |
    |:--|:--|:--|:--|
    | Tanımlayıcı (Varlık Kimliği) | Bazı uygulamalar için gereklidir | Bazı uygulamalar için gereklidir | Çoklu oturum açma yapılandırmasının yapıldığı uygulamayı benzersiz olarak tanımlar. Azure AD tanımlayıcısı SAML belirteç hedef kitlesi parametre olarak uygulamasına gönderir. Bunu doğrulamak için uygulamayı bekleniyor. Bu değer ayrıca uygulama tarafından sağlanan SAML meta verilerinde Varlık Kimliği olarak da görünür.|
    | Yanıt URL'si | İsteğe Bağlı | Gerekli | Uygulamanın SAML belirtecini almayı beklediği konumu belirtir. Yanıt URL'si, Onay Belgesi Tüketici Hizmeti (ACS) URL'si olarak da bilinir. |
    | Oturum açma URL'si | Gerekli | Belirtmeyin | Kullanıcı bu URL'yi açtığında hizmet sağlayıcısı kimlik doğrulaması ve oturum açma için Azure AD'ye yönlendirir. Azure AD, Office 365 veya Azure AD erişim paneli uygulamayı başlatmak için URL'yi kullanır. Boş bırakıldığında, Azure AD çoklu oturum açma kullanıcı uygulamayı başlattığında başlatmak için kimlik sağlayıcısını kullanır.|
    | Geçiş Durumu | İsteğe Bağlı | İsteğe Bağlı | Uygulamaya kimlik doğrulaması tamamlandıktan sonra kullanıcının yönlendirileceği yeri belirtir. Genellikle uygulama için geçerli bir URL değerdir. Ancak, bazı uygulamalar farklı bu alanı kullanın. Daha fazla bilgi için uygulama satıcısına danışın.
    | Oturum Kapatma URL'si | İsteğe Bağlı | İsteğe Bağlı | Uygulamaya SAML oturum kapatma yanıtları göndermek için kullanılır.

1. Temel SAML yapılandırma seçeneklerini düzenlemek için seçin **Düzenle** sağ üst köşesindeki simgeyi (Kalem) **temel SAML yapılandırma** bölümü.

     ![Temel SAML yapılandırma seçenekleri Düzenle](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

1. Adım 1'de uygulama satıcısı tarafından sağlanan bilgileri sayfasında ilgili alanlara girin.
1. Sayfanın üst kısmında seçin **Kaydet**.

## <a name="configure-user-attributes-and-claims"></a>Kullanıcı öznitelikleri ve talepler yapılandırın

Bir kullanıcı oturum açtığında uygulamaya SAML belirtecindeki bilgileri Azure AD'ye gönderir denetleyebilirsiniz. Kullanıcı öznitelikleri yapılandırarak bu bilgi kontrol edebilirsiniz. Örneğin, bir kullanıcı oturum açtığında, kullanıcının adı, e-posta ve çalışan kimliği uygulamasına göndermek için Azure AD yapılandırabilirsiniz.

Çoklu oturum açma işlevinin düzgün çalışması için bu öznitelikler gerekli veya isteğe bağlı olabilir. Daha fazla bilgi için [uygulamaya özgü öğreticiye](../saas-apps/tutorial-list.md) bakın veya uygulama satıcısına sorun.

1. Kullanıcı öznitelikleri ve talepler düzenlemek için seçin **Düzenle** sağ üst köşesindeki simgeyi (Kalem) **kullanıcı öznitelikleri ve talepler** bölümü.

   **Ad tanımlayıcı değeri** varsayılan değeriyle ayarlayarak *user.principalname*. Kullanıcı tanımlayıcısı, uygulama içinde her kullanıcıyı benzersiz olarak tanımlar. Örneğin e-posta adresi hem kullanıcı adı hem de benzersiz tanıtıcı olarak kullanılıyorsa değeri *user.mail* olarak ayarlayın.

1. Değiştirilecek **ad tanımlayıcı değeri**seçin **Düzenle** simgesini (Kalem) **ad tanımlayıcı değeri** alan. Gerektiğinde, kaynak ve tanımlayıcı biçimi uygun değişiklikleri yapın. İşiniz bittiğinde, değişiklikleri kaydedin. Talep özelleştirme hakkında daha fazla bilgi için bkz. [kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](../develop/active-directory-saml-claims-customization.md) nasıl yapılır makalesi.
1. Bir talep eklemek için seçin **Ekle yeni talep** sayfanın üstünde. Girin **adı** ve uygun kaynak seçin. Seçerseniz **özniteliği** kaynağı seçin gerekir **kaynak özniteliği** kullanmak istiyorsunuz. Seçerseniz **çeviri** kaynağı seçin gerekir **dönüştürme** ve **parametresi 1** kullanmak istiyorsunuz.
1. **Kaydet**’i seçin. Yeni Talep tabloda görüntülenir.

## <a name="generate-a-saml-signing-certificate"></a>SAML imzalama sertifikası oluşturma

Azure AD, uygulamaya gönderdiği SAML belirteçlerini imzalamak için bir sertifika kullanır.

1. Yeni bir sertifika oluşturmak üzere **Düzenle** sağ üst köşesindeki simgeyi (Kalem) **SAML imzalama sertifikası** bölümü.
1. İçinde **SAML imzalama sertifikası** bölümünden **yeni sertifika**.
1. Görünen yeni sertifika satırda ayarlamak **sona erme tarihi**. Kullanılabilir yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [Gelişmiş Sertifika İmzalama Seçenekleri](certificate-signing-options.md) makalesi.
1. Seçin **Kaydet** en üstündeki **SAML imzalama sertifikası** bölümü.

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

Çoklu oturum açma çeşitli kullanıcılar veya gruplar uygulamayı kuruluşunuzun sunmaya önce test etmek için iyi bir fikirdir.

> [!NOTE]
> Bu adımları atmanız **kullanıcılar ve gruplar** Portalı'nda yapılandırma bölümü. İşiniz bittiğinde, geri gidin gerekecektir **çoklu oturum açma** öğreticiyi tamamlamak için bölüm.

Uygulamaya kullanıcı veya grup atamak için:

1. Zaten açık değilse portalda uygulamayı açın.
1. Uygulama için sol gezinti panelinde seçin **kullanıcılar ve gruplar**.
1. Seçin **Kullanıcı Ekle**.
1. İçinde **atama Ekle** bölümünden **kullanıcılar ve gruplar**.
1. Belirli bir kullanıcı bulmak için kullanıcı adını yazın. **üye seçin veya bir dış kullanıcıyı davet** kutusu. Ardından, kullanıcının profil fotoğrafı veya logoyu seçin ve ardından **seçin**.
1. İçinde **atama Ekle** bölümünden **atama**. İşiniz bittiğinde, Seçilen kullanıcılara görünür **kullanıcılar ve gruplar** listesi.

## <a name="set-up-the-application-to-use-azure-ad"></a>Uygulama Azure AD'yi kullanacak şekilde ayarlama

Neredeyse bitti.  Son adım olarak, uygulamanın SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde ayarlamanız gerekir. 

1. Ekranı aşağı kaydırarak **ayarlanan \<applicationName >** bölümü. Bu öğreticide, bu bölümde çağrılır **GitHub testi ayarlama**.
1. Bu bölümdeki her satırın değerini kopyalayın. Ardından, her değeri uygun satıra yapıştırın **temel SAML yapılandırma** bölümü. Örneğin, kopyalama **oturum açma URL'si** değerini **GitHub testi ayarlama** yapıştırın ve bölüm **işareti bulunan URL'si** alanındaki **temel SAML yapılandırma**  bölümü ve benzeri.
1. Tüm değerleri uygun alanlara yapıştırdığım zaman seçin **Kaydet**.

## <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Ayarlarınızı test etme hazırsınız demektir.  

1. Uygulamanızın çoklu oturum açma ayarlarını açın.
1. Kaydırma **ile çoklu oturum açmayı doğrula \<applicationName >** bölümü. Bu öğreticide, bu bölümde çağrılır **GitHub testi ayarlama**.
1. Seçin **Test**. Test seçenekleri açılır.
1. Seçin **geçerli kullanıcı olarak oturum açma**. Bu test çoklu oturum açma, yönetici çalışmıyorsa ilk görmenizi sağlar

Bir hata varsa, bir hata iletisi görüntülenir. Aşağıdaki adımları tamamlayın:

1. Hatayla ilgili bilgileri kopyalayıp **Hata neye benziyor?** kutusuna yapıştırın.

    ![Çözüm rehberlik almak için "Ne hata aşağıdaki gibi görünür" kutusunu kullanın](media/configure-single-sign-on-portal/error-guidance.png)

1. Seçin **çözümleme Hadoop'u**. Kök nedeni ve çözümü kılavuzu görünür.  Bu örnekte, kullanıcı uygulamaya atanan değildi.
1. Çözüm Kılavuzu okuyun ve ardından, mümkünse sorunu düzeltin.
1. Testi başarıyla tamamlana kadar tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulama için çoklu oturum açma ayarları yapılandırıldı. Yapılandırmayı tamamladıktan sonra uygulamaya kullanıcı atadınız ve uygulamayı SAML tabanlı çoklu oturum açmayı kullanacak şekilde yapılandırdınız. Bu işlerin tümünü tamamladığınızda SAML oturum açma işlevinin düzgün çalıştığını doğruladınız.

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Çoklu oturum açma modu olarak SAML seçtiniz
> * Etki alanını ve URL'leri yapılandırmak için uygulama satıcısıyla iletişime geçtiniz
> * Kullanıcı özniteliklerini yapılandırdınız
> * SAML imzalama sertifikası oluşturdunuz
> * Uygulamaya el ile kullanıcı veya grup atadınız
> * Uygulama SAML kimlik sağlayıcısı Azure AD'yi kullanacak şekilde yapılandırılmış
> * SAML tabanlı çoklu oturum açmayı test ettiniz

Kuruluşunuzda daha fazla kullanıcı uygulamayı kullanıma alma için otomatik kullanıcı hazırlama'ı kullanın.

> [!div class="nextstepaction"]
> [Otomatik sağlama ile kullanıcı atamayı öğrenin](configure-automatic-user-provisioning-portal.md)
