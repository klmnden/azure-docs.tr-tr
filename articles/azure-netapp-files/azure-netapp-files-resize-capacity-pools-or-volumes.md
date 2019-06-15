---
title: Azure için NetApp dosyaları kapasitesi havuzu ya da bir birim yeniden boyutlandırma | Microsoft Docs
description: Kapasitesi havuzu ya da bir birim boyutunu değiştirmek açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: c58ceef57b984f46b86bb2a8577c53b75082b78b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65794623"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Kapasitesi havuzunu veya birimi yeniden boyutlandırma
Kapasitesi havuzu ya da gerektiği gibi bir birim boyutunu değiştirebilirsiniz. 

## <a name="resize-the-capacity-pool"></a>Kapasitesi havuzu yeniden boyutlandırma 

1 TiB artırır veya azaltır kapasitesi havuzu boyutu değiştirebilirsiniz. Ancak, kapasitesi havuzu boyutu 4 TiB küçük olamaz. Kapasitesi havuzu yeniden boyutlandırmayı, satın alınan Azure NetApp dosyaları kapasite değiştirir.

1. NetApp hesabı yönet dikey penceresinden, yeniden boyutlandırmak istediğiniz kapasitesi havuzu tıklayın. 
2. Kapasitesi havuzu adına sağ tıklayın veya bağlam menüsünü görüntülemek için kapasite havuzun satırın sonunda "..." simgesine tıklayın. 
3. Bağlam menüsü seçenekleri, yeniden boyutlandırma veya kapasitesi havuzu silmek için kullanın.

## <a name="resize-a-volume"></a>Bir birim yeniden boyutlandırma

Gerekirse bir birimin boyutunu değiştirebilirsiniz. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.

1. NetApp hesabı yönet dikey penceresinden tıklayın **birimleri**. 
2. Bağlam menüsünü görüntülemek için birimin satırın sonunda "..." simgesine tıklayın veya yeniden boyutlandırmak istediğiniz birim adına sağ tıklayın.
3. Bağlam menüsü seçenekleri, yeniden boyutlandırma veya birimi silmek için kullanın.

