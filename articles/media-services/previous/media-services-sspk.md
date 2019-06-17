---
title: Lisans Microsoft® kesintisiz akış istemci taşıma kitini
description: Hakkında bilgi edinmek için Microsoft® kesintisiz akış istemci taşıma Kiti lisanslama.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: xpouyat
ms.openlocfilehash: 505def9cde7cddf2ddcc23408fa3159de886167a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61472722"
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Lisans Microsoft® kesintisiz akış istemci taşıma kitini 
## <a name="overview"></a>Genel Bakış
Microsoft kesintisiz akış istemci taşıma Kiti (**SSPK** kısaca) katıştırılmış cihaz üreticileri, kablo ve mobil işleçleri, içerik hizmet sağlayıcıları, ahize yardımcı olmak için optimize edilmiş bir kesintisiz akış istemci uygulaması Üreticiler, bağımsız yazılım satıcılarına (ISV) ve çözüm sağlayıcıları ürünleri ve Hizmetleri için kesintisiz akış biçiminde Uyarlamalı içerik akışı oluşturmak için. Bir cihaz ve platformdan bağımsız uygulama tarafından bir lisans herhangi bir cihaz ve platform için unity'nin kesintisiz akış istemci SSPK olduğu. 

Aşağıda üst düzey mimarisi ve IIS kesintisiz akış taşıma Seti kutusu Microsoft tarafından sağlanan Smooth Streaming Client uygulamasıdır ve kesintisiz akış içeriği, kayıttan yürütme için tüm çekirdek mantığını içerir. Bu içerik daha sonra belirli bir cihaz veya platformu iş ortakları tarafından uygun arabirimleri uygulama tarafından verilir. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Açıklama
SSPK mükemmel iş değeri sunan koşullarınızda lisanslanır. Sektör SSPK lisans sağlar:

* C++'ta kesintisiz akış taşıma Seti kaynak 
  * Smooth Streaming Client işlevselliğini uygular
  * ayrıştırma, buluşsal yöntemler, arabelleğe alma mantığını vb. biçimi ekler.
* Oynatıcı uygulaması API'leri 
  * bir medya yürütücü uygulamasına etkileşim için programlama arabirimleri
* Platform Soyutlama Katmanı (PAL) arabirimi 
  * işletim sistemi (iş parçacıkları, yuva) ile etkileşim için programlama arabirimleri
* Donanım özet düzeyi (HAL) arabirimi 
  * programlama arabirimleri A donanım ile etkileşim için / V kod çözücüleri (kod çözme, oluşturma)
