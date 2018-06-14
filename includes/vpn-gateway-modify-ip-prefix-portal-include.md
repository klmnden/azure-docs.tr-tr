---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3b8049515f753cbcf8ca068c1790f716f02d30b6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197950"
---
### <a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

#### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. IP adres alanı Ekle *ek adres aralığı Ekle* kutusu.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. Tıklatın **'...'** kaldırmak istediğiniz önek içeren satırı üzerinde.
3. Tıklatın **kaldırmak**.
4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

### <a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **bağlantıları**.
2. Tıklatın **...**  her bağlantı için satırda ardından **silmek**.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-address-prefixes"></a>2. Adres öneklerini değiştirme.

Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. IP adres alanı ekleyin.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. Tıklatın **...**  kaldırmak istediğiniz önek içeren satırı üzerinde.
3. Tıklatın **kaldırmak**.
4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="3-recreate-the-connection"></a>3. Bağlantısını yeniden oluşturun.

1. Sanal ağ geçidi için sanal ağınızı gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi olarak **ayarları** 'yi tıklatın **bağlantıları**.
3. Tıklatın **+ Ekle** açmak için **Bağlantı Ekle** dikey.
4. Bağlantınızı yeniden oluşturun.
5. Tıklatın **Tamam** bağlantı oluşturmak için.