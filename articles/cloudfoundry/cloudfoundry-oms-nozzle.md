---
title: Bulut Foundry izlemek için Azure günlük analizi kafa dağıtma | Microsoft Docs
description: Azure günlük analizi için bulut Foundry loggregator kafa dağıtma hakkında adım adım yönergeler için. Başlık bulut Foundry sistem sağlık ve performans ölçümlerini izlemek için kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: ningk
manager: timlt
editor: ''
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: b900a42196eedab89af8e55d71a336ed7adc45a4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Bulut Foundry sistem izleme için Azure günlük analizi kafa dağıtma

[Azure günlük analizi](https://azure.microsoft.com/services/log-analytics/) bir Azure hizmetidir. Toplamak ve analiz etmek, buluttan oluşturulur ve şirket içi ortamları verileri yardımcı olur.

Günlük analizi başlık (başlık) ölçümleri ileten bir bulut Foundry (CF) bileşeni olan [bulut Foundry loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) firehose günlük analizi için. Başlık ile toplamak, görüntülemek ve, CF sistem sağlık ve performans ölçümleri, birden çok dağıtımlar arasında analiz edin.

Bu belgede, kafa CF ortamınıza dağıtmak ve ardından veri günlük analizi konsolundan erişim hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Başlık dağıtmak için Önkoşullar adımlardır.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Azure CF veya Bileşendirler bulut Foundry bir ortamda dağıtmak

Başlık bir açık kaynak CF dağıtımı veya Bileşendirler bulut Foundry (PCF) dağıtım ile kullanabilirsiniz.

* [Azure üzerinde bulut Foundry dağıtma](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Azure üzerinde Bileşendirler bulut Foundry dağıtma](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Başlık dağıtmak için CF komut satırı araçları'nı yükleme

Başlık CF ortamında bir uygulama olarak çalışır. Uygulamayı dağıtmak için CF CLI gerekir.

Başlık da loggregator firehose ve bulut denetleyicisi için erişim izni gerekir. Oluşturma ve kullanıcı yapılandırmak için kullanıcı hesabı ve kimlik doğrulaması (UAA) hizmeti gerekir.

* [Bulut Foundry CLI yükleme](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Bulut Foundry UAA komut satırı İstemcisi'ni yükleme](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

UAA komut satırı istemci ayarlamadan önce Rubygems yüklü olduğundan emin olun.

### <a name="3-create-a-log-analytics-workspace-in-azure"></a>3. Günlük analizi çalışma alanı oluşturma

Günlük analizi çalışma alanı el ile veya bir şablon kullanarak oluşturabilirsiniz. Başlık dağıtım işlemini tamamladıktan sonra önceden yapılandırılmış OMS görünümleri ve Uyarıları yükleyin.

Çalışma alanı el ile oluşturmak için:

1. Azure portalında Azure Marketi'nde hizmetlerin listesini arama ve günlük analizi seçin.
2. Seçin **oluşturma**ve ardından aşağıdaki öğeler için seçenekleri seçin:

   * **OMS çalışma**: çalışma alanınız için bir ad yazın.
   * **Abonelik**: CF dağıtımınız ile aynı olan birden çok aboneliğiniz varsa, seçin.
   * **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya aynı CF dağıtımınızla birlikte kullanın.
   * **Konum**: konumu girin.
   * **Fiyatlandırma katmanı**: seçin **Tamam** tamamlamak için.

Daha fazla bilgi için bkz: [günlük Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

Alternatif olarak, günlük analizi çalışma alanını OMS şablonunu kullanarak oluşturabilirsiniz. Bu yöntemde, şablon önceden yapılandırılmış OMS görünümleri ve uyarılar otomatik olarak yükler. Daha fazla bilgi için bkz: [bulut Foundry için Azure günlük analizi çözümü](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-cloudfoundry-solution).

## <a name="deploy-the-nozzle"></a>Başlık dağıtma

Birkaç kafa dağıtmanın farklı yolu vardır: PCF döşeme veya CF uygulama olarak.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>Başlık PCF Ops Manager kutucuğu dağıtma

Ops Manager kullanarak PCF dağıttıktan sonra adımlarını izleyin. [kafa PCF için yükleyip](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html). Başlık Ops Manager ile bir kutucuk olarak yüklenir.

### <a name="deploy-the-nozzle-as-a-cf-application"></a>Başlık CF uygulama dağıtma

Başlık PCF Ops Manager kullanmıyorsanız, bir uygulama olarak dağıtın. Aşağıdaki bölümlerde bu işlemi açıklanmaktadır.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>CF dağıtımınızı CF CLI aracılığıyla yönetici olarak oturum açın

Şu komutu çalıştırın:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

"SYSTEM_DOMAIN", CF etki alanı adıdır. Bunu "SYSTEM_DOMAIN" arayarak, CF dağıtım bildirim dosyasında alabilirsiniz. 

"CF_User" CF yönetici adıdır. Adı ve parola almak "scım'yi" bölümünde arama, adı ve CF dağıtım bildirim dosyası "cf_admin_password" için arama.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>CF kullanıcı oluşturmak ve gerekli ayrıcalıkları

Aşağıdaki komutları çalıştırın:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

"SYSTEM_DOMAIN", CF etki alanı adıdır. Bunu "SYSTEM_DOMAIN" arayarak, CF dağıtım bildirim dosyasında alabilirsiniz.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>En son günlük analizi başlık sürümü indirme

Şu komutu çalıştırın:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Şimdi, geçerli dizininizde manifest.yml dosyasında ortam değişkenleri ayarlayabilirsiniz. Başlık uygulama bildirimi gösterir. Değerleri, belirli günlük analizi çalışma alanı bilgileriyle değiştirin.

```
OMS_WORKSPACE             : Log Analytics workspace ID: open OMS portal from your Log Analytics workspace, select Settings, and select connected sources.
OMS_KEY                   : OMS key: open OMS portal from your Log Analytics workspace, select Settings, and select connected sources.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to Log Analytics. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to Log Analytics. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to Log Analytics. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to Log Analytics as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to Log Analytics. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>Uygulamanızı geliştirme bilgisayarınızın uygulamadan anında iletme

Oms-günlük-Analytics'i-firehose-başlık klasörü altında olduğundan emin olun. Şu komutu çalıştırın:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Başlık yüklemeyi doğrulama

### <a name="from-apps-manager-for-pcf"></a>Uygulamaları Manager'dan (için PCF)

1. OPS Manager için oturum açın ve döşeme yükleme Panoda görüntülendiğinden emin olun.
2. Uygulamaları Yöneticisi için oturum açın, başlık için oluşturduğunuz alan kullanımı raporu listelendiğinden emin olun ve durum normal olduğundan emin olun.

### <a name="from-your-development-computer"></a>Geliştirme bilgisayarınızdan

CF CLI penceresinde yazın:
```
cf apps
```
OMS kafa uygulama çalıştığından emin olun.

## <a name="view-the-data-in-the-oms-portal"></a>OMS Portalı'nda verileri görüntüleme

### <a name="1-import-the-oms-view"></a>1. OMS görünümünü Al

OMS Portalı'ndan Gözat **Görünüm Tasarımcısı** > **alma** > **Gözat**ve omsview dosyalarından birini seçin. Örneğin, seçin *bulut Foundry.omsview*ve görünümü kaydedin. Bir kutucuk gösterilir artık **genel bakış** sayfası. Görselleştirilmiş ölçümlerini görmek için seçin.

Bu görünümler özelleştirebilir veya aracılığıyla yeni görünümler oluşturma **Görünüm Tasarımcısı**.

*"Bulut Foundry.omsview"* bulut Foundry OMS görünüm şablonu Önizleme sürümüdür. Bu tam olarak yapılandırılmış bir varsayılan şablonudur. Öneriniz ya da şablon hakkında geri bildirim varsa, bunları göndermeniz [sorun bölüm](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues).

### <a name="2-create-alert-rules"></a>2. Uyarı kuralları oluşturma

Yapabilecekleriniz [uyarı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts), sorgular ve ve eşik değerleri gerektiği gibi özelleştirin. Aşağıdaki uyarıları önerilir:

| Arama sorgusu                                                                  | Şuna bağlı olarak uyarı oluştur: | Açıklama                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Sonuçları < 1 sayısı   | **BBS. Domain.cf uygulamaları** cf uygulamaları etki alanı güncel olup olmadığını gösterir. Bu, bulut denetleyicisi CF uygulama isteklerinden bbs için eşitlenir anlamına gelir. Yürütme için LRPsDesired (AIS Diego istenen). Alınan veri cf uygulamaları etki alanı belirtilen zaman penceresinde güncel değil anlamına gelir. |
| Type=CF_ValueMetric_CL Origin_s=rep Name_s=UnhealthyCell Value_d>1            | Sonuçları > 0 sayısı   | Diego hücreler için 0 sağlıklı anlamına gelir ve 1 sağlıksız anlamına gelir. Belirtilen zaman penceresi için birden fazla sağlıksız Diego hücre algılanmazsa uyarı ayarlayın. |
| Type=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Sonuçları > 0 sayısı | 1 sağlıklı bir sistemidir ve sistem sağlıklı değil 0 anlamına anlamına gelir. |
| Type=CF_ValueMetric_CL Origin_s=route_emitter Name_s=ConsulDownMode Value_d>0 | Sonuçları > 0 sayısı   | Consul düzenli aralıklarla sistem durumunu gösterir. 0 anlamına gelir sağlıklı bir sistemidir ve rota verici Consul kapalı olduğunu algılar 1 anlamına gelir. |
| Type=CF_CounterEvent_CL Origin_s=DopplerServer (Name_s="TruncatingBuffer.DroppedMessages" or Name_s="doppler.shedEnvelopes") Delta_d>0 | Sonuçları > 0 sayısı | Delta bilerek Doppler tarafından geri baskısı nedeniyle bırakılan ileti sayısı. |
| Type=CF_LogMessage_CL SourceType_s=LGR MessageType_s=ERR                      | Sonuçları > 0 sayısı   | Loggregator yayar **LGR** günlüğe kaydetme işlemi sorunları belirtmek için. Günlük iletisi çıkış çok yüksek olduğunda bir sorun bir örnektir. |
| Type=CF_ValueMetric_CL Name_s=slowConsumerAlert                               | Sonuçları > 0 sayısı   | Başlık loggregator yavaş tüketici uyarı aldığında, gönderdiği **slowConsumerAlert** ValueMetric günlük analizi için. |
| Type=CF_CounterEvent_CL Job_s=nozzle Name_s=eventsLost Delta_d>0              | Sonuçları > 0 sayısı   | Kayıp Olay delta sayısı eşiği ulaşırsa, başlık çalıştıran bir sorun olabilecek anlamına gelir. |

## <a name="scale"></a>Ölçek

Başlık ve loggregator ölçeklendirebilirsiniz.

### <a name="scale-the-nozzle"></a>Ölçeği başlık

Başlık en az iki örnek ile başlamanız gerekir. Firehose kafa tüm örneklerinde iş yükü dağıtır.
Başlık tutabilir firehose veri trafiği ile emin olmak için ayarlanan **slowConsumerAlert** uyarı ("uyarı kuralları oluşturmak" önceki bölümde listelenen). Uyarı sonra izleyin [yavaş kafa yönelik yönergeler](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) ölçeklendirme gerekli olup olmadığını belirlemek için.
Başlık ölçeklendirmek için örnek sayı veya bellek veya disk kaynakları için Kafa artırmak için uygulamaları Yöneticisi veya CF CLI kullanın.

### <a name="scale-the-loggregator"></a>Ölçek loggregator

Loggregator gönderen bir **LGR** günlüğü iletisi günlüğe kaydetme işlemi sorunları gösterir. Loggregator ölçeklendirilmiş gerekip gerekmediğini belirlemek için uyarı izleyebilirsiniz.
Loggregator ölçeklendirmek için Doppler arabellek boyutunu artırabilir veya ek Doppler sunucu örnekleri CF bildiriminde ekleyebilirsiniz. Daha fazla bilgi için bkz: [loggregator ölçeklendirmeye yönelik bir yönerge](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Güncelleştirme

Başlık daha yeni bir sürümüyle güncelleştirmek için yeni başlık sürümünü indirin, yukarıdaki "kafa dağıtma" bölümündeki adımları izleyin ve uygulamayı yeniden gönderme.

### <a name="remove-the-nozzle-from-ops-manager"></a>Başlık Ops Yöneticisi'nden kaldırın

1. Ops Manager için oturum açın.
2. Bulun **PCF için Microsoft Azure günlük analizi kafa** döşeme.
3. Çöp simgesini seçin ve silme işlemini onaylayın.

### <a name="remove-the-nozzle-from-your-development-computer"></a>Başlık Geliştirme bilgisayarınızdan kaldırın

CF CLI pencerenizde yazın:
```
cf delete <App Name> -r
```

Başlık kaldırırsanız, OMS portalında verileri otomatik olarak kaldırılmaz. Günlük analizi saklama ayarı göre süresi dolar.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure günlük analizi başlık olarak açık kaynaklı. Sorular ve için geri bildirim gönder [GitHub bölüm](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Bir Azure destek isteği'ni açmak için "bulut Foundry çalıştıran sanal makine" servis kategorisi seçin. 

## <a name="next-step"></a>Sonraki adım

Başlık kapsamdaki bulut Foundry ölçümleri yanı sıra, (örneğin, Syslog, performans, uyarılar, Envanter) VM düzeyinde işlemsel veri almak için OMS Aracısı'nı kullanabilirsiniz. OMS Aracısı CF Vm'leriniz için Bosh eklenti olarak yüklenir.

Ayrıntılar için bkz [dağıtmak OMS Aracısı bulut Foundry dağıtımınıza](https://github.com/Azure/oms-agent-for-linux-boshrelease).
