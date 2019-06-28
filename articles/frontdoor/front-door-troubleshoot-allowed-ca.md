---
title: Sertifika yetkililerini Azure ön kapısı hizmeti üzerinde özel HTTPS'yi etkinleştirmek için izin verilen | Microsoft Docs
description: Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için kendi sertifika kullanıyorsanız, bunu oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: e2bab9f9e1ae099952b34e66f7250e7ecbc8e689
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330740"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Sertifika yetkililerini Azure ön kapısı hizmeti üzerinde özel HTTPS'yi etkinleştirmek için izin verilir.

Bir Azure ön kapısı Service özel etki alanı için olduğunda, [kendi sertifikası kullanarak HTTPS özelliğini etkinleştirme](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), SSL sertifikanızı oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir. Aksi takdirde, izin verilen olmayan bir CA ya da kendinden imzalı bir sertifika kullanıyorsanız, isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
