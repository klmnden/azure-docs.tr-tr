---
title: Windows Defender Gelişmiş tehdit koruması ile Azure Güvenlik Merkezi
description: Bu belge Azure Güvenlik Merkezi ve Windows Defender Gelişmiş tehdit koruması arasında tümleştirme sağlar.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2018
ms.author: monhaber
ms.openlocfilehash: 15232c92e60d21d759bec59597cb161480b8c2ea
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743223"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Windows Defender Gelişmiş tehdit koruması ile Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, [Windows Defender Gelişmiş Tehdit Koruması](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp) (ATP) tümleştirmesiyle Bulut İş Yükü Koruma Platformlarını genişletmektedir.
Bu değişiklik, kapsamlı Uç Nokta Koruma ve Yanıt (EDR) özelliklerini de beraberinde getirmektedir. Windows Defender ATP ile tümleştirme, prosesler nokta. Ayrıca, algılamak ve sunucu uç noktaları Azure Güvenlik Merkezi tarafından izlenen Gelişmiş saldırıları yanıtlar.

## <a name="windows-defender-atp-features-in-security-center"></a>Güvenlik Merkezi'nde Windows Defender ATP özellikleri

Windows Defender ATP kullandığınızda olursunuz:

- **Yeni nesil post ihlali algılama sensörlerden**: Windows Defender ATP algılayıcı Windows sunucuları için birçok çeşit davranış sinyalleri toplayın.

- **Analytics tabanlı, bulut destekli post ihlali algılama**: Windows Defender ATP, tehditleri değiştirmek için hızlı bir şekilde uyum sağlar. Gelişmiş analiz ve büyük veri kullanır. Windows Defender ATP gücünü sinyallerle Intelligent Security Graph tarafından Windows, Azure ve Office arasında bilinmeyen tehditleri algılamak için yükseltilmiş. Bu eyleme dönüştürülebilir uyarıları sağlar ve hızlı yanıt vermesini sağlar.

- **Tehdit bilgileri**: Windows Defender ATP, saldırgan araçları, teknikleri ve yordamları tanımlar. Bunlar algıladığında uyarılar oluşturur. Microsoft tehdit arayanlar tarafından oluşturulan verileri ve iş ortakları tarafından sağlanan zeka tarafından Genişletilmiş Güvenlik takımlar kullanır.

Bu özellikler Azure Güvenlik Merkezi'nde kullanıma sunulmuştur:

- **Otomatik ekleme**: Windows Defender ATP algılayıcı, Azure Güvenlik Merkezi'ne eklenen Windows sunucuları için otomatik olarak etkinleştirilir.

- **Tek cam bölmeyle**: Azure Güvenlik Merkezi Konsolu, Windows Defender ATP uyarıları görüntüler.

- **Ayrıntılı makine araştırma**: Azure Güvenlik Merkezi müşteriler, Windows Defender ATP konsolu bir ihlal kapsamına ortaya çıkarmak için bir ayrıntılı araştırma gerçekleştirmenizi erişebilir.

![Azure Güvenlik Merkezi, uyarılar ve her uyarı hakkında genel bilgi listesini görüntüleme](media/security-center-wdatp/image1.png)

Uyarı Windows Defender ATP'ye özetleme tarafından daha fazla araştırabilirsiniz. Burada, uyarı işlem ağacını ve olay grafik gibi ek bilgileri görebilirsiniz. Bir geçmiş en fazla altı ay boyunca her davranışını gösteren ayrıntılı makine zaman çizelgesi de görebilirsiniz.

