---
title: Azure AD hak yönetimi (Önizleme) - Azure Active Directory içinde görevler için temsilci seçme
description: Azure Active Directory Hak Yönetimi'nde Görevler için temsilci atadığınız rolleri hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/07/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fa0be8e2af7644564ba27e6d58fda09b1ae7bc7
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191505"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Azure AD hak yönetimi (Önizleme) içinde görevler için temsilci seçme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Varsayılan olarak, genel Yöneticiler ve kullanıcı oluşturabilir ve Azure AD Hak Yönetimi'nin tüm özelliklerini yönetebilir. Ancak, bu rollerdeki kullanıcıların erişim paketleri gerekli olduğu senaryolar bilemeyebilirsiniz. Genellikle kullanıcılar bölümleri içinde olduğu işbirliği yapmak gereksinim duyan kim öğrenin.

Yönetici olmayanlar için sınırsız izinleri vermek yerine, kullanıcıların en az işini gerçekleştirmek ve çakışan oluşturmaktan kaçınmak için ihtiyaç duydukları izinleri veya uygunsuz erişim hakları verebilirsiniz. Bu makalede, Hak Yönetimi'nde çeşitli görevler için temsilci atadığınız rolleri açıklanır.

## <a name="delegate-example-for-departmental-adoption"></a>Departman benimsenmesine yönelik temsilci örneği

Hak Yönetimi'nde Görevler nasıl temsilci anlamak için bu örneği göz önünde bulundurun yardımcı olur. Aşağıdaki beş kullanıcılar, kuruluşunuzun sahip olduğunu varsayın:

| Kullanıcı | Departman | Notlar |
| --- | --- | --- |
| Alice | BT | Genel yönetici |
| Bob | Araştırma | Bob ayrıca bir araştırma grubunun sahibi değil |
| Ceren | Araştırma |  |
| Dave | Pazarlama |  |
| Elisa | Pazarlama | Elisa de pazarlama uygulamanın sahibi: |

Araştırma ve pazarlama departmanları kullanıcıları için hak yönetimi kullanmak istiyorsunuz. Alice henüz diğer bölümlerden hak yönetimi hazır değil. Alice araştırma ve pazarlama departmanları görevlere temsilci seçebilecek yöntemlerinden biri aşağıda verilmiştir.

1. Alice oluşturur, yeni bir Azure AD güvenlik grubu için katalog creators ve Bob ve Carol, Dave ve Elisa o grubun üyesi olarak ekler.

1. Alice, katalog creators rolüne bu gruba eklemek için hak yönetimi ayarlarını kullanır.

1. Carol oluşturur bir **araştırma** katalog ve Bob kataloğu bir ortak sahip olarak ekler. Bob bir kaynak olarak Kataloğu'na sahip olan research grubu ekler; böylece bir erişim paketinde araştırma işbirliği için kullanılabilir.

1. Dave oluşturur bir **pazarlama** katalog ve Elisa kataloğu bir ortak sahip olarak ekler. Elisa kendisi bir kaynak olarak Kataloğu'na sahip pazarlama uygulaması ekler; böylece işbirliği pazarlama için bir erişim paketinde kullanılabilir.

Şimdi araştırma ve pazarlama departmanları hak yönetimi kullanabilir. Bob ve Carol, Dave ve Elisa oluşturabilir ve erişim paketleri kendi ilgili kataloglarını yönetmek.

![Hak Yönetimi temsilci örneği](./media/entitlement-management-delegate/elm-delegate.png)

## <a name="entitlement-management-roles"></a>Hak yönetim rolleri

Hak Yönetimi, hak yönetimi için özel olan aşağıdaki rol yok.

| Rol | Açıklama |
| --- | --- |
| Katalog Oluşturucusu | Oluşturun ve kataloglarını yönetin. Bir genel yönetici veya bir kaynak koleksiyonu için bir kaynak sahibi olmayan genellikle BT yöneticisi. Katalog otomatik olarak oluşturan kişinin Kataloğu'nun ilk katalog sahibi olur ve ek katalog sahipleri ekleyebilirsiniz. |
| Katalog sahibi | Düzenle ve mevcut kataloglarını yönetin. Genellikle BT yöneticisi veya kaynak sahiplerinin veya katalog sahibinin atadığı bir kullanıcı. |
| Erişim Paket Yöneticisi | Düzenle ve Katalog içindeki tüm var olan erişim paketleri yönetin. |

Rol olmasa da ek olarak, belirlenen bir onaylayan ve bir istek sahibi, bir erişim paketinin da hakları vardır.
 
* Onaylayan: Onaylayın veya reddedin paketleri erişim istekleri için bir ilke tarafından yetkili, ancak bu erişim paket tanımlarını değiştiremez.
* İstek sahibi: Bu erişim paket isteği için bir erişim paket İlkesi tarafından yetkili.

