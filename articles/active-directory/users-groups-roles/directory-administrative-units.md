---
title: Yönetim birimleri Yönetimi (Önizleme) - Azure Active Directory | Microsoft Docs
description: Daha ayrıntılı temsilci izinleri Azure Active Directory'de Yönetim birimleri kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77f1a6e5b1e8191c1497e437cc26e1caf1255ba7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60472379"
---
# <a name="administrative-units-management-in-azure-active-directory-public-preview"></a>Yönetim birimleri Yönetimi Azure Active Directory'de (genel Önizleme)

Bu makalede yeni bir Azure Active Directory (Azure AD) kapsayıcı kullanıcılar alt kümeleri üzerinde yönetim izinleri için temsilci seçme ve bir kullanıcı alt ilkeleri uygulamak için kullanılan kaynakların Yönetim birimleri – açıklanır. Azure Active Directory'de Yönetim birimleri merkezi yöneticileri izinler için bölgesel yöneticileri temsilci veya ayrıntılı düzeyde ilke ayarlamak için etkinleştirin.

Bu bağımsız bölümlere, örneğin, birbirinden bağımsız olan birçok otonom okullar (iş, okul, mühendislik Okul vb.), yapılan büyük üniversite bir kuruluşlarıyla faydalıdır. Bu tür bölümler, erişim denetimi, kullanıcıları yönetme ve kendi bölme için özel ilkeler, kendi BT yöneticileri vardır. Bu bölümsel vermek mümkün olmasını istediğiniz merkezi yöneticileri kullanıcıları, belirli Departmanlar üzerinden yöneticileri izinleri. Daha açık belirtmek gerekirse bu örneği kullanarak, bir merkezi Yöneticisi, örneği için bir yönetim birimindeki belirli bir school (iş ve Okul) için oluşturabilir ve yalnızca iş Okul kullanıcılar ile doldurun. Daha sonra merkezi bir yönetici iş Okul BT personeli kapsamlı bir role ekleyebilirsiniz. diğer bir deyişle, BT çalışanlarının iş Okul yönetimsel izinler yalnızca iş Okul yönetim birimi verin.

> [!IMPORTANT]
> Yönetim birimleri kullanmak için bir Azure Active Directory Premium lisansı ve tüm kullanıcılar için Azure Active Directory temel Lisansı yönetim birimi için bir yönetim birimi kapsamlı yönetim gerektirir. Daha fazla bilgi için [Azure AD Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md).
>


Merkezi yönetici açısından bakıldığında, bir yönetim birimi oluşturulan ve kaynakları ile doldurulmuş bir dizin nesnesidir. **Bu önizleme sürümünde bu kaynakların yalnızca kullanıcılar olabilir.** Oluşturulur ve doldurulur sonra bir yönetim birimi yalnızca yönetici biriminde bulunan kaynaklar üzerinden verilen kısıtlamak için kapsamı olarak kullanılabilir.

## <a name="managing-administrative-units"></a>Yönetim birimleri yönetme
Bu önizleme sürümünde oluşturabilir ve Azure Active Directory için Windows PowerShell modülü cmdlet'lerini kullanarak yönetim birimlerini yönetme. Bunun hakkında daha fazla bilgi edinmek için [Yönetim birimleri ile çalışma](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Yazılım gereksinimleri ve Azure AD modülünü yükleme hakkında daha fazla bilgi ve söz dizimi, parametre açıklamalarını ve örnekleri dahil olmak üzere Yönetim birimleri yönetmek için Azure AD modülü cmdlet'leri hakkında daha fazla bilgi için bkz. [Azure Active Dizin PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md)
