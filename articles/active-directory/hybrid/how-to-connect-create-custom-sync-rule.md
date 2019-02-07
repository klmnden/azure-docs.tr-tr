---
title: Bir Azure AD CONNECT'te eşitleme Kuralı'nı özelleştirme | Microsoft Docs
description: Bu konuda, Azure AD Connect yükleme sorunlarını gidermek için adımları sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 1407df2dbc0cc590ea919ca0ead727959b1e08b4
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773924"
---
# <a name="how-to-customize-a-synchronization-rule"></a>Eşitleme kuralı özelleştirme

## <a name="recommended-steps"></a>**Önerilen Adımlar**

Eşitleme kuralı Düzenleyicisi, düzenlemek veya yeni bir eşitleme kuralı oluşturmak için kullanabilirsiniz. Eşitleme kuralları için değişiklik yapmak için Gelişmiş bir kullanıcı olması gerekir. Yanlış değişikliklerin hedef dizin nesneleri silme işlemi neden olabilir. Lütfen okuma [önerilen belgeleri](#recommended-documents) eşitleme kuralları uzmanlığı elde etmek için. Değiştirmek için eşitleme kuralı geçtikleri adımları izleyin:

* Uygulama menüsünde aşağıda gösterildiği gibi masaüstü eşitleme düzenleyicisinden başlatın:

    ![Eşitleme kuralı Düzenleyicisi menüsü](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/syncruleeditormenu.png)

* Varsayılan bir eşitleme kuralı özelleştirmek için standart varsayılan kuralı bir kopyasını oluşturur ve bunu devre dışı eşitleme kuralları Düzenleyicisi, "Düzenle" düğmesine tıklayarak mevcut bir kuralı'nı kopyalayın. 100'den daha az bir önceliğe sahip kopyalanan bir kuralı kaydedin.  Bir öznitelik akışı çakışması varsa hangi kural bir çakışma (düşük sayısal değer) WINS öncelik belirler.

    ![Eşitleme kuralı Düzenleyicisi](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/clonerule.png)

* İdeal olarak belirli bir öznitelik değiştirirken, kopyalanan kuralda değiştirme özniteliği yalnızca tutmalısınız.  Daha sonra kopyalanan kuralından değiştirilmiş öznitelik gelir ve diğer özniteliklerini varsayılan standart kuralından seçilir varsayılan kuralı etkinleştirin. 

* Lütfen burada değiştirilen özniteliğin hesaplanan değeri NULL, kopyalanan kuralınızı ve varsayılan standart NULL olmadığından bu durumda daha sonra değil kuralı olduğunu unutmayın. NULL değer kazanır ve NULL değer yerini alır. Bir NULL değer olmasını istemiyorsanız, bir NULL değer daha sonra Ata AuthoritativeNull, kopyalanan bir kuralı değiştirin.

* Değiştirilecek bir **giden** kural, eşitleme kuralı Düzenleyicisi'nden filtresini değiştirin.

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect eşitlemesi: Teknik Kavramlar](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect eşitlemesi: Mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect eşitlemesi: Bildirim Temelli Sağlamayı Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect eşitlemesi: Bildirim Temelli Sağlama İfadelerini Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect eşitlemesi: Varsayılan yapılandırmayı anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect eşitlemesi: Kullanıcıları, Grupları ve Kişileri Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect eşitlemesi: Gölge öznitelikler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md).
- [Karma kimlik nedir? ](whatis-hybrid-identity.md).