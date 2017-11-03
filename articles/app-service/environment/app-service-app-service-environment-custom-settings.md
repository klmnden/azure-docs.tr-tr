---
title: "Uygulama hizmeti ortamları için özel ayarları"
description: "Uygulama hizmeti ortamları için özel yapılandırma ayarları"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 687475fae0c90713c15e8abbb92b71059eae81c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Uygulama hizmeti ortamları için özel yapılandırma ayarları
## <a name="overview"></a>Genel Bakış
Uygulama hizmeti ortamları tek bir müşteri için yalıtılmış olduğundan, özel olarak App Service ortamları için uygulanabilecek bazı yapılandırma ayarları vardır. Bu makale App Service ortamları için kullanılabilen çeşitli belirli özelleştirmelere içermektedir.

Bir uygulama hizmeti ortamı yoksa bkz [bir uygulama hizmeti ortamı oluşturmak nasıl](app-service-web-how-to-create-an-app-service-environment.md).

Bir dizi yeni kullanarak uygulama hizmeti ortamı özelleştirmeleri depolayabilirsiniz **clusterSettings** özniteliği. Bu öznitelik "Özellikler" sözlükte bulunan *hostingEnvironments* Azure Resource Manager varlığı.

Resource Manager şablonu kod parçacığında gösterildiği aşağıdaki kısaltılmış **clusterSettings** özniteliği:

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

**ClusterSettings** özniteliği uygulama hizmeti ortamı güncelleştirmek için bir Resource Manager şablonu dahil edilebilir.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Bir uygulama hizmeti ortamı güncelleştirmek için Azure kaynak Gezgini'ni kullanma
Alternatif olarak, uygulama hizmeti ortamı kullanarak güncelleştirebileceğinizi [Azure kaynak Gezgini](https://resources.azure.com).  

1. Kaynak Gezgini, uygulama hizmeti ortamı için düğümüne gidin (**abonelikleri** > **resourceGroups** > **sağlayıcıları** > **Microsoft.Web** > **hostingEnvironments**). Güncelleştirmek istediğiniz belirli uygulama hizmeti ortamı'ye tıklayın.
2. Sağ bölmede **okuma/yazma** kaynak Gezgini'nde düzenleme etkileşimli izin vermek için üst araç çubuğunda.  
3. Mavi tıklatın **Düzenle** Resource Manager şablonu düzenlenebilir olmasını sağlamak için düğmesi.
4. Sağ bölmede altına gidin. **ClusterSettings** buradan girin veya değerini güncelleştirme çok altındaki bir özniteliktir.
5. Yazın (veya kopyalayıp yapıştırın), istediğiniz yapılandırma değerleri dizisi **clusterSettings** özniteliği.  
6. Yeşil tıklatın **PUT** , düğme için uygulama hizmeti ortamı değişikliği kaydetmek için sağ bölmedeki üstünde bulunan.

Değişiklik Gönder ancak değişikliğin etkili olması için uygulama hizmeti ortamında ön uçlar sayısı ile çarpılır yaklaşık 30 dakika sürer.
Örneğin, bir uygulama hizmeti ortamı dört ön uçlar varsa, kabaca tamamlamak yapılandırma güncelleştirmesi için iki saat sürer. Yapılandırma değişikliği alınıyor sırada başka bir ölçeklendirme işlemleri veya yapılandırma değişiklik işlemleri uygulama hizmeti ortamı yerinde alabilir.

## <a name="disable-tls-10"></a>TLS 1.0 devre dışı bırak
Yinelenen bir soru müşterilerden özellikle PCI uyumluluğunu ile ilgilenen müşteriler denetimleri, TLS 1.0 için uygulamalarını açıkça devre dışı bırakma.

TLS 1.0 devre dışı bırakılabilir aşağıdaki aracılığıyla **clusterSettings** girişi:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Değişiklik TLS şifre paketi sırası
Başka bir soru müşterilerden kendi sunucu tarafından anlaşılan şifre listesi değiştirebilirsiniz ve bu değiştirerek elde olan **clusterSettings** aşağıda gösterildiği gibi. Şifre paketleri kullanılabilir Listesi penceresinden alınabilir [bu MSDN makalesine](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> SChannel anlayamıyor şifre paketi için yanlış değerleri ayarlarsanız sunucunuza tüm TLS iletişim çalışmasını durdurabilir. Böyle bir durumda kaldırmanız gerekecek *FrontEndSSLCipherSuiteOrder* girdisinden **clusterSettings** ve varsayılan şifre paketi ayarlarına geri dönmek için güncelleştirilmiş Resource Manager şablonu gönderin.  Lütfen bu işlevi dikkatli kullanın.
> 
> 

## <a name="get-started"></a>başlarken
Bir şablonu temel tanımıyla Azure hızlı başlangıç Resource Manager şablonu sitesi içeren [bir uygulama hizmeti ortamı oluşturma](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
