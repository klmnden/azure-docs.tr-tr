---
title: Azure AD hak yönetimi (Önizleme) - Azure Active Directory içinde yeni bir erişim paketi oluştur
description: Azure Active Directory hak yönetimi (Önizleme) paylaşmak istediğiniz kaynakların yeni bir erişim paket oluşturmayı öğrenin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/24/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ad6570a3f30e40e4074502a8ce85bf739f58d3f
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866431"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD hak yönetimi (Önizleme) yeni bir erişim paketi oluştur

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir erişim paket ömrü boyunca erişim paket erişimi otomatik olarak yönetir kaynaklarının ve ilkeleri tek seferlik Kurulumu yapmanıza olanak sağlar. Bu makalede yeni bir erişim paketinin nasıl oluşturulacağını açıklar.

## <a name="overview"></a>Genel Bakış

Tüm erişim paketleri katalog olarak adlandırılan bir kapsayıcıda yerleştirmeniz gerekir. Katalog hangi kaynaklara erişim paketinizi ekleyebilirsiniz tanımlar. Katalog belirtmezseniz, genel kataloğa erişim paketinizi yerleştirilir. Şu anda var olan erişim paketini farklı bir kataloğa taşıyamazsınız.

Tüm erişim paketleri en az bir ilkesi olmalıdır. İlkeleri olan istek erişim paket ve ayrıca onay ve sona erme ayarları belirtin. Yeni bir erişim paket oluşturduğunuzda, yönetici doğrudan atamalar yalnızca dizininize'nda kullanıcılar için dizininizdeki kullanıcılar için ilk bir ilke oluşturabilirsiniz veya daha sonra ilkeyi oluşturmayı tercih edebilirsiniz.

Aşağıdaki diyagram, yeni bir erişim paketi oluşturmak için üst düzey bir işlem gösterilir.

![Bir erişim paket işlemi oluşturma](./media/entitlement-management-access-package-create/access-package-process.png)

## <a name="start-new-access-package"></a>Yeni erişim paket Başlat

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Tıklayın **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **erişim paketleri**.

    ![Azure portalında hak yönetimi](./media/entitlement-management-shared/elm-access-packages.png)

1. Tıklayın **yeni erişim paket**.

## <a name="basics"></a>Temel Bilgiler

Üzerinde **Temelleri** sekmesinde, erişim paket bir ad verin ve erişim paketi oluşturmak için hangi Katalog değerini belirtin.

1. Bir görünen ad ve erişim paketi için bir açıklama girin. Bunlar erişim paket için bir istek gönderdiğinde, kullanıcılar bu bilgileri görür.

1. İçinde **Kataloğu** erişim oluşturmak istediğiniz katalog paket içinde aşağı açılan listesinde seçin. Örneğin, istenebilir pazarlama kaynaklarını yöneten bir katalog sahip olabilir. Bu durumda, pazarlama Kataloğu seçebilirsiniz.

    Yalnızca katalog göreceksiniz erişim paketleri oluşturma izniniz yok. Mevcut bir katalogda erişim paketi oluşturmak için en az bir yönetici kullanıcı, katalog sahibi veya erişim Paket Yöneticisi olması gerekir.

    ![Erişim package - temel](./media/entitlement-management-access-package-create/basics.png)

    Yeni bir katalogda erişim paketinizi oluşturmak istiyorsanız, tıklayın **Yeni Oluştur**. Katalog adı ve açıklama girin ve ardından **Oluştur**.

    Oluşturmakta olduğunuz erişim paketi ve içerdiği tüm kaynakları yeni Kataloğu'na eklenir. Ayrıca, otomatik olarak ilk katalog sahibi olur. Ek katalog sahipleri ekleyebilirsiniz.

    Yeni katalog oluşturmak için en az bir yönetici kullanıcı veya katalog oluşturucusu olmalıdır.

1. **İleri**’ye tıklayın.

## <a name="resource-roles"></a>Kaynak rolleri

Üzerinde **kaynak rolleri** sekmesini erişim paket içerisine dâhil etmek kaynakları seçin.

1. Eklemek istediğiniz kaynak türüne tıklayın (**grupları**, **uygulamaları**, veya **SharePoint siteleri**).

1. Görüntülenen Select bölmesinde, listeden bir veya daha fazla kaynak seçin.

    ![Erişim package - kaynak rolleri](./media/entitlement-management-access-package-create/resource-roles.png)

    Genel katalog veya yeni bir katalog erişim paket oluşturuyorsanız, herhangi bir kaynağa sahip olduğunuz dizinden alacak şekilde mümkün olacaktır. En az bir kullanıcı Yöneticisi olması veya Oluşturucu katalog gerekir.

    Mevcut bir katalogda erişim paket oluşturuyorsanız, zaten sahip olmadan katalogda olan herhangi bir kaynak seçebilirsiniz.

    Kullanıcı Yöneticisi veya katalog sahibi, henüz kataloğunda olmayan olduğunuz kaynakları seçme ek seçeneğiniz vardır. Kaynakları şu anda seçili katalogda seçerseniz, bu kaynakları diğer katalog yöneticileri ile erişim paketleri oluşturmak için katalog de eklenir. Yalnızca şu anda seçili katalog, onay kaynaklar'ı seçmek istiyorsanız **yalnızca** Select pan üstündeki onay kutusu.

1. İçinde kaynaklar'ı seçtikten sonra **rol** listesinde, kaynak için atanmış kullanıcılar istediğiniz rolü seçin.

    ![Erişim package - Kaynak rolü seçimi](./media/entitlement-management-access-package-create/resource-roles-role.png)

1. **İleri**’ye tıklayın.

## <a name="policy"></a>İlke

Üzerinde **ilke** sekmesi, kimin istek erişim paket ve ayrıca onay ve sona erme ayarları belirtmek için ilk ilkesi oluşturma. Daha sonra ek kendi onay ve sona erme ayarları ile erişim paket isteği için kullanıcı gruplarına izin vermek için daha fazla ilke oluşturabilirsiniz. Daha sonra ilkeyi oluşturmayı tercih edebilirsiniz.

1. Ayarlama **ilk ilkesi oluşturma** geç **artık** veya **sonra**.

    ![Erişim package - ilke](./media/entitlement-management-access-package-create/policy.png)

1. Seçerseniz **sonra**, aşağı atla [gözden geçir + Oluştur](#review--create) erişim paketinizi oluşturmak için bölümü.

1. Seçerseniz **artık**, aşağıdaki ilke bölümlerden birine adımları gerçekleştirin.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="review--create"></a>Gözden geçirme + oluşturma

Üzerinde **gözden + Oluştur** sekmesinde, tüm doğrulama hatalarını denetleme ve ayarlarınızı gözden geçirin.

1. Erişim paket ayarlarını gözden geçirin

    ![Erişim package - ilke etkinleştirmek ilke ayarı](./media/entitlement-management-access-package-create/review-create.png)

1. Tıklayın **Oluştur** erişim paketi oluşturmak için.

    Yeni erişim paket erişim paketler listesinde görünür.

## <a name="next-steps"></a>Sonraki adımlar

- [Düzenleme ve var olan erişim paketini yönetme](entitlement-management-access-package-edit.md)
- [Oluşturma ve kataloğunu yönetme](entitlement-management-catalog-create.md)
