---
title: Azure Hizmetleri için Klasik uyarıları oluşturmak için platformlar arası Azure CLI kullanın | Microsoft Docs
description: E-postalar veya bildirimleri tetiklemek veya belirttiğiniz koşullar karşılandığında Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/24/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 8112b868bc8d2ca2a9478d38ee702d8b3350d48e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287121"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Klasik ölçüm uyarılar için Azure Hizmetleri Azure İzleyicisi'nde oluşturmak için platformlar arası Azure CLI kullanma 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>
> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni, daha iyi ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md). Bu uyarılar, birden çok ölçümleri izleyin ve boyutlu ölçümleri uyarmak için izin verebilirsiniz. Yeni ölçüm uyarılar için Azure CLI desteği yakında geliyor.
>
>

Bu makalede, platformlar arası komut satırı arabirimi (Azure CLI) kullanarak Azure Klasik ölçüm uyarıları ayarlamak nasıl gösterilmektedir.

> [!NOTE]
> Azure İzleyicisi "Azure Öngörüler" olarak adlandırılmıştı için yeni 25 Eylül 2016'ya kadar adıdır. Ancak, ad alanları ve bu nedenle burada açıklanan komutları hala "Öngörüler." word içeriyor

Azure hizmetlerinizi ölçümleri temelinde veya Azure içinde gerçekleşen olaylara dayanarak bir uyarı alabilirsiniz.

* **Ölçüm değerleri**: herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler zaman ilk koşul ve ardından zaman bu koşulu artık karşılanmıyor.    

* **Etkinlik günlüğü olaylarını**: bir uyarıyı tetiklemek *her* olay veya belirli olaylar olduğunda. Etkinlik günlükleri hakkında daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları (Klasik) oluşturmak](monitoring-activity-log-alerts.md). 

Tetikler, aşağıdakileri yapmak için Klasik bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönderin. 
* Belirttiğiniz e-posta için e-posta adresleri gönderin.
* Bir Web kancası çağırın.
* (Yalnızca Azure portalından şu anda) Azure bir runbook'un yürütülmesi başlatın.

Yapılandırma ve aşağıdakileri kullanarak Klasik ölçüm uyarı kuralları hakkında bilgi alın: 

* [Azure portalı](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure CLI](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Bir komutu yazarak komutlar için Yardım alabilirsiniz **-Yardım** sonunda. Aşağıda bir örnek verilmiştir: 

```console
 azure insights alerts -help
 azure insights alerts actions email create -help
 ```

## <a name="create-alert-rules-by-using-azure-cli"></a>Azure CLI kullanarak uyarı kuralları oluşturma
1. Önkoşulları yükledikten sonra Azure'da oturum açın. Bkz: [Azure İzleyici CLI örnekleri](insights-cli-samples.md) başlamak için ihtiyacınız komutlar için. Bu komutlar imzalı, kullanmakta olduğunuz hangi aboneliğe Göster ve Azure İzleyici komutları çalıştırmak için hazırlamanız yardımcı olur.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Bir kaynak grubu üzerinde mevcut kurallar listelemek için aşağıdaki biçimi kullanın: 

   **Uyarı kuralı listesi azure Öngörüler** *[Seçenekler] &lt;kaynak grubu&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Bir kural oluşturmak için birkaç önemli bilgi parçasından önce olması gerekir.
    * **Kaynak kimliği** için uyarı ayarlamak istediğiniz kaynak için.
    * **Ölçüm tanımlarını** bu kaynak için kullanılabilir.

     Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğunu varsayarsak Portalı'nda seçin. Ardından, sonraki dikey penceresinde de **ayarları** bölümünde, select **özellikleri**. **Kaynak Kimliği** sonraki dikey bir alandır. 
     
     Kaynak kimliği kullanarak da elde edebilirsiniz [Azure kaynak Gezgini](https://resources.azure.com/).

     Bir web uygulaması için bir örnek kaynak kimliği aşağıdadır:

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Önceki örnekte kaynak ölçümleri için kullanılabilir ölçümlere ve birimlerin listesini almak için aşağıdaki Azure CLI komutu kullanın:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* kullanılabilir ölçüm (1 dakikalık aralıklarla) ayrıntı kalıyor. Farklı ayrıntı kullandığınızda, farklı ölçüm seçeneğiniz vardır.
     
4. Ölçüm tabanlı bir uyarı kuralı oluşturmak için aşağıdaki biçimde bir komut kullanın:

    **Uyarı kuralı ölçüm kümesi azure Öngörüler** *[Seçenekler] &lt;ruleName&gt; &lt;konumu&gt; &lt;resourceGroup&gt; &lt;pencereboyutu&gt; &lt;işleci&gt; &lt;eşik&gt; &lt;uç noktası Targetresourceıd&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    Aşağıdaki örnek bir Web sitesi kaynakta bir uyarı ayarlar. 5 dakikada bir ve yeniden zaman 5 dakika boyunca hiçbir trafik aldığı için herhangi bir trafik tutarlı bir şekilde aldığında uyarı tetikler.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Bir Web kancası oluşturma veya Klasik bir ölçüm uyarı oluşturulduğunda bir e-posta göndermek için önce e-posta veya Web kancası oluşturun. Kural hemen oluşturmak daha sonra. Web kancası veya e-postaları önceden oluşturduğunuz kurallar ile ilişkilendiremezsiniz.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Uyarılarınızı düzgün bir şekilde tek bir kuralı bakarak oluşturulduğunu doğrulayabilirsiniz.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Kural silmek için aşağıdaki biçimde bir komut kullanın:

    **Uyarı kuralı silme Öngörüler** [Seçenekler] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Bu komutlar, bu makalede daha önce oluşturulan kuralları silin.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md), toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Otomasyon çalışma kitabı](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlüklerinin toplanması genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetiniz için ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
