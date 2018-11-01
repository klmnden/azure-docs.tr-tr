---
title: Azure için Avere vFXT için bağlantı noktası beyaz liste
description: Azure için Avere vFXT tarafından kullanılan bağlantı noktalarının listesi
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 394415ffc7593d058d8d0bf1c0040b88ec25073b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634500"
---
# <a name="required-ports"></a>Gerekli bağlantı noktaları

Bu bölümde listelenen bağlantı noktaları için vFXT kullanılan gelen ve giden iletişimi.

Hiçbir zaman vFXT küme veya genel İnternet'e doğrudan için küme denetleyici örneği oluşturur.

## <a name="api"></a>API

| Gelen: | | |
| --- | ---- | --- |
| TCP | 22  | SSH  |
| TCP | 443 | HTTPS|



| Giden: |     |       |
|----------|-----|-------|
| TCP      | 443 | HTTPS |

 
## <a name="nfs"></a>NFS

| Gelen ve Giden  | | |
| --- | --- | ---|
| TCP/UDP | 111  | RPCBIND  |
| TCP/UDP | 2049 | NFS      |
| TCP/UDP | 4045 | NLOCKMGR |
| TCP/UDP | 4046 | MOUNTD   |
| TCP/UDP | 4047 | DURUM   |

