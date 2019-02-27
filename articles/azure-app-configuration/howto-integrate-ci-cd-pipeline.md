---
title: Azure Uygulama Yapılandırması'nı kullanarak sürekli tümleştirme ve teslim işlem hattı ile tümleştirin. | Microsoft Docs
description: Azure uygulama yapılandırmasında, sürekli tümleştirme ve teslim sırasında verileri kullanarak bir yapılandırma dosyası oluşturmayı öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 0e6b24175ffa28b2a0f361bc46fd6c41e09cae72
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885441"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Bir CI/CD işlem hattı ile tümleştirin

Uygulamanızı Azure uygulama yapılandırması ile erişmek boyutlandırılmamışsa uzak olasılığını karşı dayanıklılığı artırmak için geçerli yapılandırma verilerini, başlatma sırasında dağıtılan uygulamayla ve yerel olarak yüklenen bir dosyaya paketi . Bu yaklaşım, uygulamanızın varsayılan ayar değerleri en az olacağını garanti eder. Kullanılabilir olduğunda bu değerleri bir uygulama yapılandırma deposu yeni değişiklikler yazılır.

## <a name="automate-configuration-data-retrieval"></a>Yapılandırma veri alma otomatikleştirin

Kullanarak [ **dışarı** ](./howto-import-export-data.md#export-data) işlevi Azure uygulama yapılandırmasına, geçerli yapılandırma verilerini tek bir dosya olarak alma işlemini otomatik hale getirebilirsiniz. Ardından, sürekli tümleştirme ve sürekli dağıtım işlem hattı bir derleme veya dağıtım adımda bu dosya dahil edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: ASP.NET web uygulaması oluşturma](quickstart-aspnet-core-app.md)  
