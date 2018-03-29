---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9fa18b14b82376a25bb434acd770d340b1ef9262
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
Bağlanmayla ilgili sorun yaşıyorsanız aşağıdakileri denetleyin:

- Bir istemci sertifikasını dışarı aktardıysanız, 'Mümkünse sertifika yolundaki tüm sertifikaları ekle' varsayılan değeri ile bir .pfx dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı bu değeri kullanarak dışarı aktardığınızda, kök sertifika bilgileri de dışarı aktarılır. Sertifika istemci bilgisayara yüklendiğinde, .pfx dosyasında yer alan kök sertifika da istemci bilgisayara yüklenir. İstemci bilgisayarda kök sertifika bilgileri yüklü olmalıdır. Denetlemek için, **Kullanıcı sertifikalarını yönet** menüsüne ve **Güvenilen Kök Sertifika Yetkilileri\Sertifikalar**’a gidin. Kök sertifikanın listelenmiş olduğunu doğrulayın. Kimlik doğrulamasının çalışması için kök sertifika mevcut olmalıdır.

- Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir.

- P2S hakkında ek sorun giderme bilgileri için bkz. [P2S bağlantılarının sorunlarını giderme](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).