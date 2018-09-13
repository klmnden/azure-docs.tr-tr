---
title: Azure aboneliği, üst düzey bilgileri verme | Microsoft Docs
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
ms.date: 06/15/2018
ms.author: adpick
ms.openlocfilehash: 7bc9f4df6e98dd86283c4389466b13b8f6bb4d15
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35970253"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Dışarı aktarma ve üst düzey abonelik bilgilerinizi görüntüleyin
Abonelik kimliklerini, kullanıcı kimlik bilgileriyle ilişkili kümesini görüntülemek istiyorsanız [abonelik bilgilerinizi içeren bir .json dosyası Azure hesap Merkezi'nden](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

İndirilen bir .json dosyası, aşağıdaki bilgileri sağlar:
- E-posta: hesabınızla ilişkili e-posta adresi.
- PUID: fatura hesabınızla ilişkilendirilmiş benzersiz tanımlayıcısı.
- Subscriptionıds: Abonelik kimliğine göre listelenmiş hesabınıza ait aboneliklerin listesi

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
