---
title: Bir Azure AD galeri uygulamasına kullanıcı hazırlamayı olan alma saat veya daha fazla bilgi | Microsoft Docs
description: Uygulamanıza sağlama neden kaydolmadığı nasıl beklediğinizden uzun sürüyor
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 28e688edffe79990b76b92b17636484eab353aa3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55157967"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Alma saat veya daha fazla olduğundan, bir Azure AD galeri uygulamasına kullanıcı sağlama

Önce bir uygulama için otomatik sağlamayı etkinleştirme, ilk eşitleme herhangi bir 20 dakika veya Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat alabilir. 

İlk eşitleme sonrasında sonraki eşitlemeler sağlama hizmeti sonraki eşitlemeler performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depolar daha hızlı olması.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansını artırma

İlk eşitleme birkaç saat sürerse, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsamı belirleme filtreleri.** Kapsam belirleme filtrelerini sağlama hizmeti, kullanıcıların belirli bir öznitelik değerlerine göre filtreleme yaparak Azure AD'den ayıklar veri yönelik ince ayarlar yapmanızı sağlar. Kapsam filtreleri ile ilgili daha fazla bilgi için bkz: [kapsam filtreleri ile öznitelik tabanlı uygulama sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)

