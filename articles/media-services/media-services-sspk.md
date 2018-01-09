---
title: "Kit taşıma lisanslama Microsoft® kesintisiz akış istemcisi"
description: "Hakkında bilgi almak için Microsoft® kesintisiz akış istemci bağlantı noktası oluşturma Seti lisans."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 87a5a1981b05722f25a70fcb73a06db65bcbe0fd
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Kit taşıma lisanslama Microsoft® kesintisiz akış istemcisi
## <a name="overview"></a>Genel Bakış
Microsoft kesintisiz akış istemci bağlantı noktası oluşturma Seti (**SSPK** kısaca) katıştırılmış cihaz üreticisinin, kablo ve mobil işleçleri, içerik hizmet sağlayıcıları, ahize yardımcı olmak için iyileştirilmiş kesintisiz akış bir istemci uygulaması Üreticiler, bağımsız yazılım satıcılarının (ISV'ler) ve çözüm sağlayıcıları ürünleri ve Hizmetleri kesintisiz akış biçiminde Uyarlamalı içeriği akışla oluşturmak için. Bir cihaz ve cihaz ve platform edinmediyseniz tarafından bağlantı noktası kurulmuş kesintisiz akış istemci platformdan bağımsız uygulaması SSPK olur. 

Aşağıda bir üst düzey mimari ve IIS kesintisiz akış bağlantı noktası oluşturma Seti kutusunu Microsoft tarafından sağlanan Smooth Streaming Client uygulamasıdır ve kesintisiz akış içeriği kayıttan yürütmeyi tüm çekirdek mantığını içerir. Bu içerik daha sonra belirli bir aygıt veya platformu için iş ortakları tarafından uygun arabirimler uygulama tarafından verilen. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Açıklama
SSPK mükemmel iş değerini sunan koşullarınızda lisanslıdır. SSPK lisans ile endüstri sağlar:

* C++'ta kesintisiz akış bağlantı noktası oluşturma Seti kaynak 
  * Smooth Streaming Client işlevselliğini hayata Geçiren
  * ayrıştırma, arabelleğe alma mantığı, vb. buluşsal yöntemler, biçimi ekler.
* Oynatıcı uygulaması API'leri 
  * medya oynatıcı uygulaması ile etkileşim için programlama arabirimleri
* Platform Soyutlama Katmanı (PAL) arabirimi 
  * işletim sistemi (iş parçacıkları, yuva) ile etkileşim için programlama arabirimleri
* Donanım özet düzeyi (HAL) arabirimi 
  * programlama arabirimleri donanım A ile etkileşim için / (kod çözme, işleme) V kod çözücüleri
* Dijital Hak Yönetimi (DRM) arabirimi 
  * DRM DRM özet düzeyi (DAL) aracılığıyla işlemek için programlama arabirimleri
  * Microsoft PlayReady bağlantı noktası oluşturma Seti ayrı olarak gelir ancak bu arabirimi aracılığıyla tümleştirir. Microsoft PlayReady Device lisanslama hakkında daha fazla ayrıntı için tıklatın [burada](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Uygulama örnekleri 
  * Linux için örnek PAL uygulama
  * Örnek HAL uygulama GStreamer için

## <a name="licensing-options"></a>Lisanslama Seçenekleri
Microsoft kesintisiz akış istemci bağlantı noktası oluşturma Seti kullanımına sunulur lisans sahipleri için iki farklı lisans sözleşmelerini altında: kesintisiz akış istemci geçici ürünleri ve son kullanıcılar için kesintisiz akış istemci son ürünler dağıtmak için başka bir geliştirme için bir tane.

* Yonga kümesi üreticileri, sistem tümleştiricileri veya bağımsız yazılım için bir kaynak isteyen satıcıları (ISV) geçiş ürünler, bir Microsoft kesintisiz akış istemci bağlantı noktası oluşturma Seti geliştirmek için bağlantı noktası oluşturma Seti kod **geçici Ürün lisans** yürütülmelidir.
* Cihaz üreticileri veya dağıtım hakları kesintisiz akış istemci son ürünler için gereken son kullanıcı, Microsoft kesintisiz akış istemci bağlantı noktası oluşturma Seti ISV'ler **son ürün lisans** yürütülmelidir.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming Client Seti geçici Ürün lisans bağlantı noktası oluşturma
Bu lisansı altında bir kesintisiz akış istemci bağlantı noktası oluşturma Seti ve geliştirmek ve kesintisiz akış istemci son ürünler dağıttığınız diğer kesintisiz akış istemci bağlantı noktası oluşturma Seti aygıt lisans sahipleri için kesintisiz akış istemci geçici ürünleri dağıtmak için gerekli fikri mülkiyet hakları Microsoft sunar.

#### <a name="fee-structure"></a>Ücret yapısı
ABD 50.000 tek seferlik lisans ücret kesintisiz akış istemci bağlantı noktası oluşturma Seti için erişim sağlar. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming Client Seti son ürün lisans bağlantı noktası oluşturma
Bu lisansı altında tüm gerekli fikri mülkiyet hakları diğer kesintisiz akış istemci bağlantı noktası oluşturma Seti lisans sahipleri için kesintisiz akış istemci geçici ürünleri almak ve şirket markalı kesintisiz akış istemci son ürünler son kullanıcılara dağıtmak için Microsoft sunar.

#### <a name="fee-structure"></a>Ücret yapısı
Kesintisiz akış istemci son ürünün lisanslı model olarak altında altında sunulur:

* cihaz uygulaması başına 0,10 sevk
* Lisanslı 50.000 her yıl tutulabilir
* İlk 10.000 cihaz uygulamaları her yıl için hiçbir lisanslı 

## <a name="licensing-procedure-and-sspk-access"></a>Lisans yordamı ve SSPK erişim
E-posta [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) tüm lisans sorgular.

[SSPK dağıtım portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) kayıtlı geçici lisans sahipleri için erişilebilir.

Lisans sahipleri için geçici ve son SSPK teknik sorular için gönderme [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft istemci geçici ürün sözleşmesi lisans sahipleri akış kesintisiz
* Adroit işletme çözümleri, Inc
* Gelişmiş dijital yayın SA'sı
* AirTies Kablosuz Iletism Sanayive Dış Ticaret A.S.
* Albis teknolojileri Ltd.
* Alticast Corporation
* Amazon dijital Hizmetleri, Inc.
* Arion Technology, Inc.
* AVC multimedya yazılım Co., Ltd.
* Cavium, Inc.
* EchoStar Corporation'ın satın alma
* Enseo, Inc.
* Fluendo Güney Amerika
* HANDAN BroadInfoCom Co., Ltd.
* Infomir GMBH
* Irdeto ABD Inc.
* iWEDIA Güney Amerika 
* Serbest genel Hizmetleri BV
* MediaTek Inc.
* MStar Co, Ltd
* Nintendo Co., Ltd.
* OpenTV, Inc.
* Saffron dijital sınırlı
* Sichuan Changhong elektrik Co., Ltd
* SoftAtHome
* Sony Corporation
* Tatung Technology Inc.
* TCL teknoloji elektronik bileşenleri (Huizhou) Co., Ltd.
* Üst sırrı Yatırımlar, Ltd
* Vestel Elektronik Sanayi ullanıcı Ticaret A.S.
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft istemci son ürün sözleşmesi lisans sahipleri akış kesintisiz
* Gelişmiş dijital yayın SA'sı
* AirTies Kablosuz Iletism Sanayive Dış Ticaret A.S.
* Albis teknolojileri Ltd.
* Amazon dijital Hizmetleri, Inc.
* AmTRAN teknoloji Co., Ltd.
* Arcadyan teknoloji Corporation
* Arion Technology, Inc.
* ATMACA ELEKTRONİK SAN. HEDEFTEKİ TİC. A.Ş
* İngiliz Sky yayın sınırlı
* CastPal Technology Inc. Shenzhen
* Compal Electronics, Inc.
* Dongguan dijital AV teknolojisi Corp., Ltd
* EchoStar Corporation'ın satın alma
* Enseo, Inc.
* Filmflex filmler sınırlı
* Fluendo Güney Amerika
* Gibson yenilikleri sınırlı
* Haier bilgi Applicantion S.R.L
* HANDAN BroadInfoCom Co., Ltd.
* Hisense uluslararası Co., Ltd. 
* Homecast Co., Ltd
* Hon Hai duyarlık endüstri Co., Ltd.
* Infomir GMBH
* Kaonmedia Co., Ltd.
* KDDI Corporation
* Nintendo Co., Ltd.
* Turuncu SA'sı
* Saffron dijital sınırlı
* Sagemcom geniş bant SAS
* Shenzhen Coship Electronics CO., LTD
* Shenzhen Jiuzhou elektrik Co., Ltd
* Shenzhen Skyworth dijital teknoloji Co., Ltd
* Sichuan Changhong elektrik Co., Ltd.
* Skardin endüstriyel Corp.
* Deutschland Fernsehen GmbH & Co. KG sky
* SmarDTV Güney Amerika
* SoftAtHome
* Sony Corporation
* TCL deniz aşırı pazarlama (Offshore Makao ticari) sınırlı
* Technicolor teslim teknolojileri, SAS
* Tongfang genel Ltd.
* Üst sırrı Yatırımlar, Ltd
* Toshiba Lifestyle ürünler ve hizmetler Corporation
* Evrensel medya Corporation /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

