---
title: Kurumsal güvenlik
titleSuffix: Azure Machine Learning service
description: 'Azure Machine Learning hizmetine güvenli bir şekilde kullanın: kimlik doğrulaması, yetkilendirme, ağ güvenliği, veri şifreleme ve izleme.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/10/2019
ms.openlocfilehash: b950e7d38235d089c6236c76136d8ec2fc7a1f74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821309"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için Kurumsal güvenlik

Bu makalede, Azure Machine learning hizmeti kullanılabilir güvenlik özellikleri hakkında bilgi alacaksınız.

Bir bulut hizmeti kullanılırken, ihtiyaç duymayan kullanıcıların erişimini kısıtlamak için en iyi bir uygulamadır. Bu, hizmet tarafından kullanılan kimlik doğrulama ve yetkilendirme modeli anlayarak başlatır. Ayrıca ağ erişimi kısıtlamak istediğiniz veya güvenli bir şekilde bu bulut ile şirket içi ağınızdaki kaynaklara katılın. Veri şifreleme Ayrıca, hem beklerken hem de veri hizmetleri arasında taşırken önemlidir. Son olarak, hizmet izleme ve Denetim günlüğüne tüm etkinliklerin üretmek gerekir.

## <a name="authentication"></a>Kimlik Doğrulaması
Azure Active Directory (Azure AD) için yapılandırılmışsa, çok faktörlü kimlik doğrulaması desteklenir.
* İstemci, Azure AD ile günlüğe kaydeder ve Azure Resource Manager belirtecini alır.  Kullanıcı ve hizmet sorumluları tam olarak desteklenir.
* İstemci belirteci için Azure Resource Manager ve tüm Azure Machine Learning hizmetleri sunar.
* Azure Machine Learning hizmeti kullanıcı işlem için bir Azure Machine Learning belirteci sağlar. Örneğin, Machine Learning işlem. Bu Azure Machine Learning belirteci kullanıcı tarafından kullanılan işlem geri Azure Machine Learning hizmeti (çalışma alanına kapsam sınırlar), sonrasında çalıştırma çağırmak için tamamlandıktan.

![Kimlik doğrulaması Azure Machine Learning hizmetinde nasıl çalıştığını gösteren ekran görüntüsü](./media/enterprise-readiness/authentication.png)

### <a name="authentication-keys-for-web-service-deployment"></a>Web hizmeti dağıtımı için kimlik doğrulaması anahtarları

Kimlik doğrulaması için bir dağıtım'ı etkinleştirdiğinizde otomatik olarak kimlik doğrulaması anahtarları oluşturun.

* Azure Kubernetes Service'e dağıtırken, kimlik doğrulaması varsayılan olarak etkindir.
* Azure Container Instances'a dağıtırken kimlik doğrulaması varsayılan olarak devre dışıdır.

Kimlik doğrulaması denetlemek için kullanın `auth_enabled` oluştururken veya bir dağıtım güncelleştirme parametresi.

Kimlik doğrulamasını etkinleştirdiyseniz, kullanabileceğiniz `get_keys` yönteminin bir birincil ve ikincil kimlik doğrulama anahtarı almak için:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarın yeniden oluşturulması gerekiyorsa kullanın [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)


## <a name="authorization"></a>Yetkilendirme

Her bir çalışma alanı birden çok kişi tarafından paylaşılabilir ve birden çok çalışma alanı oluşturabilirsiniz. Bir çalışma alanı paylaştığınızda, kullanıcılara aşağıdaki rolleri atayarak erişim iznini kontrol edebilirsiniz:
* Sahip
* Katılımcı
* Okuyucu
    
Aşağıdaki tabloda ana Azure Machine Learning hizmeti işlemleri ve bunları gerçekleştirebileceği rolleri bazılarını listeler:

| Azure Machine Learning hizmeti işlemi | Sahip | Katılımcı | Okuyucu |
| ---- |:----:|:----:|:----:|
| Çalışma Alanı Oluştur | ✓ | ✓ | |
| Çalışma alanı paylaşın | ✓ | |  |
| İşlem oluşturma | ✓ | ✓ | |
| İşlem ekleme | ✓ | ✓ | |
| Veri depoları ekleme | ✓ | ✓ | |
| Bir denemeyi çalıştırma | ✓ | ✓ | |
| Çalıştırmaları/ölçümlerini görüntüleme | ✓ | ✓ | ✓ |
| Modeli kaydetme | ✓ | ✓ | |
| Görüntü oluştur | ✓ | ✓ | |
| Web hizmetini dağıtma | ✓ | ✓ | |
| Görünüm modelleri/görüntüleri | ✓ | ✓ | ✓ |
| Web hizmeti çağrısı | ✓ | ✓ | ✓ |

