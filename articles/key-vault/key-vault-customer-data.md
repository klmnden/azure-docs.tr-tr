---
title: Azure anahtar kasası müşteri veri özellikleri | Microsoft Docs
description: Anahtar kasası müşteri verilerine hakkında bilgi edinin
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/22/2018
ms.author: barclayn
ms.openlocfilehash: 1ddc74b1960095509a77d4b3072017847df42d90
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637370"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure anahtar kasası müşteri veri özellikleri

Azure anahtar kasası oluşturma veya güncelleştirme kasaları, anahtarları, gizli, sertifikalar ve yönetilen depolama hesapları sırasında müşteri verilerini alır. Bu müşteri verilerini Azure portalında ve REST API'si aracılığıyla doğrudan görünür olur. Müşteri verileri düzenlenmesine veya güncelleştirme ya da verileri içeren nesneyi silme silindi.

Bir kullanıcı veya uygulama anahtar kasası eriştiğinde sistem erişim günlükleri üretilir. Ayrıntılı erişim günlükleri, Azure Öngörüler kullanan müşteriler için kullanılabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Aşağıdaki bilgileri, müşteri verileri Azure anahtar kasası içinde tanımlar:

- Azure anahtar kasası için erişim ilkeleri içeren nesne kimlikleri kullanıcıları, grupları veya uygulamaları temsil eden
- Sertifika konuları e-posta adresleri veya başka bir kullanıcı veya kuruluş tanımlayıcılar içerebilir
- Sertifika kişiler kullanıcı e-posta adresleri, adları veya telefon numaraları içerebilir
- Sertifika verenler e-posta adresleri, adları, telefon numaralarını, hesap kimlik bilgilerini ve kuruluş ayrıntıları içerebilir
- Azure anahtar Kasası'nda nesnelere rasgele etiketleri uygulanabilir. Bu nesneler kasaları, anahtarları, gizli, sertifikalar ve depolama hesaplarını içerir. Kullanılan etiketleri kişisel verileri içerebilir
- Azure anahtar kasası erişim günlükleri içeren nesne kimlikleri [UPN'ler](../active-directory/connect/active-directory-aadconnect-userprincipalname.md), her REST API çağrısı için ve IP adresi
- Azure anahtar kasası tanılama günlükleri nesne kimlikleri ve IP adresleri için REST API çağrıları içerebilir

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Ayrıca REST API'leri, Portal deneyimi ve kasa, anahtarları, gizli, sertifikalar ve yönetilen depolama hesapları oluşturmak için kullanılan SDK'ları güncelleştirin ve bu nesneleri silmek mümkün değildir.

Geçici silme, silinen veriler 90 gün sonra silme işlemi için kurtarmanıza olanak tanır. Geçici silme kullanırken, verileri kalıcı olarak temizleme işlemi gerçekleştirerek saklama dönemi sona 90 gün önce silinebilir. Kasa veya abonelik için yapılandırılmışsa, blok temizleme işlemleri, zamanlanmış bekletme süresi geçene kadar verileri kalıcı olarak silmek mümkün değildir.

## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma

Aynı REST API'leri, portal deneyimi ve kasa, anahtarları, gizli, sertifikalar oluşturmak için kullanılan SDK'ları ve ayrıca hesaplardır yönetilen depolama görüntülemek ve bu nesneleri dışarı aktarmak sağlar.

Azure anahtar kasası erişim günlüğü için açık isteğe bağlı bir özellik olan her REST API çağrısı için günlükler oluşturur. Bu günlükler, kuruluşunuzun gereksinimlerini karşılayan bekletme ilkesini uygulamak burada aboneliğinizde bir depolama hesabı aktarılır.

Kişisel veriler içeren azure anahtar kasası tanılama günlükleri, kullanıcı gizlilik Portalı'nda bir dışarı aktarma isteği yaparak alınabilir. Bu istek Kiracı Yöneticisi tarafından yapılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası Günlüğü](key-vault-logging.md)

- [Azure anahtar kasası soft-delete genel bakış](key-vault-soft-delete-cli.md)

- [kasaları](https://docs.microsoft.com/rest/api/keyvault/vaults)

- [Azure anahtar kasası anahtar işlemleri](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure anahtar kasası gizli anahtar işlemleri](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Azure anahtar kasası sertifikaları ve ilkeleri](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Sertifika verenler](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers)

- [Azure anahtar kasası depolama hesabı işlemleri](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
