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
ms.openlocfilehash: 2bfa10441cf5531c9383527a21b033da26322b34
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073247"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Mevcut bir teklifi Azure Marketi için güncelleştirme 
==============================================

Örneğin canlı gider sonra teklifinizi yapmak isteyebilirsiniz güncelleştirmeleri çeşitli tür vardır:

1.  Yeni ekleme \"paket\" mevcut bir SKU için
2.  Yeni SKU'ları için mevcut bir teklif ekleme
3.  Teklif/SKU Market meta verilerini güncelleştirme

Bu değişiklikleri uygulamak için bulut iş ortağı portalını kullanın. sonra teklifinizi yeniden yayımlayın. Bu makalede Azure uygulaması teklifinizi güncellemek farklı yönleriyle gösterilmektedir.

<a name="unpermitted-changes-to-azure-application-offer"></a>Azure uygulama teklif unpermitted değişiklikler 
-----------------------------------------------

Bir Azure uygulaması teklif/Azure Marketi'nde teklif etkin hale gelir sonra değiştirilemeyen SKU'su öznitelikleri vardır.

* Teklif kimliği ve teklif yayımcı kimliği.
* Mevcut bir SKU'ları SKU kimliği.
* Yayımlanmış bir paket güncelleştirin.

<a name="adding-a-new-package-to-an-existing-sku"></a>Yeni bir paket için mevcut bir SKU ekleme 
---------------------------------------

Yayımcı, var olan bir paketini güncelleştirmek için yeni bir paket sürümü eklemek isteyebilirsiniz. Bu ayrıca, farklı bir sürüm numarası olan yeni bir paket karşıya yükleyerek gerçekleştirebilirsiniz.

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com)
2.  İçinde **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulunamadı
3.  Üzerinde **SKU'ları** formunda, SKU üzerinde tıklayın kimin\'s paket olduğu gibi güncelleştirmek için
4.  Tıklayarak \"yeni paket\" ve yeni bir sürümünü sağlayın
5.  Güncelleştirilmiş şablon dosyasını içeren yeni bir .zip dosyasını karşıya yükle
6.  Çalışmaya yeni SKU'nuz için yayımlama iş akışı istiyorsanız Yayımla'yı tıklatın.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Var olan bir teklif için yeni bir SKU'ya ekleme
-------------------------------------

Yeni bir SKU'ya mevcut teklifte aşağıdaki adımları için kullanılabilir hale getirmek tercih edebilirsiniz:

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com)
2.  İçinde **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulunamadı
3.  Üzerinde **SKU'ları** form Ekle yeni SKU üzerinde tıklayın ve açılan bir SKU kimliği sağlayın.
4.  Belirtilen adımları izlemeden [burada](./cloud-partner-portal-managed-app-publish.md).
5.  Tıklayarak **Yayımla** yayınlayın yeni SKU'nuz için yayımlama iş akışını başlatmak için.

<a name="updating-offer-marketplace-metadata"></a>Market teklifi meta verilerini güncelleştirme 
-----------------------------------

Güncelleştirme şirket logoları, vb. gibi teklifinizle ilişkili Market meta verilerini güncelleştirmek için gerek duyduğunuz senaryolara sahip. Aşağıdaki adımları izleyin.

1.  Bulut iş ortağı portalında oturum açın.
2.  İçinde **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulunamadı
3.  Goto Market form ve yönergeleri [burada](../cloud-partner-portal/azure-applications/cpp-marketplace-tab.md) değişiklik yapmak için.
4.  Yaptığınız değişiklikleri yayınlayın için yayımlama iş akışı istiyorsanız Yayımla'yı tıklatın.
