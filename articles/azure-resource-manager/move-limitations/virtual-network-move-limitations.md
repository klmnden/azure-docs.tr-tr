---
title: Azure sanal ağ kaynakları için yeni abonelik veya kaynak grubu taşıma | Microsoft Docs
description: Azure Resource Manager sanal ağları bir yeni kaynak grubuna veya aboneliğe taşıma için kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 809d403a463048910a284e7f8fcdc18cf7c65b69
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723499"
---
# <a name="move-guidance-for-virtual-networks"></a>Sanal ağlar için Taşıma Kılavuzu

Bu makalede, sanal ağlar belirli senaryoları için nasıl taşırım açıklanır.

## <a name="dependent-resources"></a>Bağımlı kaynaklar

Bir sanal ağ taşırken, bağımlı kaynaklarını da taşımanız gerekir. VPN ağ geçitleri için IP adresleri, sanal ağ geçitleri ve tüm ilişkili bağlantı kaynakları taşımanız gerekir. Yerel ağ geçitleri farklı kaynak grubunda olabilir.

Bir ağ arabirimi kartı ile bir sanal makineyi taşımak için tüm bağımlı kaynaklarla taşımanız gerekir. Sanal ağ için ağ arabirimi kartına, sanal ağ ve VPN ağ geçitleri için tüm diğer ağ arabirim kartları taşıyın.

## <a name="peered-virtual-network"></a>Eşlenen sanal ağ

Eşlenmiş sanal ağın taşımak için önce sanal ağ eşlemesi devre dışı bırakmanız gerekir. Devre dışı sonra sanal ağ taşıyabilirsiniz. Taşıma sonrasında, sanal ağ eşlemesi yeniden etkinleştirin.

## <a name="subnet-links"></a>Alt ağ bağlantıları

Bir alt ağ kaynak gezinti bağlantılarının bulunduğu sanal ağı içeriyorsa bir sanal ağ başka bir aboneliğe taşınamıyor. Örneğin, bir Azure önbelleği için Redis kaynak bir alt ağa dağıtılırsa, bu alt ağ kaynak Gezinti bağlantısı vardır.

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
