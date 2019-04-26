---
title: Azure Active Directory kimlik koruması risk olayları başvurusu | Microsoft Docs
description: Azure Active Directory kimlik koruması, risk olayları başvuru.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d009022d4ec03990d3ed0321ebcb13c72c674ed8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60294217"
---
# <a name="azure-active-directory-identity-protection-risk-events-reference"></a>Azure Active Directory kimlik koruması, risk olayları başvurusu

Güvenlik ihlallerini büyük çoğunluğu göz önüne bir yerde saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde edin. Tehlikeye atılmış kimlik keşfetme hiçbir kolay bir görevdir. Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Her bir kayıt adı verilen risk olayı şüpheli eylem depolanır algılandı.


## <a name="anonymous-ip-address"></a>Anonim IP adresi

**Algılama türü:** Gerçek zamanlı  
**Eski adı:** Anonim IP adresinden oturum açma


Bu risk olayı türü anonim bir IP adresi (örneğin, Tor tarayıcı anonymizer VPN'ler) oturum açma işlemleri gösterir.
Bu IP adresleri, genellikle kötü amaçlı olabilecek hedefi için kendi oturum açma (IP adresi, konum, cihaz, vb.) telemetri gizlemek istediğiniz aktör tarafından kullanılır.


## <a name="atypical-travel"></a>Alışılmadık seyahat

**Algılama türü:** Çevrimdışı  
**Eski adı:** Alışılmadık konumlara imkansız seyahat


Davranışı verilen burada konumları en az biri de kullanıcı için alışılmadık olabilir, coğrafi olarak uzak konumlardan gerçekleştirilen iki oturum açma Bu risk olayı türünü tanımlar. Diğer çeşitli faktörler arasında bu makine öğrenimi algoritmasının iki oturum açma ve bu kullanıcının ilk konumdan farklı bir kullanıcı aynı kullandığını gösteren ikinci, seyahat alacağı saat arasındaki süre dikkate alır kimlik bilgileri.

Algoritma "VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi mümkün olmayan seyahat koşullar katkıda bulunan belirgin hatalı pozitif sonuçlar" yok sayar. Sistem 14 gün veya bu sırada yeni bir kullanıcının oturum açma davranışı öğrenir 10 oturumları, en erken bir öğrenme dönemi sahiptir.


## <a name="leaked-credentials"></a>Kimlik bilgilerinin sızdırılması

**Algılama türü:** Çevrimdışı  
**Eski adı:** Sızan kimlik bilgilerine sahip kullanıcılar


Kullanıcının geçerli kimlik bilgilerinin sızdırılması Bu risk olayı türünü gösterir.
Kullanıcıların geçerli parolalarını cybercriminals tehlikeye, Suçları genellikle bu kimlik bilgilerini paylaşır. Bu genellikle, bunları herkese açık şekilde koyu Yapıştır ya da web sitelerinde veya ticari veya kimlik bilgilerini siyah piyasadaki satış yayınlayarak da gerçekleştirilir. Microsoft kimlik bilgilerinin sızdırılması hizmet edinme kullanıcı adı / parola çiftlerini ortak veya koyu web sitelerini izleme ve çalışma tarafından:

- Araştırmacılar

- Yasal makamlar

- Microsoft Güvenlik takımlar

- Diğer güvenilen kaynaklardan

Hizmet kullanıcı kimlik bilgilerini koyu web, Yapıştır siteler ya da yukarıdaki kaynakları edinme, bunlar geçerli eşleşiyor bulmak için Azure AD kullanıcılarının geçerli geçerli kimlik bilgilerine karşı denetlenir.


## <a name="malware-linked-ip-address"></a>Kötü amaçlı yazılım bağlantısı içeren IP adresi

**Algılama türü:** Çevrimdışı  
**Eski adı:** Bulaşma olan cihazlardan oturum açma işlemleri


Bu risk olayı türünü etkin bir bot sunucusuyla iletişim kurmak için bilinen kötü amaçlı yazılım bulaşmış IP adreslerinden oturum açma işlemleri gösterir. Bu, karşı bir bot sunucusu iletişim kurmayan bot sunucunun etkin olduğu sırada olan IP adresleri kullanıcı cihazının IP adreslerini karşılaştırılarak ilişkilendirilmesi yoluyla belirlenir.


## <a name="unfamiliar-sign-in-properties"></a>Bilinmeyen oturum açma özellikleri

**Algılama türü:** Gerçek zamanlı  
**Eski adı:** Alışılmadık konumlardan oturum açma işlemleri

Bu risk olayı türünü oturum açma geçmişini göz önünde bulundurur (IP, enlem / boylam ve ASN) anormal oturum açma için aranacak. Sistem, bir kullanıcı tarafından kullanılan önceki konumları hakkında bilgi depolar ve bu "tanıdık" konumlar göz önünde bulundurur. Risk olayı bilinen konumları listesinde olmayan bir konumdan oturum açma meydana geldiğinde tetiklenir. Yeni oluşturulan kullanıcılar "öğrenme modunda", bir süre için kullanıcının davranışını algoritmalarınızı öğrenmenin yanı sıra hangi alışılmadık oturum açma özelliklerinde risk olaylarını devre dışı olması. Öğrenme modu süresi dinamiktir ve bağlıdır üzerinde ne kadar süre sürer algoritması kullanıcının oturum açma düzenleri hakkında yeterli bilgi toplayın. En düşük süre beş gündür. Bir kullanıcı modu uzun bir süre etkin olmayıp öğrenme moduna geri dönebilirsiniz. Tanıdık cihazlardan ve coğrafi olarak bilinen bir konuma yakın olan konumlardan oturum açma işlemleri de yoksayar. 

Ayrıca bu algılama için temel kimlik doğrulaması (veya eski protokolleri) çalıştırıyoruz. Bu protokollerin istemci kimliği gibi modern özellikleri olmadığı için hatalı pozitif sonuçları azaltmak için sınırlı telemetri yoktur. Müşterilerimize modern kimlik doğrulaması için taşımanız önerilir.