Aşağıdaki tabloda, bu rolleri gerçekleştirebileceğiniz görevler listelenmektedir.

| Görev | Katalog Oluşturucusu | Katalog sahibi | Erişim Paket Yöneticisi | Onaylayan |
| --- | :---: | :---: | :---: | :---: |
| [Yeni bir katalog oluşturun](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Katalog için bir kaynak ekleyin](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Katalog Düzenle](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Katalog silme](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [Katalog sahibi veya bir erişim Paket Yöneticisi Kataloğu'na ekleyin.](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Bir katalogda yeni erişim paketi oluştur](entitlement-management-access-package-create.md) |  | :heavy_check_mark: |  |  |
| [Bir erişim pakette kaynak rolleri yönetme](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Kimin bir erişim paketini talep edebilir belirtin](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Doğrudan bir erişim paketi için kullanıcı atama](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketi atamaya sahip görüntüle](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketin istekleri görüntüleme](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir isteğin teslim hataları görüntüleyin](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bekleyen isteği iptal et](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir access paketi Gizle](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketini Sil](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim isteği Onayla](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Gerekli rolleri için bir katalog kaynakları eklemek için

Genel yönetici ekleyebilir veya herhangi bir grubu (bulut oluşturduğunuz güvenlik gruplarını veya Office 365 grupları bulut oluşturulan), uygulama veya SharePoint Online sitesine bir katalogda kaldırabilirsiniz. Kullanıcı Yöneticisi ekleyebilir veya herhangi bir grup veya uygulama Kataloğu'nda kaldırabilirsiniz.

Kullanıcı grupları, uygulamaları veya SharePoint Online siteleri bir Kataloğu'na eklemek için bir genel yönetici veya kullanıcı yönetici değil bir kullanıcı olmalıdır *hem* Azure AD dizini rol ve Katalog sahibi yetkilendirme gerekli Yönetim rolü. Aşağıdaki tabloda, kaynakların Kataloğu'na eklenmesi gereken rol birleşimlerini listelenmektedir. Kaynakların bir Kataloğu'ndan kaldırmak için aynı rolleri olması gerekir.

| Azure AD dizin rolü | Hak Yönetimi rolü | Güvenlik grubuna ekleyebilirsiniz | Office 365 grup ekleyebilirsiniz | Uygulama eklemek için | SharePoint Online sitesine ekleyebilirsiniz |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Genel yönetici](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Kullanıcı Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | yok |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [Takımlar Hizmet Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  |  |
| [SharePoint Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Uygulama Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| [Bulut uygulaması Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md) | Katalog sahibi |  |  | :heavy_check_mark: |  |
| Kullanıcı | Katalog sahibi | Yalnızca Grup sahibi | Yalnızca Grup sahibi | Yalnızca uygulama sahibi |  |

## <a name="add-a-catalog-creator"></a>Katalog Oluşturucu Ekle

Katalog oluşturma temsilci seçmek istiyorsanız, katalog Oluşturucu rolüne kullanıcılar ekleyin.  Bireysel kullanıcılar ekleyebilirsiniz veya için kolaylık üyeleri kataloglar oluşturmak mümkün olan daha sonra bir grup ekleyebilirsiniz. Katalog Oluşturucu rolüne bir kullanıcı atamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Soldaki menüde içinde **hak yönetimi** bölümünde **ayarları**.

1. **Düzenle**‘ye tıklayın.

1. İçinde **temsilci yetkilendirme Yönetim** bölümünde **Kataloğu oluşturucular ekleme** kullanıcıları veya üyeleri için bu yetkilendirme yönetim rolü grupları seçin.

1. **Seç**'e tıklayın.

1. **Kaydet**’e tıklayın.

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>Katalog sahibi veya bir erişim Paket Yöneticisi ekleme

Bir katalog ya da erişim paketlerin katalogdaki yönetim temsilci seçmek istiyorsanız, Katalog sahibini veya erişim Paket Yöneticisi rolleri kullanıcılar ekleyin. Katalog kişi oluşturur, ilk katalog sahibi olur. Katalog sahibini veya erişim Paket Yöneticisi rolüne bir kullanıcı atamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya sahibi Kataloğu

1. Azure portalında **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **katalogları** ve Yöneticiler için eklemek istediğiniz katalog açın.

1. Sol menüde **roller ve yöneticiler**.

1. Tıklayın **sahipler eklemeyi** veya **erişim paket yöneticilerini ekleme** bu rollerinin üyeleri seçin.

1. Tıklayın **seçin** bu üyeleri eklemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayan Ekle](entitlement-management-access-package-edit.md#policy-request)
- [Katalog için kaynak ekleme](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
