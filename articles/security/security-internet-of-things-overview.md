---
title: Nesnelerin interneti (IOT) azure'da Internet bağlantınızın güvenliğini sağlama | Microsoft Docs
description: " Azure internet interneti (IOT) Hizmetleri çok çeşitli özellikler sunar. Bu makalede azure'da IOT çözümlerinizin güvenliğini nasıl anlamanıza yardımcı olur. "
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: db1c2b9c852479b9af3674f6c5e1f1135ee289f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62107458"
---
# <a name="internet-of-things-security-overview"></a>Nesnelerin interneti güvenliğine genel bakış
Azure internet interneti (IOT) Hizmetleri çok çeşitli özellikler sunar. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

* Cihazlardan veri toplama
* Hareket halinde veri akışı çözümleme
* Büyük veri gruplarını depolama ve sorgulama
* Gerçek zamanlı ve geçmiş verileri görselleştirme
* Arka ofis sistemleriyle tümleştirme

Özellikleri, bu Azure IOT Çözüm Hızlandırıcıları paketi önceden yapılandırılmış çözümler olarak birden çok Azure hizmetini özel uzantılarla birlikte sunmak için. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. IOT yazılım geliştirme setleri kullanarak, özelleştirebilir ve kendi gereksinimlerinizi karşılamak için bu çözümleri genişletin. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Azure IOT Çözüm Hızlandırıcıları, IOT ihtiyaçlarınız için güçlü çözümdür. Ancak, IOT çözümlerinizi başından itibaren güvenlikten ödün tasarlanmıştır upmost önemini olur. IOT cihazları çalıştırmaları sayısı nedeniyle, her türlü güvenlik olayıyla hızla önemli sonuçlar ile yaygın bir olay olabilir.

IOT çözümlerinizin güvenliğini nasıl anlamanıza yardımcı olması için aşağıdaki bilgileri sahibiz.

## <a name="security-architecture"></a>Güvenlik mimarisi
Sistem tasarlanırken bu sistemde olası tehditleri anlamak ve sistem tasarlanmış ve desteklemesi için uygun savunmaları buna göre eklemek önemlidir. Güvenlikten ödün anlama nasıl bir saldırganın bir sistemden olabilir emin uygun Azaltıcı olmasına yardımcı olur çünkü baştan yerinde başlangıç ürün tasarlamak önemlidir.

Okuyarak IOT güvenlik mimarisi hakkında bilgi edinebilirsiniz [şeyler güvenlik mimarisi Internet](/azure/iot-fundamentals/iot-security-architecture).

Bu makalede, aşağıdaki konular ele alınmaktadır:

* [Bir tehdit modeli ile güvenlik başlar](/azure/iot-fundamentals/iot-security-architecture#security-starts-with-a-threat-model)
* [IOT güvenliği](/azure/iot-fundamentals/iot-security-architecture#security-in-iot)
* [Azure IOT başvuru Mimarisi Modelleme tehdit](/azure/iot-fundamentals/iot-security-architecture)

## <a name="security-from-the-ground-up"></a>Baştan sona güvenlik
IOT işletmelere dünya çapındaki benzersiz güvenlik, gizlilik ve uyumluluk sorunları doğurur. Burada bu sorunları nasıl uygulandığını ve yazılım etrafında çalışmalarınızı geleneksel siber teknoloji, IOT siber ve fiziksel ortamdan yakınsama ne olur ilgilidir. IOT çözümleri koruma sağlayarak, bu cihazlar Bulut ve işlem ve depolama sırasında buluta güvenli veri koruma arasında güvenli bağlantı cihazların güvenli sağlama gerektirir. Bu işlevselliğin karşı çalışıyor, ancak, kaynak kısıtlı cihazları, dağıtımları coğrafi dağılımı ve birçok cihaz bir çözüm içinde uygulanır.

Bu alanlarda güvenlik okuyarak işlemek nasıl öğrenebilirsiniz [nesnelerin interneti güvenliği baştan](/azure/iot-fundamentals/iot-security-ground-up).

Aşağıdaki konuları anlatılmaktadır:

* [Baştan güvenli bir altyapı](/azure/iot-fundamentals/iot-security-ground-up#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – işletmeniz için güvenli bir IOT altyapısı](/azure/iot-fundamentals/iot-security-ground-up#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>En İyi Uygulamalar
Bir IOT altyapısı güvenli hale getirme sıkı bir güvenlik savunma stratejisi gerektirir. Bulutta cihazları, güvenli bir şekilde sağlama için genel internet üzerinden aktarım sırasında veri bütünlüğünü koruma verileri güvenli hale getirme her katman, daha yüksek güvenlik güvencesi genel altyapı oluşturur.

Nesnelerin interneti güvenliği hakkında okuyarak en iyi uygulamaları öğrenebilirsiniz [en iyi güvenlik uygulamaları, nesnelerin interneti](/azure/iot-fundamentals/iot-security-best-practices).

Aşağıdaki konuları anlatılmaktadır:

* [IOT donanım üreticisi/entegratörü](/azure/iot-fundamentals/iot-security-best-practices#iot-hardware-manufacturerintegrator)
* [IOT çözümü geliştirme](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-developer)
* [IOT çözüm dağıtıcı](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-deployer)
* [IOT çözümü işleci](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-operator)
