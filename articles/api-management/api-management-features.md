---
title: Azure API Management katmanları özellik tabanlı karşılaştırması | Microsoft Docs
description: Bu makalede, API Management katmanları sundukları özelliklere göre karşılaştırır.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: 34c4ef2885a82b6c392b814eeb624e616e341d48
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304344"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management katmanları özellik tabanlı karşılaştırması

Her API Management [fiyatlandırma katmanı](https://aka.ms/apimpricing) özelliklerinin ve birim başına ayrı bir kümesi sunar [kapasite](api-management-capacity.md). Aşağıdaki tabloda her katmanlarının temel özellikleri özetlenmektedir. Bazı özellikler farklı çalışır veya katmana bağlı olarak farklı özelliklere sahiptir. Bu gibi durumlarda farklar bu tek tek özellikleri açıklayan belge makalelerine çağrılır.

| Özellik                                                                                      | Tüketim | Geliştirici      | Temel          | Standart       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD tümleştirmesi<sup>1</sup>                                                             | Hayır                            | Evet            | Hayır             | Evet            | Evet            |
| Sanal ağ (VNet) desteği                                                               | Hayır                            | Evet            | Hayır             | Hayır             | Evet            |
| Çok bölgeli dağıtım                                                                      | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Birden çok özel etki alanı adı                                                                 | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Geliştirici Portalı<sup>2</sup>                                                                 | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik önbelleği                                                                               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik analitik                                                                           | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [SSL ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Evet                            | Evet            | Evet            | Evet            | Evet            |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Evet                           | Evet            | Evet            | Evet            | Evet            |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Evet                | Evet            | Evet            | Evet            | Evet            |
| [Yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [Git üzerinden Yönetimi](api-management-configuration-repository-git.md)                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Doğrudan yönetim API'si                                                                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Azure İzleyici günlükleri ve ölçümler                                                               | Hayır                | Evet            | Evet            | Evet            | Evet            |

<sup>1</sup> Azure AD'ye kullanılmasına olanak tanır (ve Azure AD B2C) bir kimlik sağlayıcısı kullanıcı için oturum açın Geliştirici portalında.<br/>
<sup>2</sup> ilgili işlevleri dahil olmak üzere örn kullanıcıları, grupları, sorunları, uygulamaları ve e-posta şablonları ve bildirimleri.<br/>
