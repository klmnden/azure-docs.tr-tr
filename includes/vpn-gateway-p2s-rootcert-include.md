---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: afee9450bc1485f781eb0d70b5d4dd2f50424573
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44343171"
---
Kurumsal bir çözüm kullanılarak oluşturulmuş bir kök sertifika kullanabilir (önerilir) veya otomatik olarak imzalanan bir sertifika oluşturabilirsiniz. Kök sertifikayı oluşturduktan sonra, ortak sertifika verilerini (özel anahtarı değil) Base-64 olarak kodlanmış X.509 .cer dosyası olarak dışarı aktarın ve ortak sertifika verilerini Azure’a yükleyin.

* **Kurumsal sertifika:** Kurumsal bir çözüm kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifika için .cer dosyasını alın.
* **Otomatik olarak imzalanan kök sertifika:** Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan kök sertifika oluşturmanız gerekir. Aşağıdaki P2S sertifika makalelerinde bulunan adımları izlemeniz önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve istemcileriniz bağlantı kurmaya çalıştığında bağlantı hatası alır. Azure PowerShell, MakeCert veya OpenSSL kullanabilirsiniz. Verilen makalelerdeki adımlar uyumlu bir sertifika oluşturur:

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Bu yönergeler, sertifika oluşturmak için Windows 10 ve PowerShell gerektirir. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için kullanabileceğiniz bir Windows 10 bilgisayarınızın yoksa MakeCert kullanın. MakeCert kullanım dışıdır, ancak sertifika oluşturmak için MakeCert kullanmaya devam edebilirsiniz. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md)
