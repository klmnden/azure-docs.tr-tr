---
title: Key Vault sürümleri
description: Azure Key Vault çeşitli sürümleri
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: bryanla
ms.openlocfilehash: 50d35f0dbfb180628577286d6e658daca3c41dc8
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42058291"
---
# <a name="key-vault-versions"></a>Key Vault sürümleri

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01 - yönetilen depolama hesabı anahtarları

Yaz 2017 - Azure depolama ile kolay tümleştirme depolama hesabı anahtarlarını özelliği eklendi. Daha fazla bilgi için genel bakış konusuna [yönetilen depolama hesabı anahtarlarına genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01-geçici silme

Yaz 2017 - geçici silme özelliği nesneleri anahtar kasalarını ve anahtar kasası geliştirilmiş veri koruma için eklendi. Daha fazla bilgi için genel bakış konusuna [geçici silmeyi genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01 - sertifika yönetimi

Sertifika yönetiminin bir özelliği olarak GA sürümü için 2015-06-01 26 Eylül 2016'da eklenir.

## <a name="2015-06-01---general-availability"></a>2015-06-01 - genel kullanılabilirlik

Genel kullanılabilirlik sürümü 2015-06-01 24 Haziran 2015'te duyurdu.

Bu sürümde aşağıdaki değişiklikler yapılmıştır:

- Bir anahtarı silme - "kullanın. alan kaldırıldı".
- Bir anahtar hakkında - "kaldırıldı alan kullan" bilgi alın.
- Bir kasaya anahtar aktarma - "kullanın. alan kaldırıldı".
- Bir anahtarı geri yükleme - "kullanın. alan kaldırıldı".
- Değiştirilen "RSA_OAEP" için "RSA-OAEP" RSA algoritmalar için. Bkz: [anahtarlara, parolalara ve sertifikalara hakkında](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-Önizleme 

İkinci Önizleme sürümü 2015-02-01-preview, 20 Nisan 2015'te duyurdu. Daha fazla bilgi için [REST API güncelleştirme](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blog gönderisi.

Aşağıdaki görevleri güncelleştirildi:

- Bir kasadaki - eklenen sayfalandırma destek işleme anahtarları listesi.
- Sürümleri Listele işleminin bir anahtarın sürümlerini listeleme için bir anahtar - eklendi.
- Bir kasadaki - eklenen sayfalandırma destek gizli anahtarları listeleme.
- Bir gizli anahtarın sürümlerini listesi - bir gizli anahtarın sürümlerini listelemek için işlem ekleyin.
- Tüm işlemler - öznitelikleri oluşturulan/güncelleştirilen zaman damgası eklenir.
- -Gizli dizi oluşturma için gizli dizileri Content-Type eklendi.
- -Anahtar oluşturma etiketler isteğe bağlı bilgi eklenir.
- -Gizli dizi oluşturma etiketler isteğe bağlı bilgi eklenir.
- Bir anahtarı - güncelleştirme etiketler isteğe bağlı bilgi eklenir.
- Gizli anahtarı - güncelleştirme etiketler isteğe bağlı bilgi eklenir.
- Parolalar için maksimum boyut 10 K 25 K bayt ile değiştirildi. Bkz, [anahtarlara, parolalara ve sertifikalara hakkında](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>2014-12-08-Önizleme

8 Ocak 2015'te ilk önizleme sürümü 2014-12-08-preview, duyurdu.

## <a name="see-also"></a>Ayrıca bkz.
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
