---
title: Yönetilmeyen bir dizini - Azure Active Directory iş veya Okul hesabınızı kapatmak | Microsoft Docs
description: Yönetilmeyen bir Azure Active Directory iş veya Okul hesabınızı kapatmak nasıl.
services: active-directory
author: rolyon
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 05/20/2019
ms.author: rolyon
ms.reviewer: ''
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39b359ef7feeaec541ba17e98a5d1e9b74c6403a
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65958008"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-directory"></a>Yönetilmeyen bir dizini iş veya Okul hesabınızı kapatın

Yönetilmeyen bir Azure Active Directory (Azure AD) kuruluş ve kullanıcının bundan böyle olması durumunda söz konusu kuruluştaki uygulamaları kullanın veya tüm ilişkilendirmeleri, hesabınızı dilediğiniz zaman kapatabilirsiniz korumak gerekir. Yönetilmeyen bir dizin genel Yöneticisi yok. Kullanıcılar yönetilmeyen bir dizin üzerinde kendi hesaplarına bir yöneticiye başvurun gerek kalmadan kapatabilirsiniz.

Kullanıcılar yönetilmeyen bir dizinde, genellikle sırasında Self Servis kaydolma oluşturulur. Örnek bir ücretsiz hizmete kaydolan bir bilgi çalışanı bir kuruluşta olabilir. Self Servis kayıt hakkında daha fazla bilgi için bkz: [Self Servis nedir kaydolmak için Azure Active Directory?](directory-self-service-signup.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Hesabınızı kapatabilirsiniz önce aşağıdaki öğeleri onaylaması:

* Yönetilmeyen bir Azure kullanıcısı olduğundan emin olun AD dizini. Yönetilen bir dizine ait olması durumunda hesabınızı kapatamazsınız. Yönetilen bir dizine ait ve hesabınızı kapatmak istiyorsanız yöneticinize başvurmanız gerekir. Yönetilmeyen bir dizine ait olup olmadığını belirleme hakkında daha fazla bilgi için bkz: [yönetilmeyen Kiracısından kullanıcı silme](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant).

* Korumak istediğiniz verileri kaydedin. Bir dışarı aktarma isteği gönderme hakkında daha fazla bilgi için bkz: [erişme ve yönetilmeyen kiracılar için sistem tarafından oluşturulan günlükleri dışarı aktarma](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants).

> [!WARNING]
> Hesabınızı kapatma işlemi geri alınamaz. Hesabınızı kapattıktan, tüm kişisel veriler kaldırılır. Artık hesabınızı ve verilerinizi hesabınızla ilişkili erişiminiz.

## <a name="close-your-account"></a>Hesabınızı kapatın

Bir yönetilmeyen iş veya Okul hesabını kapatmak için şu adımları izleyin:

1. Oturum [hesabınızı kapatmak](https://go.microsoft.com/fwlink/?linkid=873123), kapatmak istediğiniz hesabı kullanarak.

1. Üzerinde **veri isteklerim**seçin **hesabı Kapat**.

    ![Veri isteklerim - hesabı Kapat](./media/users-close-account/close-account.png)

1. Onay iletisine gözden geçirin ve ardından **Evet**.

    ![Verilerim istekleri - Kapatma Onayı](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Self Servis nedir kaydolmak için Azure Active Directory?](directory-self-service-signup.md)
- [Yönetilmeyen Kiracısından kullanıcı silme](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Erişim ve yönetilmeyen kiracılar için sistem tarafından oluşturulan günlükleri dışarı aktarma](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)
