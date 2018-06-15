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
ms.openlocfilehash: 125561d61afe0fb7f704144efa1c8c20ecf03db1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29821054"
---
_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. _&lt;deploymentLocalGitUrl-from-create-step>_ değerini [Web uygulaması oluşturma](#create) bölümünde kaydettiğiniz Git uzak URL’si ile değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git Kimlik Bilgisi Yöneticisi kimlik bilgilerini sorduğunda, azure portalında oturum açmak için kullandığınız kimlik bilgilerini değil, [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) bölümünde oluşturduğunuz kimlik bilgilerini girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
