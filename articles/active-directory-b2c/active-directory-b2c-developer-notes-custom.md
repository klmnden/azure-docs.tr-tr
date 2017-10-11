---
title: "Azure Active Directory B2C: özel ilkelerini kullanma Geliştirici Notları | Microsoft Docs"
description: "Yapılandırma ve Azure AD B2C ile özel ilkeler koruma geliştiriciler için Notlar"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Azure Active Directory B2C özel ilke genel Önizleme için sürüm notları
Özel ilke özellik kümesi artık genel Önizleme için tüm Azure Active Directory B2C altında değerlendirme için kullanılabilir (Azure AD B2C) müşteriler. Bu özellik kümesi, Gelişmiş kimlik geliştiricileri en karmaşık kimlik çözümleri oluşturma yöneliktir.  

Günümüzde, bu özellik kümesi XML dosyasını düzenleyerek aracılığıyla doğrudan kimlik deneyimi Framework yapılandırmak geliştiricilere gerektirir. Bu yapılandırma, güçlü ve karmaşık yöntemidir. Gelişmiş kimlik kimlik deneyimi Framework kullanan geliştiriciler kılavuzlarına tamamladıktan ve başvuru belgeleri okuma biraz zaman yatırım planlamanız gerekir. 

## <a name="features-included-in-this-public-preview"></a>Bu genel önizlemede bulunan özellikler
Genel önizlemede sunulan yeni özelliklerle geliştiriciler aşağıdaki görevleri gerçekleştirebilirsiniz:<br>

* Özel ilkeler kullanarak yazar ve karşıya yükleme özel kimlik doğrulama kullanıcı Yolculuklar. 
   * Kullanıcı Yolculuklar alışverişleri adım adım talep sağlayıcıları arasında açıklanmaktadır. 
   * Koşullu kullanıcı Yolculuklar dallanma tanımlayın. 
* Özel kimlik doğrulama kullanıcı Yolculuklar REST API etkin Hizmetleri'nde tümleştirin.  
* Standart Openıdconnect ile uyumlu olan kimlik sağlayıcıları ile Federasyon ekleyin. <br>
* SAML 2.0 protokolünü kullanan kimlik sağlayıcıları ile Federasyon ekleyin. 

## <a name="terms-of-the-public-preview"></a>Genel Önizleme koşulları

* Yalnızca değerlendirme amacıyla yeni özellikleri kullanmak için önerilir.<br>
* Yeni özellikler, bir üretim ortamında kullanılması amaçlanmamıştır.<br>
* Hizmet düzeyi sözleşmelerine (SLA) için yeni özellikleri geçerli değildir. <br>
* Destek istekleri normal destek kanallarını Dosyalanan. <br>
* Genel kullanılabilirlik taahhüt edilen tarih yoktur.<br>
* Bizim tedbirli ve herhangi bir nedenle, Microsoft bayrak ve reddetme veya senaryoları ve bir müşteri kimlik ve erişim yönetimi (CIAM) platformu olarak hizmet vermek için Azure AD B2C ürün kurucu kapsamını aşan kullanıcı Yolculuklar kısıtlama.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Özel ilke özellik kümesi geliştiricilerin sorumlulukları
El ile ilke yapılandırması, Azure AD B2C temel platform düşük düzeyli erişim verir ve benzersiz, tam olarak özelleştirilebilir güven framework oluşturulmasında sonuçlanır. Olası alternatifler özel kimlik sağlayıcıları, güven ilişkileri dış hizmetler ve adım adım iş akışları ile tümleştirmeleri bunları kullanan gelişmiş geliştiriciler üzerinde büyük taleplerini yerleştirin.

Genel Önizlemesi'nden tam olarak yararlanmak için özel İlkesi özellik kümesini kullanan geliştiriciler için aşağıdaki yönergelere uyması öneririz:
* Anahtar/parolalar yönetimi ve kimlik deneyimi altyapısı yapılandırma dili ile Windows'un öğrenin.
* Senaryolar ve özel tümleştirmeler sahipliğini alın.
* Sistemli senaryo test gerçekleştirin.
* Yazılım geliştirme ve en iyi yöntemlerle en az bir geliştirme ve test ortamı ve bir üretim ortamında hazırlama izleyin.
* Kimlik sağlayıcılar ve Hizmetleri ile tümleştirme yeni gelişmeler hakkında bilgi sahibi olma. Örneğin, değişiklikleri gizli ve hizmet zamanlanmış ve zamanlanmamış değişiklikleri takip edin.
* Etkin izleme işlevini ayarlama ve üretim ortamlarını yanıtlama izleyin.
* İletişim e-posta adresleri güncel tutun ve Microsoft live site takım e-postalar için yanıt verebilir durumda kalır.
* Microsoft live site ekibi tarafından bunu tavsiye zaman zamanında eylemi gerçekleştirin. 


>[!NOTE]
>Bu özellikler sonunda tüm geliştiriciler için daha erişilebilir hale getirme Azure AD yerleşik İlkeleri'nde bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md).
