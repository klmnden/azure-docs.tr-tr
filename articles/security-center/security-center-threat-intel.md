---
title: Azure Güvenlik Merkezi'ndeki tehdit bilgileri | Microsoft Docs
description: Azure Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak VM'lerinizdeki ve bilgisayarlarınızdaki potansiyel tehditleri tanımlamayı öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2018
ms.author: terrylan
ms.openlocfilehash: 8f1c6aa2e691a11e8920db8ca8bfdef5b8eb61b9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434199"
---
# <a name="threat-intelligence-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tehdit bilgileri
Bu makale Azure Güvenlik Merkezi'ndeki tehdit bilgilerini kullanarak güvenlikle ilgili sorunları gidermenizi sağlar.

## <a name="what-is-threat-intelligence"></a>Tehdit bilgileri nedir?
BT yöneticileri Güvenlik Merkezi'ndeki tehdit bilgileri seçeneğini kullanarak ortama yönelik güvenlik tehditlerini belirleyebilir. Örneğin belirli bir bilgisayarın botnetin parçası olup olmadığını saptayabilir. Bilgisayar korsanları yasa dışı yollarla bu bilgisayarı bir komut veya denetime bağlayan kökü amaçlı bir yazılım yüklediklerinde, bilgisayarlar botnette düğümlere dönüşür. Tehdit bilgileri ayrıca, dark web gibi yeraltı iletişim kanallarından gelen olası tehditleri de belirleyebilir.

Güvenlik Merkezi bu tehdit bilgilerini toplamak için Microsoft içindeki farklı kaynaklardan gelen verileri kullanır. Güvenlik Merkezi bu verileri kullanarak ortamınızdaki potansiyel tehditleri tanımlar. **Tehdit bilgileri** bölmesi üç ana seçenekten oluşur:

- Algılanan tehdit türleri
- Tehdit kaynağı
- Tehdit bilgileri haritası


## <a name="when-should-you-use-threat-intelligence"></a>Tehdit bilgilerini ne zaman kullanmalısınız?
[Güvenlik olayı yanıt süreci](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) adımlarından biri, tehlike altındaki sistemlerin önem derecesini tanımlamaktır. Bu aşamada aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Saldırının yapısını belirleme.
- Saldırının kaynak noktasını belirleme.
- Saldırının amacını belirleme. Saldırı kuruluşunuza belirli bilgileri elde etme amacıyla mı yapıldı yoksa rastgele bir saldırı mıydı?
- Gizliliği tehlikeye girmiş sistemleri tanımlayın.
- Erişilen dosyaları ve bu dosyaların gizlilik düzeyini tanımlayın.

Bu görevlerde yardımcı olması için Güvenlik Merkezi'ndeki tehdit bilgilerini kullanabilirsiniz.

## <a name="access-the-threat-intelligence"></a>Tehdit bilgilerine erişme
Ortamınız için güncel tehdit bilgilerini görselleştirmek için bilgilerinizin bulunduğu çalışma alanını seçmeniz gerekir. Birden fazla çalışma alanınız yoksa çalışma alanı seçicisini atlayıp doğrudan **Tehdit Bilgileri** panosuna gidersiniz. Panoya erişmek için:

1. **Güvenlik Merkezi** panosunu açın.

1. Sol bölmede altında **tehdit koruması** seçin **tehdit bilgileri**. Bir harita açar.

    ![Tehdit bilgileri haritası](./media/security-center-threat-intel/security-center-threat-intel.png)

1. Haritanın üstünde seçin **görüntülemek Klasik tehdit zekası**. **Tehdit bilgileri** panosu açılır.

    ![Tehdit bilgileri panosu](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > En sağdaki sütunda **PLANI YÜKSELT** görünüyorsa çalışma alanı ücretsiz aboneliği kullanmaktadır. Bu özelliği kullanmak için Standart sürüme yükseltin. En sağdaki sütunda **YÜKSELTME GEREKİYOR** görünüyorsa bu özelliği kullanmak için [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)'i yükseltin. Fiyatlandırma planı hakkında daha fazla bilgi için Azure Güvenlik Merkezi fiyatlandırma sayfasını inceleyin.
    >
1. Araştırmanız gereken birden fazla çalışma alanı varsa **Kötü Amaçlı IP** sütununa göre araştırmada öncelik belirleyin. Bu alan çalışma alanındaki güncel kötü amaçlı IP sayısını gösterir. Kullanmak istediğiniz çalışma alanını seçtiğinizde **Tehdit bilgileri** panosu açılır.

    ![Tehdit bilgileri](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

1. Pano dört kutucuğa ayrılmıştır:

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
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
