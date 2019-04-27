---
title: Web hizmetlerini yönetme
titleSuffix: Azure Machine Learning Studio
description: Microsoft Azure Machine Learning Web Hizmetleri portalını kullanarak, Machine Learning yeni ve Klasik Web Hizmetleri yönetin. Klasik Web Hizmetleri ve yeni Web hizmetleri üzerinde farklı temel teknolojileri aldığından, biraz farklı yönetim özellikleri bunların her biri için sizde.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 02/28/2017
ms.openlocfilehash: 711cb674cb00a880eadda11b03da87631df90b0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60861744"
---
# <a name="manage-a-web-service-using-the-azure-machine-learning-studio-web-services-portal"></a>Azure Machine Learning Studio Web Hizmetleri portalını kullanarak bir web hizmetini yönetme
Machine Learning yeni ve Klasik Web Hizmetleri Microsoft Azure Machine Learning Web Hizmetleri portalını kullanarak yönetebilirsiniz. Klasik Web Hizmetleri ve yeni Web hizmetleri üzerinde farklı temel teknolojileri aldığından, biraz farklı yönetim özellikleri bunların her biri için sizde.

Machine Learning Web Hizmetleri portalında şunları yapabilirsiniz:

* Web hizmeti nasıl kullanıldığını izleyin.
* Açıklama yapılandırın, anahtarları güncelleştir (yalnızca yeni) web hizmeti, günlük, depolama hesabı anahtarı (yalnızca yeni), etkinleştirme güncelleştirin ve etkinleştirmek veya devre dışı örnek veriler.
* Web hizmeti silin.
* (Yalnızca yeni), delete veya update faturalandırma planları oluşturun.
* Ekleme ve silme uç noktaları (yalnızca klasik)

>[!NOTE]
>Klasik web Hizmetleri içinde de yönetebilirsiniz [Machine Learning Studio](https://studio.azureml.net) üzerinde **Web Hizmetleri** sekmesi.

## <a name="permissions-to-manage-new-resources-manager-based-web-services"></a>Yeni Kaynak Yöneticisi'ni yönetmek için izinleri tabanlı web Hizmetleri

Yeni web Hizmetleri, Azure kaynakları olarak dağıtılır. Bu nedenle, dağıtmak ve yeni web hizmetleri yönetmek için doğru izinlere sahip olmalıdır.  Yeni web hizmetleri ve dağıtımı, web hizmeti dağıtıldığı abonelik üzerinde katkıda bulunan veya yönetici rol atanması gerekir. Machine learning çalışma alanı için başka bir kullanıcı davet, dağıtmak veya web hizmetlerini yönetme önce bir abonelik üzerinde katkıda bulunan veya yönetici rolü atamanız gerekir. 

Kullanıcı Azure Machine Learning Web Hizmetleri portalında kaynaklara erişmek için doğru izinlere sahip değilse, bunlar bir web hizmetini dağıtma çalışırken şu hatayı alırsınız:

*Web hizmeti dağıtımı başarısız oldu. Bu hesap, çalışma alanını içeren Azure aboneliğine yeterli erişimi yok. Bir Web hizmetini Azure'a dağıtmak için aynı hesabı çalışma alanına davet edilmesi gerektiğini ve çalışma alanını içeren Azure aboneliğine erişim verilmesi.*

Bir çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [oluşturma ve paylaşma bir Azure Machine Learning Studio çalışma](create-workspace.md).

Erişim izinleri ayarlama hakkında daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).


## <a name="manage-new-web-services"></a>Yeni Web hizmetlerini yönetme
Yeni Web hizmetlerinizi yönetmek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) Portalı'nı kullanarak Microsoft Azure hesabı - Azure aboneliğiyle ilişkili hesabı kullanın.
2. Menüsünde **Web Hizmetleri**.

Bu, dağıtılan Web Hizmetleri, aboneliğiniz için bir listesini görüntüler. 

Bir Web hizmetini yönetmek için Web Hizmetleri Ekle'ye tıklayın. Web Hizmetleri sayfasında, şunları yapabilirsiniz:

* Web hizmeti yönetmek için tıklatın.
* Faturalama güncelleştirmek için Plan web hizmeti için tıklayın.
* Bir web hizmeti silin.
* Bir web hizmetine kopyalayabilir ve başka bir bölgeye dağıtın.

Bir web hizmeti, web hizmeti hızlı başlangıç sayfası açılır. Web hizmeti hızlı başlangıç sayfası, web hizmetini yönetmenize olanak sağlayan iki menü seçenekleri vardır:

