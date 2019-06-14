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
ms.date: 04/01/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: d69bd931f2f8c72fd1e6fc79c16662ea367617d6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60248252"
---
# <a name="restore-or-remove-a-recently-deleted-user-using-azure-active-directory"></a>Geri yükleme ya da Azure Active Directory kullanarak son silinen bir kullanıcıyı kaldırma
Bir kullanıcı sildikten sonra hesabı 30 gün için askıya alınmış durumda kalır. Bu 30 günlük penceresi sırasında kullanıcı hesabı, tüm özellikleriyle birlikte geri yüklenebilir. Bu 30 günlük penceresini geçtikten sonra kullanıcı, otomatik olarak ve kalıcı olarak silinir.

Geri yüklenebilen kullanıcılarınızın görüntülemek, silinen bir kullanıcıyı geri yükleme veya Azure portalında Azure Active Directory (Azure AD) kullanarak bir kullanıcıyı kalıcı olarak sil.

>[!Important]
>Ne, ne de Microsoft müşteri desteği kalıcı olarak silinmiş bir kullanıcıyı geri yükleyebilirsiniz.

## <a name="required-permissions"></a>Gerekli izinler
Geri yüklemek ve kullanıcılar kalıcı olarak silmek için aşağıdaki rollerden biri olması gerekir.

- Genel yönetici

- Partner Tier1 Desteği

- Partner Tier2 Desteği

- Kullanıcı Yöneticisi

## <a name="view-your-restorable-users"></a>Geri yüklenebilen kullanıcılarınızın görüntüleyin
30 günden kısa süre önce silinmiş olan tüm kullanıcıları görebilirsiniz. Bu kullanıcılar geri yüklenebilir.

### <a name="to-view-your-restorable-users"></a>Geri yüklenebilen kullanıcılarınızın görüntülemek için
1. Oturum [Azure portalında](https://portal.azure.com/) kuruluş için bir genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **silinmiş kullanıcılarını**.

    Geri yüklemek kullanılabilir olan kullanıcıların listesini gözden geçirin.

    ![Kullanıcılar - silinen Kullanıcılar sayfasında, yine de geri kullanıcılarla](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Yakın zamanda silinen bir kullanıcıyı geri yükleme

Kuruluştan bir kullanıcı hesabı silindiğinde, hesabı askıya alınmış durumda ve tüm ilişkili kuruluş bilgileri korunur. Bir kullanıcıyı geri yükleme yaptığınızda bu kuruluş bilgileri de geri yüklenir.

> [!Note]
> Kullanıcı geri yüklendikten sonra olsa bile hiçbir lisans lisansları için kullanılabilir silme anda kullanıcıya atanmış lisans da geri yüklenir. Ardından kullanıyorsa daha fazla lisans satın aldığınız fazlasını, kuruluşunuzun geçici olarak lisans kullanımı için uyumsuz olabilir.

### <a name="to-restore-a-user"></a>Bir kullanıcıyı geri yükleme için
1. Üzerinde **kullanıcılar - silinen kullanıcılar** sayfasında aramak ve kullanıcıların kullanımına birini seçin. Örneğin, _Mary Parker_.

2. Seçin **geri yükleme kullanıcı**.

    ![Kullanıcılar - geri yükleme kullanıcı seçeneğinin vurgulandığı ile silinen kullanıcılar sayfası](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Bir kullanıcıyı kalıcı olarak sil
Bir kullanıcı kuruluşunuzdan otomatik olarak silinmesi için 30 gün beklemeden kalıcı olarak silebilirsiniz. Siz başka bir yönetici ya da Microsoft müşteri desteği tarafından kalıcı olarak silinmiş bir kullanıcıyı geri yüklenemez.

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

- [Başka bir kuruluşta Konuk kullanıcıları ekleme](../b2b/what-is-b2b.md)

Diğer kullanılabilir kullanıcı yönetim görevleri hakkında daha fazla bilgi için [Azure AD kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
