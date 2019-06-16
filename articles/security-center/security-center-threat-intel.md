---
title: Tehdit zekası ve Azure Güvenlik Merkezi'nde Güvenlik Uyarısı harita | Microsoft Docs
description: Kullanarak Vm'lerinizdeki ve bilgisayarlarınızdaki potansiyel tehditleri belirlemek için Azure Güvenlik Merkezi'nde Güvenlik Uyarısı Haritası ve tehdit zekası özelliğinin kullanmayı öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 36201bad64e5516375afe1ec9ce141c3fd311d48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64574357"
---
# <a name="security-alerts-map-and-threat-intelligence"></a>Güvenlik uyarıları haritası ve tehdit bilgileri
Bu makalede Azure Güvenlik Merkezi'nde güvenlik uyarıları harita ve güvenlik olay-tabanlı tehdit bilgileri Haritası güvenlikle ilgili sorunları gidermek üzere kullanmanıza yardımcı olur.

> [!NOTE]
> Güvenlik *olayları* Haritası düğmesinin 31 Temmuz 2019 üzerinde kullanımdan kaldırılacak. Daha fazla bilgi ve diğer hizmetler için bkz. [devre dışı bırakılması, Güvenlik Merkezi özelliklerini (Temmuz 2019)](security-center-features-retirement-july2019.md#menu_securityeventsmap).


## <a name="how-the-security-alerts-map-works"></a>Güvenlik uyarıları nasıl eşleştiği çalışır
Güvenlik Merkezi, size yardımcı olacak bir harita ortama yönelik güvenlik tehditlerini belirleyebilir sağlar. Örneğin, belirli bir bilgisayarın botnetin parçası olup olmadığını ve tehdit geldiğini tanımlayabilirsiniz. Saldırganlar yasa dışı komut ve denetimle botnet yöneten gizlice etkileşime giren bir kötü amaçlı yazılım yükleme sırasında bilgisayarları düğümlere hale gelebilir. 

Bu harita oluşturmak için Güvenlik Merkezi, Microsoft içindeki farklı kaynaklardan gelen verileri kullanır. Güvenlik Merkezi, olası tehditlere karşı ortamınızı eşlemek için bu verileri kullanır. 

[Güvenlik olayı yanıt süreci](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) adımlarından biri, tehlike altındaki sistemlerin önem derecesini tanımlamaktır. Bu aşamada aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Saldırının yapısını belirleme.
- Saldırının kaynak noktasını belirleme.
- Saldırının amacını belirleme. Saldırı kuruluşunuza belirli bilgileri elde etme amacıyla mı yapıldı yoksa rastgele bir saldırı mıydı?
- Gizliliği tehlikeye girmiş sistemleri tanımlayın.
- Erişilen dosyaları ve bu dosyaların gizlilik düzeyini tanımlayın.

Güvenlik Merkezi'nde güvenlik uyarıları harita, bu görevler için kullanabilirsiniz.

## <a name="access-the-security-alerts-map"></a>Erişim güvenliğini uyarılar Haritası
Geçerli ortamınızı tehditler görselleştirmek için güvenlik uyarıları harita açın:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede altında **tehdit koruması** seçin **güvenlik uyarıları harita**. Harita açar.
3. Uyarı hakkında daha fazla bilgi edinin ve düzeltme adımları almak için harita üzerinde uyarı nokta tıklayın ve yönergeleri izleyin. 
 
Güvenlik Uyarıları eşleme ile ilgili uyarılar temel alır. Bu uyarılar için hangi ağ iletişimi bilinen bir riskli IP adresi (örneğin, bilinen bir cryptominer) IP adresi olup olmadığını başarıyla çözümlendi, bir IP adresi veya tanınmayan bir IP adresi ile ilişkili etkinlikleri temel alan daha önce riskli olarak. Harita, Azure'da daha önce seçtiğiniz tüm Aboneliklerdeki uyarılar sağlar. 

Burada kaynaklanan olarak algılanır ve bunların önem derecesine göre renk kodludur coğrafi konuma göre harita üzerinde uyarılar görüntülenir. 
    ![Tehdit bilgileri](./media/security-center-threat-intel/security-center-alert-map.png)

## <a name="viewing-the-event-based-threat-intelligence-dashboard"></a>Olay tabanlı tehdit zekası panosunu görüntüleme
Tehdit bilgileri Haritası ham güvenlik etkinliklere göre görüntülemek için bu yordamı takip edebilirsiniz. Bu harita risk, örneğin bilinen bir botnet IP adresi olarak kabul edilir bir IP adresi içeren olayları görüntüler.

1. **Güvenlik Merkezi** panosunu açın.

1. Sol bölmede altında **tehdit koruması** seçin **güvenlik uyarıları harita**. Harita açar.
2. Sağ üst köşedeki, tıklayın **güvenlik olayları eşlemesine Git**.
3. Panoyu görüntülemek istediğiniz çalışma alanını seçin.
4. Haritanın üstünde seçin **görüntülemek Klasik tehdit zekası**. **Tehdit bilgileri** panosu açılır.

   > [!NOTE]
   > En sağdaki sütunda **PLANI YÜKSELT** görünüyorsa çalışma alanı ücretsiz aboneliği kullanmaktadır. Bu özelliği kullanmak için Standart sürüme yükseltin. En sağdaki sütunda **yükseltme gerekiyor**, güncelleştirme [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bu özelliği kullanmak için. Fiyatlandırma planı hakkında daha fazla bilgi için Azure Güvenlik Merkezi fiyatlandırma sayfasını inceleyin.
   >
5. Araştırmanız gereken birden fazla çalışma alanı varsa **Kötü Amaçlı IP** sütununa göre araştırmada öncelik belirleyin. Bu alan çalışma alanındaki güncel kötü amaçlı IP sayısını gösterir. Kullanmak istediğiniz çalışma alanını seçtiğinizde **Tehdit bilgileri** panosu açılır.

    ![Tehdit bilgileri](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

6. Pano dört kutucuğa ayrılmıştır:

    a.  **Tehdit türleri**. Seçilen çalışma alanında algılanan tehdit türlerinin özeti yer alır.

    b.  **Çıkış ülkesi**. Trafik miktarını kaynak konuma göre gösterir.

    c.  **Tehdit konumu**. Dünya üzerinde ortamınızla iletişim kuran noktaları tanımlamanıza yardımcı olun. Gösterilen haritada turuncu (gelen) ve kırmızı (giden) oklar trafik yönlerini gösterir. Bu oklardan birini seçtiğinizde tehdit türü ve trafik yönü görünür.

    d.  **Tehdit ayrıntıları**. Haritada seçtiğiniz tehdit hakkında ayrıntılı bilgiler gösterilir.

Belirlediğiniz seçenek kutucuğundan bağımsız olarak görünen pano Günlük Araması sorgusuna bağlıdır. Tek fark, sorgu türü ve sonuçtur.

### <a name="threat-types"></a>Tehdit türleri
**Tehdit türleri** kutucuğunu seçerek **Günlük Araması** panosunu açın. Filtre seçenekleri solda, sorgu sonuçları da sağda yer alır.

![Günlük araması](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Sorgu sonucu tehditleri ada göre gösterir. Sol bölmeyi kullanarak filtrelemek istediğiniz özniteliği seçebilirsiniz. Örneğin yalnızda bağlı makinelerle ilgili tehditleri görmek için **SESSIONSTATE** bölümünde **Bağlı** > **Uygula**'yı seçin.

![Oturum Durumu](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Azure VM'leri için **Tehdit bilgileri** panosunda yalnızca aracıdan geçen ağ verileri görüntülenir. Aşağıdaki veri türleri de tehdit bilgileri tarafından kullanılır:

- CEF Verileri (Type=CommonSecurityLog)
- WireData (Type= WireData)
- IIS Günlükleri (Type=W3CIISLog)
- Windows Güvenlik Duvarı (Type=WindowsFirewall)
- DNS Olayları (Type=DnsEvents)


## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak şüpheli etkinlikleri tanımlamayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi'nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi sorun giderme kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını bulun.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
