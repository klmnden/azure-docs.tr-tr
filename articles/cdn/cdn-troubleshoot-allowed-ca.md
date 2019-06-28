---
title: Azure CDN özel HTTPS'yi etkinleştirmek için sertifika yetkililerini izin | Microsoft Docs
description: Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için kendi sertifika kullanıyorsanız, bunu oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 8ce141fbf3d767159d16495bff5a93d791224c17
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331892"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN özel HTTPS'yi etkinleştirmek için sertifika yetkililerini izin

İçin bir Azure Content Delivery Network (CDN) özel etki alanı üzerinde bir **Azure CDN standart'ı Microsoft** uç noktası olduğunda, [kendi sertifikası kullanarak HTTPS özelliğini etkinleştirme](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), izin verilen kullanmanız gerekir Sertifika yetkilisi (CA) SSL sertifikanızı oluşturmak için. Aksi takdirde, izin verilen olmayan bir CA ya da kendinden imzalı bir sertifika kullanıyorsanız, isteğiniz reddedilir.

> [!NOTE]
> Özel HTTPS için kendi sertifika kullanma seçeneğini yalnızca **Azure CDN standart Microsoft gelen** profilleri. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

