---
title: "Azure PowerShell Betiği örnek - hesaplamak blob kapsayıcı boyutu | Microsoft Docs"
description: "Azure Blob depolamada kapsayıcı boyutu her bloblarını boyutunu toplanmasıyla hesaplayın."
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
ms.date: 10/23/2017
ms.author: fryu
ms.openlocfilehash: 46f3eac0129da41062caba2da090f9e532505d67
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Bir Blob Depolama kapsayıcısını boyutu hesaplanamadı

Bu komut, BLOB kapsayıcısında boyutunu toplanmasıyla Azure Blob depolamada kapsayıcı boyutu hesaplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>Blob Depolama kapsayıcısını boyutunu anlama

Blob Depolama kapsayıcısını toplam boyutu, kapsayıcının kendisi boyutunu ve tüm BLOB kapsayıcısı altında boyutunu içerir.

Depolama kapasitesi Blob kapsayıcılar ve Bloblar için nasıl hesaplandığını açıklar. İçinde Len(X) dizedeki karakter sayısını gösterir.

### <a name="blob-containers"></a>BLOB kapsayıcıları

Blob kapsayıcısı harcanan depolama alanı miktarı tahmin etmek nasıl verilmiştir:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Çözümleme verilmiştir:
* her kapsayıcı için yük 48 bayt son değiştirilme zamanı, izinler, genel ayarları ve bazı sistem meta verileri içerir.
* Kapsayıcı adı, Unicode böylece karakter sayısını alın ve 2 ile çarpın olarak depolanır.
* Blob kapsayıcı meta verileri depolanan için her (ASCII olarak depolanır) adının uzunluğu artı dize değeri uzunluğu depolarız.
* Başlangıç saati, sona erme saati ve izinler, imzalı tanımlayıcı başına 512 bayt imzalı tanımlayıcı adı içerir.

### <a name="blobs"></a>Bloblar

Blob harcanan depolama alanı miktarı tahmin etmek nasıl verilmiştir:

* Blok Blob (temel blob veya anlık görüntü)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* Sayfa blobu (temel blob veya anlık görüntü)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

Çözümleme verilmiştir:

* Son değiştirilme zamanı'nı içeren blob yükü 124 bayt boyutu, Cache-Control Content-Type, Content-Language, Content-Encoding, Content-MD5, izinler, anlık görüntü bilgileri, kiralama ve bazı sistem meta verileri.
* Blob adı, Unicode böylece alırken karakter sayısı ve birden çok 2 tarafından depolanır.
* Ardından her meta verileri için saklanan (ASCII olarak depolanır) adının uzunluğu artı dize değeri uzunluğu.
* Ardından blok Bloblar için
    * engelleme listesi için 8 bayt
    * Blok kimliği boyutunu bayt cinsinden zaman bloğu sayısı.
    * Ek olarak kabul edilen ve kaydedilmemiş blokları tümünde veri boyutu. Not, anlık görüntüler kullanıldığında, bu boyut yalnızca bu temel ya da anlık görüntü blob'u için benzersiz veriler içerir. Kaydedilmemiş blokları bir hafta sonra kullanılmıyorsa toplanacak olur ve ardından o anda bunlar artık faturalama bundan sonra doğru sayılacaktır.
* Daha sonra sayfa Bloblarını
    * Veri ardışık olmayan sayfa aralıklarını sayısı 12 bayt zaman. GetPageRanges API çağrılırken gördüğünüz benzersiz sayfa aralıklarını sayısıdır.
    * Ek olarak tüm depolanmış sayfaları bayt cinsinden veri boyutu. Not, anlık görüntüler kullanıldığında, bu boyut yalnızca temel blob veya anlık görüntü blob sayılan için benzersiz sayfalar içerir.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Sonraki adımlar

Azure depolama faturalama hakkında daha fazla bilgi için bkz: [anlama Windows Azure depolama faturalama](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek depolama alanı PowerShell komut dosyası örnekleri bulunabilir [Azure Storage PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
