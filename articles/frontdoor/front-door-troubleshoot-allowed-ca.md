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
ms.openlocfilehash: 5cf94079dcd68887d9725ffbe9124f9b6c897dd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736165"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Sertifika yetkililerini Azure ön kapısı hizmeti üzerinde özel HTTPS'yi etkinleştirmek için izin verilir.

Bir Azure ön kapısı Service özel etki alanı için olduğunda, [kendi sertifikası kullanarak HTTPS özelliğini etkinleştirme](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), SSL sertifikanızı oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir. Aksi takdirde, izin verilen olmayan bir CA ya da kendinden imzalı bir sertifika kullanıyorsanız, isteğiniz reddedilir.

Aşağıdaki CA'lar, kendi sertifikanızı oluşturduğunuzda verilir:

- AddTrust dış CA kök
- AlphaSSL kök CA
- AME Infra CA 01
- AME Infra CA 02
- Ameroot
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
- DigiCert High Assurance EV Root CA
- Doğrulama sunucu CA DigiCert SHA2 genişletilmiş
- DigiCert SHA2 Yüksek güvence sunucu sertifika yetkilisi
- DigiCert SHA2 güvenli sunucu CA
- DST Root CA X3
- D güven kök sınıfı 3 CA 2 2009
- Her yerde şifreleme DV TLS CA
- Güvenilen kök sertifika yetkilisi
- Kök sertifika yetkilisi - G2 entrust
- Entrust.NET sertifika yetkilisi (2048)
- GeoTrust genel CA
- GeoTrust birincil sertifika yetkilisi
- GeoTrust birincil sertifika yetkilisi - G2
- Geotrust RSA CA 2018
- GlobalSign
- Genişletilmiş Doğrulama CA - SHA256 - G2 GlobalSign
- GlobalSign kuruluş doğrulama CA - G2
- GlobalSign kök CA
- Go Daddy kök sertifika yetkilisi - G2
- G2 Daddy güvenli sertifika yetkilisi - Git
- RapidSSL RSA CA 2018
- Kök kurumu
- Symantec sınıfı 3 EV SSL CA - G3
- Symantec sınıf 3 güvenli sunucu CA - G4
- Microsoft için Symantec Kurumsal mobil kök
- Thawte birincil kök CA
- Thawte birincil kök CA - G2
- Thawte birincil kök CA - G3
- Thawte RSA CA 2018
- Thawte zaman damgası CA
- TrustAsia TLS RSA CA
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL CA
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL SGC CA
- VeriSign sınıf 3 birincil ortak sertifika yetkilisi - G5
- VeriSign uluslararası sunucusu CA - 3 sınıfı
- VeriSign zaman damgası hizmeti kök
- VeriSign Evrensel kök sertifika yetkilisi
