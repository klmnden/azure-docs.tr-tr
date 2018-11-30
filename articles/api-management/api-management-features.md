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
ms.openlocfilehash: a00328608c582dcd28dbc78b5b56829f9d1ab500
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585585"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management katmanları özellik tabanlı karşılaştırması

Her API Management [fiyatlandırma katmanı](https://aka.ms/apimpricing) özelliklerinin ve birim başına ayrı bir kümesi sunar [kapasite](api-management-capacity.md). Aşağıdaki tabloda her katmanlarının temel özellikleri özetlenmektedir. Bazı özellikler farklı çalışır veya katmana bağlı olarak farklı özelliklere sahiptir. Bu gibi durumlarda farklar bu tek tek özellikleri açıklayan belge makalelerine çağrılır.

| Özellik                                                                                      | Tüketim<sup>Önizleme</sup> | Geliştirici      | Temel          | Standart       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD tümleştirmesi<sup>1</sup>                                                             | Hayır                            | Evet            | Hayır             | Evet            | Evet            |
| Sanal ağ (VNet) desteği                                                               | Hayır                            | Evet            | Hayır             | Hayır             | Evet            |
| Çok bölgeli dağıtım                                                                      | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Birden fazla özel etki alanı adı                                                                 | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Geliştirici Portalı<sup>2</sup>                                                                 | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik önbelleği                                                                               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik analiz                                                                           | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [SSL ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Evet                           | Hayır<sup>3</sup> | Hayır<sup>3</sup> | Hayır<sup>3</sup> | Hayır<sup>3</sup> |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Hayır<sup>4</sup>                | Evet            | Evet            | Evet            | Evet            |
| [Yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [Git üzerinden Yönetimi](api-management-configuration-repository-git.md)                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Doğrudan yönetim API'si                                                                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Azure İzleyici günlükleri ve ölçümler                                                               | Hayır<sup>5</sup>                | Evet            | Evet            | Evet            | Evet            |

<sup>1</sup> Azure AD'ye kullanılmasına olanak tanır (ve Azure AD B2C) Geliştirici portalında kullanıcı oturumu açma için kimlik sağlayıcısı.<br/>
<sup>2</sup> ilgili işlevleri dahil olmak üzere örn kullanıcıları, grupları, sorunları, applicationsn ve e-posta şablonları ve bildirimleri.<br/>
<sup>3</sup> Bu katman için dış önbellek desteği yakında.<br/>
<sup>4</sup> istemci sertifikası kimlik doğrulaması, tüketim katmanına genel kullanılabilirliğini önce eklenir.<br/>
<sup>5</sup> tüketim katmana tam Azure İzleyici destek eklenecektir.
