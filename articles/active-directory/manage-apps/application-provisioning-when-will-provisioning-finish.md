---
title: Bir Azure AD galeri uygulamasına kullanıcı hazırlamayı olan alma saat veya daha fazla bilgi | Microsoft Docs
description: Uygulamanıza sağlama neden kaydolmadığı nasıl beklediğinizden uzun sürüyor
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 2b46fecc44803130e4f79667d0e083314c4419c2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357798"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Alma saat veya daha fazla olduğundan, bir Azure AD galeri uygulamasına kullanıcı sağlama

Önce bir uygulama için otomatik sağlamayı etkinleştirme, ilk eşitleme herhangi bir 20 dakika veya Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat alabilir. 

İlk eşitleme sonrasında sonraki eşitlemeler sağlama hizmeti sonraki eşitlemeler performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depolar daha hızlı olması.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansını artırma

İlk eşitleme birkaç saat sürerse, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsamı belirleme filtreleri.** Kapsam belirleme filtrelerini sağlama hizmeti, kullanıcıların belirli bir öznitelik değerlerine göre filtreleme yaparak Azure AD'den ayıklar veri yönelik ince ayarlar yapmanızı sağlar. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)

