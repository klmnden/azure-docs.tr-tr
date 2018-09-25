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
ms.openlocfilehash: de709133099674a0aa0386113b6459f8bc05e378
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048295"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Sertifika yetkililerini Azure ön kapısı hizmeti üzerinde özel HTTPS'yi etkinleştirmek için izin verilir.

Bir Azure ön kapısı Service özel etki alanı için olduğunda, [kendi sertifikası kullanarak HTTPS özelliğini etkinleştirme](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), SSL sertifikanızı oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir. Aksi takdirde, izin verilen olmayan bir CA ya da kendinden imzalı bir sertifika kullanıyorsanız, isteğiniz reddedilir.

Aşağıdaki CA'lar, kendi sertifikanızı oluşturduğunuzda verilir:

- AddTrust dış CA kök
- AP kök CA
- AP kök sertifika yetkilisi 2013
- AP kök sertifika yetkilisi 2014
- APCA DM3P
- AutoPilot kök CA
- Baltimore CyberTrust kök
- Sınıf 3 birincil ortak sertifika yetkilisi
- COMODO RSA sertifika yetkilisi
- COMODO RSA etki alanı doğrulama güvenli sunucu CA
- D GÜVEN kök sınıfı 3 CA 2 2009
- CA-1 DigiCert bulut Hizmetleri
- DigiCert genel kök CA
- DigiCert Yüksek güvence CA-3
- DigiCert Yüksek güvence EV kök CA
- DigiCert SHA2 Yüksek güvence sunucu sertifika yetkilisi
- DigiCert SHA2 güvenli sunucu CA
- GeoTrust genel CA
- GeoTrust birincil sertifika yetkilisi
- GeoTrust birincil sertifika yetkilisi - G2
- GlobalSign
- Genişletilmiş Doğrulama CA - SHA256 - G2 GlobalSign
- GlobalSign kuruluş doğrulama CA - G2
- GlobalSign kök CA
- Go Daddy kök sertifika yetkilisi - G2
- Microsoft Authenticode(tm) kök yetkilisi
- Microsoft Exchange hizmetleri CA 2015
- Microsoft iç kurumsal kök
- Microsoft BT ASLAN SSL CA 1
- Microsoft BT SSL SHA1
- Microsoft BT SSL SHA2
- Microsoft BT TLS CA 1
- Microsoft BT TLS CA 2
- Microsoft BT TLS CA 4
- Microsoft BT TLS CA 5
- Microsoft kök yetkilisi
- Microsoft kök sertifika yetkilisi
- Microsoft kök sertifika yetkilisi 2010
- Microsoft kök sertifika yetkilisi 2011
- Microsoft Güvenli sunucu CA 2011
- Kök Microsoft iş ortağı
- Microsoft Zaman Damgalaması hizmet kök
- Microsoft Windows Donanım uyumluluğu
- MSIT CA Z2
- MSIT Kurumsal CA'yı 1
- MSIT Kurumsal CA'yı 3
- Kök kurumu
- Symantec sınıfı 3 EV SSL CA - G3
- Symantec sınıf 3 güvenli sunucu CA - G4
- Microsoft için Symantec Kurumsal mobil kök
- Thawte birincil kök CA
- Thawte birincil kök CA - G2
- Thawte birincil kök CA - G3
- Thawte zaman damgası CA
- UTN USERFirst nesnesi
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL CA
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL SGC CA
- VeriSign sınıf 3 birincil ortak sertifika yetkilisi - G5
- VeriSign uluslararası sunucusu CA - 3 sınıfı
- VeriSign zaman damgası hizmeti kök
- VeriSign Evrensel kök sertifika yetkilisi
- WMSvc SHA2 DALEDGE1008
