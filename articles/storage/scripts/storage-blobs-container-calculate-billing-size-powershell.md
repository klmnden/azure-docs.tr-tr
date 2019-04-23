---
title: Azure PowerShell betik örneği - blob kapsayıcısı toplam fatura boyutunu hesaplamak | Microsoft Docs
description: Azure Blob depolamadaki bir kapsayıcıda toplam boyutu, faturalandırma için hesaplayın.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 11/07/2017
ms.author: fryu
ms.openlocfilehash: 02b4cfcc6d88430701f653665269532a4eb7092f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799261"
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>Bir blob kapsayıcısı toplam fatura boyutunu Hesapla

Bu betik, faturalandırma maliyetlerini tahmin etmek amacıyla Azure Blob depolamadaki bir kapsayıcıda boyutunu hesaplar. Betik, bir kapsayıcıdaki blobları boyutunu toplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Bu PowerShell Betiği, faturalandırma için bir kapsayıcı boyutu hesaplar. Diğer amaçlar için kapsayıcı boyutu hesaplanıyor olup [toplam Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) bir tahmin sağlar daha basit bir komut dosyası.

## <a name="determine-the-size-of-the-blob-container"></a>Blob kapsayıcısı boyutunu belirler

Blob kapsayıcısı toplam boyutu kapsayıcının boyutu ve kapsayıcıdaki tüm blobları boyutunu içerir.

Aşağıdaki bölümlerde blob kapsayıcılar ve bloblar için depolama kapasitesini nasıl hesaplandığını açıklar. Aşağıdaki bölümde, dizedeki karakter sayısını Len(X) anlamına gelir.

### <a name="blob-containers"></a>BLOB kapsayıcıları

Hesaplama aşağıdaki blob kapsayıcı tüketilen depolama miktarını tahmin edin açıklar:

```
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
```

Dökümü aşağıda verilmiştir:
* her kapsayıcı için yük 48 bayt son değiştirilme zamanı, izinler, genel ayarları ve bazı sistem meta verileri içerir.

* Kapsayıcı adı Unicode olarak depolanır, bu nedenle karakter sayısını almak ve iki ile çarpın.

* Her depolanan blob kapsayıcı meta veri bloğu için dize değeri uzunluğu (ASCII) adının uzunluğu depolarız.

* Oturum tanımlayıcısı başına 512 bayt imzalı tanımlayıcı adı, başlangıç zamanı, süre sonu ve izinleri içerir.

### <a name="blobs"></a>Bloblar

Aşağıdaki hesaplamaları blob başına kullanılan depolama miktarı tahmin etmek nasıl gösterir.

* Blok blobu (temel blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   ```

* Sayfa blobu (temel blob veya anlık görüntü):

   ```
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   ```

Dökümü aşağıda verilmiştir:

* 124 bayt yük içeren blob için:
    - Son Değiştirme Tarihi
    - Boyut
    - Önbellek denetimi
    - Content-Type
    - İçerik dili
    - İçerik kodlama
    - İçerik MD5
    - İzinler
    - Anlık görüntü bilgileri
    - Kiralama
    - Bazı sistem meta verileri

* Blob adı Unicode olarak depolanır, bu nedenle karakter sayısını almak ve iki ile çarpın.

* Her depolanmış meta veri bloğu için dize değeri uzunluğu (ASCII saklanır), adının uzunluğu ekleyin.

* Blok blobları için:
  * engelleme listesi için 8 bayt.
  * Blok kimliği boyutu bayt cinsinden zaman blokların sayısı.
  * Kaydedilen ve kaydedilmeyen bloğu tüm verilerin boyutu.

    >[!NOTE]
    >Bu boyut, anlık görüntüler kullanıldığında, bu temel ya da anlık görüntü blob yalnızca benzersiz verilerini içerir. İşlenmemiş blokları bir hafta sonra kullanılmıyorsa, atık olarak toplanmış değildirler. Bundan sonra bunların doğru faturalama sayılmaz.

* Sayfa blobları için:
  * Veri ardışık olmayan sayfa aralıklarını 12 bayt sayısı çarpı. Bu benzersiz sayfa aralıklarını çağırırken gördüğünüz sayısıdır **GetPageRanges** API.

  * Verilerin tüm saklı sayfalarının bayt cinsinden boyutu.

    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyutun yalnızca temel blob veya sayılmakta anlık görüntü blob için benzersiz sayfalar içerir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [toplam Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) kapsayıcı boyutu tahmini sağlayan basit bir komut dosyası.

- Azure depolama faturalamasını hakkında daha fazla bilgi için bkz: [anlama Windows Azure depolama Faturalaması](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

- Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

- Ek depolama PowerShell Betiği örnekleri bulabilirsiniz [Azure depolama için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
