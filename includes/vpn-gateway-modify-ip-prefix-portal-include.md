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
ms.openlocfilehash: 1199819d274590cc81d0234680f8765f9cc36c0a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120990"
---
### <a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

#### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **yapılandırma**.
2. IP adres alanı Ekle *ek adres aralığı Ekle* kutusu.
3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

#### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **yapılandırma**.
2. Tıklayın **'...'** kaldırmak istediğiniz ön eki içeren satırı üzerinde.
3. Tıklayın **Kaldır**.
4. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

### <a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **bağlantıları**.
2. Tıklayın **...**  her bağlantı için satıra, ardından **Sil**.
3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-address-prefixes"></a>2. Adres ön eklerini değiştirin.

Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **yapılandırma**.
2. IP adres alanı ekleyin.
3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **yapılandırma**.
2. Tıklayın **...**  kaldırmak istediğiniz ön eki içeren satırı üzerinde.
3. Tıklayın **Kaldır**.
4. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

#### <a name="3-recreate-the-connection"></a>3. Bir bağlantı oluşturabilirsiniz.

1. Sanal ağınıza ait sanal ağ geçidine gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi, şirket içinde **ayarları** bölümünde **bağlantıları**.
3. Tıklayın **+ Ekle** açmak için **Bağlantı Ekle** dikey penceresi.
4. Bağlantınızı yeniden oluşturun.
5. Tıklayın **Tamam** bağlantı oluşturmak için.