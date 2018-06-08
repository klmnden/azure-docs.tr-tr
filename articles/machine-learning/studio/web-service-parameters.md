---
title: Azure Machine Learning Web hizmeti parametreleri kullanarak | Microsoft Docs
description: Web hizmeti erişilen modelinizin davranışını değiştirmek için Azure Machine Learning Web hizmeti parametreleri kullanma
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.openlocfilehash: 91b3c9df8a7fd0e1abb79c21b1e1d833e57c24d5
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835935"
---
# <a name="use-azure-machine-learning-web-service-parameters"></a>Azure Machine Learning Web Hizmeti Parametrelerini kullanma
Bir Azure Machine Learning web hizmetini yapılandırılabilir parametrelerle modülleri içeren bir denemeyi yayımlayarak oluşturulur. Bazı durumlarda, web hizmetinin çalıştığı sırada modülü davranışı değiştirmek isteyebilirsiniz. *Web hizmeti parametreleri* bu görevi yapmanıza olanak sağlar. 

Yaygın bir örnek ayarlama [veri içeri aktarma] [ reader] modülü böylece web hizmeti erişildiğinde yayımlanan web hizmeti kullanıcı farklı bir veri kaynağına belirtebilirsiniz. Veya yapılandırma [verileri dışa aktar] [ writer] modülü böylece farklı bir hedef belirtilebilir. Bazı diğer BITS sayısını değiştirme örnekler [özellik karma] [ feature-hashing] modül veya sayısı istenen özellikleri [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] modülü. 

Web hizmeti parametreleri ayarlayabilir ve bunları bir veya daha fazla modülü parametrelerle denemenizde ilişkilendirmeyi ve gerekli veya isteğe bağlı oldukları belirtebilirsiniz. Web hizmeti çağırdığınızda web hizmeti kullanıcı ardından bu parametreler için değerler sağlayabilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a>Ayarlama ve Web hizmeti parametreleri kullanma
Parametresi için bir modül yanındaki simgeyi tıklatarak ve "web hizmeti parametresi Ayarla" seçerek bir Web hizmeti parametre tanımlayın. Bu yeni bir Web hizmeti parametre oluşturur ve bu modülü parametreye bağlanır. Ardından, web hizmeti erişildiğinde kullanıcı Web hizmeti parametresi için bir değer belirtebilir ve modülü parametresi uygulanır.

Bir Web hizmeti parametresi tanımladıktan sonra başka bir modül parametre denemede için kullanılabilir. Bir modül için bir parametre ile ilişkili bir Web hizmeti parametresi tanımlarsanız, aynı türde bir değer parametresini beklemektedir sürece, aynı Web hizmeti parametresi diğer herhangi bir modül için kullanabilirsiniz. Web hizmeti parametresi bir sayısal değer ise, örneğin, daha sonra yalnızca sayısal bir değer beklediğiniz modülü parametreleri için kullanılabilir. Kullanıcı Web hizmeti parametresi için bir değer ayarlar, tüm ilişkilendirilmiş modülü parametreleri uygulanır.

Web hizmeti parametresi için varsayılan bir değer sağlamak karar verebilirsiniz. Bunu yaparsanız, parametre web hizmeti kullanıcı için isteğe bağlıdır. Varsayılan değer sağlamazsanız, kullanıcı web hizmeti erişildiğinde bir değer girmesini gereklidir.

Web hizmeti için API belgeleri web hizmeti kullanıcı için Web hizmeti parametresi program aracılığıyla web hizmeti erişirken nasıl belirtileceği hakkında bilgi içerir.

> [!NOTE]
> Klasik web hizmeti için API belgeleri sağlanır **API Yardım sayfası** web hizmeti bağlantı **PANO** Machine Learning Studio'da. Yeni bir web hizmeti için API belgeleri sağlanır [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/Quickstart) portalını **Tüket** ve **Swagger API'si** web hizmetiniz için sayfa.
> 
> 

## <a name="example"></a>Örnek
Bir örnek, bir deneme ile sahibiz varsayalım bir [verileri dışa aktar] [ writer] bilgileri Azure blob depolama alanına gönderir modülü. Web hizmeti "yol Blob" adlı bir parametre tanımlama olasılığınız yüksektir hizmet erişildiğinde blob depolama alanına yolunu değiştirmek web hizmeti kullanıcı verir.

1. Machine Learning Studio'da tıklatın [verileri dışa aktar] [ writer] modülü seçin. Özelliklerini deneme tuvaline sağındaki Özellikler bölmesinde gösterilir.
2. Depolama türünü belirtin:
   
   * Altında **Lütfen verileri hedef belirtin**, "Azure Blob Storage" seçin.
   * Altında **Lütfen kimlik doğrulama türünü belirtin**, "Hesap" seçin.
   * Azure blob depolama hesabı bilgilerini girin. 

3. Simgesine sağ tarafındaki **kapsayıcı parametresi ile başlayan blob yolu**. Şöyle görünür:
   
   ![Web hizmeti parametresi simgesi][icon]
   
   "Web hizmeti parametresi Ayarla" seçin.
   
   Bir giriş altında eklenen **Web hizmeti parametreleri** alt kısmındaki "kapsayıcı ile başlayan blob yolu" adıyla Özellikler bölmesi. Bu Web hizmeti, şu anda parametredir bununla ilişkili [verileri dışa aktar] [ writer] modülü parametresi.
4. Web hizmeti parametresi yeniden adlandırmak için adına tıklayın, "Blob yolu" girin ve basın **Enter** anahtarı. 
5. Web hizmeti parametresi için varsayılan bir değer sağlamak için adının sağındaki simgesini tıklatın, "varsayılan değer sağla" seçin, (örneğin, "container1/output1.csv"), bir değer girin ve basın **Enter** anahtarı.
   
   ![Web hizmeti parametresi][parameter]
6. **Çalıştır**’a tıklayın. 
7. Tıklatın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]** veya **Web hizmeti dağıtma [Yeni]** web hizmeti dağıtmak için.

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

Web hizmeti kullanıcı için yeni bir hedef artık belirtebilirsiniz [verileri dışa aktar] [ writer] web hizmeti erişirken modülü.

## <a name="more-information"></a>Daha fazla bilgi
Daha ayrıntılı bir örnek için bkz: [Web hizmeti parametreleri](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) girişi [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Machine Learning web hizmetine erişim ile ilgili daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmeti kullanmak nasıl](consume-web-services.md).

<!-- Images -->
[icon]: ./media/web-service-parameters/icon.png
[parameter]: ./media/web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

