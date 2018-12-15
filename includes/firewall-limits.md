---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 12/14/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 7d905550114bb76a0a091146b3972bab4a652022
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430061"
---
| Kaynak | Varsayılan limit |
| --- | --- |
| İşlenen veriler |1000 TB/güvenlik duvarı/ay <sup>1</sup> |
|Kurallar|10k - tüm kural türleri birleştirilmiş|
|Genel eşleme|Desteklenmiyor. Bölge başına en az bir güvenlik duvarı dağıtımına sahip olmalıdır.|
|Bir tek ağ kuralı en fazla bağlantı noktaları|15<br>Bir bağlantı noktası aralığı (örneğin: 2 - 10) iki olarak kabul edilir.
|En düşük AzureFirewallSubnet boyutu |/26|
|Bağlantı noktası aralığında ağ ve uygulama kuralları|0-64, 000. İş, bu sınırlama gevşetmek için devam ediyor.|
|


<sup>1</sup> durumda bu sınırları artırmak için Azure Destek birimine başvurun.
