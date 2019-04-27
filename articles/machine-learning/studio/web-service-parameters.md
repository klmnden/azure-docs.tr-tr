---
title: Web hizmeti parametreleri - Azure Machine Learning Studio | Microsoft Docs
description: Web hizmeti erişim sağlandığında modelinizi davranışını değiştirmek için Azure Machine Learning Web hizmeti parametrelerini kullanma
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/12/2017
ms.openlocfilehash: a236043d5622e5a2e1ffd572c887fb5ffac2174a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345456"
---
# <a name="use-azure-machine-learning-studio-web-service-parameters"></a>Azure Machine Learning Studio web hizmeti parametrelerini kullanma
Bir Azure Machine Learning web hizmeti modülleri ile yapılandırılabilir parametreler içeren bir denemeyi yayımlayarak oluşturulur. Bazı durumlarda, web hizmetinin çalıştığı sırada modülü davranışını değiştirmek isteyebilirsiniz. *Web hizmeti parametreleri* bu işi yapmanıza olanak sağlar. 

Yaygın olarak karşılaşılan örneklerden ayarlama [verileri içeri aktarma] [ reader] modülü web hizmeti erişim sağlandığında yayımlanan web hizmeti kullanıcı farklı bir veri kaynağına belirtebilirsiniz. Veya yapılandırma [verileri dışarı aktarma] [ writer] modülü böylece farklı bir hedef belirtilebilir. Diğer örnekler için bit sayısı kadar değiştireceğiniz [özellik karma] [ feature-hashing] modül veya sayısı istenen özellikleri [özellik seçimi süzgeç tabanlı] [ filter-based-feature-selection] modülü. 

Web hizmeti parametreleri ayarlama ve bunları denemenizde bir veya daha fazla modül parametrelerini ile ilişkilendirin ve bunlar gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Web hizmeti aradıklarında web hizmeti kullanıcı daha sonra bu parametreler için değerleri sağlayabilirsiniz. 



## <a name="how-to-set-and-use-web-service-parameters"></a>Ayarlayın ve Web hizmeti parametrelerini kullanma
Parametresi bir modül için yanındaki simgeye tıklayarak ve "web hizmeti parametre olarak Ayarla" seçerek bir Web hizmeti parametre tanımlayın. Bu yeni bir Web hizmeti parametresi oluşturur ve bu modülü parametreye bağlanır. Ardından, web hizmeti erişim sağlandığında kullanıcı Web hizmeti parametresi için bir değer belirtebilir ve modülü parametresi uygulanır.

Bir Web hizmeti parametresi tanımladıktan sonra başka bir modül parametre denemede için kullanılabilir. Bir modül için bir parametre ile ilişkili bir Web hizmeti parametresi tanımlarsanız, değer aynı türde bir parametre bekliyor sürece, aynı Web hizmeti parametre başka herhangi bir modül için kullanabilirsiniz. Web hizmeti parametresi sayısal bir değerdir, örneğin, daha sonra yalnızca sayısal bir değer beklediğiniz modül parametrelerini kullanılabilir. Kullanıcı, Web hizmeti parametresi için bir değer ayarlar, tüm ilişkili modül parametrelerini için uygulanır.

Web hizmeti parametresi için varsayılan bir değer sağlamak karar verebilirsiniz. Bunu yaparsanız, parametre web hizmeti kullanıcı için isteğe bağlıdır. Varsayılan değer sağlamazsanız, web hizmeti erişim sağlandığında bir değer girmek için kullanıcı gereklidir.

Web hizmeti için API belgeleri, web hizmeti kullanıcı için Web hizmeti parametresi web hizmetine erişirken programlı olarak belirtme hakkında bilgi içerir.

> [!NOTE]
> Klasik web hizmeti için API belgeleri sağlanır **API Yardım sayfası** web hizmeti bağlantı **PANO** Machine Learning Studio'da. API belgeleri için yeni bir web hizmeti aracılığıyla sağlanan [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/Quickstart) Şirket portalı **Tüket** ve **Swagger API'si** web sayfaları hizmeti.
> 
> 

## <a name="example"></a>Örnek
Örnek olarak, varsayalım deneme ile sahip olduğumuz bir [verileri dışarı aktarma] [ writer] bilgileri Azure blob depolamaya gönderir modülü. Web hizmeti "Blob yolu" adlı bir parametre tanımlarız hizmet eriştiğinde, blob depolama alanına yolunu değiştirmek web hizmeti kullanıcı verir.

1. Machine Learning Studio'da tıklayın [verileri dışarı aktarma] [ writer] modülü seçin. Özellikler bölmesinde deneme tuvaline sağındaki özellikleri gösterilmektedir.
2. Depolama türünü belirtin:
   
   * Altında **veri hedef zadejte**, "Azure Blob Depolama" seçin.
   * Altında **Lütfen kimlik doğrulama türünü belirtin**, "Hesap" seçin.
   * Azure blob depolama hesabı bilgilerini girin. 

3. Simgesini sağ tarafındaki **kapsayıcı parametresi ile başlayan blob yolu**. Şöyle görünür:
   
   ![Web hizmeti parametre simgesi](./media/web-service-parameters/icon.png)
   
   "Web hizmeti parametre olarak Ayarla" seçin.
   
   Bir girdi altında eklenir **Web hizmeti parametrelerini** Özellikler bölmesinde "blob kapsayıcısı ile başlayan yol" adlı alt kısmındaki. Bu Web hizmeti, şu anda parametredir bununla ilişkili [verileri dışarı aktarma] [ writer] modülü parametresi.
4. Web hizmeti parametreyi yeniden adlandır, adına tıklayın, "Blob yolu" girin ve basın **Enter** anahtarı. 
5. Web hizmeti parametresi için varsayılan bir değer sağlamak için adının sağ tarafındaki simgesine tıklayın, "sağlar. varsayılan değer"'i seçin, (örneğin, "container1/output1.csv"), bir değer girin ve basın **Enter** anahtarı.
   
   ![Web hizmeti parametresi](./media/web-service-parameters/parameter.png)
6. **Çalıştır**’a tıklayın. 
7. Tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]** veya **Web hizmeti dağıtma [Yeni]** web hizmeti dağıtmak için.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md). 

Web hizmeti kullanıcı için yeni bir hedef artık belirtebilirsiniz [verileri dışarı aktarma] [ writer] web hizmetine erişirken modülü.

## <a name="more-information"></a>Daha fazla bilgi
Daha ayrıntılı bir örnek için bkz. [Web hizmeti parametrelerini](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) girişi [Machine Learning Web günlüğü](https://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Machine Learning web hizmetine erişim ile ilgili daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

