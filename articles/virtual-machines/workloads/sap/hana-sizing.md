---
title: SAP hana (büyük örnekler) azure'da boyutlandırma | Microsoft Docs
description: SAP hana (büyük örnekler) azure'da boyutlandırma.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c7a81468fd19d3bd5d6eee79bb8a75396e3021f1
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707275"
---
# <a name="sizing"></a>Boyutlandırma

Boyutlandırma HANA büyük örneği için HANA için genel olarak boyutlandırma değerinden farklı değildir. Mevcut ve taşımak için diğer RDBMS HANA için SAP sağlayan çok sayıda mevcut SAP sistemlerinde çalışan bir rapor istediğiniz sistemlerini dağıtılabilir. Veritabanı için HANA taşınırsa, bu raporlar verileri kontrol edip HANA örneği için bellek gereksinimleri hesaplayın. Bu raporları çalıştırmak ve bunların en son düzeltme eklerinin veya sürümler elde etme hakkında daha fazla bilgi için aşağıdaki SAP notları okuyun:

- [SAP notu #1793345 - SAP Suite on HANA ve boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [Not #1872170 - Suite HANA ve s/4 HANA boyutlandırma rapor üzerinde SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP notu #2121330 - SSS: SAP BW on HANA ve rapor boyutlandırma](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP BW on HANA ve boyutlandırma raporu #1736976 - Not](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP notu #2296290 - yeni rapor, BW on HANA ve için boyutlandırma](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamalar için hızlı Boyutlandırıcı SAP HANA SAP yazılımlarını uygulamasını bellek gereksinimlerini hesaplamak kullanılabilir.

Veri hacmi büyüdükçe, HANA için bellek gereksinimleri artırın. Bu gelecekte olacak şekilde neler olduğunu tahmin yardımcı olması için geçerli bellek tüketimi dikkat edin. Bellek gereksinimlerine bağlı olarak, ardından, isteğe bağlı bir HANA büyük örneği için başka bir SKU eşleyebilirsiniz.

**Sonraki adımlar**
- Başvuru [ekleme gereksinimleri](hana-onboarding-requirements.md)