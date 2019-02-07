---
title: Azure Portalı - Microsoft sağlık API'lerini kullanarak Azure için açık kaynak FHIR sunucusu dağıtma
description: Bu hızlı başlangıçta, Azure portalını kullanarak Microsoft açık kaynak FHIR sunucusu dağıtma işlemi açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 2bd7a7d0e332d281a0ca9a9017219b50a3fbb472
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823537"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak açık kaynak FHIR sunucusu dağıtma

Bu hızlı başlangıçta, Azure portalını kullanarak azure'da bir açık kaynak FHIR sunucusu dağıtmayı öğreneceksiniz. Kolay dağıtım bağlantıları kullanacağız [açık kaynak depo](https://github.com/Microsoft/fhir-server)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="github-open-source-repository"></a>Açık kaynak GitHub deposu

Gidin [GitHub dağıtım sayfası](https://github.com/Microsoft/fhir-server/blob/master/docs/DefaultDeployment.md) "Azure'a dağıtın" düğmeleri bulun:

![Açık kaynak dağıtım sayfası](media/quickstart-oss-portal/deployment-page-oss.png)

Dağıtım düğmesine tıklayın ve Azure portalı açılır.

## <a name="fill-in-deployment-parameters"></a>Dağıtım parametrelerini doldurun

Yeni bir kaynak grubu oluşturun ve buna bir ad vermek seçin. Diğer gerekli parametresi yalnızca bir hizmet adıdır.

![Özel dağıtım parametreleri](media/quickstart-oss-portal/deployment-custom-parameters.png)

Dağıtım kaynak kodunu doğrudan GitHub üzerinde açık kaynak depodan çeker dikkat edin. Deponun çatalını, sizin için kendi işaret edebilir ve belirli bir dala.

Ayrıntılar doldurduktan sonra dağıtım başlayabilirsiniz.

## <a name="validate-fhir-server-is-running"></a>Doğrulama FHIR çalıştırıyorsa

Dağıtım tamamlandıktan sonra tarayıcınızı gösterebilir `https://SERVICENAME.azurewebsites.net/metadata` bir yetenek ifadesi elde edilir. Veya sunucunun ilk kez yanıt vermesi için bunu bir dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve tüm ilgili kaynakları silin. Bunu yapmak için sağlanan kaynakları içeren kaynak grubunu seçin, seçin **kaynak grubunu Sil**, ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHIR sunucu aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)