---
title: Bir Azure AD galeri uygulamasına kullanıcı hazırlamayı olan alma saat veya daha fazla bilgi | Microsoft Docs
description: Uygulamanıza sağlama neden kaydolmadığı nasıl beklediğinizden uzun sürüyor
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8622a7bbd913710a2173399048baab7067d2fae7
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65783809"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Alma saat veya daha fazla olduğundan, bir Azure AD galeri uygulamasına kullanıcı sağlama

Önce bir uygulama için otomatik sağlamayı etkinleştirme, ilk eşitleme herhangi bir 20 dakika veya Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat alabilir. 

İlk eşitleme sonrasında sonraki eşitlemeler sağlama hizmeti sonraki eşitlemeler performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depolar daha hızlı olması.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansını artırma

İlk eşitleme birkaç saat sürerse, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsamı belirleme filtreleri.** Kapsam belirleme filtrelerini sağlama hizmeti, kullanıcıların belirli bir öznitelik değerlerine göre filtreleme yaparak Azure AD'den ayıklar veri yönelik ince ayarlar yapmanızı sağlar. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)

