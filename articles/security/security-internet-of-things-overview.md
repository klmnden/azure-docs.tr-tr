---
title: "Nesnelerin interneti (IOT) Azure Internet bağlantınızın güvenli | Microsoft Docs"
description: " Azure Internet interneti (IOT) Hizmetleri çok çeşitli özellikler sunar. Bu makalede, Azure IOT çözümlerinizde güvenliğini sağlama anlamanıza yardımcı olur. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 3793f5453b74b6c06d9e58b426d89099298e1288
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-overview"></a>Nesnelerin interneti güvenliğine genel bakış
Azure Internet interneti (IOT) Hizmetleri çok çeşitli özellikler sunar. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

* Cihazlardan veri toplama
* Hareket halinde veri akışı çözümleme
* Büyük veri gruplarını depolama ve sorgulama
* Gerçek zamanlı ve geçmiş verileri görselleştirme
* Arka ofis sistemleriyle tümleştirme

Bu özellikler, Azure IOT paketi paketleri önceden yapılandırılmış çözümler olarak birden çok Azure hizmetini özel uzantılarla birlikte sunmak için. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. IOT yazılım geliştirme setleri kullanarak, özelleştirme ve kendi gereksinimlerinizi karşılamak için bu çözümleri genişletir. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Azure IOT paketi, IOT gereksinimleriniz için güçlü bir çözümdür. Ancak, IOT çözümlerinizi göz önünde bulundurularak başından tasarlanmıştır upmost önem derecesi vardır. IOT cihazları çalıştırmaları sayısı nedeniyle, herhangi bir güvenlik olayı hızlı bir şekilde önemli sonuçları yaygın bir olayla haline gelebilir.

IOT çözümlerinizi güvenliğini sağlama anlamanıza yardımcı olması için aşağıdaki bilgileri sahibiz.

## <a name="security-architecture"></a>Güvenlik mimarisi
Sistem tasarlanırken, bu sistemde olası tehditler anlamak ve sistem tasarlanmış ve tasarlanmış gibi uygun savunma buna göre eklemek önemlidir. Nasıl bir saldırganın bir sistemden olabilir emin uygun Azaltıcı Etkenler hale getirir anlama olduğundan başlangıçtan itibaren yerinde göz önünde bulundurularak ile başlangıç ürün tasarlamanız önemlidir.

Okuyarak IOT güvenlik mimarisi hakkında bilgi edinebilirsiniz [şeyler güvenlik mimarisi Internet](../iot-suite/iot-security-architecture.md).

Bu makalede aşağıdaki konular ele alınmıştır:

* [Bir tehdit modeli ile güvenlik başlar](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [IOT güvenlik](../iot-suite/iot-security-architecture.md#security-in-iot)
* [Tehdit modelleme Azure IOT başvuru mimarisi](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Baştan sona güvenlik
IOT işletmelere dünya çapındaki benzersiz güvenlik, gizlilik ve uyumluluk sorunları doğurur. Bu sorunları yazılım ve nasıl uygulandığı burada Uzayda Döndür geleneksel siber teknolojisi farklı olarak, sanal ve fiziksel dünyaları yakınsama ne olur IOT ilgilidir. IOT çözümleri koruma cihazları, bu cihazlar ve Bulut ve işleme ve depolama sırasında bulutta güvenli veri koruması arasında güvenli bağlantı güvenli sağlanmasını sağlama gerektirir. Bu tür işlevselliği karşı çalışan, ancak, kaynak kısıtlı cihazları, dağıtımları coğrafi dağıtılması ve bir çözüm içinde çok sayıda aygıt değildir.

Bu alanlarda güvenlik okuyarak nasıl ele alınacağını öğrenin [nesnelerin interneti güvenlik sıfırdan](../iot-suite/securing-iot-ground-up.md).

Makaleyi aşağıdaki konular ele alınmıştır:

* [Sıfırdan güvenli altyapısı](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – işletmeniz için güvenli IOT altyapısı](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>En İyi Uygulamalar
Bir IOT altyapısını koruma sıkı güvenlik derinlemesine stratejisi gerektirir. Cihazları, güvenli bir şekilde hazırlanması genel internet üzerinden aktarım sırasında veri bütünlüğü koruma verileri bulutta güvenli hale getirme her katman genel altyapısında daha yüksek güvenlik güvencesi oluşturur.

Okuyarak nesnelerin interneti güvenlikle ilgili en iyi yöntemler öğrenebilirsiniz [en iyi güvenlik uygulamalarını, nesnelerin interneti](../iot-suite/iot-security-best-practices.md).

Makaleyi aşağıdaki konular ele alınmıştır:

* [IOT donanım üreticisinin/Tümleştirici](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [IOT Çözüm geliştiricisi](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [IOT çözüm dağıtıcı](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [IOT çözüm işleci](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
