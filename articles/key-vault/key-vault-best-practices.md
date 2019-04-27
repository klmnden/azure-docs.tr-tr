---
title: En iyi uygulamalar, Azure Key Vault anahtar kasası - kullanılacak | Microsoft Docs
description: Bu belge, anahtar kasası için en iyi bazılarını açıklar
services: key-vault
author: mbaldwin
manager: barbkess
tags: azure-key-vault
ms.service: key-vault
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: ee4418700cec5de1dc404c3669dd5de315aab983
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640778"
---
# <a name="best-practices-to-use-key-vault"></a>Anahtar kasası için en iyi uygulamalar

## <a name="control-access-to-your-vault"></a>Kasanızı erişimi denetleme

Azure Key Vault, sertifikalar, bağlantı dizeleri ve parolalar gibi şifreleme anahtarlarını ve gizli koruyan bir bulut hizmetidir. Bu veri büyük/küçük harfe duyarlıdır ve iş açısından kritik, güvenli anahtar kasalarınıza erişimi yalnızca yetkili uygulamaların ve kullanıcıların vererek gerekir çünkü. Bu [makale](key-vault-secure-your-key-vault.md) anahtar kasası erişim modeline genel bakış sağlar. Kimlik doğrulama ve yetkilendirme açıklanmakta ve anahtar kasalarınıza erişim güvenliğinin nasıl sağlanacağını açıklar.

Kasanızı erişimi denetleme sırasında önerileri aşağıdaki gibidir:
1. Abonelik, kaynak grubu ve anahtar Kasası'nı (RBAC) erişimi kilitleme
2. Her kasa için erişim ilkeleri oluşturun.
3. Erişim vermek için en az ayrıcalık erişim sorumlusu kullanın
4. Güvenlik duvarını ve [sanal ağ hizmet uç noktaları](key-vault-overview-vnet-service-endpoints.md)

## <a name="use-separate-key-vault"></a>Ayrı Key Vault'u kullanma

Her ortam (geliştirme, ön üretim ve üretim) uygulama başına bir kasa Bizim önerimiz kullanmaktır. Bu, gizli dizileri ortamlar genelinde paylaşılmaz yardımcı olur ve ayrıca bir ihlal durumunda tehdidi azaltır.

## <a name="backup"></a>Backup

Normal geri yedeklerine emin olmak sizin [kasası](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) güncelleştirme/silme/oluştururken bir kasa içinde nesne.

## <a name="turn-on-logging"></a>Günlük özelliğini açar

[Günlük özelliğini açar](key-vault-logging.md) kasanız için. Ayrıca, uyarılar ayarlayın.

## <a name="turn-on-recovery-options"></a>Kurtarma seçeneklerini Aç

1. Açma [geçici silme](key-vault-ovw-soft-delete.md).
2. İsterseniz geçici silme bile açıldıktan sonra kasa gizli dizi zorla silinmesini karşı koruma sağlamak temizleme korumasını etkinleştirin.
