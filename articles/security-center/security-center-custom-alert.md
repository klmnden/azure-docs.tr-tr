---
title: Azure Güvenlik Merkezi'ndeki Özel Uyarı Kuralları | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde özel uyarı kuralları oluşturmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f335d8c4-0234-4304-b386-6f1ecda07833
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: rkarlin
ms.openlocfilehash: 984bd4d5db210679884655721be0cbcdac8c1705
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485290"
---
# <a name="custom-alert-rules-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi'ndeki Özel Uyarı Kuralları (Önizleme)
Bu belge, Azure Güvenlik Merkezi'nde özel uyarı kuralları oluşturmanıza yardımcı olur.

> [!NOTE]
> Özel uyarılar kullanımdan kaldırıldı Güvenlik Merkezi'nden. 

## <a name="retirement-of-custom-alert-rules-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde devre dışı bırakılması özel uyarı kuralları

Özel uyarı deneyimi 30 Haziran 2019, altyapının üzerinde yararlandı emeklilik nedeniyle kullanımdan kaldırılmıştır. önce retriement tanımlanan herhangi bir özel uyarı etkisi olmuyor ve bu özel uyarılar kurallara göre güvenlik uyarıları değil oluşturuluyor. Yine de bunları aşağıda belirtilen alternatifleri de yeniden oluşturmak için Güvenlik Merkezi'ndeki özel uyarı kurallarınızı sorguları görüntüleyebilirsiniz:

Kullanıcılar için önerilir:
- Etkinleştirme [Azure Gözcü](https://azure.microsoft.com/services/azure-sentinel/) ve yerleşik [analytics](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) işlevselliği, uyarı kuralları yeniden oluşturmak için
- Azure izleyici günlüğü uyarıları, uyarılarla yeniden oluşturun
                                     
Mevcut uyarılarınızı tutun ve Azure Gözcü bunları yeniden oluşturmak için lütfen [Azure Gözcü başlatma](https://portal.azure.com/#create/Microsoft.ASI/preview). İlk adımı olarak özel uyarıları depolandığı çalışma alanını seçin ve ardından, özel uyarı kuralları ayarlamak için 'Analytics' menü öğesini seçin. Lütfen [belgeleri](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) ek bilgi için.

> [!NOTE]
> Kullanarak özel uyarılar [arama](https://docs.microsoft.com/azure/azure-monitor/log-query/search-queries) veya [birleşim](https://docs-analytics-eus.azurewebsites.net/queryLanguage/query_language_unionoperator.html) deyimleri sorgular Azure Gözcü içinde desteklenmez. Geçişi gerçekleştirmeden önce lütfen bu uyarılar düzenleyin.

Azure İzleyici günlük uyarıları kullanarak uyarıları yeniden oluşturmak için lütfen bkz: [Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) günlüğü uyarıları oluşturma hakkında yönergeler için. Azure İzleyici'de günlüğü uyarılarına genel bakış için tıklayın [burada](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

## <a name="what-are-custom-alert-rules-in-security-center"></a>Güvenlik Merkezi'ndeki özel uyarı kuralları nelerdir?

Güvenlik Merkezi'nde bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen önceden tanımlı [güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) vardır. Bazı senaryolarda ortamınızın ihtiyaçlarına uyacak özel bir uyarı oluşturmak isteyebilirsiniz.

Güvenlik Merkezi'ndeki özel uyarı kuralları, ortamınızdan toplanmış olan verilerle yeni güvenlik uyarıları tanımlamanızı sağlar. Sorgu oluşturabilir, bu sorguların sonuçlarını özel kuralın ölçütü olarak kullanabilir ve ölçütler karşılandığında kuralın yürütülmesini sağlayabilirsiniz. Özel sorgularınızı oluşturmak için bilgisayarların güvenlik olaylarını, iş ortağı güvenlik çözümü günlüklerini veya API'ler kullanılarak toplanan verileri kullanabilirsiniz.

> [!NOTE]
> Özel uyarılar, Güvenlik Merkezi'nin içinde desteklenmez [araştırma özelliği](security-center-investigation.md).
>
>

## <a name="how-to-create-a-custom-alert-rule-in-security-center"></a>Güvenlik Merkezi'nde özel uyarı kuralı nasıl oluşturulur?

Özel uyarı kuralı oluşturmak için **Güvenlik Merkezi** panosunu açın ve aşağıdaki adımları izleyin:

1.  Sol bölmede **Algılama**'nın altında **Özel uyarı kuralları (Önizleme)** öğesine tıklayın.
2.  **Güvenlik Merkezi – Özel uyarı kuralları (Önizleme)** sayfasında **Yeni özel uyarı kuralı**'na tıklayın.

    ![Özel uyarı](./media/security-center-custom-alert/security-center-custom-alert-fig1.png)

3.  Açılan Özel uyarı kuralı oluştur sayfasında aşağıdaki seçenekler yer alır:

    ![Create](./media/security-center-custom-alert/security-center-custom-alert-fig2.png)

4.  **Ad** alanına bu özel kuralın adını yazın.
5.  **Açıklama** alanına bu kuralın amacını belirten kısa bir açıklama yazın.
6.  **Güvenlik** alanında ihtiyaçlarınıza uygun önem derecesini (Yüksek, Orta, Düşük) seçin.
7.  **Abonelik** alanında kuralın uygulanacağı aboneliği seçin.
8.  Bölümünde bu kuralla izlemek istediğiniz çalışma alanını seçin **çalışma** alan ve **arama sorgusu** alanında da sonuçları almak için kullanmak istediğiniz sorgu.

    > [!NOTE]
    > Özel Uyarınız depolamak için seçtiğiniz çalışma alanında yazma izni.
    >
    >

    Sorgunun sonucu uyarıyı tetikler. Geçerli bir sorgu yazdığınızda bu alanın sağ köşesinde yeşil renkli onay işareti görünür:

    ![Sorgu](./media/security-center-custom-alert/security-center-custom-alert-fig3.png)

10. **Dönem** alanında yukarıdaki sorgunun yürütüleceği zaman aralığını seçin. Bu alanın en altında görünen arama sonucu seçtiğiniz zaman aralığına göre değişecektir.

    ![Dönem](./media/security-center-custom-alert/security-center-custom-alert-fig4.png)

11. **Değerlendirme** alanında bu kuralın değerlendirilme ve yürütülme sıklığını seçin.
12. **Sonuç sayısı** alanında işleci (büyüktür veya küçüktür) seçin.
13. **Eşik** alanına önceki bölümde seçilen işleç için referans olarak kullanılacak bir sayı girin.
14. Güvenlik Merkezi'nin bu kural için başka bir uyarı göndermeden önce bir süre beklemesini istiyorsanız **Uyarıları Bastır** seçeneğini etkinleştirin.
15. Ayarlamayı bitirmek için **Tamam**'a tıklayın.

Yeni uyarı kuralını oluşturmayı tamamladıktan sonra özel uyarı kuralları listesinde görebilirsiniz. İlgili kuralın koşulları yerine getirildiğinde yeni bir uyarı tetiklenir ve **Güvenlik Uyarıları** panosunda görüntülenir.

![Uyarı](./media/security-center-custom-alert/security-center-custom-alert-fig5.png)

Kural oluşturma aşamasında belirlenen parametreler (arama sorgusu, eşik vs.) bu özel kuralın uyarısında kullanılabilir.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde bir özel uyarı kuralının nasıl oluşturulacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
