---
title: Azure Active Directory B2C oluşturma kiracılar sorunlarını giderme | Microsoft Docs
description: Sorunlar ve çözümleri bir Azure Active Directory veya Azure Active Directory B2C kiracısı oluşturma.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 90d9d2fb80dfbd094754850b7d1270a5fafcdd96
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712513"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Bir Azure Active Directory veya Azure Active Directory B2C kiracısı oluşturma sorunlarını giderme 

## <a name="create-an-azure-ad-tenant"></a>Azure AD kiracısı oluşturma
İlk denemede bir Azure Active Directory (Azure AD) Kiracı oluşturamıyorsanız, yeniden deneyin. Sorun devam ederse, Azure desteğine başvurun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma
Sorunlarla karşılaşırsanız olduğunda, [bir Azure Active Directory B2C oluşturun (Azure AD B2C) Kiracı](active-directory-b2c-get-started.md), aşağıdaki seçenekleri deneyin:

* Azure AD B2C kiracısı kiracılar listenizde göstermez, Kiracı oluşturmak yeniden deneyin.
* Azure AD B2C kiracısı kiracılar listenizde göstermez ve aşağıdaki hata iletisini görüyorsanız, Kiracı silin ve yeniden oluşturun:

    "'Contosob2c' B2C kiracısı oluşturma işlemini tamamlayamadı. Lütfen bu ziyaret [bağlantı](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) daha fazla yardım için. "
* Var olan Azure AD B2C kiracısı silin ve yeniden oluşturun aynı etki alanı adını kullanarak, bilinen sorunlar vardır. Yeni bir Azure AD B2C kiracısı oluşturduğunuzda, farklı etki alanı adı kullanmanız gerekir.
* Bu çözümler işe yaramazsa, Azure desteğine başvurun. Daha fazla bilgi için bkz: [Azure AD B2C için dosya desteği istekleri](active-directory-b2c-support.md).

