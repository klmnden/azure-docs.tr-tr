---
title: "Azure Active Directory'de idari birim yönetimi Önizleme"
description: "Daha ayrıntılı Azure Active Directory izinleri için temsilci seçme için idari birim kullanma"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 1e85300676eeee9259e40faa0e0ede94a36f6167
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Azure AD - genel Önizleme idari Birim Yönetimi
Bu makalede idari birim – yeni bir Azure Active Directory kapsayıcısı kullanıcılar ve kullanıcı alt kümesine ilkelerini uygulama kümelerine üzerinden yönetim izinleri temsilci için kullanılabilir kaynakların açıklanmaktadır. Azure Active Directory'de merkezi yöneticileri temsilci izinleri bölgesel yöneticilere veya ayrıntılı bir düzeyde İlkesi ayarlamak için idari birim etkinleştirin.

Bu, bağımsız bölümler, örneğin, birbirinden bağımsız olan birçok otonom okullar (iş okul, mühendislik Okul ve benzeri) oluşan bir büyük üniversite olan kuruluşlarda yararlıdır. Bu tür bölümler, özellikle kendi bölme için ilkeler ayarlama erişimi denetlemek ve kullanıcıları yönetmek, kendi BT yöneticileri vardır. Merkezi Yöneticiler olması istiyorsanız bu bölümsel vermek kullanıcılar kendi belirli bölümler üzerinden Yöneticiler izinleri. Daha belirgin olarak bu örneği kullanarak, merkezi bir yönetici, örneği için belirli bir okul (iş Okul) için bir yönetim birimi oluşturabilir ve yalnızca iş Okul kullanıcılarla doldurmak. Daha sonra bir merkezi yönetici iş Okul BT personeli kapsamlı bir role ekleyebilirsiniz diğer bir deyişle, iş Okul yönetim izinleri BT personeli yalnızca Okul yönetim departman verin.

> [!IMPORTANT]
> Yalnızca Azure Active Directory Premium etkinleştirirseniz yönetimsel birim kapsamlı yönetici rollerini atayabilir. Daha fazla bilgi için bkz: [Azure AD Premium ile çalışmaya başlama](active-directory-get-started-premium.md).
>


Merkezi yöneticisinin bakış açısından, bir yönetici oluşturulan ve kaynaklarla doldurulan bir dizin nesnesi birimdir. **Bu önizleme sürümünde bu kaynakları yalnızca kullanıcılar olabilir.** Oluşturulur ve doldurulur sonra Yönetim birimi verilen yönetim biriminde bulunan kaynakları üzerinden erişimi kısıtlamak için kapsam olarak kullanılabilir.

## <a name="managing-administrative-units"></a>İdari Birim Yönetimi
Bu önizleme sürümünde oluşturun ve Azure Active Directory için Windows PowerShell modülü cmdlet öğelerini kullanarak idari birim yönetin. Bunun hakkında daha fazla bilgi edinmek için [idari birim ile çalışma](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Sözdizimi, parametre açıklamaları ve örnekler dahil olmak üzere idari birim yönetmek için Azure AD modülü cmdlet'ler hakkında bilgi ve yazılım gereksinimleri ve Azure AD modülünü yükleme hakkında daha fazla bilgi için bkz: [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory sürümleri](active-directory-editions.md)
