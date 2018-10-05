---
title: Mevcut bir teklifi Azure Marketi için güncelleştirme
description: Mevcut bir teklifi Azure Marketi için güncelleştirme
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5633392bdf1293ee9196fafe67cf901e0d8c8014
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811432"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Mevcut bir teklifi Azure Marketi için güncelleştirme 
==============================================

Çeşitli Canlı gider sonra teklifinizi yapmak isteyebilirsiniz güncelleştirmeleri vardır.

1.  Yeni ekleme \"paket\" mevcut bir SKU için
2.  Yeni SKU'ları için mevcut bir teklif ekleme
3.  Teklif/SKU Market meta verilerini güncelleştirme

Bulut iş ortağı Portalı'nda teklifinizi güncelleştirin ve yeniden yayımlamanız gerekir. Bu makalede Azure uygulaması teklifinizi güncellemek farklı yönleriyle gösterilmektedir.

<a name="unpermitted-changes-to-azure-application-offersku"></a>Azure uygulama teklifini/SKU'yu unpermitted değişiklikler 
--------------------------------------------------

Bir Azure uygulaması teklif/Azure Marketi'nde teklif etkin hale gelir sonra değiştirilemeyen SKU'su öznitelikleri vardır.

1.  Teklif kimliği ve teklif yayımcı kimliği.
2.  Mevcut bir SKU'ları SKU kimliği.
3.  Yayımlanmış bir paket güncelleştirin.

<a name="adding-a-new-package-to-an-existing-sku"></a>Yeni bir paket için mevcut bir SKU ekleme 
---------------------------------------

Yayımcı, var olan bir paketini güncelleştirmek için yeni bir paket sürümü eklemek isteyebilirsiniz. Bu, farklı bir sürüm numarası olan yeni bir paket karşıya yükleyerek yapılabilir.

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com)
2.  Güncelleştirmek istediğiniz teklif tüm teklif bulunamadı
3.  SKU üzerinde SKU'ları formuna tıklayın kimin\'s paket olduğu gibi güncelleştirmek için
4.  Tıklayarak \"yeni paket\" ve yeni bir sürümünü sağlayın
5.  Güncelleştirilmiş şablon dosyasını içeren yeni bir .zip dosyasını karşıya yükle
6.  Çalışmaya yeni SKU'nuz için yayımlama iş akışı istiyorsanız Yayımla'yı tıklatın.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Var olan bir teklif için yeni bir SKU'ya ekleme
-------------------------------------

Yeni bir SKU'ya mevcut teklifte için kullanılabilir hale getirmek isteyebilirsiniz. İzleyin etkinleştirmek için aşağıdaki adımları.

1.  Oturum açma [bulut iş ortağı portalı](http://cloudpartner.azure.com)
2.  Güncelleştirmek istediğiniz teklif tüm teklif bulunamadı
3.  SKU'lar hakkında form Ekle yeni SKU üzerinde clik ve pencerede bir SKU kimliği sağlayın.
4.  Belirtilen adımları izlemeden [burada](./cloud-partner-portal-managed-app-publish.md).
5.  Çalışmaya yeni SKU'nuz için yayımlama iş akışı istiyorsanız Yayımla'yı tıklatın.

<a name="updating-offer-marketplace-metadata"></a>Market teklifi meta verilerini güncelleştirme 
-----------------------------------

Güncelleştirme şirket logoları, vb. gibi teklifinizle ilişkili Market meta verilerini güncelleştirmek için gerek duyduğunuz senaryolara sahip. Aşağıdaki adımları izleyin.

1.  Bulut iş ortağı portalında oturum açın
2.  Güncelleştirmek istediğiniz teklif tüm teklif bulunamadı
3.  Goto Market form ve yönergeleri [burada](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-push-to-staging) değişiklik yapmak için.
4.  Yaptığınız değişiklikleri yayınlayın için yayımlama iş akışı istiyorsanız Yayımla'yı tıklatın.
