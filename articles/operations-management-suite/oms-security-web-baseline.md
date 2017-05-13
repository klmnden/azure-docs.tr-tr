---
title: "Operations Management Suite Güvenlik ve Denetim Çözümü Web Temeli | Microsoft Docs"
description: "Bu belgede; uyumluluk ve güvenlik amaçlarıyla, izlenen tüm web sunucularında bir web temeli değerlendirmesinin gerçekleştirilmesi için OMS Güvenlik ve Denetim çözümünün nasıl kullanılacağı açıklanmaktadır."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.contentlocale: tr-tr
ms.lasthandoff: 05/03/2017


---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Operations Management Suite Güvenlik ve Denetim Çözümünde Web Temeli Değerlendirmesi
Bu belge, izlenen kaynaklarınızın güvenlik durumuna erişmek için [Operations Management Suite (OMS) Güvenlik ve Denetim Çözümü](operations-management-suite-overview.md) web temeli değerlendirmesi özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-web-baseline-assessment"></a>Web Temeli Değerlendirmesi nedir?
OMS Güvenliği şu anda işletim sistemleri için güvenlik temeli değerlendirmesi sağlamaktadır. Sunucularınızın işletim sistemi ayarlarını 24 saatte bir taramakta ve savunmasız olabilecek ayarları görüntülemeyi sağlamaktadır. Bu seçenek hakkında daha fazla bilgi için [Operations Management Suite Güvenlik ve Denetim Çözümünde Temel Değerlendirmesi](oms-security-baseline.md) makalesini okuyun.

Web temeli değerlendirmesinin amacı, savunmasız olabilecek web sunucusu ayarlarını bulmaktır. Web temeli yapılandırmaları için üç birincil kaynak şunlardır: .NET, ASP.NET ve IIS yapılandırması.  İşletim sistemi temel değerlendirmesinde olduğu gibi, OMS Güvenliği web sunucularınızı 24 saatte bir tarar ve güvenlik durumlarını görüntüler.  Internet Information Service (IIS) hizmetinde yapılandırmalar yüksek oranda özelleştirilebilir durumdadır, sonuç olarak çeşitli site ve uygulama düzeyleri geçersiz kılınabilir. Tarayıcı varsayılan kök düzeye ek olarak her bir uygulama/site düzeyindeki ayarları denetler. Bunun yapılması, savunmasız olabilecek ayarların konumlarını belirlemenize ve hızlıca düzeltmenize yardımcı olur.


## <a name="web-security-baseline-assessment"></a>Web Güvenlik Temeli Değerlendirmesi
Bu önizleme sürümünde bu özelliğe OMS Arama seçeneği kullanılarak erişilecektir. Uygun sorguyu gerçekleştirmek için aşağıdaki adımları izleyin:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
2. **Güvenlik ve Denetim** panosunda **Günlük Araması** düğmesine tıklayın.
3. Kullanabileceğiniz ilk sorgu **Web Temeli Değerlendirme Özeti**’dir. **Aramaya buradan başlayın** alanına şu sorguyu yazın: Type*=SecurityBaselineSummary BaselineType=web*. Aşağıdaki ekranda bir örnek çıktı gösterilmektedir:

![Web temeli değerlendirme özeti](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> Bu sorguda her kayıt, tek bir sunucu üzerindeki değerlendirme özetini gösterir.

**Günlük Araması** menüsüne geldikten sonra, web temeli değerlendirmesi hakkında daha fazla bilgi almak için farklı sorgular yazabilirsiniz. Önceki sorguya ek olarak, bu önizlemede aşağıdaki sorguyu kullanabilirsiniz.

**Web Temeli Kural Değerlendirmesi**: Her kayıt, tek bir sunucudaki tek bir web temeli kural değerlendirmesini temsil eder. Bu kayıt; kural, konum, beklenen sonuç ve gerçek sonucun tüm verilerini içerir.

**Sorgu**: Type*=SecurityBaseline BaselineType=web*

![Web temeli kural değerlendirmesi](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Belirli bir sunucu için tüm sonuçları göster**: Bu sorgu belirli bir sunucunun sonuçlarının nasıl görüntüleneceğini gösterir.

**Sorgu**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Tüm sonuçlar](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Bu kayıtları/sorguları, kendi pano, rapor veya uyarılarınızı oluşturmak için de kullanabilirsiniz. Aşağıdaki ekranda panonuza ekleyebileceğiniz bir örnek kullanıcı arabirimi denetimi gösterilmektedir. OMS Görünüm Tasarımcısı kullanarak verilerinizi nasıl görselleştirebileceğinizi [buradan](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) öğrenebilirsiniz. Aşağıdaki ekran, bu özelleştirme yapıldıktan sonra kutucuğun nasıl görüneceğine ilişkin bir örnektir.

![Örnek Kullanıcı Arabirimi](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Temel değerlendirme için işaretlenen ayarları bilmek istiyorsanız, bu ayarları içeren [Excel elektronik tablosunu](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) indirebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede OMS Güvenlik ve Denetim web temeli değerlendirmesi hakkında bilgi edindiniz. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)


