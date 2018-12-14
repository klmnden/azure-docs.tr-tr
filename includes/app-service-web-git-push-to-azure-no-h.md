---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 026540290710d039dbc06c394ab538ebe2d7c12f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344678"
---
_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. _&lt;deploymentLocalGitUrl-from-create-step>_ değerini [Web uygulaması oluşturma](#create-a-web-app) bölümünde kaydettiğiniz Git uzak URL’si ile değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git Kimlik Bilgisi Yöneticisi kimlik bilgilerini sorduğunda, azure portalında oturum açmak için kullandığınız kimlik bilgilerini değil, [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) bölümünde oluşturduğunuz kimlik bilgilerini girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
