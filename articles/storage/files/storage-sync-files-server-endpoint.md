---
title: Bir Azure dosya eşitleme (Önizleme) sunucu uç nokta Ekle/Kaldır | Microsoft Docs
description: Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin.
services: storage
documentationcenter: ''
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 26e4af814bad988da02d4e0cf36f17e1beec872e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="addremove-an-azure-file-sync-preview-server-endpoint"></a>Bir Azure dosya eşitleme (Önizleme) sunucu uç nokta Ekle/Kaldır
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

A *sunucusu uç noktası* üzerinde belirli bir konumu temsil eden bir *kayıtlı sunucu*, örneğin server birim veya birim kökünde bir klasör. Kendi ad alanları (örneğin, F:\sync1 ve F:\sync2) çakışan, birden çok sunucu bitiş noktaları aynı birimde bulunabilir. Bulut katmanlama ilkeleri her sunucusu uç noktası için ayrı ayrı yapılandırabilirsiniz. Bir sunucu konumu ile var olan bir dosya sunucusu uç noktası eşitleme grubuna eklerseniz, bu dosyaları eşitleme grubundaki diğer uç nokta zaten bulunan diğer dosyaları ile birleştirilir.

Bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md) Azure dosya eşitleme uçtan uca dağıtma hakkında bilgi için.

## <a name="prerequisites"></a>Önkoşullar
Sunucusu uç noktası oluşturmak için önce aşağıdaki ölçütleri karşılandığından emin olmalısınız: 
- Sunucu, Azure dosya eşitleme aracısının yüklü olduğundan ve kaydedildi. Azure dosya eşitleme Aracısı'nı yüklemek için yönergeleri bulunabilir [kaydı/kaydı sunucu bir Azure dosya eşitleme (Önizleme) ile](storage-sync-files-server-registration.md) makalesi. 
- Bir depolama eşitleme hizmeti dağıtıldıktan emin olun. Bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md) depolama eşitleme hizmetinin nasıl dağıtılacağı hakkında ayrıntılı bilgi için. 
- Eşitleme grubu dağıtıldı emin olun. Bilgi edinmek için nasıl [eşitleme grubu oluşturma](storage-sync-files-deployment-guide.md#create-a-sync-group).
- Sunucu internet'e bağlı olduğunu ve Azure erişilebilir olduğundan emin olun. Hizmetimiz ile sunucu arasındaki tüm iletişimi için 443 numaralı bağlantı noktasını kullanın.

## <a name="add-a-server-endpoint"></a>Bir sunucu uç nokta ekleyin
Sunucusu uç noktası eklemek için istenen eşitleme grubuna gidin ve "sunucusu uç noktası Ekle"'yi seçin.

![Eşitleme grubu Bölmesi'nde yeni bir sunucu uç noktası ekleme](media/storage-sync-files-server-endpoint/add-server-endpoint-1.png)

Aşağıdaki bilgiler altında gerekli **sunucusu uç noktası ekleme**:

- **Kayıtlı sunucu**: sunucu veya sunucusu uç noktası oluşturmak için küme adını.
- **Yol**: eşitleme grubunun bir parçası eşitlenecek Windows Server'da yolu.
- **Bulut Katmanlandırma**: hangi etkinleştirir sık kullanılan veya erişmek için Azure dosyaları katmanlı dosyaları etkinleştirmek veya Bulut katmanlandırma, devre dışı bırakmak için bir anahtar.
- **Birim boş alanı**: sunucusu uç noktası bulunduğu birimde ayrılan boş alan miktarı. Birim boş alanı, bir tek sunucu uç noktası olan bir birimde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları katmanlı. Öğesinden bağımsız olarak mı bulut katmanlandırma etkinleştirildiğinde, Azure dosya paylaşımınızı verilerin tam bir kopyasını eşitleme grubundaki her zaman vardır.

Seçin **oluşturma** sunucusu uç noktası eklemek için. Bir eşitleme grubundaki bir ad alanı içindeki dosyalar artık eşitlenmiş tutulacak. 

## <a name="remove-a-server-endpoint"></a>Sunucusu uç noktası Kaldır
Katmanlama sunucusu uç noktası için etkinleştirildiğinde, bulut *katmanı* , Azure dosya paylaşımları için dosya. Bu dosya sunucusu üzerindeki alanı verimli kullanılmasını sağlamak için veri kümesi, tam bir kopyasını yerine bir önbellek olarak davranmak üzere şirket içi dosya paylaşımları sağlar. Ancak, **sunucusu uç noktası hala yerel olarak sunucuda katmanlı dosyalarla kaldırılırsa, bu dosyaları erişilemez hale**. Bu nedenle, dosya erişimini şirket içi dosya paylaşımlarında istenen etseydi, tüm katmanlı dosyaları Azure dosyalarından sunucusu uç noktası siliniyor ile devam etmeden önce geri çekmek gerekir. 

Bu PowerShell cmdlet'ini aşağıda gösterildiği gibi yapılabilir:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Barındırma sunucusu yerel birim tüm katmanlı verileri geri çekmek için yeterli boş alan yoksa `Invoke-StorageSyncFileRecall` cmdlet'i başarısız.  

Sunucusu uç noktası kaldırmak için:

1. Sunucunuz, kayıtlı depolama eşitleme hizmetine gidin.
2. İstenen eşitleme grubuna gidin.
3. Depolama eşitleme hizmeti eşitleme grubunda istediğiniz sunucusu uç noktası kaldırın. Bu, eşitleme grubu bölmesinde ilgili sunucusu uç noktası sağ tıklayarak gerçekleştirilebilir.

    ![Sunucusu uç noktası eşitleme grubundan kaldırma](media/storage-sync-files-server-endpoint/remove-server-endpoint-1.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme (Önizleme) ile bir sunucu kaydı/kaydını Kaldır](storage-sync-files-server-registration.md)
- [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