* **PANO** -Web hizmeti kullanım görüntülemenize olanak sağlar.
* **Yapılandırma** - açıklayıcı metin eklemenize izin verir Web hizmeti ile ilişkili depolama hesabı için anahtarı güncelleştirmek ve etkinleştirin veya örnek verilerini devre dışı bırakın.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Web hizmeti nasıl kullanıldığını izleme
Tıklayın **PANO** sekmesi.

Panodan, genel kullanım Web hizmeti bir süre içinde görüntüleyebilirsiniz. Sağ üst köşedeki kullanım grafik dönem açılan menüden görüntülemek için süresi seçebilirsiniz. Pano aşağıdaki bilgileri gösterir:

* **Zaman içinde istekleri** seçilen süre boyunca, istek sayısı bir adım grafiğini görüntüler. Kullanımındaki ani yaşıyorsanız belirlemenize yardımcı olabilir.
* **İstek-yanıt istekleri** hizmet, seçilen zaman aralığı ve kaç tanesinin başarısız üzerinden aldığı istek-yanıt çağrıları toplam sayısını görüntüler.
* **Ortalama işlem istek-yanıt süresi** ortalama alınan istekler yürütmek için gereken süreyi görüntüler.
* **Toplu istekleri** Batch hizmeti, seçilen zaman aralığı ve kaç tanesinin başarısız üzerinden aldı isteklerinin toplam sayısını görüntüler.
* **Ortalama işlem gecikme süresi** ortalama alınan istekler yürütmek için gereken süreyi görüntüler.
* **Hataları** web hizmetine çağrı ortaya çıkan hataları toplam sayısını görüntüler.
* **Hizmetleri maliyetleri** hizmetle ilişkili faturalandırma planı ücretleri görüntüler.

### <a name="configuring-the-web-service"></a>Web hizmetini yapılandırma
Tıklayın **yapılandırma** menü seçeneği.

Aşağıdaki özellikleri güncelleştirebilirsiniz:

* **Açıklama** Web hizmeti için bir açıklama girmenize olanak sağlar.
* **Başlık** Web hizmeti için bir başlık girmenizi sağlar
* **Anahtarları** birincil ve ikincil API anahtarlarınızı döndürmenizi sağlar.
* **Depolama hesabı anahtarı** , Web hizmeti ile ilişkilendirilmiş depolama hesabı anahtarı güncelleştirmenizi sağlar. 
* **Örnek verileri etkinleştir** istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek veriler göndermenizi sağlar. Web hizmetini Machine Learning Studio'da oluşturduysanız, örnek veri modelinizi eğitmek için kullanılan, verileri alınır. Program aracılığıyla hizmet oluşturduysanız, verileri JSON paketinin bir parçası sağlanan örnek verileri alınır.

### <a name="managing-billing-plans"></a>Faturalandırma planları yönetme
Tıklayın **planları** web hizmetleri hızlı başlangıç sayfasından menü seçeneği. Bu planı yönetmek için belirli bir Web hizmeti ile ilişkili plana de tıklayabilirsiniz.

* **Yeni** yeni bir plan oluşturmanıza olanak sağlar.
* **Ekle/Kaldır Plan örneği** "Kapasite eklemek için var olan bir planı ölçeğini genişletmek" sağlar.
* **Yükseltme/indirgeme** "Kapasite eklemek için var olan bir planı ölçeği artırma işleminin" sağlar.
* **Silme** planı silme olanak tanır.

Panosunu görüntülemek istediğiniz bir plana tıklayın. Pano, seçilen bir dönemdeki anlık görüntü veya planı kullanım sağlar. Görüntülenecek zaman aralığını seçmek için tıklayın **süresi** panonun sağ üst açılır. 

Plan Panosu aşağıdaki bilgileri sağlar:

* **Plan açıklaması** planıyla ilişkili kapasite ve maliyetleri hakkında bilgileri görüntüler.
* **Plan kullanım** işlemler ve işlem saatleri, planına göre ücret sayısını görüntüler.
* **Web Hizmetleri** bu planı kullanan Web Hizmetleri sayısını görüntüler.
* **İlk olarak, Web hizmeti çağrıları** planına göre ücret çağrıları yapan ilk dört Web hizmetleri görüntüler.
* **Web Hizmetleri tarafından işlem düğümü üst** planına göre ücret işlem kaynaklarını kullanarak ilk dört Web hizmetlerini görüntüler.

