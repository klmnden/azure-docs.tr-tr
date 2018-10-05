---
title: Kapsayıcıları | Microsoft Docs
description: Bir kapsayıcı görüntüsü yayımlama adımları.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
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
ms.openlocfilehash: 5653e4b2ec9aa5e21a107611b9d4142168630d4a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811600"
---
<a name="publishing-a-container-image-in-the-cloud-partner-portal"></a>Bir kapsayıcı görüntüsü bulut iş ortağı Portalı'nda yayımlama
========================================================

Bu makalede yeni bir kapsayıcı görüntüsü bulut iş ortağı Portalı'nda yayımlama açıklanır.

Yeni bir kapsayıcı görüntüsü yayımlamak için aşağıdaki adımları kullanın.

1. Seçin **yeni teklif** seçip **kapsayıcıları**

    ![Yeni Teklif kapsayıcıları seçeneğini belirleyin](media/cpp-containers-guide/azure-container-offer.png)

2. Teklif ayarları sekmesinde, Teklif kimliği girin **Teklif kimliği**, **yayımcı kimliği**, ve **adı**.

    ![Teklif kimliği](media/cpp-containers-guide/containers-offer-settings.png)

3. SKU'ları sekmesinde seçin **yeni SKU**.
    >[!NOTE]
    >Birden fazla SKU ekleyebilirsiniz.

    ![Yeni SKU istemi](media/cpp-containers-guide/containers-sku-settings.png)

4. SKU ve kapsayıcı bilgileri sağlar. Her SKU, bir kapsayıcı görüntüsüne karşılık gelir. Bir SKU iki bölümü vardır:

    -   SKU meta verileri
    -   Kapsayıcı meta verileri

    SKU kapsayıcı görüntüleri için görüntüleme bilgileri içerir.

    ![SKU meta verileri](media/cpp-containers-guide/containers-sku-details.png)

    Kapsayıcı meta verileri, Azure container Registry'de (ACR) içinde görüntü deposu ayrıntıları başvuru bilgilerini içerir. Azure Market genel Market kayıt defterine bu görüntüyü kopyalar ve müşteriler için sertifika sonra kullandırılır. Market kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü kullanmak için Azure kullanıcı gelen tüm isteklerin sunulur.

   ![Kapsayıcı meta verileri](media/cpp-containers-guide/containers-image-repository.png)

    Görüntü deposu ayrıntılarını önceki ekran görüntüsünde, aşağıdaki alanları içerir:

    -   **Abonelik kimliği** -ACR kayıt defteri olduğu mevcut Azure abonelik kimliği.
    -   **Kaynak grubu adı** -ACR kayıt defterinin kaynak grubu adı.
    -   **Kayıt defteri adı** -ACR kayıt defteri adı.
    -   **Depo adı** -depo adı. Bir kez ayarlandıktan sonra bu değer daha sonra değiştirilemez. Diğer herhangi bir teklif hesabınız kapsamında aynı ada sahip olacak şekilde bu benzersiz bir ad verilmelidir.
    -   **Kullanıcı adı** -ACR (yönetici kullanıcı adı) ile ilişkili kullanıcı adı.
    -   **Parola** -ACR ile ilişkili parola.

    >[!NOTE]
    >Kullanıcı adı ve parola iş ortakları yayımlama işleminde belirtilen ACR erişiminiz olduğundan emin olmak için gereklidir.

    Yayımlanırken bir kapsayıcı görüntüsü, bir veya daha fazla görüntü etiketler de sağlayabilir. Bir resim etiketi yanı sıra iş ortaklarının SHA özetler de sağlayabilirsiniz. Lütfen eklediğinizden emin olun bir **test etiketi** görüntünüze test sırasında görüntünün belirleyebilir.

5. Pazarlama özgü içeriğinizi Marketi sekmesinden ekleyin.

    ![Market'teki bilgileri](media/cpp-containers-guide/containers-marketplace-tab.png)

6. Destek sekmede mühendislik birimi ilgili kişisi ve müşteri destek bilgilerini girin.

   ![Destek bilgileri](media/cpp-containers-guide/containers-support-tab.png)

7. Seçin **Yayımla** teklif yayımlamak için. Yayımlama seçtikten sonra kapsayıcı görüntünüzü yayımlama ilgili adımları gösteren bir zaman çizelgesi görürsünüz.