Yerleşik roller gereksinimleriniz için yeterli değil, ayrıca özel roller oluşturabilirsiniz. Çalışma alanını ve Machine Learning işlem işlemleri için destekliyoruz yalnızca özel roller olduğunu unutmayın. Özel roller bulunabilir okuma, yazma veya çalışma alanını ve bu çalışma alanındaki işlem kaynağı izinlerini silme. Rol kullanılabilir belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde yapabilirsiniz. Daha fazla bilgi için [kullanıcıları ve rolleri bir Azure Machine Learning çalışma alanı yönetme](how-to-assign-roles.md)

### <a name="securing-compute-and-data"></a>İşlem ve veri güvenliğini sağlama
Sahipleri ve katkıda bulunanlar çalışma alanına bağlı olan tüm işlem hedefleri ve veri depoları da kullanabilirsiniz.  
Her bir çalışma alanı bir ilişkili sistem tarafından atanan yönetilen kimliğe (ile aynı ada sahip bir çalışma alanı) çalışma alanında kullanılan ekli kaynaklar üzerinde aşağıdaki izinlere sahip da sahiptir:

Yönetilen kimlikleri hakkında daha fazla bilgi için bkz. [kimliklerini Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

| Kaynak | İzinler |
| ----- | ----- |
| Çalışma alanı | Katılımcı | 
| Depolama Hesabı | Depolama Blob Verileri Katkıda Bulunanı | 
| Key Vault | Tüm anahtarları, parolaları, sertifikaları erişimi | 
| Azure Container Registry | Katılımcı | 
| Çalışma alanı içeren bir kaynak grubu | Katılımcı | 
| Key Vault (çalışma alanı içeren bir farklıysa) içeren bir kaynak grubu | Katılımcı | 

Yöneticiler yönetilen kimlik yukarıda belirtilen kaynaklara erişimini iptal etmeyin önerilir. Erişim anahtarlarını yeniden eşitleme işlemi geri yüklenebilir.

Azure Machine Learning hizmeti aboneliğinizde katkıda bulunan düzeyinde erişim her çalışma alanı bölgesi için ek bir uygulama (adı aml - şununla başlar) oluşturur. İçin örn. bir çalışma alanı Doğu ABD ve Kuzey Avrupa'da başka bir çalışma alanı ile aynı abonelikte varsa, bu tür uygulamalar 2 görürsünüz. Hizmet yönetmenize yardımcı olan Azure Machine Learning işlem kaynakları, bu gereklidir.


## <a name="network-security"></a>Ağ güvenliği

İşlem kaynakları için diğer Azure hizmetleriyle Azure Machine Learning hizmeti kullanır. İşlem kaynakları (hedef işlem), modelleri eğitme ve kullanılır. Bu işlem, hedef sanal ağ içinde oluşturulabilir. Örneğin, Microsoft Veri bilimi sanal makinesi bir modeli eğitmek ve sonra model Azure Kubernetes Service (AKS) dağıtmak için kullanabilirsiniz.  

Daha fazla bilgi için [denemeleri ve çıkarım bir sanal ağ içinde çalıştırma](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Veri şifrelemesi

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
#### <a name="azure-blob-storage"></a>Azure Blob Depolama
Azure Machine Learning hizmeti, Azure Machine Learning hizmeti çalışma alanına bağlıdır ve kullanıcının abonelikte bulunan Azure Blob Depolama hesabında anlık görüntüleri, çıkış ve Günlükler depolar. Azure Blob depolamada depolanan tüm verileri Microsoft-Managed anahtarlarıyla bekleme sırasında şifrelenir.

Azure Blob Depolama'da depolanan veriler için kendi anahtarlarınızı hale getirme hakkında daha fazla bilgi için bkz. [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Eğitim işlem için erişilebilir olduğundan emin eğitim verileri genellikle aynı zamanda Azure Blob Depolama alanında depolanır. Bu depolama değil Azure Machine Learning tarafından yönetilen ancak bağlanan bir uzak dosya sistemi olarak hesaplamak için.

#### <a name="cosmos-db"></a>Cosmos DB
Azure Machine Learning hizmeti, Azure Machine Learning hizmeti tarafından yönetilen bir Microsoft abonelikte ölçümleri ve yaşadığı Cosmos DB için meta verileri depolar. Cosmos DB'de depolanan tüm veriler, bekleme durumunda Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)
Tüm kapsayıcı görüntülerini (ACR) kayıt defterinizde bekleme durumundayken şifrelenir. Azure, otomatik olarak bir görüntü depolamadan önce şifreler ve Azure Machine Learning hizmeti görüntü alınır, üzerinde halindeyken şifresini çözer.

#### <a name="machine-learning-compute"></a>Machine Learning İşlemi
Her işlem düğümü, Azure Depolama'da depolanan için işletim sistemi diski, Azure Machine Learning hizmeti depolama hesaplarında Microsoft yönetilen anahtarları kullanılarak şifrelenir. Bu işlem, kısa ömürlü ve kümeleri genellikle ölçeklendirilir çalıştırma yok olduğunda kuyruğa alınmış. Temel sanal makine serbest sağlanan ve işletim sistemi diski silindi. Azure disk şifrelemesi, işletim sistemi diski için desteklenmiyor.
Her bir sanal makine yerel geçici disk için işletim sistemi işlemlerini de vardır. Bu disk, aşaması için eğitim verilerini de bir isteğe bağlı olarak kullanılabilir. Bu disk şifreli değil. Azure'da bekleyen şifreleme birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure veri şifreleme bekleyen](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest). 

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Her iki iç arasındaki iletişimi çeşitli mikro hizmetler Azure Machine Learning Puanlama uç noktasını çağırma dış iletişim SSL ile desteklenir. Tüm Azure depolama erişimi güvenli bir kanal üzerinden de var. Daha fazla bilgi için [SSL kullanarak güvenli Azure Machine Learning web Hizmetleri](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Azure Key Vault kullanma
Çalışma alanı ile ilişkilendirilmiş Key Vault örneği, çeşitli türlerdeki kimlik bilgilerini depolamak için Azure Machine Learning hizmeti tarafından kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure kapsayıcı deposuna örneklerine parolaları
* Bağlantı için veri depolarını dizeleri. 

SSH parolaları ve anahtarlar HDI HDInsight ve VM gibi hedefleri hesaplamak için Microsoft abonelik ile ilişkilidir ayrı bir anahtar Kasası'nda depolanır. Azure Machine Learning hizmeti parolaları depolamak veya anahtarlarının kullanıcı tarafından yerine oluşturur, yetkisi verir ve VM / denemeleri çalıştırmak için HDInsight için bağlanmak için kendi SSH anahtarlarını depolar sağlanan. Her bir çalışma alanı bir ilişkili sistem tarafından atanan yönetilen anahtar Kasası'nda tüm anahtarlara, parolalara ve sertifikalara erişim sahibi kimliği (ile aynı ada sahip bir çalışma alanı) sahiptir.

 
## <a name="monitoring"></a>İzleme

Kullanıcılar, etkinlik günlüğü çalışma alanı gerçekleştirilen çeşitli işlemleri görmek ve işlem adı, olayı başlatan: zaman damgası vb. gibi temel bilgileri almak için çalışma alanı altında görebilirsiniz.

Aşağıdaki ekran görüntüsünde, bir çalışma alanı için Etkinlik günlüğünü gösterir:

![Bir çalışma alanı altında ekran gösteren etkinlik günlüğü](./media/enterprise-readiness/workspace-activity-log.png)


Puanlama isteği ayrıntıları, kullanıcının abonelikte çalışma alanı oluşturma sırasında oluşturulan Appınsights içinde depolanır. Bu HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, süre vb. gibi alanları içerir.


## <a name="data-flow-diagram"></a>Veri akış diyagramı

### <a name="create-workspace"></a>Çalışma Alanı Oluştur
Aşağıdaki diyagramda oluşturma çalışma iş akışı gösterilmektedir.
Kullanıcı, desteklenen Azure Machine Learning hizmeti (CLI, Python SDK'sı, Azure portalı) istemcilerden herhangi biri Azure AD'den oturum açtığı ve uygun Azure Resource Manager belirteci ister.  Kullanıcı daha sonra çalışma alanı oluşturmak için Azure Resource Manager çağırır.  Azure Resource Manager kişiler Azure Machine Learning çalışma alanı sağlamak için kaynak sağlayıcısı hizmeti.  Çalışma alanı oluşturma sırasında müşterinin aboneliğindeki ek kaynaklar oluşturulur:
* KeyVault (gizli dizileri depolamak için)
* Bir Azure depolama hesabı (Blob ve dosya paylaşımını dahil)
* Azure Container Registry (çıkarım ve deneme için docker görüntülerini depolamak için)
* Application ınsights'ı (telemetri depolamak için)

Diğer işlemler (Azure Kubernetes hizmeti, VM vb.) bir çalışma alanına bağlı gerektiği gibi müşteriler tarafından da sağlanabilir. 

![Çalışma alanı iş akışı gösteren ekran görüntüsü oluşturma](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Kaynak kodu (eğitim betikleriniz) Kaydet
Kod anlık görüntü iş akışı aşağıdaki diyagramda gösterilmiştir.
Bir Azure Machine Learning ile ilişkili hizmeti çalışma alanında olduğu dizinleri (denemeler) (eğitim betikleriniz) kaynak kodu içeren.  Bu, bulutta (Azure Blob Depolama müşterinin hizmet aboneliği altında) ve Müşteri'nin yerel makinede depolanır. Bu kod anlık görüntüler, yürütme veya geçmiş denetim için denetleme için kullanılır.

![Çalışma alanı iş akışı gösteren ekran görüntüsü oluşturma](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Eğitim
Eğitim iş akışı aşağıdaki diyagramda gösterilmiştir.
* Azure Machine Learning hizmetine kaydettiğiniz kod anlık görüntü için anlık görüntü kimliği ile çağrılır
* Azure Machine Learning hizmeti oluşturur çalıştırma kimliği (isteğe bağlı) & geri Azure Machine Learning hizmeti ile iletişim kurmasına daha sonra Machine Learning işlem/VM gibi işlem hedefler tarafından kullanılan Azure Machine Learning hizmet belirteci
* Ya da bir yönetilen bilgi işlem (ör. seçebilirsiniz. Machine Learning işlem) veya yönetilmeyen işlem (ör. VM), eğitim işleri çalıştırabilirsiniz. Veri akışı, her iki senaryo için aşağıda açıklanmıştır:
* (VM/HDInsight/yerel – erişilen SSH kimlik bilgileri, Microsoft abonelikte anahtar Kasası'nda kullanarak) Azure Machine Learning Hizmeti Yönetim kodu işlem hedefte, çalıştırır:
    1.  Ortamı hazırlar (Not: Docker VM/yerel bir seçenek de var. Machine Learning docker kapsayıcı çalıştığı hakkında nasıl çalışan deneme anlamak işlem aşağıdaki için adımlara bakın)
    2.  Kod indirir
    3.  Ortam değişkenleri/yapılandırmaları ayarlar
    4.  Kullanıcı komut dosyası (Yukarıdaki kod snapshot) çalışır
* (Makine öğrenimi işlemi yönetilen çalışma alanı kimliği kullanılarak erişildiği –) Machine Learning işlem yönetilen bir işlem olduğundan diğer bir deyişle, Microsoft tarafından yönetiliyorsa, sonuç olarak Microsoft abonelik altında çalıştığı dikkat edin.
    1.  Uzak Docker oluşturma gerekirse başlatılır
    2.  Azure dosya paylaşımı kullanıcıya yönetim kodu yazma
    3.  İlk başlatıldığında kapsayıcıyla diğer bir deyişle, Yukarıdaki adımda yönetim kodu komutu


#### <a name="querying-runs--metrics"></a>Çalıştırmalar & ölçümlerini sorgulama
Bu adım eğitim yazma burada bilgi işlem akış içinde gösterilen *çalıştırma ölçümleri* Cosmos DB'de depolandığı öğesinden geri Azure Machine Learning hizmetine. İstemciler, sırayla ölçümleri Cosmos DB'den çekme ve istemciye geri dönüş Azure Machine Learning hizmetine çağrı yapabilir.

![Çalışma alanı iş akışı gösteren ekran görüntüsü oluşturma](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Web hizmetleri oluşturma
Aşağıdaki diyagramda, modeli bir web hizmeti olarak dağıtıldığı çıkarım iş akışı gösterilmektedir.
Ayrıntılar için aşağıya bakın:
* Kullanıcı istemci gibi Azure ML SDK'sını kullanarak bir model kaydeder
* Model ve puanlama dosyası diğer model bağımlılıklar görüntü kullanıcı oluşturur
* Docker görüntüsü oluşturulur ve ACR depolanır
* Yukarıda oluşturulan görüntüyü kullanarak bir işlem hedefine (ACI/AKS) dağıtılan Web hizmeti
* Kullanıcının abonelikte Appınsights Puanlama isteği ayrıntılarının saklandığı
* Telemetri ayrıca Microsoft/Azure aboneliğine gönderiliyor

![Çalışma alanı iş akışı gösteren ekran görüntüsü oluşturma](./media/enterprise-readiness/inferencing.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Batch Öngörüler çalıştırma](how-to-run-batch-predictions.md)
* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)
* [Azure Machine Learning hizmeti SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning hizmeti ile Azure sanal ağları kullanın.](how-to-enable-virtual-network.md)
* [Öneri sistemleri oluşturma için en iyi yöntemler](https://github.com/Microsoft/Recommenders)
* [Azure üzerinde gerçek zamanlı bir öneri API'si oluşturun](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
