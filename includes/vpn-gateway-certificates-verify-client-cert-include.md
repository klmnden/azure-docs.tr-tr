---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 12/11/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: c0ce4e882f270f5e0c789a608aaada5c6c9cba92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320209"
---
Bağlantı sorunları yaşıyorsanız aşağıdakileri denetleyin:

- Bir istemci sertifikası ile veriliyorsa **Sertifika Verme Sihirbazı**, .pfx dosyası olarak dışarı ve seçili olduğundan emin olun **mümkünse sertifika yolundaki tüm sertifikaları dahil et**. Bu değer ile dışarı aktardığınızda, kök sertifika bilgileri de dışarı aktarılır. İstemci bilgisayarda sertifika yükledikten sonra kök sertifika .pfx dosyasında da yüklenir. Kök sertifikasının yüklendiğini doğrulamak için açık **kullanıcı sertifikalarını Yönet** seçip **güvenilen kök sertifika Yetkilileri\Sertifikalar**. Hangi kimlik doğrulamasının çalışması için mevcut kök sertifikayı listelendiğini doğrulayın.

- Kimlik doğrulaması yapamaz ve bir kuruluş CA çözümü tarafından verilmiş bir sertifika kullanılan, istemci sertifikası kimlik doğrulama sırasını doğrulayın. Kimlik doğrulama listesinin sırasını istemci sertifikası çift tıklayarak kontrol seçerek **ayrıntıları** sekmesini seçip ardından **Gelişmiş anahtar kullanımı**. Emin *istemci kimlik doğrulaması* listesindeki ilk öğedir. Aksi takdirde sahip kullanıcı şablonu temel alan bir istemci sertifikasını vermek *istemci kimlik doğrulaması* listedeki ilk öğe olarak.

- P2S hakkında ek sorun giderme bilgileri için bkz. [P2S bağlantılarının sorunlarını giderme](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
