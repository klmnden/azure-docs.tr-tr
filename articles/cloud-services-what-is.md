<properties urlDisplayName="What is a Cloud Service" pageTitle="Bulut hizmeti nedir - Azure hizmet yönetimi" metaKeywords="Azure bulut hizmetlerine giriş, bulut hizmetlerine genel bakış, bulut hizmetleri temelleri" description="An introduction to the cloud service in Azure." metaCanonical="" services="cloud-services" documentationCenter="" title="What is a cloud service?" authors="ryanwi" solutions="" manager="timlt" editor="" />

<tags ms.service="cloud-services" ms.workload="tbd" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="10/23/2014" ms.author="ryanwi" />




# Bulut hizmeti nedir?
Bir uygulama oluşturup Azure'da çalıştırdığınızda bu, kod ve yapılandırma ile birlikte Azure bulut hizmeti olarak adlandırılır (önceki Azure sürümlerinde *barındırılan hizmet* olarak bilinir).

Bir bulut hizmeti oluşturarak Azure'da çok katmanlı bir web uygulaması dağıtabilir, işlemi dağıtmak için birden çok rol tanımlayabilir ve uygulamanızın esnek ölçeklendirilmesini sağlayabilirsiniz. Bir bulut hizmeti, her biri kendi uygulama dosyalarına ve yapılandırmasına sahip bir veya daha fazla web rolünden ve/veya çalışan rolünden oluşur. Azure Web Siteleri ve Sanal Makineler ayrıca Azure'da web uygulamalarını etkinleştirir.  Bulut hizmetlerinin başlıca avantajı, daha karmaşık çok katmanlı mimarileri destekleyebilmesidir. Ayrıntılı bir karşılaştırma için, bkz. [Azure Web Siteleri, Bulut Hizmetleri ve Sanal Makineler karşılaştırması][Comparison].

Bir bulut hizmeti için Azure, rutin bakım yaparak, işletim sistemlerine düzeltme eki uygulayarak ve hizmet ve donanım hatalarını kurtarmaya çalışarak altyapının bakımını sizin yerinize yapar. Her role ait en az iki örnek tanımlarsanız, birçok bakım ve hizmet yükseltmesi herhangi bir hizmet kesintisi olmadan gerçekleştirilebilir. Bir bulut hizmetinin Azure Hizmet Düzeyi Sözleşmesi'ne uygun olması için her rolün en az iki örneğe sahip olması gerekir; böylece zamanın %99,95'inde İnternet'e yönelik rolleriniz için dış bağlantı garantisi verilir. 

