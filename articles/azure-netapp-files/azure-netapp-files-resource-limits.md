---
title: Azure NetApp dosyaları için kaynak sınırları | Microsoft Docs
description: NetApp hesapları, kapasitesi havuzu, birimler, anlık görüntüler ve temsilci alt ağ için sınırları dahil olmak üzere, NetApp dosyaları Azure kaynakları için limitler açıklanmıştır.
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
ms.date: 05/02/2019
ms.author: b-juche
ms.openlocfilehash: b55467d77beb8f97b8e392b72682268ae0407e54
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65826371"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp Files için kaynak sınırları

Azure için NetApp dosyaları kaynak sınırları anlama birimlerinizi yönetmenize yardımcı olur.

## <a name="resource-limits"></a>Kaynak sınırları

Aşağıdaki tabloda, Azure NetApp dosyaları için kaynak sınırları açıklanmaktadır:

|  Resource  |  Varsayılan limit  |  Destek isteği aracılığıyla ayarlanabilir  |
|----------------|---------------------|--------------------------------------|
|  Azure aboneliği başına NetApp hesabı sayısı   |  10    |  Evet   |
|  NetApp hesap başına kapasitesi havuzu sayısı   |    25     |   Evet   |
|  Birim kapasitesi havuzu başına sayısı     |    500   |    Evet     |
|  Birim başına anlık görüntü sayısı       |    255     |    Hayır        |
|  Azure sanal ağ başına Azure NetApp dosyaları (Microsoft.NetApp/volumes) temsilci alt ağ sayısını    |   1   |    Hayır    |
|  En fazla VM sayısı (eşlenen sanal ağlar'ı içerir) bir birime bağlanabilir     |    1000   |    Hayır   |
|  En küçük boyut tek kapasitesi havuzu   |  4 TiB     |    Hayır  |
|  En büyük boyutunu tek kapasitesi havuzu    |  500 TiB   |   Hayır   |
|  Tek bir birim, en küçük boyut    |    100 GiB    |    Hayır    |
|  Maksimum atanan kota, bir tek birim *   |   92 TiB   |    Hayır   |
|  Bir tek birim * en büyük boyutu     |    100 TiB    |    Hayır       |

\* Bir birim el ile oluşturulan veya maximally 92 TiB olarak yeniden boyutlandırıldı. Ancak, bir birim, en fazla 100 tib'a kadar fazla kullanım bir senaryoda büyüyebilir. Bkz: [Azure NetApp dosyaları için maliyet modeli](azure-netapp-files-cost-model.md) kapasite fazla kullanım hakkında ayrıntılı bilgi için. 

## <a name="request-limit-increase"></a>Sınır artırma isteğinde 

Yukarıdaki tablodaki ayarlanabilir sınırları artırmak için Azure destek isteği oluşturabilirsiniz. 

Azure portalında gezinme düzleminde: 

1. Tıklayın **Yardım + Destek**.
2. Tıklayın **+ yeni destek isteği**.
3. Temel bilgiler sekmesinde, aşağıdaki bilgileri sağlayın: 
    1. Sorun türü: Seçin **hizmet ve abonelik sınırlarını (kotalar)** .
    2. Abonelikler: İlişkin kotayı gereken kaynak için abonelik seçin.
    3. Kota türü: Seçin **Depolama: Azure NetApp dosyaları sınırlar**.
    4. Tıklayın **sonraki: Çözümleri**.
4. Ayrıntılar sekmesi üzerinde:
    1. Açıklama kutusunda, ilgili kaynak türü için aşağıdaki bilgileri belirtin:

        |  Resource  |    Üst kaynak      |    İstenen yeni sınırları     |    Kota artışı nedeni       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Hesap |  *Abonelik kimliği*   |  *İstenen yeni maksimum **hesabı** numarası*    |  *Hangi senaryo veya kullanım örneği isteği istenir?*  |
        |  Havuz    |  *Abonelik kimliği, hesap URI'si*  |  *İstenen yeni maksimum **havuzu** numarası*   |  *Hangi senaryo veya kullanım örneği isteği istenir?*  |
        |  Birim  |  *Abonelik kimliği, hesabı URI'si, havuzu URI'si*   |  *İstenen yeni maksimum **birim** numarası*     |  *Hangi senaryo veya kullanım örneği isteği istenir?*  |

    2. Uygun yöntemini desteklemezler ve sözleşme bilgilerinizi belirtin.

    3. Tıklayın **sonraki: Gözden geçir + Oluştur** isteği oluşturmak için. 


## <a name="next-steps"></a>Sonraki adımlar  

- [NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
- [Azure NetApp dosyaları için maliyet modeli](azure-netapp-files-cost-model.md)
