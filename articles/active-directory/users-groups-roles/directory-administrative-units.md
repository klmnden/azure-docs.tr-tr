---
title: Azure Active Directory'de Yönetim birimleri Yönetimi önizlemesi
description: Daha ayrıntılı temsilci izinleri Azure Active Directory'de Yönetim birimleri kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.component: users-groups-roles
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 0884726e59d9ab3f5a5cfe7bb0608f6b5a5da250
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450306"
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Azure AD'de - genel Önizleme idari Birim Yönetimi
Bu makalede yeni bir Azure Active Directory kapsayıcısı kullanıcılar ve kullanıcıların bir alt kümesine uygulanan ilkeler kümelerine üzerinden yönetimsel izinlere temsilci için kullanılabilir kaynakların Yönetim birimleri – açıklanır. Azure Active Directory'de Yönetim birimleri merkezi yöneticileri izinler için bölgesel yöneticileri temsilci veya ayrıntılı düzeyde ilke ayarlamak için etkinleştirin.

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
