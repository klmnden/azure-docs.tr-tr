---
title: "Azure'da kaynak erişimini anlama | Microsoft Docs"
description: "Bu konuda tam Azure portalında kaynak erişimi denetlemek için abonelik yöneticileri kullanmayla ilgili kavramları açıklar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 9492afeda8c11d9d4df866e416a2c2c7e1684569
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="understanding-resource-access-in-azure"></a>Azure'da kaynak erişimini anlama

Azure erişim denetimi faturalandırma açısından başlatır. Ziyaret ederek erişilen bir Azure hesabı sahibinin [Azure hesaplar Merkezi](https://account.windowsazure.com/subscriptions), Hesap Yöneticisi'nin (AA) değil. Abonelik faturalama için bir kapsayıcı olsa da, bunlar aynı zamanda bir güvenlik sınırı hareket: her abonelik bir Hizmet Yöneticisi (kimin eklemek, kaldırmak ve bu abonelik Azure kaynaklarında kullanarak değiştirme SA) sahip [Azure portal](https://portal.azure.com/). Yeni bir aboneliğin varsayılan SA AA olur ancak AA SA Azure hesapları Center'da değiştirebilirsiniz.

<br><br>![Azure hesapları][1]

Abonelikler, bir dizin ile bir ilişkilendirme de vardır. Dizin, bir kullanıcı kümesini tanımlar. Bu kullanıcılar iş veya Okul dizini oluşturulmuş olabilir veya dış kullanıcılar (diğer bir deyişle, Microsoft Accounts) olabilir. Abonelikler, bir alt kümesini Hizmet Yöneticisi (SA) veya ortak yönetici (CA) olarak atanmış olan dizin kullanıcılar tarafından erişilebilir; eski nedeniyle, Microsoft Accounts (eskiden Windows Live kimliği) SA veya CA dizinde bulunduğundan olmadan atanabilir, yalnızca istisnadır.

<br><br>![Azure erişim denetimi][2]

Klasik Azure portalı içinde işlevselliğini etkinleştiren bir abonelik kullanarak ilişkili dizini değiştirmek için bir Microsoft Account kullanarak imzalanmış SAs **dizini Düzenle** komutunu  **Abonelikler** sayfasındaki **ayarları**. Bu işlem bu aboneliğin erişim denetimini etkileri olduğuna dikkat edin.

> [!NOTE]
> **Dizini Düzenle** Klasik Azure portalındaki komutu iş kullanarak oturum açan kullanıcılar için kullanılabilir değil ya da Okul hesabı olduğundan, bu hesaplar yalnızca ait oldukları dizin için oturum açın.
> 
> 

<br><br>![Basit kullanıcı oturum açma akışı][3]

En basit durumda, bir kuruluş (örneğin, Contoso) faturalama zorlamak ve aynı kümesini abonelikler arasında erişim denetimi. Diğer bir deyişle, tek bir Azure hesabı tarafından sahip olunan aboneliklere ilişkili dizindir. Başarılı oturum açma sırasında Klasik Azure portalı, kullanıcıların iki koleksiyonları (turuncu önceki çizimde gösterilen) kaynakların bakın:

* (Veya yabancı sorumlu eklenen kaynaklanan) kullanıcı hesapları bulunduğu dizinleri. Dizinlerinizi günlüğe nerede bağımsız olarak her zaman gösterilecek şekilde oturum açma için kullanılan dizin Bu hesaplama için uygun olmadığını unutmayın.
* Oturum açma için kullanılan dizin ile ilişkili olan ve kullanıcı (SA veya CA oldukları) erişebileceğiniz abonelikleri parçası olan kaynaklar.

<br><br>![Birden çok aboneliğe ve dizine sahip kullanıcı][4]

Birden çok dizin Aboneliklerde kullanıcılarla geçerli bağlamı, Klasik Azure portalı abonelik filtresini kullanarak geçiş yapmak için sahipsiniz. Perde altında bu ayrı bir oturum açma için farklı bir dizin olur, ancak bu sorunsuz çoklu oturum açma (SSO) kullanılarak gerçekleştirilir.

Kaynakları abonelikler arasında taşıma gibi işlemleri abonelikleri sonucunda bu tek bir dizin görünüm daha zor olabilir. Kaynak aktarımı gerçekleştirmek için ilk kullanım için gerekebilir **dizini Düzenle** abonelikler sayfasında komutunu **ayarları** abonelik aynı dizine ilişkilendirilecek.

## <a name="next-steps"></a>Sonraki Adımlar
* Bir Azure aboneliğine yönelik olarak yöneticileri değiştirme hakkında daha fazla bilgi için bkz. [Azure yönetici rollerini ekleme veya değiştirme](../billing/billing-add-change-azure-subscription-administrator.md)
* Azure Active Directory Azure aboneliğinize ilişkilendirilme şekli ile ilgili daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](active-directory-how-subscriptions-associated-directory.md)
* Azure AD'de rol atama hakkında daha fazla bilgi için bkz. [Azure Active Directory'de yönetici rolü atama](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
