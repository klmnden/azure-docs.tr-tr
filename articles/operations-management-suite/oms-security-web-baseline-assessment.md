---
title: "Operations Management Suite Güvenlik ve Denetim Çözümünde Web Temeli Değerlendirmesi | Microsoft Docs"
description: "Bu belgede; uyumluluk ve güvenlik amaçlarıyla, izlenen tüm web sunucularında bir temel değerlendirmenin gerçekleştirilmesi için OMS Güvenlik ve Denetim çözümünde web temeli değerlendirmesinin nasıl kullanılacağı açıklanmaktadır."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: 40b0c6ca933ea02ac9f5fe3bfaaf87a310542a8d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Operations Management Suite Güvenlik ve Denetim Çözümünde Web Temeli Değerlendirmesi
Bu belge, izlenen kaynaklarınızın güvenli durumunu değerlendirmek üzere OMS Güvenlik ve Denetim web temeli değerlendirmesi özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-web-baseline-assessment"></a>Web temeli değerlendirmesi nedir?
OMS Güvenliği şu anda işletim sistemleri için güvenlik temeli değerlendirmesi sağlamaktadır. Sunucularınızın işletim sistemi ayarlarını 24 saatte bir taramakta ve savunmasız olabilecek ayarları görüntülemeyi sağlamaktadır. Bu seçenek hakkında daha fazla bilgi için [Operations Management Suite Güvenlik ve Denetim Çözümünde Temel Değerlendirmesi](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline) makalesini okuyun.

Web Temeli değerlendirmesinin amacı, savunmasız olabilecek web sunucusu ayarlarını bulmaktır. Web temeli yapılandırmaları için üç birincil kaynak şunlardır: .NET, ASP.NET ve IIS yapılandırması.  İşletim sistemi temel değerlendirmesinde olduğu gibi, OMS Güvenliği web sunucularınızı 24 saatte bir tarar ve güvenlik durumlarını görüntüler.  Internet Information Service (IIS) hizmetinde yapılandırmalar yüksek oranda özelleştirilebilir durumdadır, sonuç olarak çeşitli site ve uygulama düzeyleri geçersiz kılınabilir. Tarayıcı varsayılan kök düzeye ek olarak her bir uygulama/site düzeyindeki ayarları denetler. Bunun yapılması, savunmasız olabilecek ayarları tanımlayıp, bu ayarlara ilişkin önerilerimizle birlikte hızlıca düzeltmenize yardımcı olur.

>[!NOTE] 
>OMS güvenliği tarafından kullanılan Ortak Yapılandırma Tanımlayıcılarını ve Temel Kuralları [bu sayfadan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0) indirebilirsiniz.


## <a name="web-security-baseline-assessment"></a>Web güvenlik temeli değerlendirmesi

Bu önizlemede özelliğe OMS Arama seçeneği ile OMS Güvenlik ve Denetim Panosu’ndan erişilebilir. Uygun sorguyu gerçekleştirmek için aşağıdaki adımları izleyin:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
2. **Güvenlik ve Denetim** panosunda işletim sistemi temel perspektifinin yanında Web Temeli perspektifini görebilirsiniz.
   
    ![OMS Güvenlik ve Denetim Web Güvenliği Temeli Değerlendirmesi](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. Sol bölmede, temele kıyasla Web Sunucusu sayısı, değerlendirilen tüm sunucularda geçirilen kuralların ortalama yüzdesi ve değerlendirilmiş Sunucuların listesi gösterilir.
4. Sağ bölmede *Önem Derecesi* ve *RuleType* parametrelerine göre başarısız olan benzersiz kurallar listelenmektedir. Sağ bölmedeki kurallardan herhangi birine tıklandığında bu kuralın ayrıntıları gösterilir. Aşağıdaki görüntüde bir örnek gösterilmiştir. Değerlendirilen kural, *Kural Ayarı* altında listelenmiştir. Microsoft tarafından temel kuralları izlemek için kullanılan her bir kurala yönelik benzersiz tanıtıcı olan *AzId* alanı. Buna ek olarak, kullanıcılar *Beklenen Sonucu* (Microsoft'un önerdiği değer) ve kuralın güvenlik etkisi ile ilgili diğer ayrıntıları görebilir.
    
    ![Sorgu](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. Sonuçlarını gözden geçirmek için kendi sorgularınızı oluşturabilirsiniz. 

Kullanabileceğiniz ilk sorgu **Web Temeli Değerlendirme Özeti**’dir. **Aramaya buradan başlayın** alanına şu sorguyu yazın: Type*=SecurityBaselineSummary BaselineType=Web*. Aşağıda bir örnek çıktı gösterilmektedir:

![Sorgu Sonucu](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>Bu sorguda her kayıt, tek bir sunucu üzerindeki değerlendirme özetini gösterir.

**Günlük Araması** menüsüne geldikten sonra, web temeli değerlendirmesi hakkında daha fazla bilgi almak için farklı sorgular yazabilirsiniz. Önceki sorguya ek olarak, bu önizlemede aşağıdaki sorguyu kullanabilirsiniz:

**Web Temeli Kural Değerlendirmesi**: Her kayıt, tek bir sunucudaki tek bir web temeli kural değerlendirmesini temsil eder. Başarısız bir kurala ilişkin tüm verileri, kuralın değerlendirildiği *SitePath* değerini, *Beklenen Sonuç* ve *Gerçek Sonuç* değerlerini içerir.

Sorgu: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Sorgu Sonucu 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Belirli bir sunucu için tüm sonuçları göster**: Bu sorgu, belirli bir sunucuya ait sonuçlara nasıl bakılacağını gösterir: Sorgu: *Type=SecurityBaseline BaselineType=Web Computer=BaselineTestVM1*

![Sorgu Sonucu 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Bu kayıtları/sorguları, kendi pano, rapor veya uyarılarınızı oluşturmak için kullanabilirsiniz. Panonuza ekleyebileceğiniz bir örnek kullanıcı arabirimi denetimi aşağıda gösterilmektedir. OMS Görünüm Tasarımcısı kullanarak verilerinizi nasıl görselleştirebileceğinizi [buradan](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) öğrenebilirsiniz. Aşağıdaki ekran, bu özelleştirme yapıldıktan sonra kutucuğun nasıl görüneceğine ilişkin bir örnektir.

![pano](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede OMS Güvenlik ve Denetim Web Temeli değerlendirmesi hakkında bilgi edindiniz. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)

