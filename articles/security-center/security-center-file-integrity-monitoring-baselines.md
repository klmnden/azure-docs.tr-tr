---
title: Dosya bütünlüğünü izleme Azure Güvenlik Merkezi'nde taban çizgileri karşılaştırın | Microsoft Docs
description: Dosya bütünlüğünü izleme Azure Güvenlik Merkezi'nde taban çizgileri karşılaştırma öğrenin.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: e403a9bd4d3f8668544dab1d81e9052b37839bef
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358446"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Dosya bütünlüğünü izleme (FIM) kullanarak temelleri karşılaştırın

Dosya bütünlüğünü izleme (FIM) araştırmak ve yetkisiz etkinlik yönelik değişiklikler hassas alanlara kullanarak kaynaklarınızı olduğunda sizi bilgilendirir. FIM Windows dosyaları, Windows kayıt defterleri ve Linux dosyaları izler.

Bu konuda dosya ve kayıt defterleri FIM etkinleştirme açıklanmaktadır. FIM hakkında daha fazla bilgi için bkz: [dosya bütünlüğünü izleme Azure Güvenlik Merkezi'nde](security-center-file-integrity-monitoring.md).

## <a name="why-use-fim"></a>FIM neden kullanmalısınız?

İşletim sistemi, uygulamalar ve ilişkili yapılandırmaları kaynaklarınızın davranışlar ve güvenlik durumunu denetler. Bu nedenle, saldırganların hedef kaynağın işletim sistemi overtake ve/veya algılanmayacak etkinlikleri yürütmek için kaynaklarınızı denetleyen dosyaları.

FIM denetimler uygulamak, PCI-DSS ve ISO 17799 gibi birçok Mevzuata uygunluk standartları gerektirir.  

## <a name="enable-built-in-recursive-registry-checks"></a>Yerleşik özyinelemeli kayıt defteri denetimlerini etkinleştir

FIM kayıt defteri kovanı varsayılan değerleri yaygın güvenlik alanlarına özyinelemeli değişiklikleri izlemek için kullanışlı bir yol sağlar.  Örneğin, bir saldırgan, başlatma veya kapatma sırasında bir yürütme yapılandırarak LOCAL_SYSTEM bağlamda yürütülecek bir betik yapılandırabilirsiniz.  Bu tür değişiklikleri izlemek için yerleşik denetimi etkinleştirin.  

![Kayıt defteri](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Özyinelemeli denetimleri yalnızca önerilen güvenlik yığınlarını ve özel kayıt defteri yolları için geçerlidir.  

## <a name="adding-a-custom-registry-check"></a>Bir özel kayıt defteri denetimi ekleme

FIM temelleri, işletim sistemi için bilinen iyi duruma özelliklerini tanımlayan ve uygulama destekleyen başlatın.  Bu örnekte, biz parola ilkesi yapılandırmalarını ve üzeri için Windows Server 2008 üzerinde odaklanır.


|İlke Adı                 | Kayıt defteri ayarı|
|---------------------------------------|-------------|
|Etki alanı denetleyicisi: Makine hesabı parola değişikliklerini reddet| MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RefusePasswordChange|
|Etki alanı üyesi: Dijital olarak şifrele veya güvenli kanal verisini (her zaman) oturum açın|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RequireSignOrSeal|
|Etki alanı üyesi: (Uygun olduğunda) güvenli kanal verisini dijital olarak şifrele|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\SealSecureChannel|
|Etki alanı üyesi: Dijital olarak imzala güvenli kanal verisini (uygun olduğunda)|MACHINE\System\CurrentControlSet\Services   \Netlogon\Parameters\SignSecureChannel|
|Etki alanı üyesi: Makine hesabı parola değişikliklerini devre dışı bırak|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DisablePasswordChange|
|Etki alanı üyesi: En fazla hesap parolası yaşı|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\MaximumPasswordAge|
|Etki alanı üyesi: Güçlü (Windows 2000 veya sonrasını) oturum anahtarı gerektir|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RequireStrongKey|
|Ağ güvenliği: NTLM'yi kısıtla:  Bu etki alanında NTLM kimlik doğrulaması|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\RestrictNTLMInDomain|
|Ağ güvenliği: NTLM'yi kısıtla: Bu etki alanında sunucu özel durumları Ekle|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\DCAllowedNTLMServers|
|Ağ güvenliği: NTLM'yi kısıtla: Bu etki alanında NTLM kimlik doğrulaması denetleme|MACHINE\System\CurrentControlSet\Services \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Çeşitli işletim sistemi sürümleri tarafından desteklenen kayıt defteri ayarları hakkında daha fazla bilgi için bkz [Grup İlkesi ayarları başvurusu elektronik](https://www.microsoft.com/en-us/download/confirmation.aspx?id=25250).

*Kayıt defteri temellerini izleme FIM yapılandırmak için:*

1. İçinde **için değişiklik izleme Windows kayıt defteri Ekle** penceresi içinde **Windows kayıt defteri anahtarı** metin kutusunda, kayıt defteri anahtarı girin.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![Bir kayıt defterindeki FIM etkinleştir](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Windows dosyaları değişiklikleri izleme

1. İçinde **değişiklik izleme için Windows dosyası ekleme** penceresi içinde **Enter yolu** metin kutusunda, izlemek istediğiniz dosyaları içeren klasörü girin. Örnekte aşağıdaki şekilde, **Contoso Web uygulamasına** D:\ içinde yer alıyor içinde sürücü **ContosWebApp** klasör yapısı.  
1. Özel bir Windows dosya girişi sağlayan bir ayar sınıfın adını, özyineleme etkinleştirmek ve bir joker (*) sonekiyle en üst düzey bir klasör belirterek oluşturun.

    ![Bir dosya çubuğunda FIM etkinleştir](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Değişiklik verilerini alma

Dosya bütünlüğünü veriler, Azure Log Analytics içinde yer izleme / ConfigurationChange tablosu ayarlama.  

 1. Kaynak tarafından değişikliklerin özetini almak için bir zaman aralığı ayarlayın.
Aşağıdaki örnekte, biz tüm değişiklikleri kayıt defteri ve dosya kategorilerdeki son on dört gün içinde alıyor:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Kayıt defteri değişikliklerini ayrıntılarını görüntülemek için:

    1. Kaldırma **dosyaları** gelen **burada** yan tümcesi 
    1. Özetleme satırını kaldırır ve bir sıralama yan tümcesi ile değiştirin:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Raporları CSV arşivleme ve/veya bir Power BI raporuna channeled aktarılabilir.  

![FIM veri](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)