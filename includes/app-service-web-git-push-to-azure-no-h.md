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
ms.openlocfilehash: aa6f18d4f667862687083c5db3679ce9d8e188cd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188853"
---
_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. _&lt;deploymentLocalGitUrl-from-create-step>_ değerini [Web uygulaması oluşturma](#create-a-web-app) bölümünde kaydettiğiniz Git uzak URL’si ile değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgisi Yöneticisi kimlik bilgileri istendiğinde, Azure portalında oturum açmak için kullandığınız kimlik bilgilerini değil bir dağıtım kullanıcısı Yapılandır oluşturduğunuz kimlik bilgilerini girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
