---
title: "Azure Güvenlik Merkezi'ndeki Tehdit Bilgileri | Microsoft Docs"
description: "Bu belge, Azure Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak VM'lerinizdeki ve bilgisayarlarınızdaki potansiyel tehditleri tanımlamanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c492662aa3ee444347c55d9771790573ad969150
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="threat-intelligence-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki Tehdit Bilgileri
Bu belge Azure Güvenlik Merkezi'ndeki Tehdit Bilgilerini kullanarak güvenlikle ilgili sorunları gidermenizi sağlar.

## <a name="what-is-threat-intelligence"></a>Tehdit bilgileri nedir?
Güvenlik Merkezi'ndeki Tehdit Bilgileri seçeneğini kullanarak, BT yöneticileri ortama yönelik güvenlik tehditlerini belirleyebilir, örneğin belirli bir bilgisayarın botnetin parçası olup olmadığını saptayabilir. Bilgisayar korsanları yasa dışı yollarla bu bilgisayarı bir komut veya denetime bağlayan kökü amaçlı bir yazılım yüklediklerinde, bilgisayarlar botnette düğümlere dönüşür. Ayrıca, darknet gibi yeraltı iletişim kanallarından gelen olası tehditleri de belirleyebilir.

Güvenlik Merkezi bu tehdit bilgilerini toplamak için Microsoft içindeki farklı kaynaklardan gelen verileri kullanır. Güvenlik Merkezi bu verileri kullanarak ortamınızdaki potansiyel tehditleri tanımlar. Tehdit Bilgileri bölmesi üç ana seçenekten oluşur:

- Algılanan tehdit türleri
- Tehdit kaynağı
- Tehdit bilgileri haritası


## <a name="when-should-i-use-threat-intelligence"></a>Tehdit bilgilerini ne zaman kullanmalıyım?
[Güvenlik olayı yanıt süreci](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) adımlarından biri, tehlike altındaki sistemlerin önem derecesini tanımlamaktır. Bu aşamada aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Saldırının yapısını belirleme
- Saldırının kaynak noktasını belirleme
- Saldırının amacını belirleme. Saldırı kuruluşunuza özellikle belirli bilgileri elde etme amacıyla mı yapıldı yoksa rastgele bir saldırı mıydı?
- Gizliliği tehlikeye girmiş sistemleri tanımlayın
- Erişilmiş dosyaları tanımlayın ve bu dosyaların hassasiyetini belirleyin. Bu görevler için Güvenlik Merkezi'ndeki Tehdit Bilgilerinden faydalanabilirsiniz. 

## <a name="how-to-access-the-threat-intelligence"></a>Tehdit bilgilerine nasıl erişebilirim?
Ortamınız için güncel tehdit bilgilerini görselleştirmek için bilgilerinizin bulunduğu çalışma alanını seçmeniz gerekir. Birden fazla çalışma alanınız yoksa çalışma alanı seçici görüntülenmez ve doğrudan **Tehdit Bilgileri** panosu açılır. Tehdit bilgileri panosuna erişmek için aşağıdaki adımları izleyin:

1.  **Güvenlik Merkezi** panosunu açın.
2.  Sol bölmede **Algılama**'nın altında **Tehdit Bilgileri**'ni seçin. **Tehdit Bilgileri** panosu açılır.

    ![Tehdit Bilgisi](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Son sütunda **PLANI YÜKSELT** ifadesinin görüntülenmesi bu çalışma alanının ücretsiz abonelik kullandığını ve bu özelliği kullanmak için standart sürüme yükseltmeniz gerektiğini gösterir. YÜKSELTME GEREKİYOR ifadesinin görüntülenmesi bu özelliği kullanmak için [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)'i güncelleştirmeniz gerektiğini gösterir. Fiyatlandırma planı hakkında daha fazla bilgi için Azure Güvenlik Merkezi fiyatlandırma sayfasını inceleyin. 
    > 
3. Araştırmanız gereken birden fazla çalışma alanınız varsa araştırmada ilgili çalışma alanındaki kötü amaçlı IP'leri gösteren **KÖTÜ AMAÇLI IP** sütununa göre öncelik belirleyebilirsiniz. Kullanmak istediğiniz çalışma alanını seçtiğinizde **Tehdit Bilgileri** panosu açılır.

    ![Tehdit Bilgisi](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Bu pano dört kutucuğa ayrılmıştır:
    * **Tehdit türleri**: Seçilen çalışma alanında algılanan tehdit türlerinin özeti yer alır.
    * **Çıkış ülkesi**: Trafik miktarını kaynak konuma göre gösterir.
    * **Tehdit konumu**: Dünya üzerinde ortamınızla iletişim kuran noktaları tanımlamanıza yardımcı olun. Bu haritada trafik yönünü belirten turuncu (gelen) ve kırmızı (giden) oklar bulunur. Bu oklardan birine tıkladığınızda tehdit türü ve trafik yönü gösterilir.
    * **Tehdit ayrıntıları**: Haritada seçtiğiniz tehdit hakkında ayrıntılı bilgiler gösterilir.

Seçtiğiniz seçenek kutucuğundan bağımsız olarak açılan pano [Günlük Araması](https://docs.microsoft.com/azure/security-center/security-center-search) sorgusunu temel alır ve tek fark, sorgu türü ile sonuç olacaktır.

### <a name="threat-types"></a>Tehdit türleri
**Tehdit türleri** kutucuğuna tıkladığınızda **Günlük Araması** panosu açılır ve sol tarafta filtreleme seçenekleri, sağ tarafta ise sorgu sonuçları yer alır.

![Tehdit bilgisi araması](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Sorgu sonucu tehdit adlarını ve sayılarını gösterir. Sol taraftaki bölmeyi kullanarak filtrelemek istediğiniz özniteliği seçebilirsiniz. Örneğin yalnızca makinelere bağlı olan tehditleri görmek isterseniz **SESSIONSTATE** sütununda **Bağlı** özelliğini seçin ve **Uygula** düğmesine tıklayın.

![oturum durumu](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Azure VM'leri için tehdit bilgileri panosunda yalnızca aracıdan geçen ağ verileri görüntülenir. Aşağıdaki veri türleri de tehdit bilgileri tarafından kullanılır:

- CEF Verileri (Type=CommonSecurityLog)
- WireData (Type= WireData)
- IIS Günlükleri (Type=W3CIISLog)
- Windows Güvenlik Duvarı (Type=WindowsFirewall)
- DNS Olayları (Type=DnsEvents)


## <a name="see-also"></a>Ayrıca bkz.
BU belgede Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak şüpheli etkinlikleri tanımlamayı öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin. 
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.