* Dijital Hak Yönetimi (DRM) arabirimi 
  * DRM DRM özet düzeyi (DAL) aracılığıyla işlemek için programlama arabirimleri
  * Microsoft PlayReady taşıma Seti ayrı olarak verilir, ancak bu arabirimi ile tümleştirilir. Microsoft PlayReady Device lisanslama hakkında daha fazla bilgi için tıklayın [burada](https://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Uygulama örnekleri 
  * Linux için örnek PAL uygulama
  * Örnek HAL uygulama GStreamer için

## <a name="licensing-options"></a>Lisanslama Seçenekleri
Microsoft kesintisiz akış istemci taşıma Kiti kullanılabilir yapılan lisans sahipleri için iki ayrı lisans sözleşmesi altında: kesintisiz akış istemci geçiş ürünleri ve son kullanıcılar için kesintisiz akış istemci son ürünleri dağıtmak için başka bir geliştirme için bir tane.

* Yonga üreticilerinin, sistem tümleştiricileri veya bağımsız yazılım satıcılarına (ISV) bir kaynak isteyen geçici ürünleri, bir Microsoft kesintisiz akış istemci taşıma Kiti geliştirmek için taşıma Seti kod **geçici ürün lisansı** yürütülmelidir.
* Cihaz üreticisi veya kesintisiz akış istemci son ürünler için son kullanıcılara, Microsoft kesintisiz akış istemci taşıma Kiti dağıtım hakları isteyen ISV'ler için **son ürün lisansı** yürütülmelidir.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft kesintisiz akış istemci paketi Ara ürün lisansı
Bir kesintisiz akış istemci taşıma Kiti ve geliştirmek ve diğer kesintisiz akış istemci taşıma Kiti cihaz lisans sahipleri için kesintisiz akış istemci geçiş ürünleri dağıtmak için gerekli fikri mülkiyet hakları Microsoft bu lisans çerçevesinde sunan, Kesintisiz akış istemci son ürünleri dağıtın.

#### <a name="fee-structure"></a>Ücret yapısı
50\.000 TL ABD tek seferlik lisans ücreti, kesintisiz akış istemci taşıma Kiti için erişim sağlar. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft kesintisiz akış istemci taşıma Kiti son ürün lisansı
Bu lisans çerçevesinde Microsoft diğer kesintisiz akış istemci taşıma Kiti lisans kesintisiz akış istemci geçiş ürünleri almak ve şirket markası kesintisiz akış istemci son dağıtmak için tüm gerekli fikri mülkiyet hakları sunar. Son kullanıcılara ürünleri.

#### <a name="fee-structure"></a>Ücret yapısı
Kesintisiz akış istemci son ürün altında altında lisanslı model olarak sunulur:

* Sevk cihaz uygulaması başına $0.10
* Lisanslı her yıl ücret 50.000 TL alınır
* İlk 10.000 cihaz uygulamaları her yıl için hiçbir lisanslı 

## <a name="licensing-procedure-and-sspk-access"></a>Lisans yordam ve SSPK erişim
E-posta [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) tüm lisans sorgular.

[SSPK dağıtım portalı](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) kayıtlı geçici lisans sahipleri için erişilebilir.

Teknik sorular için lisans sahipleri için geçici ve son SSPK gönderebildiği [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft kesintisiz akış istemci geçiş ürün sözleşmesi lisans
* Adroit iş çözümleri, dahil edilen
* Gelişmiş dijital yayın SA
* AirTies Kablosuz Iletism Sanayive Dış Ticaret A.S.
* Albis Technologies Ltd.
* Alticast Corporation
* Amazon Digital Services, Inc.
* Arion teknolojisi, Inc.
* AVC Multimedia Software Co., Ltd.
* Cavium, Inc.
* EchoStar Corporation'ın satın alma
* Enseo, Inc.
* Fluendo S.A.
* HANDAN BroadInfoCom Co., Ltd.
* Infomir GMBH
* Irdeto USA Inc.
* iWEDIA Güney Amerika 
* Serbest genel Hizmetleri BV
* MediaTek Inc.
* MStar Co, Ltd
* Nintendo Co., Ltd.
* OpenTV, Inc.
* Saffron dijital sınırlı
* Sichuan Changhong Electric Co., Ltd
* SoftAtHome
* Sony Corporation
* Tatung teknoloji Inc.
* TCL Technology Electronics (Huizhou) Co., Ltd.
* Üst sırrı Yatırımlar, Ltd.
* Ticaret A.S. Vestel Elektronik Sanayi Kaydet
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft kesintisiz akış istemci son ürün sözleşmesi lisans
* Gelişmiş dijital yayın SA
* AirTies Kablosuz Iletism Sanayive Dış Ticaret A.S.
* Albis Technologies Ltd.
* Amazon Digital Services, Inc.
* AmTRAN teknoloji Co., Ltd.
* Arcadyan teknoloji Corporation
* Arion teknolojisi, Inc.
* ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
* İngiliz Sky yayın sınırlı
* CastPal teknoloji Inc. Shenzhen
* Compal Electronics, Inc.
* Dongguan Digital AV Technology Corp., Ltd.
* EchoStar Corporation'ın satın alma
* Enseo, Inc.
* FilmFlex Movies Limited
* Fluendo S.A.
* FUNAI ELECTRIC CO., LTD
* Gibson yeniliklerini sınırlı
* Haier bilgi Applicantion S.R.L
* HANDAN BroadInfoCom Co., Ltd.
* Hisense International Co., Ltd. 
* Homecast Co., Ltd
* Hon Hai Precision Industry Co., Ltd.
* Infomir GMBH
* Kaonmedia Co., Ltd.
* KDDI Corporation
* Nintendo Co., Ltd.
* Turuncu SA
* Saffron dijital sınırlı
* Sagemcom geniş bant SAS
* Shenzhen Coship Electronics CO., LTD
* Shenzhen Jiuzhou Electric Co., Ltd
* Shenzhen Skyworth Digital Technology Co., Ltd
* Sichuan Changhong Electric Co., Ltd.
* Skardin endüstriyel Corp.
* Gök Deutschland Fernsehen GmbH & Co. KG
* SmarDTV S.A.
* SoftAtHome
* Sony Corporation
* Ukazovat deniz aşırı pazarlama (Macau ticari Offshore) sınırlı
* SAS Technicolor teslim teknolojileri
* Tongfang genel Ltd.
* Üst sırrı Yatırımlar, Ltd.
* Toshiba yaşam ürünler ve hizmetler Corporation
* Universal Media Corporation /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

