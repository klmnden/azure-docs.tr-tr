---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Asana yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına Asana sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Öğretici: Asana otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı Asana ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Asana sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active Active directory kiracısı
*   Asana Kiracı ile [Kurumsal](https://www.asana.com/pricing) planlayabilir ya da daha iyi etkin 
*   Asana yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE] 
> Tümleştirme sağlama Azure AD dayanan [Asana API](https://app.asana.com/api/1.0/scim/Users) olduğu Asana için kullanılabilir.

## <a name="assigning-users-to-asana"></a>Kullanıcılar için Asana atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, "Azure AD'de bir uygulamaya atanmış" kullanıcılar eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce kullanıcıların Azure AD'de Asana uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Asana uygulamanıza atayabilirsiniz:

[Kullanıcı bir kurumsal uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Kullanıcılar için Asana atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Asana atanabilir. Ek kullanıcılar daha sonra atanabilir.

## <a name="configuring-user-provisioning-to-asana"></a>Kullanıcı için Asana sağlama yapılandırma 

Bu bölümde, Azure AD API sağlama Asana kullanıcı hesabına bağlanma yoluyla size rehberlik eder ve Azure AD'de kullanıcı ataması göre Asana kullanıcı hesapları oluşturma, güncelleştirme ve devre dışı bırakmak için sağlama hizmeti yapılandırma atanır.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Asana, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Otomatik olarak bir kullanıcı hesabı için Asana Azure AD'de sağlama yapılandırmak için:

1)  İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2) Çoklu oturum açma için Asana zaten yapılandırdıysanız arama alanı kullanarak Asana Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Asana** uygulama galerisinde. Seçin **Asana** Arama sonuçlarından ve uygulamaları listenize ekleyin.

3)  Asana örneğiniz seçin ve ardından **sağlama** sekmesi.

4)  Ayarlama **sağlama modunda** için **otomatik**.

![Asana sağlama](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) Yönetici kimlik bilgileri bölümü altında belirteç oluşturmak ve içinde girmek için yönergeler aşağıdaki izleyin **gizli belirteci** metin kutusu.

* Oturum açma [Asana](https://app.asana.com) yönetici hesabı kullanarak
* Üst çubuğundan profil fotoğrafınız tıklayın ve geçerli kuruluş adı ayarlarını seçin
* Hizmet hesapları sekmesine gidin
* Hizmet Hesabı Ekle'yi tıklatın
* Ad, güncelleştirmek ve gerektiğinde fotoğraf profil, kopya **belirteci** ve değişiklikleri Kaydet'i tıklatın

6) Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Asana uygulamanıza bağlanabilir. Bağlantı başarısız olursa Asana hesabınız yönetici izinleri ve deneyin olduğundan **"Test Bağlantısı"** adım yeniden uygulayın.

7) Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

8) **Kaydet**’e tıklayın. 

9) Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Asana**.

10) İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den Asana için eşitlenecek kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri, güncelleştirme işlemlerini Asana kullanıcı hesaplarında eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın. Bkz: [kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md) daha fazla ayrıntı için

11) Azure AD hizmeti Asana için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12) **Kaydet**’e tıklayın. 

Bu, kullanıcılar bölümünde Asana atanan tüm kullanıcıların ilk eşitleme başlatır. İlk yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler gerçekleştirmek için daha uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Asana uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-asana-tutorial.md)
