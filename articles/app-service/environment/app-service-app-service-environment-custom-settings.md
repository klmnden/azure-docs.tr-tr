---
title: App Service ortamları - Azure için özel ayarlar
description: App Service ortamları için özel yapılandırma ayarları
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 6463759dbd217cd054f838c09c7cfcf99a06aa2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765083"
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>App Service ortamları için özel yapılandırma ayarları
## <a name="overview"></a>Genel Bakış
App Service ortamları (ase) tek bir müşteriye ayrılmış olduğu için yalnızca App Service ortamları için uygulanabilecek bazı yapılandırma ayarları vardır. Bu makale, App Service ortamları için kullanılabilen çeşitli belirli özelleştirmeleri içermektedir.

App Service ortamı yoksa bkz [bir App Service ortamı oluşturma](app-service-web-how-to-create-an-app-service-environment.md).

Yeni bir dizi kullanarak App Service ortamı özelleştirmeleri depolayabilirsiniz **clusterSettings** özniteliği. Bu öznitelik "Özellikler" sözlükte bulunan *hostingEnvironments* Azure Resource Manager varlığı.

Aşağıdaki Resource Manager şablonu kod parçacığında gösterildiği kısaltılmış **clusterSettings** özniteliği:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

**ClusterSettings** özniteliği App Service ortamı güncelleştirmek için bir Resource Manager şablonunda dahil edilebilir.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>App Service ortamı güncelleştirmek için Azure kaynak Gezgini'ni kullanma
Alternatif olarak, App Service ortamı güncelleştirebilirsiniz [Azure kaynak Gezgini](https://resources.azure.com).  

1. Kaynak Gezgini'nde, App Service ortamı için düğüme gidin (**abonelikleri** > **resourceGroups** > **sağlayıcıları**  >  **Microsoft.Web** > **hostingEnvironments**). Güncelleştirmek istediğiniz belirli App Service Ortamı'ye tıklayın.
2. Sağ bölmede **okuma/yazma** kaynak Gezgini'nde düzenleme etkileşimli izin vermek için üst araç çubuğunda.  
3. Mavi tıklayın **Düzenle** düğmesi Resource Manager şablonu düzenlenebilir hale getirin.
4. Sağ bölmede alt kısmına kaydırın. **ClusterSettings** burada girin veya değerini güncelleştirme çok altındaki bir özniteliktir.
5. Yazın (veya kopyalayıp yapıştırın) istediğiniz yapılandırma değerleri dizisi **clusterSettings** özniteliği.  
6. Yeşil tıklayın **PUT** , düğme için App Service ortamı değişikliği kaydetmek için sağ bölmenin üstünde bulunan.

Değişiklik gönderdiğiniz ancak değişikliğin etkili olması için App Service Ortamı'nda ön uçlar sayısıyla çarpımı yaklaşık 30 dakika sürer.
Örneğin, bir App Service ortamı dört ön uçlar varsa, tamamlanması yapılandırma güncelleştirmesi için yaklaşık iki saat sürer. Yapılandırma değişikliği kullanıma sunulacaktır, ancak başka bir ölçeklendirme işlemleri veya yapılandırma değişiklik işlemleri App Service Ortamı'nda yer alabilir.

## <a name="disable-tls-10-and-tls-11"></a>TLS 1.0 ve TLS 1.1 devre dışı bırak

Bir uygulama tarafından temelinde TLS ayarları yönetmek istediğiniz sonra ile sağlanan yönergeleri kullanarak [zorunlu TLS ayarları](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-versions) belgeleri. 

Tüm bir ase'deki uygulamalar için tüm gelen TLS 1.0 ve TLS 1.1 trafiği devre dışı bırakmak isterseniz, aşağıdakileri ayarlayabilirsiniz **clusterSettings** girişi:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

1.0 ayarının adı söylüyor ancak yapılandırıldığında, TLS 1.0 ve TLS 1.1 bırakır.

## <a name="change-tls-cipher-suite-order"></a>TLS şifre paketi sırasını değiştir
Bunlar, sunucu tarafından anlaşılan şifrelemeleri listesini değiştirebilirsiniz ve bu değiştirerek gerçekleştirilebilir müşterilerden başka bir soru ise **clusterSettings** aşağıda gösterildiği gibi. Şifre paketleri kullanılabilir listesini alınabilir [bu MSDN makalesinde](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> SChannel anlayamıyor şifre paketi için yanlış değerleri ayarlarsanız, tüm TLS iletişim sunucunuza çalışmamaya başlayabilir. Böyle bir durumda kaldırmanız gerekecek *FrontEndSSLCipherSuiteOrder* girdisinden **clusterSettings** ve varsayılan şifre paketi geri dönmek için güncelleştirilmiş Resource Manager şablonu gönderin Ayarlar.  Lütfen bu işlevleri dikkatli kullanın.
> 
> 

## <a name="get-started"></a>başlarken
Azure hızlı başlangıç Resource Manager şablonu sitenin temel tanımını şablonla içerir [bir App Service ortamı oluşturma](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
