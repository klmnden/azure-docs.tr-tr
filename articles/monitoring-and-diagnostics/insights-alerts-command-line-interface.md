---
title: "Azure Hizmetleri - platformlar arası CLI için uyarı oluşturma | Microsoft Docs"
description: "Belirttiğiniz koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: 92246a8da73a244a1c9a924bed55711d71a20fd8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a>Ölçüm uyarılar için Azure services - Azure İzleyicisi'nde, platformlar arası CLI oluşturabilir.
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Genel Bakış
Bu makalede platformlar arası komut satırı arabirimi (CLI) kullanarak Azure ölçüm uyarılarını ayarlama gösterilmiştir.

> [!NOTE]
> Azure İzleyicisi "Azure Öngörüler" olarak adlandırılmıştı için yeni 25 Eylül 2016'ya kadar adıdır. Bununla birlikte, ad alanları ve bu nedenle aşağıdaki komutları yine "ınsights" içerir.
>
>

İzleme ölçümlerini ya da olayları, Azure hizmetlerinizi göre bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler koşul ilk ve ardından daha sonra ne zaman, koşul artık karşılanıp zaman.    
* **Etkinlik günlüğü olaylarını** -bir uyarıyı tetiklemek *her* olay veya yalnızca belirli bir olaylar oluşur. Etkinlik günlüğü Uyarıları hakkında daha fazla bilgi edinmek için [burayı tıklatın](monitoring-activity-log-alerts.md)

Tetikler, aşağıdakileri yapmak için bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* bir Web kancası çağırın
* (yalnızca Azure portalından şu anda) Azure bir runbook'un yürütülmesi Başlat

Yapılandırma ve kullanma ölçüm uyarı kuralları hakkında bilgi alın

* [Azure portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Bir komut yazarak komutlar için Yardım her zaman alabilir ve koyma - sonunda yardımcı olur. Örneğin:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>CLI kullanarak uyarı kuralları oluşturma
1. Azure oturum açma ve önkoşullar gerçekleştirin. Bkz: [Azure İzleyici CLI örnekleri](insights-cli-samples.md). Kısacası, CLI'yı yükledikten ve bu komutları çalıştırın. Bunlar, oturum açmış almak için kullanmakta olduğunuz ve Azure İzleyici komutları çalıştırmak için hazırlamanız hangi aboneliğe göster.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Bir kaynak grubu üzerinde mevcut kurallar listelemek için aşağıdaki formu kullanın **uyarıları kural listesi azure Öngörüler** *[Seçenekler] &lt;kaynak grubu&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Bir kural oluşturmak için birkaç önemli bilgi parçasından önce olması gerekir.
  * **Kaynak kimliği** için uyarı ayarlamak istediğiniz kaynak için
  * **Ölçüm tanımlarını** kaynağı için kullanılabilir

     Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğu varsayılarak, portalda seçin. Sonraki dikey penceresinde, ardından *özellikleri* altında *ayarları* bölümü. *Kaynak kimliği* sonraki dikey bir alandır. Başka bir yolu kullanmaktır [Azure kaynak Gezgini](https://resources.azure.com/).

     Bir web uygulaması için bir örnek kaynak kimliği

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Bu ölçümleri önceki kaynak örnek için kullanılabilir ölçümlere ve birimlerin listesini almak için aşağıdaki CLI komutu kullanın:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* (1 dakikalık aralıklarla) kullanılabilir ölçü ayrıntı kalıyor. Farklı ayrıntı kullanarak farklı ölçüm seçenekler sunar.
4. Ölçüm tabanlı bir uyarı kuralı oluşturmak için aşağıdaki biçimde bir komut kullanın:

    **Uyarı kuralı ölçüm kümesi azure Öngörüler** *[Seçenekler] &lt;ruleName&gt; &lt;konumu&gt; &lt;resourceGroup&gt; &lt;pencereboyutu&gt; &lt;işleci&gt; &lt;eşik&gt; &lt;uç noktası Targetresourceıd&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    Aşağıdaki örnek bir web sitesi kaynakta bir uyarı ayarlar. 5 dakikada bir ve yeniden zaman 5 dakika boyunca hiçbir trafik aldığı için herhangi bir trafik tutarlı bir şekilde aldığında uyarı tetikler.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Web kancası oluşturma veya ölçüm bir uyarı oluşturulduğunda e-posta göndermek için önce e-posta ve/veya Web kancası oluşturun. Kural hemen oluşturmak daha sonra. CLI kullanarak kurallar önceden oluşturulmuş Web kancası veya e-posta ilişkilendiremezsiniz.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Uyarılarınızı düzgün bir şekilde tek bir kuralı bakarak oluşturulduğunu doğrulayabilirsiniz.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Kural silmek için formun komutu kullanın:

    **Uyarı kuralı silme Öngörüler** [Seçenekler] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Bu komutlar, bu makalede daha önce oluşturduğunuz kurallar silin.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md) toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlüklerinin toplanması genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
