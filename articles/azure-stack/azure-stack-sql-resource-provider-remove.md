---
title: SQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: c2686a2d5241af46e70263d1827028aa7e9b2138
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldırma

SQL kaynak sağlayıcısı kaldırmak için önce tüm bağımlılıkları kaldırmak için gereklidir:

1. SQL kaynak sağlayıcısı bağdaştırıcısı bu sürümü için indirdiğiniz özgün dağıtım paketi olduğundan emin olun.

2. Tüm kullanıcı veritabanlarını kaynak Sağlayıcısı'ndan silinmesi gerekir. (Kullanıcı veritabanlarını silerek verileri silmez.) Bu görev, kullanıcılar tarafından gerçekleştirilmelidir.

3. Yönetici barındırma sunucuları SQL kaynak sağlayıcısı bağdaştırıcısından silmeniz gerekir.

4. Yönetici SQL kaynak sağlayıcısı bağdaştırıcısı başvuru herhangi bir plan silmeniz gerekir.

5. Yönetici, tüm SKU'ları ve SQL kaynak sağlayıcısı bağdaştırıcısı ile ilişkili kotaları silmeniz gerekir.

6. Aşağıdaki öğeleri içeren dağıtım betiği yeniden çalıştırın:
    - Parametre kaldırma
    - Azure Resource Manager uç noktaları
    - DirectoryTenantID
    - Hizmet yöneticisi hesabı için kimlik bilgileri

