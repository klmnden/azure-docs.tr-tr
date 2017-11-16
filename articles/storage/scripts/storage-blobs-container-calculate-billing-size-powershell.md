---
title: "Azure PowerShell Betiği örnek - bir blob kapsayıcısını Fatura toplam boyutunu hesaplayabilir | Microsoft Docs"
description: "Azure Blob depolamada kapsayıcı toplam boyutu faturalandırma amacıyla hesaplayın."
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 11/07/2017
ms.author: fryu
ms.openlocfilehash: a9d7cc69fbbd037a553e877ca9c26d84c376ccc0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>Bir blob kapsayıcısını toplam fatura boyutu hesaplanamıyor

Bu komut dosyası fatura maliyetlerini tahmin etmek amacıyla Azure Blob depolamada kapsayıcı boyutu hesaplar. Betik BLOB kapsayıcısında boyutunu toplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Bu PowerShell Betiği fatura amaçlar için bir kapsayıcı boyutu hesaplar. Diğer amaçlar için kapsayıcı boyutu hesaplanıyorsa bkz [bir Blob Depolama kapsayıcısını toplam boyutu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) tahmini sağlayan basit bir komut dosyası için.

## <a name="determine-the-size-of-the-blob-container"></a>Blob kapsayıcısı boyutunu belirleme

Blob kapsayıcısı toplam boyutu, kapsayıcı boyutu ve tüm BLOB kapsayıcısı altında boyutunu içerir.

Aşağıdaki bölümlerde depolama kapasitesi blob kapsayıcılar ve bloblar için nasıl hesaplandığını açıklar. Aşağıdaki bölümde dizedeki karakter sayısını Len(X) anlamına gelir.

### <a name="blob-containers"></a>BLOB kapsayıcıları

Hesaplama aşağıdaki blob kapsayıcısı harcanan depolama alanı miktarı tahmin etmek açıklar:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Çözümleme aşağıdadır:
* her kapsayıcı için yük 48 bayt son değiştirilme zamanı, izinler, genel ayarları ve bazı sistem meta verileri içerir.

* Kapsayıcı adı Unicode olarak depolanır, böylece karakter sayısını alın ve iki ile çarpın.

* Her depolanan blob kapsayıcı meta veri bloğu için dize değeri uzunluğu (ASCII) adının uzunluğu depolarız.

* Oturum tanımlayıcısı başına 512 bayt imzalı tanımlayıcı adı, başlangıç zamanı, bitiş saati ve izinleri içerir.

### <a name="blobs"></a>Bloblar

Aşağıdaki hesaplamaları blob harcanan depolama alanı miktarı tahmin etmek nasıl gösterir.

* Blok blob (temel blob veya anlık görüntü):

   `
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   `

* Sayfa blobu (temel blob veya anlık görüntü):

   `
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   `

Çözümleme aşağıdadır:

* 124 bayt yük içeren blob için:
    - Son değiştirme tarihi
    - Boyut
    - Cache-Control
    - İçerik türü
    - İçerik dili
    - İçerik kodlama
    - Content-MD5
    - İzinler
    - Anlık görüntü bilgileri
    - Kira
    - Bazı sistem meta verileri

* Blob adı Unicode olarak depolanır, böylece karakter sayısını alın ve iki ile çarpın.

* Her depolanan meta veri bloğu için dize değeri uzunluğu (ASCII olarak depolanır) adının uzunluğu ekleyin.

* Blok bloblar için:
    * engelleme listesi için 8 bayt sayısı.
    * Blok sayısı blok kimliği boyutunu bayt cinsinden zaman.
    * Tüm tamamlanan ve kaydedilmemiş blokları verilerin boyutu. 
    
    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyut bu temel ya da anlık görüntü blobu için yalnızca benzersiz verileri içerir. Kaydedilmemiş blokları bir hafta sonra kullanılmıyorsa çöpünün toplanma oldukları. Bundan sonra bunların doğru faturalama sayılmaz.

* Sayfa bloblarını için:
    * Veri ardışık olmayan sayfa aralıklarını 12 bayt sayıda. Çağrılırken gördüğünüz benzersiz sayfa aralıklarını sayısıdır **GetPageRanges** API.

    * Tüm depolanmış sayfaları bayt cinsinden veri boyutu. 
    
    >[!NOTE]
    >Anlık görüntüler kullanıldığında, bu boyut yalnızca benzersiz sayfalar için temel blob veya kabul edilir anlık görüntü blob içerir.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [bir Blob Depolama kapsayıcısını toplam boyutu hesaplamak](../scripts/storage-blobs-container-calculate-size-powershell.md) kapsayıcı boyutu tahmini sağlayan basit bir komut dosyası için.

- Azure Storage faturalama hakkında daha fazla bilgi için bkz: [anlama Windows Azure depolama faturalama](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

- Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.4.1).

- Ek depolama PowerShell komut dosyası örnekleri bulabilirsiniz [Azure Storage PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
