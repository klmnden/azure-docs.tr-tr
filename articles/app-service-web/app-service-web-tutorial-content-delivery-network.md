---
title: "Web Uygulamasını İçerik Teslim Ağına bağlama | Microsoft Docs"
description: "Kenar düğümlerinden statik dosyalarınızı teslim etmek için bir Web Uygulamasını İçerik Teslim Ağına bağlayın."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: a12eab1f7bc4177f659771d58a58f749507c994c
ms.lasthandoff: 04/12/2017

---
# <a name="connect-a-web-app-to-a-content-delivery-network"></a>Web Uygulamasını İçerik Teslim Ağına bağlama

Bu öğreticide, statik dosyalarınızı Web Uygulamanızdan Azure CDN pop konumları üzerinden sunmak üzere bir Azure CDN Profili ve Azure CDN Uç Noktası oluşturma hakkında bilgi alacaksınız.

> [!TIP]
> [Azure CDN pop konumlarının](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations) güncel listesini gözden geçirin.
>

## <a name="step-1---login-to-azure-portal"></a>Adım 1 - Azure Portalında oturum açın

İlk olarak, sık kullandığınız tarayıcıyı açın ve Azure [Portalı](https://portal.azure.com)’na göz atın.

## <a name="step-2---create-a-cdn-profile"></a>Adım 2 - CDN Profili oluşturun

Sol gezinti bölmesindeki `+ New` düğmesine ve **Web + Mobil**’e tıklayın. Web + Mobil kategorisi altında **CDN**’yi seçin.

**Ad**, **Konum**, **Kaynak grubu**, **Fiyatlandırma katmanı** belirtip, **Oluştur**’a tıklayın.

Sol gezinti bölmesinden kaynak grupları merkezini açıp, **myResourceGroup**’u seçin. Kaynak listesinden **myCDNProfile** öğesini seçin.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Adım 3 - CDN Uç Noktası oluşturun

Arama kutusunun yanındaki komutlardan `+ Endpoint` öğesine tıkladığınızda Uç Nokta oluşturma dikey penceresi başlatılır.

**Ad**, **Kaynak türü**, **Kaynak ana bilgisayar adı** belirtip **Ekle**’ye tıklayın.

Uç Nokta oluşturulur ve İçerik Teslim Ağı uç noktası oluşturulduktan sonra durum **çalışıyor** olarak güncelleştirilir.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---leveraging-cdn"></a>Adım 4 - CDN’den yararlanın

Uç nokta çalıştığına göre, web sunucusundaki bir statik dosyaya göz atıp, `http://<app_name>.azurewebsites.net/path/to-static-file` olan ana bilgisayar adını `http://<endpoint_name>.azureedge.net/path/to-static-file` yaparak içerik pop sunucusunda mevcut olup olmadığını doğrulayabiliriz.

![app-service-web-url-to-cdn-endpoint-url](media/app-service-web-tutorial-content-delivery-network/app-service-web-url-to-cdn-endpoint-url.png)

## <a name="next-steps"></a>Sonraki Adımlar


