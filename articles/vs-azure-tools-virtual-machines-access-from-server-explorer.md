---
title: Server Explorer'dan Azure sanal makinelere erişen | Microsoft Docs
description: Görüntüleme bakış oluşturun ve Azure sanal makineleri (VM'ler) Visual Studio Sunucu Gezgininde yönetmek alın.
services: visual-studio-online
author: ghogen
manager: douge
assetId: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 8/31/2017
ms.author: ghogen
ms.openlocfilehash: a19f33c4dd2654538c5718d2cd7dbe5d018e4de1
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792894"
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Server Explorer'dan Azure sanal makineleri erişme

Azure tarafından barındırılan sanal makineler varsa, sunucu Gezgini'nde erişebilir. İlk Azure aboneliğinize mobil hizmetleri görüntülemek için oturumunuzu gerekir. Oturum açmak için sunucu Gezgini'nde Azure düğüm için kısayol menüsünü açın ve seçin **Microsoft Azure Connect**.

1. Cloud Explorer'da, bir sanal makine seçin ve sonra Özellikler penceresini göstermek için F4 anahtarı seçin.

    Aşağıdaki tabloda hangi özelliklerin kullanılabilir olduğunu gösterir, ancak tüm salt okunurdur. Bunları değiştirmek için kullanmak [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

   | Özellik | Açıklama |
   | --- | --- |
   | DNS Adı |Sanal makinenin Internet adresiyle URL'si. |
   | Ortam |Bir sanal makine için bu özellik her zaman üretim değeri. |
   | Ad |Sanal makine adı. |
   | Boyut |Kullanılabilir bellek ve disk alanı miktarını yansıtır sanal makine boyutu. Daha fazla bilgi için bkz: [sanal makine boyutlarını](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs). |
   | Durum |Değerleri başlatma, başlatıldı, durdurma, durduruldu ve alma durumunu içerir. Durum alınıyor görünürse, geçerli durumu bilinmiyor. Bu özellik için değerleri üzerinde kullanılan değerler farklı [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). |
   | Subscriptionıd |Azure hesabınız için abonelik kimliği. Üzerinde bu bilgileri gösterebilir [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) bir abonelik özelliklerini görüntüleyerek. |
2. Bir uç nokta düğümü seçin ve ardından görüntülemek **özellikleri** penceresi.
3. Uç noktaları kullanılabilir özellikler aşağıdaki tabloda açıklanmaktadır, ancak bunlar salt okunurdur. Eklemek veya bir sanal makine için bitiş noktalarını düzenlemek için kullanmak [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). 

   | Özellik | Açıklama |
   | --- | --- |
   | Ad |Uç nokta için bir tanımlayıcı. |
   | Özel bağlantı noktası |Uygulamanıza iç ağ erişimi için bağlantı noktası. |
   | Protokol |Aktarım katmanı Bu uç noktası için kullandığı protokol TCP veya UDP. |
   | Genel Bağlantı Noktası |Genel erişim uygulamanız için kullanılan bağlantı noktası. |
