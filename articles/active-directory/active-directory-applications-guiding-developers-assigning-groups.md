---
title: "Azure AD uygulamalarınız için grupları atama | Microsoft Docs"
description: "Grup ataması Azure uygulamaları için gerçekleştirme."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: 471099c58a7fc1dfd520e693873a76f64c0fbf0a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a>Bir uygulamayı Azure Active Directory grupları atama
Uygulamaya kullanıcılar ve gruplar atamadan önce kullanıcı ataması gerektirir. Kullanıcı Ataması gerektirecek şekilde öğrenmek için bkz: [gerektiren kullanıcı ataması](active-directory-applications-guiding-developers-requiring-user-assignment.md) makalesi.

Bu makale, zaten grupları, bu uygulama için kullandığınız active Directory'deki oluşturduğunuzu varsayar.

## <a name="assigning-groups-to-an-application"></a>Uygulama grupları atama
1. Azure portalı bir yönetici hesabı ile oturum açın.
2. Tıklayın **tüm öğeleri** ana menü öğesi.
3. Uygulama için kullanmakta olduğunuz dizini seçin.
4. Tıklayın **uygulamaları** sekmesi.
5. Uygulama bu dizinle ilişkili uygulamalar listesini seçin.
6. Tıklatın **kullanıcılar ve GRUPLAR** sekmesi.
7. Kullanarak active Directory'de Grup listesini filtrelemek **grupları** açılır liste.
8. Grubu seçin.
9. Tıklatın **ATAMAK**.
10. Tıklatın **Evet** istendiğinde.

## <a name="next-steps"></a>Sonraki Adımlar
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