## <a name="manage-classic-web-services"></a>Klasik Web Hizmetleri yönetme
> [!NOTE]
> Bu bölümdeki yordamlar, Klasik web hizmetleri Azure Machine Learning Web Hizmetleri Portalı aracılığıyla yönetmek için uygundur. Klasik Web Hizmetleri Machine Learning Studio ve Azure Portalı aracılığıyla yönetme hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Studio çalışma alanını yönetme](manage-workspace.md).
> 
> 

Klasik Web hizmetlerinizi yönetmek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) Portalı'nı kullanarak Microsoft Azure hesabı - Azure aboneliğiyle ilişkili hesabı kullanın.
2. Menüsünde **Klasik Web Hizmetleri**.

Klasik Web hizmeti yönetmek için tıklatın **Klasik Web Hizmetleri**. Klasik Web Hizmetleri sayfasında, şunları yapabilirsiniz:

* Web hizmetinin ilişkili uç noktaları görüntülemek için tıklayın.
* Bir web hizmeti silin.

Klasik Web hizmeti yönetirken, her bir uç nokta ayrı olarak yönetin. Bir web hizmeti Web Hizmetleri sayfasında tıkladığınızda, hizmetle ilişkili uç noktaları listesi açılır. 

Klasik Web Hizmeti uç noktası sayfasında, eklemek ve hizmet uç noktalarını silmelisiniz. Uç noktaları ekleyerek daha fazla bilgi için bkz: [uç noktaları oluşturma](create-endpoint.md).

Web hizmeti hızlı başlangıç sayfasını açmak için uç noktalar birine tıklayın. Hızlı Başlangıç sayfasında, web hizmetini yönetmenize olanak sağlayan iki menü seçeneği vardır:

* **PANO** -Web hizmeti kullanım görüntülemenize olanak sağlar.
* **Yapılandırma** - açıklayıcı metin eklemenize izin verir hata günlüğünü açma veya kapatma, Web hizmeti ile ilişkili depolama hesabı anahtarını güncelleştir etkinleştirmek ve örnek verilerini devre dışı bırakın.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Web hizmeti nasıl kullanıldığını izleme
Tıklayın **PANO** sekmesi.

Panodan, genel kullanım Web hizmeti bir süre içinde görüntüleyebilirsiniz. Sağ üst köşedeki kullanım grafik dönem açılan menüden görüntülemek için süresi seçebilirsiniz. Pano aşağıdaki bilgileri gösterir:

* **Zaman içinde istekleri** seçilen süre boyunca, istek sayısı bir adım grafiğini görüntüler. Kullanımındaki ani yaşıyorsanız belirlemenize yardımcı olabilir.
* **İstek-yanıt istekleri** hizmet, seçilen zaman aralığı ve kaç tanesinin başarısız üzerinden aldığı istek-yanıt çağrıları toplam sayısını görüntüler.
* **Ortalama işlem istek-yanıt süresi** ortalama alınan istekler yürütmek için gereken süreyi görüntüler.
* **Toplu istekleri** Batch hizmeti, seçilen zaman aralığı ve kaç tanesinin başarısız üzerinden aldı isteklerinin toplam sayısını görüntüler.
* **Ortalama işlem gecikme süresi** ortalama alınan istekler yürütmek için gereken süreyi görüntüler.
* **Hataları** web hizmetine çağrı ortaya çıkan hataları toplam sayısını görüntüler.
* **Hizmetleri maliyetleri** hizmetle ilişkili faturalandırma planı ücretleri görüntüler.

### <a name="configuring-the-web-service"></a>Web hizmetini yapılandırma
Tıklayın **yapılandırma** menü seçeneği.

Aşağıdaki özellikleri güncelleştirebilirsiniz:

* **Açıklama** Web hizmeti için bir açıklama girmenize olanak sağlar. Açıklama gerekli bir alandır.
* **Günlüğe kaydetme** etkinleştirme veya devre dışı uç noktada oturum hata verir. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning web hizmetleri için günlüğe kaydetme](web-services-logging.md).
* **Örnek verileri etkinleştir** istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek veriler göndermenizi sağlar. Web hizmetini Machine Learning Studio'da oluşturduysanız, örnek veri modelinizi eğitmek için kullanılan, verileri alınır. Program aracılığıyla hizmet oluşturduysanız, verileri JSON paketinin bir parçası sağlanan örnek verileri alınır.


