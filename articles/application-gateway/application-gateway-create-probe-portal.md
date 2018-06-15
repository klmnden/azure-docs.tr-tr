---
title: Azure uygulama ağ geçidi - Azure portalı özel bir araştırma - oluşturma | Microsoft Docs
description: Portalı kullanarak uygulama ağ geçidi için özel bir araştırma oluşturmayı öğrenin
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
ms.openlocfilehash: 45737c1c378ec56a5e2bedec8c1f7b7bc7ba6225
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33203927"
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Portalı kullanarak uygulama ağ geçidi için özel bir araştırma oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, Azure portalı üzerinden var olan bir uygulama ağ geçidi için özel bir araştırma ekleyin. Özel araştırmalara başarılı yanıt varsayılan web uygulaması üzerinde sağlamaz, uygulamaları veya belirli bir sağlık denetimi sayfası uygulamaları için kullanışlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi zaten yoksa, ziyaret [bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-portal.md) çalışmak için bir uygulama ağ geçidi oluşturmak için.

## <a name="createprobe"></a>Araştırma oluşturma

Araştırmalar iki adımlı bir işlem portal üzerinden yapılandırılır. İlk adım, araştırma oluşturmaktır. İkinci adımda, uygulama ağ geçidi arka uç http ayarları için araştırma ekleyin.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz bir aylık deneme sürümü](https://azure.microsoft.com/free)

1. Azure portal Sık Kullanılanlar bölmesi tüm kaynaklar'ı tıklatın. Uygulama ağ geçidi tüm kaynaklar dikey penceresinde'ı tıklatın. Zaten seçili abonelik çeşitli kaynaklar varsa, adına göre filtre partners.contoso.net girebilirsiniz... girebilirsiniz.

1. Tıklatın **yoklamaları** tıklatıp **Ekle** bir araştırma eklemek için düğmesi.

  ![Doldurulan bilgilerle Ekle araştırma dikey penceresi][1]

1. Üzerinde **Ekle durumu araştırması** dikey penceresinde, araştırma için gereken bilgileri doldurun ve tıklatın tamamlandığında **Tamam**.

  |**Ayar** | **Değer** | **Ayrıntılar**|
  |---|---|---|
  |**Ad**|customProbe|Bu değer, Portalı'nda erişilebilir araştırma için kolay bir addır.|
  |**Protokol**|HTTP veya HTTPS | Sistem durumu araştırması kullanan protokol.|
  |**Ana Bilgisayar**|yani contoso.com|Bu değer araştırması için kullanılan ana bilgisayar adıdır. Geçerli yalnızca çok siteli uygulama ağ geçidi üzerinde yapılandırılmış, aksi takdirde '127.0.0.1' kullanın. Bu değer, VM ana bilgisayar adından farklıdır.|
  |**Path**|/ veya başka bir yolu|Özel araştırma için tam bir url kalanı. İle başlayan geçerli bir yol '/'. Varsayılan yolu http://contoso.com yalnızca '/' |
  |**Aralığı (saniye)**|30|Ne sıklıkta araştırma için sistem durumu denetlemek için çalıştırılır. Alt ayarlamak için önerilmez 30 saniyeden.|
  |**Zaman aşımı (sn)**|30|Yoklama zaman aşımına uğramadan önce beklediği süre miktarı. Zaman aşımı aralığı bir http çağrısıyla arka uç sağlık durumu sayfasında kullanılabilir olduğundan emin olmak için yapılabilmesi için yeterince yüksek olması gerekir.|
  |**Sağlıksız durum eşiği.**|3|Sağlıksız olarak kabul edilmesi için başarısız girişim sayısı. Arka uç sistem durumu denetimi başarısız olursa, sağlıksız hemen belirlenir bir eşik 0 anlamına gelir.|

  > [!IMPORTANT]
  > Ana bilgisayar adını sunucu adı ile aynı değil. Bu değer uygulama sunucusunda çalışan sanal ana bilgisayar adıdır. Araştırma http://(host name):(port from httpsetting)/urlPath için gönderilir

## <a name="add-probe-to-the-gateway"></a>Araştırması için ağ geçidi Ekle

Araştırma oluşturulduktan sonra ağ geçidi eklemek için zaman yapılır. Araştırma ayarları uygulama ağ geçidi arka uç http ayarları ayarlanır.

1. Tıklatın **HTTP ayarları** uygulama ağ geçidinde, yapılandırma dikey penceresinde geçerli arka uç http ayarları getirmek için listelenen penceresinde.

  ![HTTPS Ayarları penceresi][2]

1. Üzerinde **appGatewayBackEndHttpSettings** dikey penceresinde, onay **kullanım özel araştırma** onay kutusunu ve oluşturduğunuz araştırmayı seçin [araştırma oluşturmak](#createprobe) bölümünde **özel araştırma** açılan...
Tamamlandığında, tıklayın **kaydetmek** ve ayarları uygulanır.

Varsayılan araştırmasını web uygulamasına varsayılan erişimi denetler. Özel bir araştırma oluşturulduktan sonra uygulama ağ geçidi arka uç sunucuları için sistem durumu izlemek için tanımlanan özel yolu kullanır. Tanımlandı ölçütleri temel alarak, uygulama ağ geçidi araştırması için belirtilen yol denetler. Ana bilgisayar: bağlantı noktası çağrısı / yolu bir HTTP 200 399 durumu yanıt döndürmez, sağlıksız eşiğine ulaşıldıktan sonra sunucu dışında döndürme alınır. Yoklama yeniden sağlıklı duruma geldiğinde belirlemek için sağlıksız örneğinde devam eder. Örnek geri sağlıklı sunucu havuzuna eklendikten sonra ona yeniden akan trafik başlar ve örneğine yoklama kullanıcı belirtilen aralıkta normal olarak devam eder.

## <a name="next-steps"></a>Sonraki adımlar

SSL boşaltma Azure uygulama ağ geçidi ile yapılandırma hakkında bilgi edinmek için [SSL boşaltma yapılandırın](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

