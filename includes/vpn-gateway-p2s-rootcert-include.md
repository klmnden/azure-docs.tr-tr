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
ms.openlocfilehash: b2a96aad793d956b3ea3de06fba74bcf68e50786
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197139"
---
Kurumsal bir çözüm kullanılarak oluşturulmuş bir kök sertifika kullanabilir (önerilir) veya otomatik olarak imzalanan bir sertifika oluşturabilirsiniz. Kök sertifikayı oluşturduktan sonra, ortak sertifika verilerini (özel anahtarı değil) Base-64 olarak kodlanmış X.509 .cer dosyası olarak dışarı aktarın ve ortak sertifika verilerini Azure’a yükleyin.

* **Kurumsal sertifika:** Kurumsal bir çözüm kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifika için .cer dosyasını alın.
* **Otomatik olarak imzalanan kök sertifika:** Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan kök sertifika oluşturmanız gerekir. Aşağıdaki P2S sertifika makalelerinde bulunan adımları izlemeniz önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve istemcileriniz bağlantı kurmaya çalıştığında bağlantı hatası alır. Azure PowerShell, MakeCert veya OpenSSL kullanabilirsiniz. Verilen makalelerdeki adımlar uyumlu bir sertifika oluşturur:

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Bu yönergeler, sertifika oluşturmak için Windows 10 ve PowerShell gerektirir. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için kullanabileceğiniz bir Windows 10 bilgisayarınız yoksa MakeCert kullanın. MakeCert kullanım dışıdır, ancak sertifika oluşturmak için MakeCert kullanmaya devam edebilirsiniz. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
