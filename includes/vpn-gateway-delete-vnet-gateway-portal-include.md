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
ms.openlocfilehash: d36d2be59010c47348a8e196b28d87e5b967868e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188242"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>1\. adım: Sanal ağ geçidine gidin

1. İçinde [Azure portalında](https://portal.azure.com), gitmek **tüm kaynakları**. 
2. Sanal ağ geçidi sayfasını açmak için tıklayın ve silmek istediğiniz sanal ağ geçidine gidin.

### <a name="step-2-delete-connections"></a>2\. adım: Bağlantıları silme

1. Sanal ağ geçidinizin sayfasında tıklayın **bağlantıları** tüm ağ geçidi bağlantılarını görüntülemek için.
2. Tıklayın **'...'** bağlantının adını satırda seçip **Sil** açılır listeden.
3. Tıklayın **Evet** bağlantıyı silmek istediğinizi onaylayın. Birden çok bağlantı varsa, her bağlantısını silin.

### <a name="step-3-delete-the-virtual-network-gateway"></a>3\. adım: Sanal ağ geçidini silme

S2S yapılandırmasının yanı sıra bu sanal ağa bir P2S yapılandırması varsa, sanal ağ geçidini silme otomatik olarak uyarı olmadan tüm P2S istemcileri kesecek olduğunu unutmayın.

1. Sanal ağ geçidi sayfasında tıklayın **genel bakış**.
2. Üzerinde **genel bakış** sayfasında **Sil** ağ geçidi silinemedi.
