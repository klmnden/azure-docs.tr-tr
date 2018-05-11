---
title: Anahtar kasası sürümleri
description: Azure anahtar kasası çeşitli sürümleri
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: beb73be66f36ccf95fe27d4d8128106cd12722a8
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="key-vault-versions"></a>Anahtar kasası sürümleri

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01 - yönetilen depolama hesabı anahtarları

Yaz 2017 - Azure Storage ile kolay tümleştirme depolama hesabı anahtarlarını özellik eklemiştir. Daha fazla bilgi için genel bakış konusuna [yönetilen depolama hesabı anahtarlarını genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01-soft-Sil

Yaz 2017 - soft-delete özelliği nesneleri anahtar kasalarını ve anahtar kasası geliştirilmiş veri koruma için eklendi. Daha fazla bilgi için genel bakış konusuna [Soft-delete genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01 - sertifika yönetimi

Sertifika yönetimi bir özellik olarak GA sürümü 2015-06-01 26 Eylül 2016 tarihinde eklenir.

## <a name="2015-06-01---general-availability"></a>2015-06-01 - genel kullanılabilirlik

Genel kullanılabilirlik sürümü 2015-06-01 24 Haziran 2015 tarihinde duyurdu.

Bu sürümde aşağıdaki değişiklikler yapıldı:

- Bir anahtar delete - "kaldırılmış alan kullan".
- Bir anahtarı hakkında - "kaldırılmış alan kullan" bilgi alın.
- Bir kasaya bir anahtar alma - "kaldırılmış alan kullan".
- Bir anahtarı geri yükleme - "kaldırılmış alan kullan".
- Değiştirilen "RSA_OAEP" "RSA-OAEP" RSA algoritmalar için. Bkz: [anahtarları, gizli ve sertifikaları hakkında](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-Önizleme 

İkinci Önizleme sürümü 2015-02-01-Önizleme, 20 Nisan 2015 tarihinde duyurdu. Daha fazla bilgi için bkz: [REST API güncelleştirme](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blog postası.

Aşağıdaki görevleri güncelleştirildi:

- Bir kasadaki - eklenen sayfalandırma desteklemek için işlem anahtarları listeler.
- Sürümleri listesinde bir anahtar - bir anahtarın sürümlerini listelemek için işlemi eklendi.
- Bir kasadaki - eklenen sayfalandırma destek gizli anahtarları listeler.
- Bir gizli anahtarın sürümlerini listesi - ekleme işlemi bir gizli anahtarın sürümlerini listeler.
- Tüm işlemleri - öznitelikleri oluşturan ve güncelleştirilen zaman damgaları eklendi.
- -Gizli anahtar oluşturma için gizli Content-Type eklendi.
- Bir anahtar - oluşturma etiketler isteğe bağlı bilgiler eklendi.
- -Gizli anahtar oluşturma etiketler isteğe bağlı bilgiler eklendi.
- Bir anahtarı - güncelleştirme etiketler isteğe bağlı bilgiler eklendi.
- Gizli - güncelleştirme etiketler isteğe bağlı bilgiler eklendi.
- Gizli anahtarları için en büyük boyutu 10 K 25 K bayt ile değiştirildi. Bakın, [anahtarları, gizli ve sertifikaları hakkında](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>2014-12-08-Önizleme

İlk önizleme sürümü 2014-12-08-Önizleme, 8 Ocak 2015 tarihinde duyurdu.

## <a name="see-also"></a>Ayrıca bkz.
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
