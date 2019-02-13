---
title: Geri yükleme ya da-Azure Active Directory yakın zamanda silinen bir kullanıcıyı kalıcı olarak Kaldır | Microsoft Docs
description: Nasıl geri yüklenebilen kullanıcıları görüntüleyin, silinen bir kullanıcıyı geri yükleme veya Azure Active Directory ile bir kullanıcıyı kalıcı olarak sil.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a810ae13d9cfb68d11293ba883c52858aa4a2deb
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164762"
---
# <a name="restore-or-remove-a-recently-deleted-user-using-azure-active-directory"></a>Geri yükleme ya da Azure Active Directory kullanarak son silinen bir kullanıcıyı kaldırma
Bir kullanıcı sildikten sonra hesabı 30 gün için askıya alınmış durumda kalır. Bu 30 günlük penceresi sırasında kullanıcı hesabı, tüm özellikleriyle birlikte geri yüklenebilir. Bu 30 günlük penceresini geçtikten sonra kullanıcı, otomatik olarak ve kalıcı olarak silinir.

Geri yüklenebilen kullanıcılarınızın görüntülemek, silinen bir kullanıcıyı geri yükleme veya Azure portalında Azure Active Directory (Azure AD) kullanarak bir kullanıcıyı kalıcı olarak sil.

>[!Important]
>Ne, ne de Microsoft müşteri desteği kalıcı olarak silinmiş bir kullanıcıyı geri yükleyebilirsiniz.

## <a name="required-permissions"></a>Gerekli izinler
Geri yüklemek ve kullanıcılar kalıcı olarak silmek için aşağıdaki rollerden biri olması gerekir.

- Şirket Yöneticisi

- Partner Tier1 Desteği

- Partner Tier2 Desteği

- Kullanıcı Hesabı Yöneticisi

## <a name="view-your-restorable-users"></a>Geri yüklenebilen kullanıcılarınızın görüntüleyin
30 günden kısa süre önce silinmiş olan tüm kullanıcıları görebilirsiniz. Bu kullanıcılar geri yüklenebilir.

### <a name="to-view-your-restorable-users"></a>Geri yüklenebilen kullanıcılarınızın görüntülemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **silinmiş kullanıcılarını**.

    Geri yüklemek kullanılabilir olan kullanıcıların listesini gözden geçirin.

    ![Kullanıcılar - silinen Kullanıcılar sayfasında, yine de geri kullanıcılarla](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Yakın zamanda silinen bir kullanıcıyı geri yükleme
Bir kullanıcının hesabı askıya alındı, ancak tüm ilgili dizin bilgileri korunur. Bir kullanıcıyı geri yükleme yaptığınızda bu dizin bilgileri de geri yüklenir.

### <a name="to-restore-a-user"></a>Bir kullanıcıyı geri yükleme için
1. Üzerinde **kullanıcılar - silinen kullanıcılar** sayfasında aramak ve kullanıcıların kullanımına birini seçin. Örneğin, _Mary Parker_.

2. Seçin **geri yükleme kullanıcı**.

    ![Kullanıcılar - geri yükleme kullanıcı seçeneğinin vurgulandığı ile silinen kullanıcılar sayfası](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Bir kullanıcıyı kalıcı olarak sil
Otomatik silme için 30 gün beklemeden dizininizdeki bir kullanıcı kalıcı olarak silebilirsiniz. Siz başka bir yönetici ya da Microsoft müşteri desteği tarafından kalıcı olarak silinmiş bir kullanıcıyı geri yüklenemez.

>[!Note]
>Kalıcı olarak bir kullanıcı yanlışlıkla silerseniz, yeni bir kullanıcı oluşturun ve önceki tüm bilgileri el ile girmeniz gerekir. Yeni kullanıcı oluşturma hakkında daha fazla bilgi için bkz. [ekleme veya silme kullanıcılar](add-users-azure-active-directory.md).

### <a name="to-permanently-delete-a-user"></a>Bir kullanıcıyı kalıcı olarak silmek için

1. Üzerinde **kullanıcılar - silinen kullanıcılar** sayfasında aramak ve kullanıcıların kullanımına birini seçin. Örneğin, _Rae Huff_.

2. Seçin **kalıcı olarak silmek**.

    ![Kullanıcılar - geri yükleme kullanıcı seçeneğinin vurgulandığı ile silinen kullanıcılar sayfası](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Sonraki adımlar
Geri veya kullanıcılarınızın silinmiş sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Başka bir dizinden konuk kullanıcılar ekleme](../b2b/what-is-b2b.md) 

Diğer kullanılabilir kullanıcı yönetim görevleri hakkında daha fazla bilgi için [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
