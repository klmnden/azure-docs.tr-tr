---
title: Tehdit algılama için Azure Güvenlik Merkezi'nde Azure hizmet katmanı | Microsoft Docs
description: Bu konuda, Azure hizmet katmanı uyarıları kullanılabilir Azure Güvenlik Merkezi'nde sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 8c1733877834f82d9ee2524cf8bf54f532e7d9c4
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571731"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Azure hizmet katmanı için tehdit algılama

Bu konu, aşağıdaki Azure hizmet katmanları izlerken kullanılabilir Güvenlik Merkezi uyarılarını gösterir.

* [Azure ağ katmanı](#network-layer)
* [Azure yönetim Katmanı (Azure Resource Manager) (Önizleme)](#management-layer)

>[!NOTE]
>Güvenlik Merkezi, iç Azure akışlarıyla olarak'nden yararlanan telemetriyi kullanarak, aşağıda verilen analytics tüm kaynak türleri için geçerlidir.

## Azure ağ katmanı<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analiz örneği temel alarak [IPFIX veri](https://en.wikipedia.org/wiki/IP_Flow_Information_Export), Azure'un çekirdek yönlendiricileri tarafından toplanan paket üst bilgilerini olduğu. Bu veri tabanlı, Güvenlik Merkezi Makine öğrenimi modellerini belirleyin ve kötü amaçlı trafik etkinlikleri bayrak. IP adresleri zenginleştirmek için Güvenlik Merkezi, Microsoft'un tehdit bilgileri veritabanı yararlanır.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Şüpheli giden RDP ağ etkinliği**|Örneklenen ağ trafiği analizinde dağıtımınızdaki kaynak kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikeye girdiğini ve deneme yanılma dış RDP uç noktası için artık kullanılan gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Şüpheli giden RDP ağ etkinliği birden fazla hedefe**|Örneği oluşturulmuş ağ trafiği analizinde birden çok hedefe, dağıtımınızdaki kaynak kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algılandı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikeye girdiğini ve deneme yanılma dış RDP uç noktaları için kullanılan şimdi gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Şüpheli giden SSH ağ etkinliği**|Örneklenen ağ trafiği analizinde dağıtımınızdaki kaynak kaynaklanan anormal giden güvenli Kabuk (SSH) iletişimi algılandı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikeye girdiğini ve deneme yanılma dış SSH uç noktası için artık kullanılan gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Şüpheli giden SSH ağ etkinliği birden fazla hedefe**|Örneği oluşturulmuş ağ trafiği analizinde birden çok hedefe, dağıtımınızdaki kaynak kaynaklanan anormal giden güvenli Kabuk (SSH) iletişimi algılandı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikeye girdiğini ve deneme yanılma dış SSH uç noktaları için kullanılan şimdi gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Birden çok kaynaktan şüpheli gelen SSH ağ etkinliği**|Örneklenen ağ trafiği analizinde anormal gelen SSH iletişimi birden çok kaynaktan bir kaynağa, dağıtımınızdaki algılandı. Çeşitli benzersiz IP'leri, kaynağa bağlanması bu ortam için anormal olarak kabul edilir. Bu etkinlik bir deneme yanılma birden çok konaktan (Botnet) SSH Arabiriminizin belirtebilir.|
|**Şüpheli gelen SSH ağ etkinliği**|Örneklenen ağ trafiği analizinde anormal gelen SSH iletişimi için bir kaynak, dağıtımınızdaki algılandı. Görece yüksek sayıda gelen bağlantıyı kaynağınıza bu ortam için anormal olarak kabul edilir. Bu etkinlik bir deneme yanılma SSH Arabiriminizin belirtebilir.
|**Birden çok kaynaktan şüpheli gelen RDP ağ etkinliği**|Örneklenen ağ trafiği analizinde anormal gelen RDP iletişimlerini birden çok kaynaktan bir kaynağa, dağıtımınızdaki algılandı. Çeşitli benzersiz IP'leri, kaynağa bağlanması bu ortam için anormal olarak kabul edilir. Bu etkinlik bir deneme yanılma birden çok konaktan (Botnet) RDP Arabiriminizin belirtebilir.|
|**Şüpheli gelen RDP ağ etkinliği**|Örneklenen ağ trafiği analizinde anormal gelen RDP iletişimi bir kaynağa, dağıtımınızdaki algılandı. Görece yüksek sayıda gelen bağlantıyı kaynağınıza bu ortam için anormal olarak kabul edilir. Bu etkinlik bir deneme yanılma SSH Arabiriminizin belirtebilir.|

Güvenlik Merkezi ağ nasıl kullanabileceğinizi anlamak için tehdit koruması uygulamak için bkz: sinyalleri ilgili [buluşsal DNS algılamalar Azure Güvenlik Merkezi'nde](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).
## Azure yönetim Katmanı (Azure Resource Manager) (Önizleme)<a name ="management-layer"></a>

>[!NOTE]
>Azure Resource Manager'a bağlı Güvenlik Merkezi koruma katmanı şu anda Önizleme aşamasındadır.

Güvenlik Merkezi, Azure Resource Manager olayları, Azure için Denetim düzlemi düşünülür yararlanarak ek bir koruma katmanı sağlar. Azure Resource Manager kayıtları analiz ederek, Güvenlik Merkezi Azure aboneliği ortamında olağan dışı veya zararlı işlemleri algılar.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**MicroBurst toolkit Çalıştır**|Ortamınızda çalıştırmak bilinen bulut ortamı keşif Araç Seti algılandı. ' % S'aracı "MicroBurst" (bkz https://github.com/NetSPI/MicroBurst) bir saldırgan (veya sızma test edici) aboneliği kaynaklarınıza harita, güvensiz yapılandırmalarını tanımlayın ve gizli bilgi sızıntısı için kullanılabilir.|
|**Azurite toolkit Çalıştır**|Ortamınızda çalıştırmak bilinen bulut ortamı keşif Araç Seti algılandı. ' % S'aracı "Azurite" (bkz https://github.com/mwrlabs/Azurite) bir saldırgan (veya sızma test edici) aboneliği kaynaklarınıza eşleyin ve güvensiz yapılandırmaları tanımlamak için kullanılabilir.|
|**Etkin olmayan bir hesap kullanarak şüpheli yönetim oturumu**|Abonelik etkinlik günlüklerini analiz bir şüpheli davranış algıladı. Saat, uzun bir süre için kullanımda olmadığı sorumlu bir saldırganın Kalıcılık güvenli hale getirebilirsiniz Eylemler artık gerçekleştiriliyor.|
|**PowerShell kullanarak şüpheli yönetim oturumu**|Abonelik etkinlik günlüklerini analiz bir şüpheli davranış algıladı. Abonelik ortamı yönetmek için PowerShell düzenli olarak kullanmayan bir asıl artık PowerShell kullanarak ve bir saldırganın Kalıcılık güvenli hale getirebilirsiniz eylemler gerçekleştirme.|
|**Gelişmiş Azure Kalıcılık teknikleri kullanımı**|Abonelik etkinlik günlüklerini analiz bir şüpheli davranış algıladı. Özelleştirilmiş roller legitimized kimlik varlıkları verildi. Bu, Azure müşteri ortamında kalıcılığı saldırgan neden olabilir.|
|**Sık erişilmeyen ülkeden etkinlik**|Yakın zamanda ya da hiç kuruluştaki herhangi bir kullanıcı tarafından ziyaret edilmedi bir konumdan etkinlik oluştu.<br/>Bu algılama yöntemi, yeni ve sık erişilmeyen konumlarını belirlemek için etkinlik konumları dikkate alır. Anomali algılama altyapısında, kuruluşunuzdaki kullanıcılar tarafından kullanılan önceki konumları hakkında bilgi depolar. 
|**Anonim IP adreslerinden etkinliği**|Kullanıcı etkinliği bir IP adresinden anonim proxy IP adresi algıladığından, belirlenmiştir. <br/>Bu proxy'ler cihazlarının IP adresini gizlemek istediğiniz ve için kötü amaçlı kullanılan kişiler tarafından kullanılır. Bu algılama yöntemi, bir makine öğrenme algoritmasına "hatalı pozitif sonuçlar", kuruluşunuzdaki kullanıcılar tarafından yaygın olarak kullanılan yanlış etiketli IP adresleri gibi azaltan yararlanır.|
|**Mümkün olmayan seyahat algılandı**|İki kullanıcı etkinlikleri (tek veya birden çok değer) oluşmuş bu süreden daha kısa bir süre içinde coğrafi olarak uzak konumlardan gerçekleştirilen sunucucu kullanıcının ilk konumdan ikinci seyahat. Bu, farklı bir kullanıcı kimlik bilgilerini kullandığını gösterir. <br/>Bu algılama yöntemi, bir makine öğrenme "VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi mümkün olmayan seyahat koşullar katkıda bulunan belirgin hatalı pozitif sonuçlar" yoksayar algoritmasına yararlanır. Bu sırada yeni bir kullanıcının etkinlik deseninin öğrenir yedi günlük bir öğrenme dönemi algılama sahiptir.|

>[!NOTE]
> Birkaç yukarıdaki analytics, Microsoft Cloud App Security (MCAS) tarafından desteklenir. Bu analytics'ten yararlanmak için etkinleştirilmiş bir MCAS lisans gereklidir. Ardından bir MCAS lisansınız varsa, bu uyarılar varsayılan olarak etkindir. Bunları devre dışı bırakmak için:
>
> 1. Güvenlik Merkezi dikey penceresinden seçin **Güvenlik İlkesi**. Değiştirmek istediğiniz abonelik için tıklatın **ayarlarını Düzenle**.
> 2. Tıklayın **tehdit algılama**.
> 3. Altında **etkinleştirme tümleştirmeler**, işaretini kaldırın **izin Microsoft bulut uygulama verilerimi erişmek için Güvenlik**, tıklatıp **Kaydet**.

>[!NOTE]
>Azure Güvenlik Merkezi ile ilgili müşteri veri kaynağı ile aynı coğrafi bölgede depolar. Microsoft Azure Güvenlik Merkezi henüz kaynağın coğrafi bölgede dağıtılmamış, ardından bu verileri Amerika Birleşik Devletleri'nde depolar. Microsoft Cloud App Security (MCAS) etkin olduğunda, bu bilgileri MCAS coğrafi konum kurallarına uygun olarak depolanır. Bkz: [daha fazla bilgi için bölgesel olmayan hizmetler için veri depolama](http://azuredatacentermap.azurewebsites.net/).
