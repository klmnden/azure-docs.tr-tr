---
title: Cloud Foundry izlemek için Azure Log Analytics Nozzle dağıtma | Microsoft Docs
description: Azure Log Analytics için Cloud Foundry loggregator Nozzle dağıtma konusunda adım adım yönergeler. Başlık, Cloud Foundry sistem durumu ve performans ölçümlerini izlemek için kullanın.
services: virtual-machines-linux
author: ningk
manager: jeconnoc
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: 6220aebdef6970f3d5f7017e4ae48f6f409ae0ce
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199406"
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Cloud Foundry sistemin izlenmesi için Azure Log Analytics Nozzle dağıtma

[Azure İzleyici](https://azure.microsoft.com/services/log-analytics/) bir Azure hizmetidir. Bulutunuzdan oluşturulur ve şirket içi Ortamlarınızdaki verileri toplayıp analiz yardımcı olur.

Log Analytics Nozzle (başlık) ölçümleri ileten bir Cloud Foundry (CF) bileşeni olduğu [Cloud Foundry loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) Azure İzleyici günlüklerine firehose. Nozzle ile toplamak, görüntüleyebilir ve birden çok dağıtımlarınızda CF sistem durumu ve performans ölçümlerinizi, analiz edin.

Bu belgede, CF ortamınıza Nozzle dağıtma ve Azure İzleyici günlüklerine konsoldan ardından verilere öğrenin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki adımlar, Nozzle dağıtmak için önkoşullardır.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Azure'daki CF veya Pivotal Cloud Foundry ortamı dağıtma

Bir açık kaynak CF dağıtım veya Pivotal Cloud Foundry (PCF) dağıtım ile Nozzle kullanabilirsiniz.

* [Azure'da cloud Foundry dağıtma](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Azure'da Pivotal Cloud Foundry dağıtma](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Nozzle dağıtma CF komut satırı araçlarını yükleyin

Nozzle CF ortamında bir uygulama olarak çalışır. CF uygulamasını dağıtmak için CLI ihtiyacınız vardır.

Nozzle loggregator firehose ve Bulutu denetleyicisi erişim izni de gerekir. Oluşturma ve yapılandırma kullanıcı için kullanıcı hesabı ve kimlik doğrulaması (UAA) hizmeti gerekir.

* [Cloud Foundry CLI yükleme](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Cloud Foundry UAA komut satırı istemcisini yükleme](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

UAA komut satırı istemci ayarlamadan önce RubyGems yüklü olduğundan emin olun.

### <a name="3-create-a-log-analytics-workspace-in-azure"></a>3. Azure'da Log Analytics çalışma alanı oluşturma

El ile veya bir şablon kullanarak Log Analytics çalışma alanı oluşturabilirsiniz. Şablon, bir önceden yapılandırılmış KPI görünümler ve uyarılar Azure İzleyici günlüklerine Konsolu Kurulumu dağıtır. 

#### <a name="to-create-the-workspace-manually"></a>Çalışma alanını el ile oluşturmak için:

1. Azure portalında Azure Marketi'nde Hizmetler listesinde arama yapın ve sonra Log Analytics çalışma alanı seçin.
2. Seçin **Oluştur**ve ardından şu öğeler için seçim:

   * **Log Analytics çalışma alanı**: Çalışma alanınız için bir ad yazın.
   * **Abonelik**: CF dağıtımınız ile aynı olan birden fazla aboneliğiniz varsa, seçin.
   * **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya aynı CF dağıtımınıza kullanın.
   * **Konum**: Konumu girin.
   * **Fiyatlandırma katmanı**: Seçin **Tamam** tamamlanması.

Daha fazla bilgi için [Azure İzleyici günlüklerine ile çalışmaya başlama](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

#### <a name="to-create-the-log-analytics-workspace-through-the-monitoring-template-from-azure-market-place"></a>Azure market yerden izleme şablonu ile Log Analytics çalışma alanı oluşturmak için:

1. Azure Portalı'nı açın.
1. "+" İşaretine tıklayın veya "kaynak sol üst köşedeki oluştur".
1. Arama penceresine "Cloud Foundry" yazın, "Cloud Foundry izleme çözümü" seçin.
1. İzleme çözümü şablonu ön sayfa yüklendiğinde, Cloud Foundry şablonu dikey penceresini başlatmak için "Oluştur"'a tıklayın.
1. Gerekli parametreleri girin:
    * **Abonelik**: Log Analytics çalışma alanı için genellikle aynı Cloud Foundry dağıtım ile bir Azure aboneliği seçin.
    * **Kaynak grubu**: Mevcut bir kaynak grubunu seçin veya yeni bir Log Analytics çalışma alanı oluşturun.
    * **Kaynak grubu konumu**: Kaynak grubunun konumunu seçin.
    * **OMS_Workspace_Name**: Bir çalışma alanı adı girin. çalışma alanı yoksa, şablonun yeni bir tane oluşturun.
    * **OMS_Workspace_Region**: Çalışma alanı konumunu seçin.
    * **OMS_Workspace_Pricing_Tier**: SKU Log Analytics çalışma alanı seçin. Bkz: [fiyatlandırma Kılavuzu](https://azure.microsoft.com/pricing/details/log-analytics/) başvuru.
    * **Yasal koşullar**: Yasal Koşulları'nı tıklatın ve yasal terimi kabul etmek için "Oluştur" a tıklayın.
1. Tüm parametreler belirttikten sonra şablonu dağıtmak için "Oluştur" a tıklayın. Dağıtım tamamlandığında durum bildirim sekme görünür.


## <a name="deploy-the-nozzle"></a>Nozzle dağıtma

Birkaç Nozzle dağıtmanın farklı yöntemleri vardır: CF uygulama olarak ya da PCF kutucuk olarak.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>PCF Ops Manager kutucuk olarak Nozzle dağıtma

Adımlarını izleyin [yükleme ve Azure Log Analytics Nozzle PCF için yapılandırma](https://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html). Bu basitleştirilmiş bir yaklaşım, PCF Ops manager kutucuk otomatik olarak yapılandıracak ve nozzle anında iletme. 

### <a name="deploy-the-nozzle-manually-as-a-cf-application"></a>Nozzle CF uygulaması olarak el ile dağıtma

PCF Ops Manager kullanmıyorsanız Nozzle bir uygulama olarak dağıtın. Aşağıdaki bölümlerde, bu işlemi açıklanmaktadır.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>CF dağıtımınıza CF CLI ile bir yönetici olarak oturum açın

Şu komutu çalıştırın:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

"SYSTEM_DOMAIN", CF etki alanı adıdır. Bu "SYSTEM_DOMAIN" arayarak, CF dağıtım bildirim dosyasında alabilirsiniz. 

"CF_User" CF yönetici addır. Adını ve parolasını almak "scım" bölümünde arama, adı ve CF dağıtım bildirim dosyasının "cf_admin_password" için arama.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>CF kullanıcı oluşturma ve gerekli ayrıcalıkları verme

Aşağıdaki komutları çalıştırın:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

"SYSTEM_DOMAIN", CF etki alanı adıdır. Bu "SYSTEM_DOMAIN" arayarak, CF dağıtım bildirim dosyasında alabilirsiniz.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>Log Analytics Nozzle en son sürümü indirin

Şu komutu çalıştırın:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Şimdi geçerli dizininizde manifest.yml dosyasında ortam değişkenlerini ayarlayabilirsiniz. Uygulama bildirimi Nozzle için aşağıda gösterilmiştir. Değerleri, belirli Log Analytics çalışma alanı bilgileri ile değiştirin.

```
OMS_WORKSPACE             : Log Analytics workspace ID: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_KEY                   : OMS key: Open your Log Analytics workspace in the Azure portal, select **Advanced settings**, select **Connected Sources**, and select **Windows Servers**.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to Azure Monitor logs. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to Azure Monitor logs. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to Azure Monitor logs. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to Azure Monitor logs as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to Azure Monitor logs. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>Uygulamanızı geliştirme bilgisayarınızın uygulamadan anında iletme

Oms-log-analytics-firehose-nozzle klasörü altında olduğundan emin olun. Şu komutu çalıştırın:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Nozzle yüklemeyi doğrulama

### <a name="from-apps-manager-for-pcf"></a>Uygulamaları Manager'dan (PCF için)

1. OPS Manager için oturum açın ve kutucuk yükleme Panoda görüntülendiğinden emin olun.
2. Uygulama Yöneticisi'ni açın, üzerinde kullanım raporu oluşturduğunuz için başlık alanı listelendiğinden emin olun ve durum normal olduğundan emin olun.

### <a name="from-your-development-computer"></a>Geliştirme bilgisayarınızdan

CF CLI penceresinde yazın:
```
cf apps
```
OMS Nozzle uygulamayı çalışır durumda olduğundan emin olun.

## <a name="view-the-data-in-the-azure-portal"></a>Azure portalında veri görüntüleme

İzleme çözümü Marketi şablon aracılığıyla dağıttıysanız, Azure portalına gidin ve çözüm bulun. Çözüm, şablonda belirtilen kaynak grubunda bulabilirsiniz. Çözüm'e tıklayın, göz atın "log analytics konsola" önceden yapılandırılmış görünümler, üst Cloud Foundry sistem KPI'ları, uygulama verileri, uyarıları ve VM sistem durumu ölçümleri listelenir. 

Log Analytics çalışma alanını el ile oluşturduysanız, görünümler ve Uyarılar oluşturmak için aşağıdaki adımları izleyin:

### <a name="1-import-the-oms-view"></a>1. OMS görünüm alma

OMS Portalı'ndan göz atın **Görünüm Tasarımcısı** > **alma** > **Gözat**, omsview dosyalarından birini seçin. Örneğin, *bulut Foundry.omsview*ve görünüm kaydedebilirsiniz. Artık bir kutucuk görüntülenir **genel bakış** sayfası. Görselleştirilen ölçümleri görmek için seçin.

Bu görünümler özelleştirme veya aracılığıyla yeni görünümler oluşturmak **Görünüm Tasarımcısı**.

*"Bulut Foundry.omsview"* Cloud Foundry OMS görünüm şablonu, bir önizleme sürümüdür. Bu tam olarak yapılandırılmış bir varsayılan şablonudur. Önerileriniz veya geri bildirim şablonuyla ilgili varsa bunları gönderebilirsiniz [sorun bölüm](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues).

### <a name="2-create-alert-rules"></a>2. Uyarı kuralları oluşturma

Yapabilecekleriniz [uyarıları oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts)ve sorgular ve eşik değerleri gereken şekilde özelleştirin. Aşağıdaki uyarılar önerilir:

| Arama sorgusu                                                                  | Bağlı olarak uyarı oluştur | Açıklama                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Sonuçları < 1 sayısı   | **BBS. Domain.cf uygulamaları** cf uygulama etki alanı güncel olup olmadığını gösterir. CF uygulamasını isteklerinden bulut denetleyicisi için bbs eşitlenir anlamına gelir. Yürütme için LRPsDesired (AIS Diego istenen). Hiç veri alınmadı cf uygulama etki alanı belirtilen zaman penceresinde güncel değil anlamına gelir. |
| Tür CF_ValueMetric_CL Origin_s = temsilcisi Name_s = UnhealthyCell Value_d = > 1            | Sonuçları > 0 sayısı   | Diego hücreler için 0 sağlıklı anlamına gelir ve 1 sağlıksız anlamına gelir. Belirtilen bir zaman penceresinde birden çok kötü Diego hücre algılanmazsa uyarı ayarlama. |
| Type=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Sonuçları > 0 sayısı | 1, sistemin iyi durumda ve sistemin iyi durumda değil 0 anlamına gelir anlamına gelir. |
| Tür CF_ValueMetric_CL Origin_s = route_emitter Name_s = ConsulDownMode Value_d = > 0 | Sonuçları > 0 sayısı   | Consul düzenli aralıklarla sistem durumunu gösterir. 0, sistemin iyi durumda ve rota verici Consul kapalı olduğunu algılar 1 anlamına gelir anlamına gelir. |
| Tür CF_CounterEvent_CL Origin_s = ("Name_s="TruncatingBuffer.DroppedMessages veya Name_s="doppler.shedEnvelopes") DopplerServer Delta_d = > 0 | Sonuçları > 0 sayısı | Kasıtlı olarak Doppler tarafından ters baskı denen nedeniyle bırakılan ileti değişim sayısı. |
| Tür CF_LogMessage_CL SourceType_s = LGR MessageType_s = hata =                      | Sonuçları > 0 sayısı   | Loggregator yayan **LGR** günlüğe kaydetme işlemi ile ilgili sorunlar belirtmek için. Günlük ileti çıktısını çok fazla olduğunda, ilgili bir sorun örneğidir. |
| Tür CF_ValueMetric_CL Name_s = slowConsumerAlert =                               | Sonuçları > 0 sayısı   | Nozzle loggregator yavaş tüketici uyarı aldığında, gönderdiği **slowConsumerAlert** ValueMetric Azure İzleyici için günlüğe kaydeder. |
| Type=CF_CounterEvent_CL Job_s=nozzle Name_s=eventsLost Delta_d>0              | Sonuçları > 0 sayısı   | Kayıp olayları delta sayısı bir eşiği ulaşırsa Nozzle çalıştırılırken bir sorun olabilir anlamına gelir. |

## <a name="scale"></a>Ölçek

Başlık ve loggregator ölçeklendirebilirsiniz.

### <a name="scale-the-nozzle"></a>Ölçek Nozzle

En az iki örneğini Nozzle ile başlamanız gerekir. Firehose Nozzle tüm örneklerinde iş yükü dağıtır.
Nozzle tutabilir veri trafiği firehose ile emin olmak için ayarlama **slowConsumerAlert** uyarı ("uyarı kuralları oluşturma" önceki bölümde listelenen). Uyarı sonra yönergeleri takip ederek [yavaş Nozzle kılavuzunu](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) ölçeklendirme gerekli olup olmadığını belirlemek için.
Nozzle ölçeklendirmek için örnek sayılar veya bellek veya disk kaynakları için Nozzle artırmak için uygulamaları Yöneticisi veya CF CLI'ı kullanın.

### <a name="scale-the-loggregator"></a>Ölçek loggregator

Loggregator gönderen bir **LGR** günlük ileti günlüğe kaydetme işlemi ile ilgili sorunlar belirtmek için. Loggregator ölçeği gerekip gerekmediğini belirlemek için uyarı izleyebilirsiniz.
Loggregator ölçeklendirmek için Doppler arabellek boyutunu artırın veya ek Doppler sunucu örnekleri CF bildiriminde ekleyin. Daha fazla bilgi için [loggregator ölçeklendirme Kılavuzu](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Güncelleştirme

Daha yeni bir sürümle Nozzle güncelleştirmek için yeni Nozzle sürümünü indirin, yukarıdaki "Nozzle dağıtma" bölümündeki adımları izleyin ve uygulamayı yeniden gönderin.

### <a name="remove-the-nozzle-from-ops-manager"></a>İşlem Yöneticisi'nden Nozzle Kaldır

1. Ops Manager için oturum açın.
2. Bulun **PCF için Microsoft Azure Log Analytics Nozzle** Döşe.
3. Çöp simgesini seçin ve silme işlemini onaylayın.

### <a name="remove-the-nozzle-from-your-development-computer"></a>Geliştirme bilgisayarınızdan Nozzle

CF CLI penceresinde şunu yazın:
```
cf delete <App Name> -r
```

OMS portalında veri Nozzle kaldırırsanız, otomatik olarak kaldırılmaz. Bağlı olarak ayarlama, Azure İzleyici günlüklerine bekletme süresi dolar.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure Log Analytics Nozzle olarak açık kaynaklı. Sorular ve geri bildirim gönder [GitHub bölümü](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Azure destek isteği açmak için "Cloud Foundry çalıştıran sanal makine" hizmet kategorisi seçin. 

## <a name="next-step"></a>Sonraki adım

PCF2.0 VM performans ölçümlerini Azure Log Analytics nozzle için sistem ölçümlerini ileticisini aktarılan ve Log Analytics çalışma alanınıza tümleşik. Artık VM performans ölçümleri için Log Analytics aracısını gerekir. Ancak Log Analytics aracısını Syslog bilgilerini toplamak için kullanmaya devam edebilirsiniz. Log Analytics aracısını CF vm'lere bir Bosh eklentisi olarak yüklenir. 

Ayrıntılar için bkz [dağıtma Log Analytics aracısını Cloud Foundry dağıtımınıza](https://github.com/Azure/oms-agent-for-linux-boshrelease).
