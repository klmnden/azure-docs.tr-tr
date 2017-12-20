---
title: "Azure sanal ağ geçidi ve bağlantıları - PowerShell sorunlarını giderme | Microsoft Docs"
description: "Bu sayfa, Azure Ağ İzleyicisi'ni kullanma açıklanmaktadır PowerShell cmdlet'ini sorun giderme"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: c3fa22bd599026b0838b134e26062d9837df703e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi PowerShell kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ kaynaklarınızı Azure anlamak için ilgili olarak Ağ İzleyicisi çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorun giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulduklarını döndürür.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesi [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Kaynak sorunlarını giderme özelliği sağlar sanal ağ geçitleri ve bağlantılarla ortaya çıkan sorunları giderme. Kaynak sorun giderme için bir istek yapıldığında, günlükleri yükleniyor sorgulanan ve sahip denetlenir. İnceleme tamamlandığında, sonuçları döndürülür. Sorun giderme istekleri uzun süredir çalışıp kaynak, hangi birden çok dakika sürebilir bir sonuç ister. Sorun giderme gelen günlükleri, belirtilen bir depolama hesabı üzerinde bir kapsayıcıda depolanır.

## <a name="troubleshoot-a-gateway-or-connection"></a>Bir ağ geçidi veya bağlantı sorunlarını giderme

1. Gidin [Azure portal](https://portal.azure.com) tıklatıp **ağ** > **Ağ İzleyicisi** > **VPN tanılama**
2. Seçin bir **abonelik**, **kaynak grubu**, ve **konumu**.
3. Kaynak sorun giderme kaynak sistem durumu hakkındaki verileri döndürür. Aynı zamanda ilgili günlükleri gözden geçirilmesi için bir depolama hesabı kaydeder. Tıklatın **depolama hesabı** bir depolama hesabı seçin.
4. Üzerinde **depolama hesapları** dikey penceresinde, mevcut bir depolama hesabını seçin veya **+ depolama hesabı** yeni bir tane oluşturmak için.
5. Üzerinde **kapsayıcıları** dikey penceresinde, var olan bir kapsayıcı seçin veya tıklatın **+ kapsayıcı** yeni bir kapsayıcı oluşturmak için. Tıklatın tamamlandığında **seçin**
6. Sorun giderme tıklatıp geçidi ve bağlantıyla kaynakları seçin **sorun gidermeyi Başlat**

Birden fazla kaynak seçtiyseniz, sorun giderme ağ geçidi kaynaklarını aynı anda çalıştırılır. Bir bağlantı üzerinden çalıştırılamaz ve aynı zamanda ağ geçidi ilişkilendirilmiş. Ağ geçitlerinde sorun giderme için önerilen ilk gibi bağlantı sorunlarını giderme daha uzun bir işlemdir. VPN tanılama bir kaynakta çalışırken **sorun giderme durum** sütununu durumunu göster **çalışan**

Tamamlandığında, Durum sütununda değişikliklerini **sağlıklı** veya **sağlıksız**.

![eksiksiz sorun giderme][2]

## <a name="understanding-the-results"></a>Sonuçları anlama

İçinde **ayrıntıları** penceresi bölümünü **durum** sekmesi gösterir son sorun giderme durumunu çalışma seçili kaynağı. En son Tanılama sonuçları gösterilen xx dakika sonra son çalıştırma olur.

|Özellik  |Açıklama  |
|---------|---------|
|Kaynak     | Kaynak bağlantı.        |
|Depolama alanı yolu     |  Depolama hesabı ve kapsayıcı günlükleri (herhangi bir çalışma sırasında oluşturulamadığı varsa) içeren yolu. Portal çıktıktan sonra bu ayarı kalmaz.        |
|Özet     | Kaynak durumu özeti.        |
|Ayrıntı     | Kaynak durumu hakkında ayrıntılı bilgiler.        |
|Son çalıştırma     | Son sorun giderme zaman saat kaldı.        |


**Eylem** sekmesi, sorunun nasıl çözümleneceği hakkında genel kılavuzluk sağlar. Bir eylem için sorunu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Durumda hiçbir ek yönergeler bulunduğu bir destek servis talebi açmaya url yanıt sağlar.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)


## <a name="next-steps"></a>Sonraki adımlar

Bu Dur VPN bağlantısı ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
