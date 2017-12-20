---
title: "Azure uygulama ağ geçidi - Azure portalı SSL boşaltma - yapılandırma | Microsoft Docs"
description: "Bu makale Azure portalını kullanarak SSL ile bir uygulama ağ geçidi oluşturma yönergelerini boşaltma sağlar"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 2f7f5d4132e28c8c192d90d5f4bfb2a9034f8b8c
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-azure-portal"></a>Azure portalını kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoyu SSL boşaltma üzerinde var olan bir uygulama ağ geçidi yapılandırma aracılığıyla alır. Senaryo adımları zaten izlediğinizden varsayar [bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi ile SSL yük boşaltmayı yapılandırmak için bir sertifika gereklidir. Bu sertifika, uygulama ağ geçidinde yüklenir ve şifrelemek ve SSL gönderilen trafiğin şifresini çözmek için kullanılır. Sertifika kişisel bilgi değişimi (.pfx) biçiminde olması gerekir. Bu dosya biçimi, uygulama ağ geçidi tarafından şifreleme ve şifre çözme trafik gerçekleştirmek için gereken özel anahtarı dışarı olanak sağlar.

## <a name="add-an-https-listener"></a>Bir HTTPS dinleyicisi ekleme

Kendi yapılandırmasına bağlı olarak trafik HTTPS dinleyicisi arar ve yardımcı olur, arka uç havuzları trafiği yönlendirmek. Bir HTTPS dinleyicisi eklemek için aşağıdaki adımları izleyin:

   1. Azure Portalı'na gidin ve var olan bir uygulama ağ geçidi seçin.

   2. Seçin **dinleyicileri**ve ardından **Ekle** düğmesi dinleyici ekleyin.

   ![Uygulama ağ geçidi'ne genel bakış bölmesi][1]


   3. Dinleyici için aşağıdaki gerekli bilgileri doldurun ve .pfx sertifikasını karşıya yükle:
      - **Ad**: dinleyici kolay adı.

      - **Ön uç IP yapılandırmasını**: dinleyici için kullanılan ön uç IP yapılandırması.

      - **Ön uç bağlantı noktası (ad/bağlantı noktası)**: uygulama ağ geçidi ve kullanılan gerçek bağlantı noktası ön ucunda kullanılan bağlantı noktası için bir kolay ad.

      - **Protokol**: HTTPS veya HTTP ön uç için kullanılıp kullanılmadığını belirlemek için bir anahtar.

      - **Sertifika (ad/parola)**: varsa SSL boşaltma kullanılır, bu ayar için bir .pfx sertifika gereklidir. Bir kolay ad ve parola de gereklidir.

   4. **Tamam**’ı seçin.

![Dinleyici bölmesi ekleme][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Bir kural oluşturmak ve dinleyiciye ilişkilendirme

Dinleyici şimdi oluşturuldu. Ardından, dinleyiciyi trafiği işlemek için bir kural oluşturun. Kuralları trafiği birden çok yapılandırma ayarlarınızı temel alan arka uç havuzları nasıl yönlendirildiğini tanımlayın. Bu ayarlar, protokol, bağlantı noktası ve sistem durumu araştırmalarının içerir ve oturum tanımlama bilgisi temelli benzeşimi kullanım olup. Oluşturmak ve bir kural dinleyicisi ilişkilendirmek için aşağıdaki adımları izleyin:


   1. Seçin **kuralları** uygulama ağ geçidi ve ardından **Ekle**.

   ![Uygulama ağ geçidi kurallar bölmesi][3]


   2. Altında **Ekle temel kural**, kural için bir kolay ad girin **adı** alan ve ardından **dinleyicisi** önceki adımda oluşturduğunuz. Uygun seçin **arka uç havuzu** ve **HTTP ayarı**ve ardından **Tamam**.

   ![HTTPS Ayarları penceresi][4]

Ayarları artık uygulama ağ geçidi için kaydedilir. Kaydetme işlemi portal veya PowerShell aracılığıyla görüntülemek kullanılabilir önce bu ayarları biraz zaman alabilir. Kaydedildikten sonra uygulama ağ geçidi şifreleme ve şifre çözme trafiği işler. Uygulama ağ geçidi ve arka uç web sunucuları arasındaki tüm trafik HTTP üzerinden ele alınacaktır. İstemciye geri, tüm iletişim HTTPS üzerinden başlattıysanız şifrelenmiş istemciye döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

Azure uygulama ağ geçidi ile bir özel durum araştırması yapılandırma konusunda bilgi edinmek için [bir özel durum araştırması oluşturmak](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png

