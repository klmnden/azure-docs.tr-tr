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
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6d9220cd2162b4c8cb77c1e7abd0372052f5454
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541623"
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

Kaynakları bir erişim paket içerisine dâhil etmek, kaynakları bir katalogda mevcut olması gerekir. Grupları, uygulamaları ve SharePoint Online sitesine ekleyebileceğiniz kaynakları türleridir.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve kaynakları eklemek istediğiniz katalog açın.

1. Sol menüde **kaynakları**.

1. Tıklayın **kaynak Ekle**.

1. Bir kaynak türü'a tıklayın: **Grupları**, **uygulamaları**, veya **SharePoint siteleri**.

    Bir katalog Oluşturucusu varsa, herhangi bir Office 365 grubu veya kataloğunuza kendi Azure AD güvenlik grubu ekleyebilirsiniz. Kullanıcılara atamak istediğiniz bir grup yoktur, ancak Grup sahibi olmadığınız, bu gruba eklemek kataloğunuza Kullanıcı Yöneticisi olması gerekir.

    Bir katalog Oluşturucusu varsa, sahip olduğunuz, kataloğunuzu için Azure AD'ye Federasyon SaaS uygulamaları ve kendi uygulamalarınızı hem de dahil olmak üzere herhangi bir Azure AD Kurumsal uygulama ekleyebilirsiniz. Kullanıcılara atamak istediğiniz, ancak sahibi olmadığınız bir uygulama ise, uygulama kataloğunuzu eklemek Kullanıcı Yöneticisi olması gerekir. Uygulama Kataloğu'nun bir parçası olduğunda, uygulamanın rollerinden herhangi birinde bir erişim paketinde seçebilirsiniz.

1. Bir veya daha fazla kaynak Kataloğu'na eklemek istediğiniz türü seçin.

1. İşiniz bittiğinde tıklayın **Ekle**.

    Bu kaynaklar katalog içindeki erişim paketler artık dahil edilebilir.

## <a name="remove-resources-from-a-catalog"></a>Kaynakları Kataloğu'ndan kaldırın

Kaynakların bir Kataloğu'ndan kaldırabilirsiniz. Kataloğu'nun erişim paketlerinin hiçbirinde kullanıldığı değil, bir kaynak yalnızca Kataloğu'ndan kaldırabilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve kaynaklardan kaldırmak istediğiniz katalog açın.

1. Sol menüde **kaynakları**.

1. Kaldırmak istediğiniz kaynakları seçin.

1. Tıklayın **Kaldır** (veya üç nokta simgesine tıklayın ( **...** ) ve ardından **kaldırmak kaynak**).

## <a name="add-catalog-owners-or-access-package-managers"></a>Katalog sahip ekleme ya da paket yöneticileri erişimi

Katalog veya erişim paketleri Kataloğu yönetimi, temsilci seçmek istiyorsanız, katalog sahipler ekleyin veya paket yöneticileri erişim. Katalog kişi oluşturur, ilk katalog sahibi olur.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve Yöneticiler için eklemek istediğiniz katalog açın.

1. Sol menüde **roller ve yöneticiler**.

1. Tıklayın **sahipler eklemeyi** veya **erişim paket yöneticilerini ekleme** bu rollerinin üyeleri seçin.

1. Tıklayın **seçin** bu üyeleri eklemek için.

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

- [Oluşturma ve erişim paket yönetme](entitlement-management-access-package-create.md)
