---
title: Sertifika yetkililerini Azure CDN üzerinde özel HTTPS etkinleştirmek için izin verilen | Microsoft Docs
description: Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için kendi sertifika kullanıyorsanız oluşturmak için bir izin verilen sertifika yetkilisi (CA) kullanmanız gerekir.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 3c41ca7e375324ff784bf7bee347bb56400ddfbd
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238302"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Sertifika yetkililerini Azure CDN üzerinde özel HTTPS etkinleştirmek için izin verilen

Üzerinde Azure içerik teslim ağı (CDN) özel etki alanı için bir **Azure CDN standart Microsoft** uç noktası, ne zaman, [kendi sertifikası kullanarak HTTPS özelliği etkinleştirmek](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), bir izin verilen kullanmanız gerekir Sertifika yetkilisi (CA) SSL sertifikası oluşturmak için. İzin verilen olmayan bir CA veya otomatik olarak imzalanan bir sertifika kullanıyorsanız, aksi takdirde, isteğiniz kabul edilmeyecek.

> [!NOTE]
> Yalnızca kendi sertifikanızı kullanarak özel HTTPS etkinleştirme seçeneği kullanılabilir **Azure CDN standart Microsoft** profilleri. 
>

Aşağıdaki CA'ları, kendi sertifika oluştururken verilir:

- AddTrust dış CA kök
- AP kök CA'sı
- AP kök sertifika yetkilisi 2013
- AP kök sertifika yetkilisi 2014
- APCA DM3P
- AutoPilot kök CA
- Baltimore CyberTrust Root
- Sınıf 3 ortak birincil sertifika yetkilisi
- COMODO RSA sertifika yetkilisi
- COMODO RSA etki alanı doğrulama güvenli sunucu CA
- D GÜVEN kök sınıf 3 CA 2 2009
- CA-1 DigiCert bulut Hizmetleri
- DigiCert genel kök CA
- DigiCert Yüksek güvence CA-3
- DigiCert Yüksek güvence EV kök CA
- DigiCert SHA2 Yüksek güvence sunucu sertifika yetkilisi
- DigiCert SHA2 güvenli sunucu sertifika yetkilisi
- GlobalSign
- Doğrulama CA - SHA256 - G2 genişletilmiş GlobalSign
- GlobalSign kuruluş doğrulama CA - G2
- GlobalSign kök CA'sı
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
- Microsoft Hizmetleri iş ortağı kök
- Microsoft zaman damgası hizmet kök
- Microsoft Windows Donanım uyumluluğu
- MSIT CA Z2
- MSIT kuruluş CA'sı 1
- MSIT kuruluş CA'sı 3
- Kök Aracısı
- Symantec sınıfı 3 EV SSL CA - G3
- Symantec sınıf 3 güvenli sunucu CA - G4
- Microsoft için Symantec Enterprise mobil kök
- Thawte zaman damgası CA
- UTN USERFirst nesnesi
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL CA
- VeriSign sınıfı 3 Genişletilmiş Doğrulama SSL SGC CA
- VeriSign sınıf 3 ortak birincil sertifika yetkilisi - G5
- VeriSign International Server CA - Sınıf 3
- VeriSign zaman damgası hizmet kök
- VeriSign Evrensel kök sertifika yetkilisi
- WMSvc SHA2 DALEDGE1008

