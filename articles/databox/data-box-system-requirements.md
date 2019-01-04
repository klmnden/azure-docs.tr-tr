---
title: Microsoft Azure Data Box sistem gereksinimleri | Microsoft Docs
description: Azure Data Box'ınızın yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/27/2018
ms.author: alkohli
ms.openlocfilehash: af7bcf2a83259b9d883a824b05312316f9f1f4f8
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794014"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box sistem gereksinimleri

Bu makalede Data Box için istemcilerin ve Microsoft Azure Data Box'ınızı için önemli sistem gereksinimlerini açıklar. Data Box'ınızı dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

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

Data Box cihaz için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Depolama hesabı** | **Notlar** |
| --- | --- |
| Klasik | Standart |
| Genel amaçlı  |Standart; V1 ve V2 desteklenir. |
| Blob |Sık ve seyrek erişimli desteklenir. |

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

Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı kullanılabilir değilse, verileri kopyalamak için 1 GbE veri bağlantısı kullanılabilir ancak kopyalama hızı etkilenir.

## <a name="next-step"></a>Sonraki adım

* [Azure Data Box'ınızı dağıtın](data-box-deploy-ordered.md)

