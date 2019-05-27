---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c8e7e5272f180c482ca7fdd44302f49eb888b25
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66157400"
---
(Önerilen) bir kuruluş çözümü ile oluşturulan bir kök sertifika kullanabilir veya otomatik olarak imzalanan bir sertifika oluşturun. Kök sertifikayı oluşturduktan sonra ortak sertifika verilerini (özel anahtarı değil) bir Base64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarın. Ardından, genel sertifika verilerini Azure sunucusuna yükleyin.

* **Kurumsal sertifika:** Bir kuruluş çözümü kullanıyorsanız, var olan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifikanın .cer dosyasını alın.
* **Otomatik olarak imzalanan kök sertifika:** Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan kök sertifika oluşturun. Aksi halde, oluşturduğunuz sertifikalar, P2S bağlantılarıyla uyumlu olmaz ve bağlanmayı denediğinizde, istemciler bir bağlantı hatası alırsınız. Azure PowerShell, MakeCert veya OpenSSL kullanabilirsiniz. Aşağıdaki makalelerdeki adımlar uyumlu otomatik olarak imzalanan kök sertifika oluşturmak nasıl açıklar:

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Bu yönergeler, sertifikalarını oluşturmak Windows 10 ve PowerShell gerektirir. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için kullanılacak bir Windows 10 bilgisayarına erişim yoksa MakeCert kullanın. MakeCert kullanım dışı bırakılmış olsa da, bu sertifikaları oluşturmak için kullanmaya devam edebilirsiniz. Kök sertifikadan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)
