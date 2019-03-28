---
title: Azure App Service için Windows Java Çalışma zamanı desteği
description: Bu konu, Windows üzerinde desteği Azure App Service için Java Çalışma zamanı deyiminin sağlar.
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 3/22/2019
ms.topic: article
ms.service: app-service
manager: dougE
ms.openlocfilehash: ab0e9b9f272108745b629d2cb284f6c5ab52c76f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58523133"
---
# <a name="java-runtime-statement-of-support-for-app-service-on-windows"></a>Destek için Windows üzerinde App Service'te Java Çalışma zamanı bildirimi

## <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve Bakım

Azure'nın desteklenen Java Development Kit (JDK) olan [Zulu](https://www.azul.com/downloads/azure-only/zulu/) aracılığıyla sağlanan [Azul Systems](https://www.azul.com/).

Ana sürüm güncelleştirmeleri ile Windows için Azure App service'taki yeni çalışma zamanı seçenekleri sağlanır. Müşteriler, App Service dağıtımı yapılandırarak Java daha yeni sürümleri için güncelleştirme ve test etmeden sorumlu ve ihtiyaçlarını karşılayan önemli güncelleştirme sağlama.

Desteklenen JDK otomatik olarak üç aylık olarak Ocak, Nisan, Temmuz ve Ekim her yıl düzeltme eki.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azul sistemlerden kullanılabilir olduklarında hemen sonra yamaları ve düzeltmeler önemli güvenlik açıkları için kullanıma sunulacaktır. "Büyük" bir güvenlik açığı 9.0 temel puanı ile tanımlanan ya da üzerinde daha büyük [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm).

## <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve devre dışı bırakma

Desteklenen Java Çalışma zamanı kullanımdan kaldırılacak, çalışma zamanı kullanımdan önce en az altı ay etkilenen çalışma zamanı kullanan Azure geliştiricileri, kullanımdan kaldırma bildirimi verilir.

## <a name="local-development"></a>Yerel geliştirme

Geliştiriciler, üretim sürümü, Azul Zulu Enterprise JDK yerel geliştirme için indirebileceği [Azul'ın indirme sitesinde](https://www.azul.com/downloads/azure-only/zulu/).

## <a name="development-support"></a>Geliştirme desteği

Ürün desteği [Azure tarafından desteklenen Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) Microsoft Azure için geliştirmeye olduğunda kullanılabilir veya [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ile bir [Azure destek planı koşullu](https://azure.microsoft.com/support/plans/).

## <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler şunları yapabilir [bir sorun açın](/azure/azure-supportability/how-to-create-azure-support-request) oluşturulduysa Azure desteği aracılığıyla Azul Zulu JDK ile bir [tam destek planı](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Bu konu, Windows üzerinde desteği Azure App Service için Java Çalışma zamanı deyiminin sağlar.
- Bkz. Azure App Service ile web uygulamalarını barındırma hakkında daha fazla bilgi edinmek için [App Service'e genel bakış](overview.md).
- Azure geliştirme Java hakkında bilgi için bkz. [Azure Java Geliştirici Merkezi için](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
