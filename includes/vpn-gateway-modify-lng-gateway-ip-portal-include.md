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
ms.openlocfilehash: a79184a5e08aa43a4675194adf5f10b9807418db
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36329561"
---
### <a name="gwipnoconnection"></a> Yerel ağ geçidi IP adresi - ağ geçidi bağlantı değiştirmek için

Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın. Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. İçinde **IP adresi** kutusunda, IP adresini değiştirin.
3. Ayarları kaydetmek için **Kaydet**’e tıklayın.

### <a name="gwipwithconnection"></a>Var olan ağ geçidi bağlantısı yerel ağ geçidi IP adresini - değiştirmek için

Bir bağlantısı olan bir yerel ağ geçidi değiştirmek için önce bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 
#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **bağlantıları**.
2. Tıklatın **...**  bağlantı için satırda ardından **silmek**.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-ip-address"></a>2. IP adresini değiştirin.

Aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. İçinde **IP adresi** kutusunda, IP adresini değiştirin.
2. Ayarları kaydetmek için **Kaydet**’e tıklayın.

#### <a name="3-recreate-the-connection"></a>3. Bağlantısını yeniden oluşturun.

1. Sanal ağ geçidi için sanal ağınızı gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi olarak **ayarları** 'yi tıklatın **bağlantıları**.
3. Tıklatın **+ Ekle** açmak için **Bağlantı Ekle** dikey.
4. Bağlantınızı yeniden oluşturun.
5. Tıklatın **Tamam** bağlantı oluşturmak için.