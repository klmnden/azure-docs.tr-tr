---
title: Azure Application Gateway - Azure portalı - özel bir araştırma oluşturma | Microsoft Docs
description: Portalı kullanarak Application Gateway için özel bir araştırma oluşturmayı öğrenin
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: 90d576fd00a39f7e871cbe0922ce131dfbe38ff0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122396"
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Portalı kullanarak Application Gateway için özel bir araştırma oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, Azure Portalı aracılığıyla mevcut bir uygulama ağ geçidi için özel bir araştırma ekleyin. Özel araştırmalar, belirli bir sistem durumu denetimi sayfası olan uygulamalar için veya başarılı bir yanıt varsayılan web uygulaması üzerinde sağlamayan uygulamalar için yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi zaten yoksa, ziyaret [bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-portal.md) çalışmak için bir uygulama ağ geçidi oluşturmak için.

## <a name="createprobe"></a>Araştırma oluşturma

Araştırmaları iki adımlı bir işlem portal üzerinden yapılandırılır. İlk adım, araştırma oluşturmaktır. İkinci adımda, uygulama ağ geçidinin arka uç http ayarları için araştırma ekleyin.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz bir aylık deneme sürümü](https://azure.microsoft.com/free)

1. Azure portalı Sık Kullanılanlar bölmesinde, tüm kaynaklar'ı tıklatın. Tüm kaynaklar dikey penceresinde Uygulama ağ geçidine tıklayın. Zaten seçili abonelikte çeşitli kaynaklar varsa, partners.contoso.net adına göre filtre girebilirsiniz... girebilirsiniz.

1. Tıklayın **araştırmaları** tıklatıp **Ekle** bir araştırma eklemek için düğme.

   ![Bilgilerle doldurulan araştırma dikey penceresi ekleme][1]

1. Üzerinde **durum araştırması Ekle** dikey penceresinde araştırma için gerekli bilgileri doldurun ve tıklatın tamamlandığında **Tamam**.

   |**Ayar** | **Değer** | **Ayrıntılar**|
   |---|---|---|
   |**Ad**|customProbe|Bu değer, portalda erişilebilir araştırması için kolay bir addır.|
   |**Protokolü**|HTTP veya HTTPS | Durum araştırması kullanan protokol.|
   |**Ana Bilgisayar**|yani contoso.com|Araştırma için kullanılan ana bilgisayar adı değerdir. Geçerli çok siteli, yalnızca uygulama ağ geçidinde yapılandırılan, aksi takdirde '127.0.0.1' kullanın. Bu değer, VM'nin ana bilgisayar adından farklıdır.|
   |**Yolu**|/ veya başka bir yol|Özel araştırma için tam bir url geri kalanında. İle başlayan geçerli bir yol '/'. Http varsayılan yolu:\//contoso.com kullanmanız yeterlidir '/' |
   |**Aralığı (saniye)**|30|Ne sıklıkta denetlemek için sistem durumu için yoklama çalıştırın. Alt ayarlamak için önerilmez 30 saniyeden.|
   |**Zaman aşımı (saniye)**|30|Yoklama zaman aşımına uğrama süre miktarı. Zaman aşımı aralığı, arka uç sistem durumu sayfası kullanılabildiğinden emin olmak için http çağrısı yapılabilir yeterince yüksek olması gerekiyor.|
   |**Sağlıksız durum eşiği**|3|Sağlıksız olarak değerlendirilmesi için başarısız girişim sayısı. Arka uç sistem durumu denetimi başarısız olursa, sağlıksız hemen belirlenir bir eşik 0 anlamına gelir.|

   > [!IMPORTANT]
   > Ana bilgisayar adı sunucu adıyla aynı değil. Bu değer uygulama sunucusunda çalışan sanal ana bilgisayar adıdır. Araştırma http://(host name):(port from httpsetting)/urlPath için gönderilir.

## <a name="add-probe-to-the-gateway"></a>Ağ geçidini araştırma ekleme

Araştırma oluşturuldu, bu ağ geçidini ekleme zamanı geldi. Araştırma ayarları uygulama ağ geçidinin arka uç http ayarları üzerinde ayarlanır.

1. Tıklayın **HTTP ayarları** uygulama ağ geçidinde, yapılandırma dikey penceresinde ardından geçerli arka uç http ayarları getirmek için listelenen penceresinde.

   ![HTTPS Ayarları penceresi][2]

1. Üzerinde **appGatewayBackEndHttpSettings** ayarlar dikey penceresi, onay **özel araştırma kullan** onay kutusu içinde oluşturduğunuz araştırmayı seçin [araştırmayı oluşturmak](#createprobe) bölümü **Özel araştırma** açılan...
   Tamamlandığında, tıklayın **Kaydet** ve ayarları uygulanır.

Varsayılan araştırmasını varsayılan web uygulamasına erişimi denetler. Özel bir araştırma oluşturuldu, application gateway arka uç sunucularının durumunu izlemek için tanımlanan özel bir yol kullanır. Tanımlanan ölçütlere göre uygulama ağ geçidi araştırması için belirtilen yol denetler. Konak: bağlantı noktası çağrısı / yolu bir HTTP 200-399 durum yanıt döndürmez, sağlıksız durum eşiği aşıldığında, sunucunun döndürme dışına alınır. Yoklama yeniden sağlıklı hale geldiğinde belirlemek için sağlıksız örneğinde devam eder. Örnek sağlıklı sunucu havuzuna eklendikten sonra kendisine yeniden akan trafiği başlar ve örneğine yoklama kullanıcı belirtilen aralıkta normal olarak devam eder.

## <a name="next-steps"></a>Sonraki adımlar

SSL boşaltma Azure Application Gateway ile yapılandırma konusunda bilgi için bkz: [SSL yük boşaltma yapılandırın](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

