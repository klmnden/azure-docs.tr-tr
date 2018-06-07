---
title: Nesnelerin interneti (IOT) Azure Internet bağlantınızın güvenli | Microsoft Docs
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
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: d5c1cb22fdfe59bd8409f9595b2fa4c3a0df771e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641246"
---
# <a name="internet-of-things-security-overview"></a>Nesnelerin interneti güvenliğine genel bakış
Azure Internet interneti (IOT) Hizmetleri çok çeşitli özellikler sunar. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

* Cihazlardan veri toplama
* Hareket halinde veri akışı çözümleme
* Büyük veri gruplarını depolama ve sorgulama
* Gerçek zamanlı ve geçmiş verileri görselleştirme
* Arka ofis sistemleriyle tümleştirme

Bu özellikler, Azure IOT Çözüm Hızlandırıcıları paketi önceden yapılandırılmış çözümler olarak birden çok Azure hizmetini özel uzantılarla birlikte sunmak için. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. IOT yazılım geliştirme setleri kullanarak, özelleştirme ve kendi gereksinimlerinizi karşılamak için bu çözümleri genişletir. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Azure IOT Çözüm Hızlandırıcıları IOT gereksinimleriniz için güçlü çözümdür. Ancak, IOT çözümlerinizi göz önünde bulundurularak başından tasarlanmıştır upmost önem derecesi vardır. IOT cihazları çalıştırmaları sayısı nedeniyle, herhangi bir güvenlik olayı hızlı bir şekilde önemli sonuçları yaygın bir olayla haline gelebilir.

IOT çözümlerinizi güvenliğini sağlama anlamanıza yardımcı olması için aşağıdaki bilgileri sahibiz.

## <a name="security-architecture"></a>Güvenlik mimarisi
Sistem tasarlanırken, bu sistemde olası tehditler anlamak ve sistem tasarlanmış ve tasarlanmış gibi uygun savunma buna göre eklemek önemlidir. Nasıl bir saldırganın bir sistemden olabilir emin uygun Azaltıcı Etkenler hale getirir anlama olduğundan başlangıçtan itibaren yerinde göz önünde bulundurularak ile başlangıç ürün tasarlamanız önemlidir.

Okuyarak IOT güvenlik mimarisi hakkında bilgi edinebilirsiniz [şeyler güvenlik mimarisi Internet](../iot-accelerators/iot-security-architecture.md).

Bu makalede aşağıdaki konular ele alınmıştır:

* [Bir tehdit modeli ile güvenlik başlar](../iot-accelerators/iot-security-architecture.md#security-starts-with-a-threat-model)
* [IOT güvenlik](../iot-accelerators/iot-security-architecture.md#security-in-iot)
* [Tehdit modelleme Azure IOT başvuru mimarisi](../iot-accelerators/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Baştan sona güvenlik
IOT işletmelere dünya çapındaki benzersiz güvenlik, gizlilik ve uyumluluk sorunları doğurur. Bu sorunları yazılım ve nasıl uygulandığı burada Uzayda Döndür geleneksel siber teknolojisi farklı olarak, sanal ve fiziksel dünyaları yakınsama ne olur IOT ilgilidir. IOT çözümleri koruma cihazları, bu cihazlar ve Bulut ve işleme ve depolama sırasında bulutta güvenli veri koruması arasında güvenli bağlantı güvenli sağlanmasını sağlama gerektirir. Bu tür işlevselliği karşı çalışan, ancak, kaynak kısıtlı cihazları, dağıtımları coğrafi dağıtılması ve bir çözüm içinde çok sayıda aygıt değildir.

Bu alanlarda güvenlik okuyarak nasıl ele alınacağını öğrenin [nesnelerin interneti güvenlik sıfırdan](../iot-accelerators/securing-iot-ground-up.md).

Makaleyi aşağıdaki konular ele alınmıştır:

* [Sıfırdan güvenli altyapısı](../iot-accelerators/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – işletmeniz için güvenli IOT altyapısı](../iot-accelerators/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>En İyi Uygulamalar
Bir IOT altyapısını koruma sıkı güvenlik derinlemesine stratejisi gerektirir. Cihazları, güvenli bir şekilde hazırlanması genel internet üzerinden aktarım sırasında veri bütünlüğü koruma verileri bulutta güvenli hale getirme her katman genel altyapısında daha yüksek güvenlik güvencesi oluşturur.

Okuyarak nesnelerin interneti güvenlikle ilgili en iyi yöntemler öğrenebilirsiniz [en iyi güvenlik uygulamalarını, nesnelerin interneti](../iot-accelerators/iot-security-best-practices.md).

Makaleyi aşağıdaki konular ele alınmıştır:

* [IOT donanım üreticisinin/Tümleştirici](../iot-accelerators/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [IOT Çözüm geliştiricisi](../iot-accelerators/iot-security-best-practices.md#iot-solution-developer)
* [IOT çözüm dağıtıcı](../iot-accelerators/iot-security-best-practices.md#iot-solution-deployer)
* [IOT çözüm işleci](../iot-accelerators/iot-security-best-practices.md#iot-solution-operator)