Her bulut hizmetinde hizmet paketinizi ve yapılandırmanızı dağıtabileceğiniz iki ortam vardır. Bir bulut hizmetini, üretime yükseltmeden önce test etmek için hazırlık ortamına dağıtabilirsiniz. Hazırlanmış bir bulut hizmetinin üretime yükseltilmesi, iki ortamla ilişkili sanal IP adreslerinin (VIP'ler) birbiriyle değiştirilmesi kadar basit bir işlemdir. 


## Kavramlar ##


- **bulut hizmeti rolü:** Bulut hizmeti rolü, uygulama dosyalarından ve bir yapılandırmadan oluşur. Bir bulut hizmetinde iki tür rol olabilir:
 
>- **web rolü:**Web rolü, ön uç web uygulamaları için kullanılan özel bir Internet Information Services (IIS) web sunucusu sağlar.

>- **çalışan rolü:** Çalışan rollerinde barındırılan uygulamalar, kullanıcı işleminden veya girişinden bağımsız, zaman uyumsuz, uzun süreli çalışan veya kalıcı görevleri çalıştırabilir.

- **rol örneği:** Rol örneği, üzerinde uygulama kodu ve rol yapılandırmasının çalıştığı bir sanal makinedir. Bir rol, hizmet yapılandırma dosyasında tanımlanmış birden çok örnek içerebilir.

- **konuk işletim sistemi:** Bir bulut hizmetine ait konuk işletim sistemi, uygulama kodunuzun üzerinde çalıştığı rol örneklerinde (sanal makineler) yüklü olan işletim sistemidir.

- **bulut hizmeti bileşenleri:** Bir uygulamayı Azure'da bulut hizmeti olarak dağıtmak için üç bileşen gereklidir:

>- **hizmet tanım dosyası:** Bulut hizmeti tanım dosyası (.csdef), rol sayısıyla birlikte hizmet modelini tanımlar.

>- **hizmet yapılandırma dosyası:** Bulut hizmeti yapılandırma dosyası (.cscfg), rol örneklerinin sayısıyla birlikte bulut hizmeti ve tek roller için yapılandırma ayarlarını sağlar.

>- **hizmet paketi:** Hizmet paketi (.cspkg), uygulama kodunu ve hizmet tanım dosyasını içerir.

- **bulut hizmeti dağıtımı:** Bulut hizmeti dağıtımı, Azure hazırlık veya üretim ortamına dağıtılan bir bulut hizmeti örneğidir. Hem hazırlıkta hem de üretimde dağıtımları sürdürebilirsiniz.

- **dağıtım ortamları:** Azure, bulut hizmetleri için iki dağıtım ortamı sunar: dağıtımınızı *üretim ortamına* yükseltmeden önce test edebileceğiniz *hazırlık ortamı*. Bu iki ortam, yalnızca bulut hizmetine erişilen sanal IP adresleriyle (VIP) birbirinden ayrılır. Hazırlık ortamında, bulut hizmetinin genel benzersiz tanımlayıcısı (GUID), URL'lerde onu tanımlar (*GUID*.cloudapp.net). Üretim ortamında URL, bulut hizmetine atanan daha kolay DNS önekine dayanır (örneğin, *myservice*.cloudapp.net).

- **dağıtımları değiştirme:** Azure hazırlık ortamındaki bir dağıtımı üretim ortamına yükseltmek için iki dağıtıma erişmek için kullanılan VIP'leri geçirerek dağıtımları "değiştirebilirsiniz". Dağıtımdan sonra bulut hizmetinin DNS adı, daha önce hazırlık ortamında olan dağıtımı işaret eder. 

- **Minimal ve ayrıntılı izleme:** Bir bulut hizmeti için varsayılan olarak yapılandırılan *minimal izleme*, rol örnekleri (sanal makineler) için ana bilgisayar işletim sistemlerinden toplanan performans sayaçlarını kullanır. *Ayrıntılı izleme*, uygulama işleme sırasında oluşan sorunların daha yakından çözümlenmesini sağlamak için rol örnekleri içindeki performans verilerine dayalı ek ölçümler toplar. Daha fazla bilgi için, bkz. [Bulut Hizmetleri Nasıl İzlenir][HTMonitorCloudServices].

- **Azure Tanılama:** Azure Tanılama, Azure'da çalışan uygulamaların tanılama verilerini toplamanızı sağlayan API'dir. Ayrıntılı izlemenin etkinleştirilmesi için bulut hizmeti rollerinde Azure Tanılama etkinleştirilmelidir. Daha fazla bilgi için, bkz. [Azure'da Tanılama Etkinleştirme][CloudServicesDiagnostics].

- **kaynak bağlama:** Bulut hizmetinizin Azure SQL Veritabanı örneği gibi diğer kaynaklara bağımlılıklarını göstermek için kaynağı bulut hizmetine "bağlayabilirsiniz". Önizleme Yönetim Portalı'ndaki **Bağlı Kaynaklar** sayfasında bağlanmış kaynakları görüntüleyebilir, durumu panoda görüntüleyebilir ve bağlanmış bir SQL Veritabanı örneğini **Ölçek** sayfasındaki hizmet rolleriyle birlikte ölçeklendirebilirsiniz. Bir kaynağın bu anlamda bağlanması, kaynağı uygulamaya bağlamaz; uygulama kodundaki bağlantıları yapılandırmanız gerekir.

- **bulut hizmetini ölçeklendirme:** Bir bulut hizmetinin ölçeği, bir rol için dağıtılmış rol örneklerinin (sanal makinelerin) sayısı artırılarak büyütülebilir. Bir bulut hizmetinin ölçeği, rol örnekleri azaltılarak küçültülebilir. Önizleme Yönetim Portalı'nda hizmet rollerinizi ölçeklendirirken SQL Veritabanı sürümünü ve en büyük veritabanı boyutunu değiştirerek, bağlanmış bir SQL Veritabanı örneğini de ölçeklendirebilirsiniz.

- **Azure Hizmet Düzeyi Sözleşmesi (SLA):** Azure Compute SLA'sı, her rol için iki veya daha fazla rol örneği dağıttığınızda bulut hizmetinize erişimin, zamanın en az yüzde 99,95'inde sürdürülmesini garanti eder. Ayrıca, bir rol örneğinin işlemi çalışmadığında zamanın yüzde 99,9'unda algılama ve düzeltme işlemi başlatılır. Daha fazla bilgi için, bkz. [Hizmet Düzeyi Sözleşmeleri] [SLA].

[HTMonitorCloudServices]:http://azure.microsoft.com/tr-tr/manage/services/cloud-services/how-to-monitor-a-cloud-service/
[SLA]: http://azure.microsoft.com/tr-tr/support/legal/sla/
[CloudServicesDiagnostics]: http://azure.microsoft.com/tr-tr/documentation/articles/cloud-services-dotnet-diagnostics/
[Comparison]: http://azure.microsoft.com/tr-tr/documentation/articles/choose-web-site-cloud-service-vm/
