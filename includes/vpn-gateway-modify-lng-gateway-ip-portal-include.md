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
ms.openlocfilehash: 52084b065ef65a69a6691b6646d1e199f011910d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121000"
---
### <a name="gwipnoconnection"></a> Ağ geçidi bağlantısı olmadan yerel ağ ağ geçidi IP adresini değiştirme

Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın. Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **yapılandırma**.
2. İçinde **IP adresi** kutusuna, IP adresini değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

### <a name="gwipwithconnection"></a>Ağ geçidi bağlantısı var olan yerel ağ ağ geçidi IP adresini değiştirme

Bir bağlantıya sahip bir yerel ağ geçidini değiştirmek için ilk olarak bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 
#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** bölümünde **bağlantıları**.
2. Tıklayın **...**  bağlantı için satıra, ardından **Sil**.
3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-ip-address"></a>2. IP adresini değiştirin.

Aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. İçinde **IP adresi** kutusuna, IP adresini değiştirin.
2. Ayarları kaydetmek için **Kaydet**’e tıklayın.

#### <a name="3-recreate-the-connection"></a>3. Bir bağlantı oluşturabilirsiniz.

1. Sanal ağınıza ait sanal ağ geçidine gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi, şirket içinde **ayarları** bölümünde **bağlantıları**.
3. Tıklayın **+ Ekle** açmak için **Bağlantı Ekle** dikey penceresi.
4. Bağlantınızı yeniden oluşturun.
5. Tıklayın **Tamam** bağlantı oluşturmak için.