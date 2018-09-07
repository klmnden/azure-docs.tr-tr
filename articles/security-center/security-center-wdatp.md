---
title: Windows Defender Gelişmiş tehdit Koruması (ATP) ile Azure Güvenlik Merkezi (genel Önizleme) | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi ve Windows Defender ATP arasında tümleştirme sağlar.
services: security-center
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2018
ms.author: barclayn
ms.openlocfilehash: f59525d6ff006ebd4560f96b2ca12bea3626a841
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057287"
---
# <a name="windows-defender-advanced-threat-protection-atp-with-azure-security-center-public-preview"></a>Windows Defender Gelişmiş tehdit Koruması (ATP) ile Azure Güvenlik Merkezi (genel Önizleme)

Azure Güvenlik Merkezi genişletme bulut iş yükü koruması platformları (CWPP) teklifini ile tümleştirerek [Windows Defender ATP](https://www.microsoft.com/WindowsForBusiness/windows-atp).
Bu değişiklik, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sunar. Prosesler spot, algılamanıza ve bu sunucu uç noktaları ASC tarafından izlenen Gelişmiş saldırıları yanıt olanak tanır.

Azure Güvenlik Merkezi müşteriler artık Windows Defender ATP'nin özelliklerini kullanabilirsiniz:

- **Yeni nesil post ihlali algılama sensörlerden:** Windows Defender ATP algılayıcısı Windows sunucuları, Gelişmiş saldırı algılama ve araştırma etkinleştirmek için davranış sinyalleri birçok çeşit toplar.

- **Analytics tabanlı, bulut destekli post ihlali algılama:** Windows Defender ATP hızla uyum sağlayan tehditleri değiştirmek için. Bu, Gelişmiş analiz ve büyük veri kullanır. Bu gücü sinyallerle Intelligent Security Graph tarafından Windows, Azure ve Office arasında bilinmeyen tehditleri algılamak için yükseltilmiş. Bu eyleme dönüştürülebilir uyarıları sağlar ve hızlı yanıt vermesini sağlar.

- **Tehdit bilgileri**: Windows Defender ATP saldırgan araçları, teknikleri ve yordamları tanımlar ve bu gözlemlenen uyarılar oluşturur. Güvenliği ekiplerinin Microsoft arayanlar tarafından oluşturulan ve iş ortakları tarafından sağlanan tehdit bilgileri tarafından Genişletilmiş verileri kullanır.

Bu özellikler Azure Güvenlik Merkezi'nde artık kullanılabilir:

- Otomatik ekleme - Windows Defender ATP algılayıcısını ASC Center'a Windows sunucuları otomatik olarak etkin

- ASC içinde-tek panel Windows Defender ATP uyarıları ASC konsolunda bulunur

- Ayrıntılı makine araştırma - ASC müşteriler, Windows Defender ATP konsolu, ihlal kapsamına ortaya çıkarmak için ayrıntılı araştırma gerçekleştirmenizi erişebilir

![* Şekil 1 tüm resmi ASC * tarafından oluşturulan uyarılar dahil olmak üzere araştırma görme](media/security-center-wdatp/image1.png)

Azure Güvenlik Merkezi'nde uyarı inceleyebilirsiniz:

![Şekil 2 araştırma - Azure Güvenlik Merkezi](media/security-center-wdatp/image2.png)

Uyarı Windows Defender ATP'ye özetleme tarafından daha fazla araştırabilirsiniz. Burada, uyarı işlem ağacını, olay grafik ve tüm davranışların bir geçmiş en fazla altı ay boyunca gösterme ayrıntılı makine zaman çizelgesi gibi ek bilgileri görebilirsiniz.

![Windows Defender ATP 3 araştırma – Şekil](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platform desteği

Bu özellik, Windows Server 2012 R2 ve Windows Server 2016 algılama destekler.

Yalnızca standart katmanda Abonelikteki sunucuları

## <a name="onboarding-instructions"></a>Ekleme yönergeleri

- Varsa, zaten yerleşik eklenen ASC ASC standart katman - hiçbir eylem sunuculara gerekli olacak otomatik olarak WDATP sunucularına.

- Varsa, hiçbir zaman sunuculara ASC standart katmanı – yerleşik ASC'ye zamanki eklenmedi.

- Varsa, eklenen sunucular üzerinden WDATP:
  - Üzerinde kılavuzu belgelerine başvurmak [çıkarma server makineleri nasıl](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - ASC ekleme

## <a name="access-to-wdatp-portal"></a>WDATP portalına erişim

Yönergeleri takip edin [portala kullanıcı erişim atama](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection)

## <a name="set-firewall-configuration"></a>Güvenlik Duvarı yapılandırmayı Ayarla

Windows Defender ATP algılayıcısını sistemi bağlamından bağlanıyor gibi bir proxy veya anonim trafik engelleyen güvenlik duvarı varsa, anonim trafiğine izin verilmesi emin olun; yönergeleri izleyerek [Windows Defender ATP hizmeti URL'leri proxy sunucusu erişimi etkinleştirme](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server)

## <a name="how-can-i-test-the-feature"></a>Özelliğin nasıl test edebilirim?

 Aşağıdaki adımlar bir zararsız WDATP test uyarısı oluşturur.

1. Windows Server Vm'leri birine RDP (2012R2 veya 2016) bir komut istemi penceresi açın ve aboneliği

2. Komut isteminde kopyalayın ve aşağıdaki komutu çalıştırın. Komut İstemi penceresi otomatik olarak kapanacak unutmayın.

    (yeni nesne System.NET.WebClient'a) **PowerShell.exe - NoExit - ExecutionPolicy atlama - WindowStyle gizli. DownloadFile ('http://127.0.0.1/1.exe', ' C:\\sınaması WDATP\\invoice.exe'); İşlemini Başlat ' C:\\sınaması WDATP\\invoice.exe' **

  ![komutu bir komut istemi penceresinde görüntüsü](media/security-center-wdatp/image4.jpeg)

3. Başarılı olursa, ASC ve WD ATP portalında birkaç dakika içinde yeni bir uyarı görüntülenir.

4. Azure Güvenlik Merkezi'nde uyarıyı gözden geçirin, Git **güvenlik uyarıları -\> şüpheli Powershell komut satırı**

5. Araştırmadan penceresi WDATP portalına yeniden yönlendirmek için bağlantıya tıklayın

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
