---
title: Azure içeri/dışarı aktarma hizmeti gereksinimleri | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için yazılım ve donanım gereksinimlerini anlayın.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/14/2018
ms.author: alkohli
ms.openlocfilehash: eb50846bd838597a6f1e9a0b6550595866e05edc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661017"
---
# <a name="azure-importexport-system-requirements"></a>Azure içeri/dışarı aktarma sistem gereksinimleri

Bu makalede Azure içeri/dışarı aktarma hizmetiniz için önemli gereksinimler açıklanır. İçeri/dışarı aktarma hizmeti kullanmak ve daha sonra geri gerekirse işlemi sırasında başvurduğu önce bilgileri dikkatle gözden geçirmenizi öneririz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki WAImportExport aracını kullanarak sabit sürücüleri hazırlamak için **BitLocker Sürücü Şifrelemesi desteği 64-bit işletim sistemi** desteklenir.


|Platform |Sürüm |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |



## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Azure içeri/dışarı aktarma hizmeti aşağıdaki Azure depolama hesaplarını destekler.
- Klasik
- Blob Depolama Hesapları
- Genel amaçlı v1 depolama hesabı. 

Her iş için veya yalnızca bir depolama hesabından veri aktarmak için kullanılabilir. Diğer bir deyişle, bir tek içeri/dışarı aktarma işi birden çok depolama hesaplarında yayılamaz. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> Azure içeri aktarma dışarı aktarma hizmeti depolama hesaplarını desteklemiyor nerede [sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md) özelliği etkinleştirildi. 

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Aşağıdaki listede, depolama türlerinin Azure içeri/dışarı aktarma hizmeti ile desteklenir.


|İş  |Depolama  |Desteklenen  |Desteklenmiyor  |
|---------|---------|---------|---------|
|İçeri Aktarma     |  Azure Blob Depolama. <br>Blok Blobları, sayfa bloblarını desteklenir. <br> Azure dosyaları desteklenir.       |         |
|Dışarı Aktarma     |   Azure Blob Depolama. <br>Blok blobları, sayfa blobları ve ekleme BLOB'ları desteklenen.       | Azure dosyaları desteklenmiyor.        |


## <a name="supported-hardware"></a>Desteklenen donanım 

Azure içeri/dışarı aktarma hizmeti için desteklenen diskiniz olması gerekir ve verileri kopyalamak için SATA bağlayıcılar desteklenir.

### <a name="supported-disks"></a>Desteklenen diskleri

Aşağıdaki listede disklerin içeri/dışarı aktarma hizmeti ile kullanım için desteklenir.


|Disk türü  |Boyut  |Desteklenen |Desteklenmiyor  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3,5"       |SATA II, SATA III         |Yerleşik USB bağdaştırıcısı ile dış HDD <br> Bir dış HDD kasa içinde disk         |


Bir tek içeri/dışarı aktarma işi sahip olabilir:
- En fazla 10 HDD/SSD.
- HDD/SSD herhangi bir boyuttaki bir karışımını.

Büyük sayıda sürücüsü birden çok iş arasında yayılabilir ve oluşturulabilir işlerin sayısı sınırı yoktur. 

İçeri aktarma işi için yalnızca ilk veri birimi sürücüde işlenir. Veri birimi NTFS ile biçimlendirilmiş olması gerekir.

### <a name="supported-external-usb-adaptors"></a>Desteklenen dış USB bağdaştırıcıları

İç HDD veri kopyalamak için kullanılan dış USB bağdaştırıcıları listesi aşağıda verilmiştir. 
- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Orico 6628SUS3-C-SİY (6628 Series)
- Thermaltake BlacX etkin takas SATA dış sabit sürücü yerleştirme istasyon (USB 2.0 & eSATA)


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport aracı ayarlamak](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure alma verme REST API örnek](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

