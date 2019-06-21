---
title: Oluşturma ve Azure AD hak yönetimi (Önizleme) - Azure Active Directory kataloğunda yönetme
description: Azure Active Directory hak yönetimi (Önizleme) kaynaklara erişim paketlerini ve yeni bir kapsayıcı oluşturmayı öğrenin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/29/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5988f4723f1ef73cf0767ef8ac1b9adf3c1435d
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190235"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Oluşturma ve Azure AD hak yönetimi (Önizleme) bir katalogda yönetme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-catalog"></a>Katalog oluşturma

Katalog, kaynaklar ve erişim paketlerin bir kapsayıcıdır. İlgili kaynakları grubu ve paketleri erişmek istediğinizde bir katalog oluşturun. Kişi kataloğu oluşturur, ilk katalog sahibi olur. Bir katalog sahip ek katalog sahipleri ekleyebilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya katalog Oluşturucusu

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Tıklayın **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları**.

    ![Azure portalında hak yönetimi katalogları](./media/entitlement-management-catalog-create/catalogs.png)

1. Tıklayın **yeni katalog**.

1. Katalog için benzersiz bir ad girin ve bir açıklama belirtin.

    Kullanıcılar bu bilgileri bir erişim paketin ayrıntılarını görürsünüz.

1. Erişim paketleri bu katalogdaki oluşturulduktan hemen sonra istemek kullanıcılar için kullanılabilir olmasını istiyorsanız, ayarlayın **etkin** için **Evet**.

1. Kullanıcılar bu katalog paketleri erişim isteği, Ayarla yapabilmek için seçili dış dizinlere izin vermek istiyorsanız **dış kullanıcılar için etkin** için **Evet**.

    ![Yeni Katalog bölmesi](./media/entitlement-management-catalog-create/new-catalog.png)

1. Tıklayın **Oluştur** kataloğu oluşturmak için.

## <a name="add-resources-to-a-catalog"></a>Katalog için kaynak ekleme

Kaynakları bir erişim paket içerisine dâhil etmek, kaynakları bir katalogda mevcut olması gerekir. Grupları, uygulamaları ve SharePoint Online sitesine ekleyebileceğiniz kaynakları türleridir. Grupları, bulut tarafından oluşturulan Office 365 grupları veya Azure bulut tarafından oluşturulmuş olabilir AD güvenlik grupları. Uygulamaları Azure AD'ye Kurumsal uygulamaları, SaaS uygulamaları ve kendi uygulamalarınızı Azure AD'ye Federasyon hem de dahil olmak üzere olabilir. SharePoint Online siteleri veya SharePoint Online site koleksiyonları, siteler olabilir.

**Önkoşul rolü:** Bkz: [gerekli rolleri için bir katalog kaynakları eklemek için](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve kaynakları eklemek istediğiniz katalog açın.

1. Sol menüde **kaynakları**.

1. Tıklayın **kaynak Ekle**.

1. Bir kaynak türü'a tıklayın: **Grupları**, **uygulamaları**, veya **SharePoint siteleri**.

    Eklemek istediğiniz bir kaynak görmüyorsanız ya da bir kaynak eklenemiyor, gerekli olduğundan emin olun. Azure AD dizin rolü ve yetkilendirmesini yönetim rolü. Birinin gerekli rollerle kataloğunuza kaynak eklemek gerekebilir. Daha fazla bilgi için [gerekli kaynakları Kataloğu'na eklemek için rolleri](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog).

1. Bir veya daha fazla kaynak Kataloğu'na eklemek istediğiniz türü seçin.

1. İşiniz bittiğinde tıklayın **Ekle**.

    Bu kaynaklar katalog içindeki erişim paketler artık dahil edilebilir.

## <a name="remove-resources-from-a-catalog"></a>Kaynakları Kataloğu'ndan kaldırın

Kaynakların bir Kataloğu'ndan kaldırabilirsiniz. Kataloğu'nun erişim paketlerinin hiçbirinde kullanıldığı değil, bir kaynak yalnızca Kataloğu'ndan kaldırabilirsiniz.

**Önkoşul rolü:** Bkz: [gerekli rolleri için bir katalog kaynakları eklemek için](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve kaynaklardan kaldırmak istediğiniz katalog açın.

1. Sol menüde **kaynakları**.

1. Kaldırmak istediğiniz kaynakları seçin.

1. Tıklayın **Kaldır** (veya üç nokta simgesine tıklayın ( **...** ) ve ardından **kaldırmak kaynak**).

## <a name="edit-a-catalog"></a>Katalog Düzenle

Ad ve açıklama katalog için düzenleyebilirsiniz. Kullanıcılar bu bilgileri bir erişim paketin ayrıntıları bakın.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve düzenlemek istediğiniz katalog açın.

1. Katalog üzerinde **genel bakış** sayfasında **Düzenle**.

1. Kataloğu'nun adını veya açıklamasını düzenleyin.

1. **Kaydet**’e tıklayın.

## <a name="delete-a-catalog"></a>Katalog silme

Herhangi bir erişim paket yoksa, ancak bir katalog silebilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve ardından silmek istediğiniz katalog açın.

1. Katalog üzerinde **genel bakış**, tıklayın **Sil**.

1. Görüntülenen ileti kutusunda **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

- [Katalog Oluşturucu Ekle](entitlement-management-delegate.md#add-a-catalog-creator)
- [Oluşturma ve erişim paket yönetme](entitlement-management-access-package-create.md)
