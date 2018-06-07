---
title: Azure aboneliği üst düzey bilgilerinizi verin | Microsoft Docs
description: Tüm Azure abonelik kimlikleri hesabınızla ilişkili nasıl görüntüleyebileceğiniz açıklanır.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/9/2017
ms.author: adpick
ms.openlocfilehash: b995b0c7154faab66a44bef1a7d74eeb8404e5c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655454"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Dışarı aktarmak ve üst düzey abonelik bilgilerinizi görüntüleyin
Abonelik kimlikleri, kullanıcı kimlik bilgileriyle ilişkili kümesini görüntülemek gerekiyorsa [abonelik bilgilerinizi içeren bir .json dosyası Azure hesap Merkezi'nden](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

İndirilen .json dosyası aşağıdaki bilgileri sağlar:
- E-posta: hesabınızla ilişkili e-posta adresi.
- PUID: fatura hesabınızla ilişkili benzersiz tanımlayıcısı.
- SubscriptionIds: Abonelik kimliğine göre numaralandırılan hesabınıza ait Aboneliklerin listesini

### <a name="subscriptionsjson-sample"></a>Subscriptions.JSON örnek
~~~~
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
~~~~
