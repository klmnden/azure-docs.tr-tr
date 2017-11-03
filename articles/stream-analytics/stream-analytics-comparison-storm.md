---
title: "Analytics platformları: akış analizi için Apache Storm karşılaştırma | Microsoft Docs"
description: "Akış analizi için Apache Storm karşılaştırma kullanarak bulut analizi platformu seçme kılavuzu edinin. Özellikler ve farklar anlayın."
keywords: "Analiz platformu analytics platformları, bulut analizi platformu, storm karşılaştırma"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: 97044cb5d7b0b3fcb3b85328df618a265bc59b61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a>Akış analizi platformu seçme: Apache Storm ve Azure akış analizi karşılaştırma
Azure veri akışı çözümleme için birden çok çözümü sağlar: [Azure akış analizi](https://docs.microsoft.com/azure/stream-analytics/) ve [Azure hdınsight'ta Apache Storm](https://azure.microsoft.com/services/hdinsight/apache-storm/). Her iki analytics platformları bir PaaS çözümü yararları sağlar. Ancak platformları nasıl yapılandırmak ve bunları yönetmek gibi yeteneklerini de önemli farklılıklar vardır. 

Bu makalede özellikleri Apache Storm ve Azure akış analizi bulut analizi platformu olarak arasında seçmenize yardımcı olmak için yan yana karşılaştırmasını sağlar. 

## <a name="general-features"></a>Genel Özellikler

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure akış analizi</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Hdınsight üzerinde Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Açık kaynak?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Hayır. Azure Stream Analytics olan bir Microsoft özel sunar.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Evet. Apache Storm bir lisanslı Apache teknolojisidir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft Destek?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Evet </p>
            </td>
            <td width="246" valign="top">
                <p>
Evet </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Donanım gereksinimleri</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
yok. Azure Stream Analytics bir Azure hizmetidir.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
yok. Apache Storm bir Azure hizmetidir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Üst düzey birim</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kullanıcılar, dağıtmak ve akış işi izleyin.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kullanıcıların dağıtmak ve diğer iş yüklerini (toplu dahil) yanı sıra birden çok Storm işleri barındırabilir tüm küme izleyin.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Fiyatlandırma</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
İşlenen veri ve iş çalışıyor saat başına gerekli akış birim sayısını hacmi tarafından fiyatlandırılır. 
                </p>
                    <p>Daha fazla bilgi için bkz: <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics fiyatları</a>.</p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Satın alma biriminin küme dayalıdır ve küme, dağıtılan işleri bağımsız çalışan süreye göre ücretlendirilir.
                </p>
                <p>
Daha fazla bilgi için bkz: <a href="http://azure.microsoft.com/pricing/details/hdinsight/">Hdınsight fiyatlandırma</a>.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a>Yazma

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure akış analizi</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Hdınsight üzerinde Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Özellikleri: SQL DSL?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Evet. Stream Analytics, Dönüşümlerin oluşturmak için SQL benzeri bir dil sağlar.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Hayır. Kullanıcılar Java veya C# kod yazmak veya Trident API'leri kullanın.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Özellikleri: Geçici işleçler?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Varsayılan olarak, pencerelenmiş toplamlar ve zamana bağlı birleştirmeler desteklenir.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Zamana bağlı işleçleri kullanıcı tarafından uygulanmalıdır.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Geliştirme deneyimi</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kullanıcılar oluşturma hata ayıklama ve canlı akış alan türetilmiş örnek verileri kullanarak Azure Portalı aracılığıyla işlerini izleyin.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
.NET kullanarak kullanıcıların geliştirmek, hata ayıklama ve Visual Studio üzerinden izlemek. Java veya diğer diller kullanan kullanıcılar, kendi seçtikleri IDE kullanabilirsiniz.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hata ayıklama desteği</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Temel iş durumunu ve işlemlerini günlüklerini hata ayıklama yardımcı olmak kullanılabilir. Stream Analytics kullanıcıların hangi içerik veya ne kadar içerik günlükleri (yani, ayrıntılı mod) dahil belirtin şu anda izin vermez.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ayrıntılı günlükleri kullanılabilir. Kullanıcılar, Visual Studio veya kümeye oturum açmayı ve günlükleri doğrudan erişimini günlükleri erişebilir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Özel kod kullanarak genişletilebilirlik?</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kısmen JavaScript UDF'ler ile destekler. Daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF tümleştirme</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Evet. Kullanıcılar, C#, Java veya üzerinde Storm desteklenen herhangi bir dil özel kod yazabilirsiniz.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a>Veri kaynakları (girdileri) ve çıkışları ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure akış analizi</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Hdınsight üzerinde Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Giriş veri kaynakları</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Azure Event Hubs ile Azure Blob Depolama.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Bağlayıcılar, Azure Event Hubs, Azure Service Bus, Kafka ve daha fazla bilgi için kullanılabilir. Kullanıcıların özel kod kullanarak ek bağlayıcı oluşturabilirsiniz.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Giriş veri biçimleri</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Avro, JSON, CSV </p>
            </td>
            <td width="246" valign="top">
                <p>
Kullanıcıların özel kod kullanarak herhangi bir biçimde uygulayabilirsiniz.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Çıkışları</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
İş akışında birden çok çıkış olabilir. Desteklenen çıkışları Azure Event Hubs, Azure Blob Depolama, Azure Table storage, Azure SQL DB ve Power BI ' dir.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm birçok çıkışları bir topolojisinde destekler ve her çıktı aşağı akış işleme için Özel mantık olabilir. Power BI, Azure Event Hubs, Azure Blob Depolama, Azure Cosmos DB, SQL ve HBase, Storm bağlayıcıları içerir. Kullanıcıların özel kod kullanarak ek bağlayıcı oluşturabilirsiniz.    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kodlama verileri biçimleri</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Veri UTF-8 kullanarak biçimlendirilmiş olması gerekir.
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
Kullanıcıların özel kod kullanarak veri kodlama biçimi uygulayabilirsiniz.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a>Yönetim ve işlemler ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure akış analizi</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Hdınsight üzerinde Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>İş dağıtım modeli</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure portalı, PowerShell ve REST API'leri.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Azure portal, PowerShell, Visual Studio ve REST API'leri.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>İzleme (istatistikleri, sayaçları, vb.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
İzleme Azure portalı ve REST API'leri kullanılarak uygulanır. Kullanıcılar ayrıca Azure uyarılar yapılandırabilirsiniz.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
İzleme, REST API'leri ve Storm kullanıcı Arabirimi kullanılarak gerçekleştirilir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ölçeklenebilirlik</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ölçeklenebilirlik, her iş için Streaming Units (SUs) sayısı tarafından belirlenir. Her akış birimi en çok 1 MB/saniye, en fazla 50 birime sahip işler. Daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">verimliliğini artırmak için ölçek</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ölçeklenebilirlik, Hdınsight Storm kümedeki düğüm sayısı tarafından belirlenir. Düğüm sayısı üst sınırı, kullanıcının Azure kota tarafından tanımlanır.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Veri işleme sınırları</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kullanıcılar, veri işleme artırın veya artırarak veya azaltarak, akış birim sayısını 1 GB/saniye üst sınır tarafından masrafları iyileştirebilirsiniz.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kullanıcıların küme boyutu yukarı veya aşağı ölçeklendirebilirsiniz.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Stop/Sürdür</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Durdur ve durdurulmuş son yerden devam edin.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Durdurun ve son sürdürme Filigran üzerinde temel durdurulmuş yerleştirin.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hizmet ve framework güncelleştirme</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kapalı kalma süresi ile otomatik düzeltme eki uygulama.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kapalı kalma süresi ile otomatik düzeltme eki uygulama.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Garantili SLA ile bir yüksek oranda kullanılabilir hizmeti aracılığıyla iş sürekliliği</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li>% 99,9 çalışma süresi SLA'sı</li>
                <li>Hatalardan otomatik kurtarma</li>
                <li>Durum bilgisi olan geçici işleçler yerleşik kurtarılması</li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
Storm kümesi % 99,9 çalışma süresi SLA'sı. 
                </p>
                <p>
Apache Storm hataya dayanıklı akış platformudur. Ancak, bu akış işi kesintisiz çalışmasını sağlamak için kullanıcının sorumluluğundadır.
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a>Gelişmiş özellikler ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure akış analizi</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Hdınsight üzerinde Apache Storm</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Geç varış ve sıra dışı olay işleme</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Yerleşik yapılandırılabilir ilkeleri olayları yeniden sıralamak, olayları bırakın veya olay zamanını ayarlayın.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kullanıcılar bu senaryo işlemek için mantığı uygulamanız gerekir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Başvuru verileri</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Başvuru verileri Azure Blob Depolama en fazla 100 MB bellek içi önbellek ile kullanılabilir. Başvuru verileri, hizmet tarafından yenilenir.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Veri boyutu sınırı. Bağlayıcılar, HBase, Azure Cosmos DB, SQL Server ve Azure için kullanılabilir. Kullanıcıların özel kod kullanarak ek bağlayıcı oluşturabilirsiniz. Başvuru verileri için özel kod kullanarak yenilenmesi gerekir.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Machine Learning ile tümleştirme</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure Machine Learning modellerini işlevleri işi oluşturma sırasında yapılandırılabilir yayımladı. Daha fazla bilgi için bkz: <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Machine Learning işlevleri için ölçek</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm Cıvatalar üzerinden kullanılabilir.
                </p>
            </td>
        </tr>
    </tbody>
</table>
