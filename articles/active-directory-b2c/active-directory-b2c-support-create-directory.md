---
title: 'Azure Active Directory B2C: oluşturma kiracılar sorunlarını giderme | Microsoft Docs'
description: Sorunlar ve çözümleri bir Azure Active Directory veya Azure Active Directory B2C kiracısı oluşturma.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.openlocfilehash: 3daf232d7fb1f95c390c1e6b8c168ec585484c65
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
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

