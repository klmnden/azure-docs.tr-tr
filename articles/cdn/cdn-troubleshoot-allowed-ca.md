---
title: Azure CDN özel HTTPS'yi etkinleştirmek için sertifika yetkililerini izin | Microsoft Docs
description: Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için kendi sertifika kullanıyorsanız, bunu oluşturmak için bir izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 79b8cfa061bafb43cbcf2ec8a43638fd14c2fcd0
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954054"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN özel HTTPS'yi etkinleştirmek için sertifika yetkililerini izin

İçin bir Azure Content Delivery Network (CDN) özel etki alanı üzerinde bir **Azure CDN standart'ı Microsoft** uç noktası olduğunda, [kendi sertifikası kullanarak HTTPS özelliğini etkinleştirme](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), izin verilen kullanmanız gerekir Sertifika yetkilisi (CA) SSL sertifikanızı oluşturmak için. Aksi takdirde, izin verilen olmayan bir CA ya da kendinden imzalı bir sertifika kullanıyorsanız, isteğiniz reddedilir.

> [!NOTE]
> Özel HTTPS için kendi sertifika kullanma seçeneğini yalnızca **Azure CDN standart Microsoft gelen** profilleri. 
>

Aşağıdaki CA'lar, kendi sertifikanızı oluşturduğunuzda verilir:

- AddTrust dış CA kök
- AlphaSSL kök CA
- DI Infra CA 01
- DI Infra CA 02
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
- DigiCert Yüksek güvence EV kök CA
- Doğrulama sunucu CA DigiCert SHA2 genişletilmiş
- DigiCert SHA2 Yüksek güvence sunucu sertifika yetkilisi
- DigiCert SHA2 güvenli sunucu CA
- DST kök CA'ın X3
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


