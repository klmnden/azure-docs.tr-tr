---
title: Kullanıcı Yönetimi Azure Active Directory'de kurumsal uygulamalar için hazırlama | Microsoft Docs
description: Kullanıcı hesabı Azure Active Directory'yi kullanarak kurumsal uygulamalar için sağlama yönetmeyi öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/01/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bf265f51f2fea16f90dd0bcf2891bd9bed5cef8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65963580"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Azure portalının kurumsal uygulamalar için sağlama kullanıcı hesabı yönetme

Bu makalede nasıl kullanılacağını [Azure portalında](https://portal.azure.com) hesabı otomatik kullanıcı hazırlama ve onu destekleyen uygulamalar için sağlamayı yönetme. Otomatik kullanıcı hesabı sağlama ve nasıl çalıştığı hakkında daha fazla bilgi için bkz: [sağlama kaldırmayı Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin ve](user-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Portalda uygulamalarınızı bulma

Azure Active Directory portalında görüntülemek ve bir dizinde çoklu oturum açma için yapılandırılmış olan tüm uygulamaları yönetmek için kullanın. Kurumsal uygulamalar, dağıtıldığı ve kuruluşunuzda kullanılan uygulamalardır. Kurumsal uygulamalarınızı yönetmek ve görüntülemek için aşağıdaki adımları izleyin:

1. Açık [Azure Active Directory portalında](https://aad.portal.azure.com).

1. Seçin **kurumsal uygulamalar** sol bölmeden. Galeriden eklenen uygulamaları dahil olmak üzere tüm yapılandırılmış uygulamaların bir listesi gösterilir.

1. Raporları görüntülemek ve uygulama ayarlarını yönetme, kaynak bölmesini yüklemek için herhangi bir uygulama seçin.

1. Seçin **sağlama** hazırlama ayarları seçili uygulama için kullanıcı hesabını yönetmek için.

   ![Uygulama kaynağı bölmesi](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Sağlama modları

**Sağlama** Bölmesi ile başlayan bir **modu** menüsünde, desteklenen bir kurumsal uygulama için sağlama modu gösterir ve bunları yapılandırmanıza olanak sağlar. Kullanılabilir seçenekler şunlardır:

* **Otomatik** -bu seçenek, Azure AD API tabanlı otomatik sağlama veya bu uygulamaya yönelik kullanıcı hesaplarının sağlamayı destekleyip desteklemediğini gösterilir. Bu mod, yöneticiler yardımcı olan bir arabirim görüntülemek için seçin:

  * Uygulamanın kullanıcı yönetimi API'sine bağlanmak için Azure AD'yi yapılandırma
  * Hesap Eşleştirmeleri ve nasıl kullanıcı hesabı verileri Azure AD akışını tanımlamanızı iş akışları oluşturma ve uygulama
  * Sağlama hizmetini Azure AD'yi yönetme

* **El ile** -bu seçenek, Azure AD kullanıcı hesaplarının bu uygulama için otomatik sağlamayı desteklemeyen gösterilir. Bu durumda, uygulama kayıtları depolanan kullanıcı hesabı (içerebilen SAML tam zamanında sağlama) bu uygulama tarafından sağlanan kullanıcı yönetimi ve sağlama yeteneklerine dayalı bir dış işlemi kullanılarak yönetilmesi gerekir.

## <a name="configuring-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma

Seçin **otomatik** yönetici kimlik bilgileri, başlatma ve durdurma, eşlemeler ve eşitleme ayarlarını belirtmek için seçeneği.

### <a name="admin-credentials"></a>Yönetici kimlik bilgileri

Genişletin **yönetici kimlik bilgileri** uygulamanın kullanıcı yönetimi API'sine bağlanmak Azure AD için gerekli kimlik bilgilerini girmek için. Gerekli giriş uygulamaya bağlı olarak değişir. Kimlik bilgisi türlerinin ve belirli uygulamalar için gereksinimleri hakkında bilgi edinmek için bkz. [belirli bir uygulama için yapılandırma öğreticisini](user-provisioning.md).

Seçin **Bağlantıyı Sına** Azure sağlayarak kimlik bilgilerini test etmek için uygulamaya bağlanmak için AD girişim sağlanan kimlik bilgilerini kullanarak uygulama sağlama.

### <a name="mappings"></a>Eşlemeler

Genişletin **eşlemeleri** görüntüleme ve Azure AD arasında akış kullanıcı özniteliklerini düzenleyin ve kullanıcı hesaplarını sağlandığında veya güncelleştirildiğinde, hedef uygulama.

Önceden yapılandırılmış bir Azure AD kullanıcı nesnelerinin ve her SaaS uygulamasının kullanıcı nesneleri arasında eşleme yoktur. Bazı uygulamalar diğer grupların veya kişilerin gibi nesnelerin türlerini yönetin. Bir eşleme burada görüntüleyebilir ve bunları özelleştirme sağa eşleme düzenleyicisini açmak için tablo seçin.

![Uygulama kaynağı bölmesi](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Desteklenen özelleştirmeleri içerir:

* Etkinleştirme ve SaaS uygulamasının kullanıcı nesnesi Azure AD kullanıcı nesnesini gibi belirli nesneler için eşlemelerini devre dışı bırakma.
* Uygulamanın kullanıcı nesnesini Azure AD'ye kullanıcı nesneden akış öznitelikleri düzenleme. Öznitelik eşlemesi hakkında daha fazla bilgi için bkz. [öznitelik eşlemesi türlerini anlama](customize-application-attributes.md#understanding-attribute-mapping-types).
* Azure AD hedeflenen uygulamaya çalışan sağlama eylemleri filtreleme. Azure AD'ye tam eşitleme sahip olmak yerine çalıştırma eylemleri sınırlayabilirsiniz. 

  Örneğin, yalnızca belirli **güncelleştirme** ve var olan kullanıcı hesaplarını bir uygulamada, ancak yenilerini oluşturmaz Azure AD'ye yalnızca güncelleştirmeler. Yalnızca belirli **Oluştur** ve Azure, yalnızca yeni kullanıcı hesaplarını oluşturur ancak mevcut olanları güncelleştirmez. Bu özellik, hesap oluşturma için farklı eşlemeler oluşturun ve iş akışları güncelleştirme yöneticileri olanak sağlar.

* Yeni bir öznitelik eşlemesi ekleniyor. Seçin **yeni eşleme Ekle** kısmındaki **eşleme özniteliği** bölmesi. Doldurun **özniteliğini Düzenle** seçin ve form **Tamam** yeni eşleme listesine eklenecek. 

### <a name="settings"></a>Ayarlar

Azure AD seçilen uygulama için hizmet sağlama başlatıp durdurabilirsiniz **ayarları** alanının **sağlama** ekran. Sağlama önbelleği temizlemek ve hizmeti yeniden başlatmak seçebilirsiniz.

Sağlama için bir uygulama ilk kez etkinleştiriliyor, hizmette değiştirerek kapatma **sağlama durumu** için **üzerinde**. Bu değişiklik, bir ilk eşitleme çalıştırmak Azure AD sağlama hizmeti neden olur. Atanan kullanıcılar okur **kullanıcılar ve gruplar** bölümü, hedef uygulama için bunları sorgular ve ardından Azure AD'de tanımlanan sağlama eylemleri çalıştırır **eşlemeleri** bölümü. Yönetilmeyen hesapları hiçbir zaman atama için kapsamda olan hedef uygulamalarının içinde işlemler sağlamayı tarafından etkilenmez. Bu nedenle bu işlem sırasında sağlama hizmeti, yönetmekte olduğu, hangi kullanıcı hesaplarını ilgili önbelleğe alınan verileri depolar. İlk eşitlemeden sonra sağlama hizmeti, kullanıcı ve Grup nesneleri otomatik olarak bir on dakikalık aralıklarla eşitler.

Değişiklik **sağlama durumu** için **kapalı** sağlama hizmeti duraklatmak için. Bu durumda, Azure değil oluşturmak, güncelleştirmek veya uygulamada herhangi bir kullanıcı veya grup nesneleri kaldırın. Değişiklik durumu geri **üzerinde** ve hizmet kaldığı seçer.

Seçin **geçerli durumu temizleyin ve eşitlemeyi yeniden başlatın** onay kutusunu seçip **Kaydet** için:

* Sağlama Hizmeti Durdur
* Azure AD yönetiminin hangi hesapların ilgili önbelleğe alınan verileri dökümü
* Hizmetleri yeniden başlatmak ve ilk eşitlemeyi yeniden çalıştırın

Bu seçenek, sağlama dağıtım işlemini baştan başlatmak yöneticileri sağlar.

### <a name="synchronization-details"></a>Eşitleme ayrıntıları

Bu bölümde, sağlama hizmeti uygulama ve kaç kullanıcı ve Grup nesneleri yönettiği karşı çalıştırıldı ilk ve son kez dahil olmak üzere sağlama hizmetinin işlemi hakkında ek ayrıntılar sağlar.

Bir bağlantı için sağlanan **sağlama etkinliği raporunu**, tüm kullanıcıları içeren bir günlük sağlar ve oluşturulmuş, güncelleştirilmiş ve kaldırılan arasında Azure AD grupları ve hedef uygulama. Bir bağlantı için de sağlanır **hata raporu sağlama**, daha ayrıntılı hata iletilerini kullanıcı ve okumak için oluşturulan'başarısız oldu. Grup nesnelerini sağlayan güncelleştirilmiş veya kaldırılamaz.
