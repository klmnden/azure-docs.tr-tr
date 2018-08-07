---
title: Azure içeri/dışarı aktarma hizmeti için gereksinimler | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti için yazılım ve donanım gereksinimleri öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 10e8fb6ac5bcce278de3924ebd3a0d9f90392217
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528063"
---
# <a name="azure-importexport-system-requirements"></a>Azure içeri/dışarı aktarma sistem gereksinimleri

Bu makalede, Azure içeri/dışarı aktarma hizmeti için önemli gereksinimler açıklanır. İçeri/dışarı aktarma hizmeti kullanmak ve ardından geri gerekirse işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Sabit sürücüleri aşağıdaki WAImportExport aracını kullanarak hazırlamanız **BitLocker Sürücü şifrelemesini destekleyen 64-bit işletim sistemi** desteklenir.


|Platform |Sürüm |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Azure içeri/dışarı aktarma hizmeti, aşağıdaki destekler [Azure depolama hesapları](storage-account-options.md).
- Genel amaçlı v1 depolama hesaplarında (hem Klasik hem de Azure Resource Manager dağıtımları)
- Blob Depolama Hesapları
- Genel amaçlı v2 depolama hesapları

Her iş için veya yalnızca bir depolama hesabından veri aktarmak için kullanılabilir. Diğer bir deyişle, bir tek içeri/dışarı aktarma işi birden çok depolama hesabında yayılamaz. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [bir depolama hesabının nasıl oluşturulacağını](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> Azure içeri dışarı aktarma hizmeti, depolama hesaplarını desteklemiyor burada [sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md) özelliği etkinleştirildi. 

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Aşağıdaki listede yer alan depolama türlerinde Azure içeri/dışarı aktarma hizmeti ile desteklenir.


|İş  |Depolama hizmeti |Desteklenen  |Desteklenmiyor  |
|---------|---------|---------|---------|
|İçeri Aktarma     |  Azure Blob depolama <br><br> Azure dosya depolama       | Desteklenen blok Blobları ve sayfa blobları <br><br> Desteklenen dosyalar          |
|Dışarı Aktarma     |   Azure Blob depolama       | Blok blobları, sayfa blobları ve ekleme BLOB'ları desteklenir         | Azure dosyaları desteklenmiyor


## <a name="supported-hardware"></a>Desteklenen donanım 

Azure içeri/dışarı aktarma hizmeti için veri kopyalamak için desteklenen disk gerekir.

### <a name="supported-disks"></a>Desteklenen disk

Aşağıdaki listede yer alan disk, içeri/dışarı aktarma hizmeti ile kullanım için desteklenir.


|Disk türü  |Boyut  |Desteklenen |Desteklenmiyor  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3,5"       |SATA II, SATA III         |Yerleşik bir USB bağdaştırıcısı ile dış HDD <br> Bir dış HDD büyük küçük harfleri içindeki diski         |


Tek içeri/dışarı aktarma işi sahip olabilir:
- En fazla 10 HDD/SSD.
- HDD/SSD her boyuttaki bir karışımını.

Çok sayıda sürücü birden fazla iş arasında yayılabilir ve oluşturulabilen iş sayısı üst sınırı yoktur yoktur. İçeri aktarma işleri için yalnızca ilk veri hacmi sürücüsünde işlenir. Veri birimi NTFS ile biçimlendirilmiş olması gerekir.

Zaman sabit sürücüleri hazırlama ve WAImportExport aracını kullanarak veri kopyalama, dış bir USB bağdaştırıcısı kullanabilirsiniz. En çok kullanıma hazır bir USB 3.0 veya üzeri bağdaştırıcıları çalışması gerekir. 


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport Aracı'nı ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure içeri dışarı aktarma REST API örneği](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

