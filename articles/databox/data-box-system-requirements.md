---
title: Microsoft Azure Data Box sistem gereksinimleri | Microsoft Docs
description: Azure Data Box'ınızın yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/23/2019
ms.author: alkohli
ms.openlocfilehash: 7d52af9e3948f40936795efab5b6671c3f71007a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746939"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box sistem gereksinimleri

Bu makalede Data Box için istemcilerin ve Microsoft Azure Data Box'ınızı için önemli sistem gereksinimlerini açıklar. Data Box'ınızı dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatle gözden geçirmeniz önerilir.

Sistem gereksinimleri şunlardır:

* **Data Box bağlanma ana bilgisayarları için yazılım gereksinimleri** -desteklenen platformlar, yerel web kullanıcı Arabirimi için tarayıcılar, SMB istemcileri ve Data Box için bağlanabilir konaklar için tüm ek gereksinimleri açıklanmaktadır.
* **Data Box için ağ gereksinimleri** -Data Box'ın en iyi işlemi için ağ gereksinimleri hakkında bilgi sağlar.


## <a name="software-requirements"></a>Yazılım gereksinimleri

Yazılım gereksinimleri, desteklenen işletim sistemleri, yerel web kullanıcı Arabirimi ile desteklenen tarayıcılar ve SMB istemcileri hakkında bilgi içerir.

### <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

Data Box cihazına bağlı istemcileri aracılığıyla veri kopyalama işlemi için desteklenen işletim sistemlerinin bir listesi aşağıda verilmiştir.

| **İşletim sistemi** | **Sürümleri** | 
| --- | --- | 
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 | 
| Windows |7, 8, 10 | 
|Linux    |         |

### <a name="supported-file-systems-for-linux-clients"></a>Linux istemcileri için desteklenen dosya sistemleri

| **Protokolleri** | **Sürümleri** | 
| --- | --- | 
| SMB |2.X ve üzeri |
| NFS | En fazla ve 4.1 dahil tüm sürümler|

### <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Desteklenen depolama hesapları ve Data Box cihaz için depolama türleri listesi aşağıda verilmiştir. Depolama hesapları ve bunların tüm özelliklerini tüm farklı türlerinin tam listesi için bkz. [türlerde depolama hesapları](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Depolama hesabı / desteklenen depolama türleri** | **Blok blobu** |**Sayfa blobu*** |**Azure dosyaları** |**Notlar**|
| --- | --- | -- | -- | -- |
| Klasik standart | E | E | E |
| Genel amaçlı v1 standart  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v1 Premium  |  | E| | |
| Genel amaçlı v2 standart  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v2 Premium  |  |E | | |
| BLOB Depolama standart |E | | |Sık ve seyrek erişimli desteklenir. |

\* *-Verileri sayfa blobları karşıya 512 bayt VHD'ler gibi hizalı olmalıdır.*

>[!NOTE]
> Azure Data Lake depolama Gen 2 hesapları desteklenmez.


### <a name="supported-storage-types"></a>Desteklenen depolama türleri

Data Box cihaz için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure blok blobu | |
| Azure sayfa blobu  | Veri 512 bayt hizalı olmalıdır.|
| Azure Dosyaları | |


### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları

Yerel web kullanıcı Arabirimi için desteklenen web tarayıcıları listesi aşağıda verilmiştir.

| **Tarayıcı** | **Sürümleri** | **Ek gereksinimler/Notlar** |
| --- | --- | --- |
| Google Chrome |En son sürümü |Chrome ile test|
| Microsoft Edge |En son sürümü | |
| FireFox | En son sürümü | FireFox ile test|
| Internet Explorer |En son sürümü |Oturum tanımlama bilgileri ve Javascript etkin olup olmadığını denetleyin. Kullanıcı Arabirimi erişimini etkinleştirmek için cihaz IP Ekle **gizlilik eylemleri** böylece cihaz tanımlama bilgilerine erişebilir. |


## <a name="networking-requirements"></a>Ağ gereksinimleri

Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı kullanılabilir değilse, 1 GbE veri bağlantısı, veri ancak hızı etkilenir kopyalama kopyalamak için kullanılabilir.

## <a name="next-step"></a>Sonraki adım

* [Azure Data Box'ınızı dağıtın](data-box-deploy-ordered.md)

