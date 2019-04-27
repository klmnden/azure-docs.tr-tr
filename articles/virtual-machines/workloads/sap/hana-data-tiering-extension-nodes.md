---
title: Veri uzantısı katmanlama ve düğümleri (büyük örnekler) Azure üzerinde SAP HANA için | Microsoft Docs
description: Veri uzantısı katmanlama ve düğümleri (büyük örnekler) Azure üzerinde SAP HANA için.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5868aaf29d6b57ea608763840a08f1991a982e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60628600"
---
# <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA veri katmanlama ve uzantı düğümleri kullanma

SAP, SAP NetWeaver farklı sürümleri, SAP BW ve SAP BW/4HANA için bir veri katmanlama modelini destekler. SAP belge veri katmanlama modeli hakkında daha fazla bilgi için bkz. [SAP BW/4hana'yı ve SAP BW on HANA ve SAP HANA uzantısı düğümlerle](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
HANA büyük örnek sayesinde, SAP HANA uzantısı düğümün seçeneği 1 yapılandırması SSS ve SAP blog belgelerinde açıklandığı gibi kullanabilirsiniz. Seçenek 2 yapılandırmaları aşağıdaki HANA büyük örneği SKU'ları ile ayarlanabilir: S72m, S192, S192m, S384 ve S384m. 

Adresindeki belgelere baktığınızda avantajı hemen görünmeyebilir. Ancak SAP boyutlandırma yönergeleri baktığınızda, SAP HANA uzantısı düğümleri seçeneği-1 ve 2 seçeneğini kullanarak bir avantajı görebilirsiniz. Örnekler şunlardır:

- SAP HANA boyutlandırma yönergeleri, genellikle bellek olarak double veri hacminin miktarı gerekir. Sık erişimli verileri, SAP HANA örneği çalıştırdığınızda, yüzde 50'yalnızca sahip olduğunuz veya daha az bellek veri ile doldurulan. Bellek geri kalanında, SAP HANA işlerini yapmak için ideal olarak tutulur.
- Bellek, çalışan bir SAP BW veritabanı 2 TB ile bir HANA büyük örneği S192 biriminde anlamına yalnızca 1 TB veri birimi olarak gerekir.
- Seçenek 1 ek bir SAP HANA uzantısı düğümüne kullanırsanız, ayrıca bir S192 HANA büyük örneği SKU, bu veri hacmi için ek bir 2 TB kapasite sağlar. Seçenek 2 Yapılandırması'nda bir ek 4 TB için normal veri hacmi alın. Sık erişimli düğüme karşılaştırıldığında, "sıcak" uzantısı düğümünün tam bellek kapasitesi seçeneği 1 için verileri depolamak için kullanılabilir. İki bellek, veri hacmi seçeneği 2 SAP HANA uzantısı düğüm yapılandırması için kullanılabilir.
- 3 TB kapasite ile verilerinize ve-1. seçenek 1:2 sıcak ile sık oranını edersiniz. 5 TB veri ve 1:4 oranı seçeneği 2 uzantı düğüm yapılandırması ile var.

Bellek kıyasla daha yüksek veri hacmini için soran sıcak veri disk depolama alanında depolanır olasılığı yüksek olan.

**Sonraki adımlar**
- Başvuru [azure'da SAP HANA (büyük örnekler) mimarisi](hana-architecture.md)