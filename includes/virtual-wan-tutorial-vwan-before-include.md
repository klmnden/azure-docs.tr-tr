---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 09f642a4c96781276d225cba37d71386d429d0c9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004184"
---
Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız varsa şirket içi ağınızdaki alt ağların bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Sanal ağınız için ağ geçidi alt ağına ihtiyaç yoktur ve sanal ağınızda sanal ağ geçidi bulunamaz. Sanal ağınız yoksa bu makaledeki adımları kullanarak oluşturabilirsiniz.
* Hub bölgenizden bir IP adresi aralığı edinin. Hub bir sanal ağdır ve hub bölgesi için belirttiğiniz adres aralığı, bağlandığınız var olan sanal ağlarla çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.