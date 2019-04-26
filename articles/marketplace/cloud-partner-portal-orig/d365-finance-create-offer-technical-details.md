---
title: Teknik bilgi formu doldurun nasıl | Microsoft Docs
description: İçin değerler girin açıklanmaktadır yeni bir Dynamics 365 Business Central uygulaması için teknik bilgi form üzerinde.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5aa118815146287d00a3bb8ee7d5fce57a6ad9ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397813"
---
<a name="how-to-fill-out-the-technical-info-form"></a>Nasıl teknik bilgi formu doldurun
===========================================

1.  İçinde **seçtiğiniz uygulama türüne** bölümünde uzantı paketi dosyanızı (.app) yükleyin ve herhangi bir uzantı paketi dosyalarını uzantınızı bir bağımlılığa sahiptir.

    ![Uygulama paketi bilgileri](./media/d365-financials/image015.png)

-   **Paket dosyası uzantıları** --gerekli - uzantı paketini dosyası (.app).

-   **Bağımlılık paket dosyası** --uygulamayı Appsource'ta yayımlanan başka bir uygulama üzerinde bir bağımlılık olup olmadığını gerekli. Geçerli uygulamanın bağımlı olduğu appsource'ta, önceden yayımlanmış uzantı bu .app dosyası. 

-   **Kitaplık paket dosyası** -uygulama, başka bir uygulama üzerinde bir bağımlılık olup olmadığını gerekli *değil* Appsource'ta yayımlandı. Değiştirilmediğinden ve Appsource'ta yayımlanmaz bu .app dosyası mevcut bir uygulamayı, ancak biri.

-   **Uygulamayı Test Otomasyonu** --gerekli - otomatik uzantılarını test için oluşturmanız gereken VS kodlanmış test paketi.

1. İçinde **uzantısı için ek bilgiler** bölümünde, daha fazla bilgi için uzantıyı yükleyin. Bu bilgiler, doğrulama sırasında kullanılır.

   ![Daha fazla bilgi için uygulama uzantısı formu](./media/d365-financials/image016.png)


-   **Ürün belgelerinin URL'si** uzantı belgelerine--gerekli - URL.

-   **Anahtar kullanım senaryoları** --gerekli - belgeye uzantısı için adım adım Kurulum ve kullanım ayrıntılarını listeler. Bir örnek makalesinde bulunabilir [kullanıcı senaryo belgeleri](https://docs.microsoft.com/dynamics-nav/compliance/apptest-userscenario/).

-   **Hedef sürüm** --gerekli - uygulamayı dağıtmak için sürüm seçin. Seçin **geçerli** piyasaya sürümü geçerli dağıtmak için. Seçin **yanındaki küçük** yayımlanacak sonraki alt sürüm ile dağıtılacak. Seçin **sonraki ana** yayımlanacak sonraki ana sürümünden ile dağıtılacak.

-   **Premium SKU gerektirir** --isteğe bağlı - Select Premium düğmesi uygulamayı Premium SKU gerektiriyorsa. Hizmet Yönetimi ve üretim yalnızca premium üzerinde kullanılabilir. Ayrıntılı bilgi temel vs Premium makalesinde bulunabilir [değiştirme özellikleri görüntülenir](https://docs.microsoft.com/dynamics365/financials/ui-experiences).

-   **Kod çözümleme hataları için açıklama** listeler ve gereksinimlerini karşılamadığı herhangi bir kod yaslar--isteğe bağlı - belge.

-   **Çekirdek işlevsellik etkilenmiş açıklaması** listeler ve sınırlı bir uzantısı tarafından herhangi bir çekirdek işlevi anlatan--isteğe bağlı - belge.

-   **Test hesapları** uzak Hizmetleri, web siteleri, uçtan uca kullanım test tamamlamak için gereken vb. için--isteğe bağlı - kullanıcı hesapları.

-   **UX gereksinimleri özel durumları** listeler ve herhangi bir kullanıcı deneyimi gereksinimi karşılanmadı uzantısı tarafından yaslar--isteğe bağlı - belge.

Sonraki adımda, teklifinizi mağaza ayrıntılarını eklemektir.