![Bir uyarı hakkında ayrıntılı bilgileri Windows Defender ATP sayfası](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platform desteği

Güvenlik Merkezi'nde Windows Defender ATP, Windows Server 2012 R2 ve Windows Server 2016 işletim sistemlerinde standart hizmet aboneliğine ait olan algılama destekler.

> [!NOTE]
> Azure Güvenlik Merkezi olarak izlemeyi kullandığınızda, Windows Defender ATP Kiracı otomatik olarak oluşturulur ve Windows Defender ATP veriler varsayılan olarak Avrupa'da depolanır. Verilerinizi başka bir konuma taşımanız gerekirse, Kiracı sıfırlamak için Microsoft Support başvurmanız gerekir.

## <a name="onboarding-servers-to-security-center"></a>Güvenlik Merkezi'ne ekleme sunucuları 

Güvenlik Merkezi'ne ekleme sunucularına tıklayın **Azure Güvenlik Merkezi'ne ekleme sunucuları'na gidin** gelen Windows Defender ATP sunucu ekleme.

1. İçinde **ekleme** dikey penceresini seçin ya da verileri depolamak için bir çalışma alanı oluşturun. <br>
2. Tüm çalışma alanlarını göremiyorsanız, yetersiz izinler nedeniyle, çalışma alanınız için Azure güvenlik standart katmanı ayarlandığından emin olun. Daha fazla bilgi için [Güvenlik Merkezi'nin standart katmanında Gelişmiş güvenlik yükseltme](security-center-pricing.md).
    
3. Seçin **sunucuları ekleme** Microsoft Monitoring Agent'ı yükleme hakkında yönergeler görüntülemek için. 

4. Ekledikten sonra makineleri altında izleyebilirsiniz **işlem ve uygulamaları**.

   ![Bilgisayarları ekleme](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-windows-defender-atp-integration"></a>Windows Defender ATP tümleştirmesini etkinleştirme

Windows Defender ATP tümleştirmesi etkin olup olmadığını görmek için seçin **Güvenlik Merkezi** > **Güvenlik İlkesi** > **abonelik**  >  **Ayarlarını Düzenle**.

  ![Azure Güvenlik Merkezi ilke yönetimi](media/security-center-wdatp/policy-management.png)

Burada, şu anda etkin tümleştirmeler görebilirsiniz.

  ![Etkin Windows Defender ATP tümleştirmesi sayesinde Azure Güvenlik Merkezi tehdit algılama ayarları sayfası](media/security-center-wdatp/enable-integrations.png)

- Zaten Azure Güvenlik Merkezi standart katmanının sunuculara eklediyseniz, başka bir eylem yapması gerekmez. Azure Güvenlik Merkezi olur yerleşik otomatik olarak sunucuları için Windows Defender ATP. Bu 24 saate kadar sürebilir.

- Hiçbir zaman sunucuları Azure Güvenlik Merkezi standart katmanına ekleme eklediyseniz her zamanki şekilde bunları Azure Güvenlik Merkezi'ne.

- Sunucular üzerinden Windows Defender ATP eklediyseniz:
  - Üzerinde kılavuzu belgelerine başvurmak [çıkarma server makineleri nasıl](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Yerleşik Azure Güvenlik Merkezi bu sunuculara.

## <a name="access-to-the-windows-defender-atp-portal"></a>Windows Defender ATP portalına erişim

Bölümündeki yönergeleri [portala kullanıcı erişimi atamak](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection).

## <a name="set-the-firewall-configuration"></a>Güvenlik duvarı yapılandırmasını ayarlayın

Windows Defender ATP algılayıcı sistemi bağlamından bağlanıyor gibi bir proxy veya anonim trafik engelleyen güvenlik duvarı varsa, anonim trafik izin verildiğinden emin emin olun. Bölümündeki yönergeleri [Windows Defender ATP hizmeti URL'leri proxy sunucusu erişimi etkinleştirme](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Bu özelliği sınama

Zararsız bir Windows Defender ATP test uyarı oluşturmak için:

1. Uzak Masaüstü, Windows Server 2012 R2 VM veya Windows Server 2016 VM erişmek için kullanın.  Bir komut istemi penceresi açın.

2. Komut isteminde kopyalayın ve aşağıdaki komutu çalıştırın. Komut İstemi penceresi otomatik olarak kapatılacak.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Yukarıdaki komutu bir komut istemi penceresi](media/security-center-wdatp/image4.jpeg)

3. Komut başarılı olursa, Azure Güvenlik Merkezi panosunu hem de Windows Defender ATP portalında yeni bir uyarı görürsünüz. Bu uyarı görünmesi birkaç dakika sürebilir.

4. Güvenlik Merkezi'nde uyarı gözden geçirmek için Git **güvenlik uyarıları** >  **şüpheli Powershell komut satırı**.

5. Araştırma penceresinden Windows Defender ATP portalına gitmek için bağlantıyı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırmayı öğrenin.
- [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Öneriler, Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
