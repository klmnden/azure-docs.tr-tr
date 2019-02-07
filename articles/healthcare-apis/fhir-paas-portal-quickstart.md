---
title: Azure API için Azure portalını kullanarak FHIR dağıtma
description: Azure API için Azure portalını kullanarak FHIR dağıtın.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 8699abfa8cc9b6675c15c8cd9b7cbb5bb5e148cd
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823548"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-portal"></a>Hızlı Başlangıç: Azure API için Azure portalını kullanarak FHIR dağıtma

Bu hızlı başlangıçta, Azure portalını kullanarak FHIR için Azure API dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-new-resource"></a>Yeni kaynak oluştur

Açık [Azure Önizleme portalı](https://preview.portal.azure.com) tıklatıp **kaynak oluştur**

![Kaynak oluşturma](media/quickstart-paas-portal/portal-create-resource.png)

## <a name="search-for-azure-api-for-fhir"></a>Azure API FHIR Ara

Arama kutusuna "FHIR" yazarak FHIR için Azure API bulabilirsiniz:

![Sağlık Hizmeti API'leri arayın](media/quickstart-paas-portal/portal-search-healthcare-apis.png)

## <a name="create-azure-api-for-fhir-account"></a>Azure API FHIR hesabı oluşturun

Seçin **Oluştur** FHIR hesabı için yeni bir Azure API oluşturmak için:

![Azure API FHIR hesabı oluşturun](media/quickstart-paas-portal/portal-create-healthcare-apis.png)

## <a name="enter-account-details"></a>Hesabı ayrıntılarını girin

Mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun, hesap için bir ad seçin ve son olarak tıklayın **gözden geçir + Oluştur**:

![Yeni sağlık API ayrıntıları](media/quickstart-paas-portal/portal-new-healthcareapi-details.png)

Oluşturma işlemini onaylamak ve FHIR API dağıtım bekler.

## <a name="additional-settings"></a>Ek ayarlar

Tıklayın **sonraki: Ek ayarlar** kimlik nesne kimliklerini FHIR için bu Azure API'sine erişim izni yapılandırmak için:

![İzin verilen nesne kimlikleri yapılandırın](media/quickstart-paas-portal/configure-allowed-oids.png)

Bkz: [kimlik nesne kimliklerini bulmak nasıl](find-identity-object-ids.md) kimlik bulun hakkında ayrıntılı bilgi için kullanıcı kimlikleri nesne ve hizmet sorumluları için.

## <a name="fetch-fhir-api-capability-statement"></a>FHIR API yetenek ifadesi getirilemedi

Yeni FHIR API hesabı sağlanmasını doğrulamak için tarayıcıda işaret ederek bir yetenek ifadesi fetch `https://<ACCOUNT-NAME>.azurehealthcareapis.com/metadata`.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Azure API FHIR için ve tüm ilgili kaynakları silin. Bunu yapmak için select FHIR hesabı için Azure API içeren kaynak grubunu seçin. **kaynak grubunu Sil**, ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure API FHIR için aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.

>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)