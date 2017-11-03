---
title: "Machine Learning Web Hizmeti uç noktalarını oluşturma | Microsoft Docs"
description: "Azure Machine Learning Web Hizmeti uç noktaları oluşturma"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 821ad87fc10b2380e5ed89c037c335bc7747009e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="creating-endpoints"></a>Uç Nokta Oluşturma
> [!NOTE]
>  Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning Web hizmeti.
> 
> 

İleri müşterilerinize satış Web Hizmetleri oluşturduğunuzda, Web hizmeti oluşturulduğu deneme hala bağlı her bir müşteri için eğitilmiş modeller sağlamanız gerekir. Ayrıca, herhangi bir güncelleştirme deneme seçerek bir uç nokta özelleştirmeleri yazmadan uygulanmalıdır.

Bunu başarmak için Azure Machine Learning, dağıtılan Web hizmeti için birden fazla uç noktası oluşturmanıza olanak sağlar. Web hizmeti içindeki her bir uç nokta bağımsız olarak ele, kısıtlanan yönetilen ve. Bir benzersiz URL ve müşterilerinize dağıtabilirsiniz yetkilendirme anahtar her uç noktadır.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Web hizmeti için uç noktaları ekleme
Bir Web hizmeti için bir uç nokta eklemenin üç yolu vardır.

* Programlama yoluyla
* Azure Machine Learning Web Hizmetleri Portalı aracılığıyla
* Ancak klasik Azure portalı

Uç nokta oluşturulduktan sonra zaman uyumlu API'leri, batch API'leri aracılığıyla kullanabilir ve excel çalışma sayfaları. Bu kullanıcı Arabirimi aracılığıyla uç noktaları ekleme ek olarak, uç nokta yönetim API'ları program aracılığıyla uç noktalarını eklemek için de kullanabilirsiniz.

> [!NOTE]
> Web hizmetine ek uç noktaları eklediyseniz, varsayılan uç silemezsiniz.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Bir uç nokta programlı olarak ekleme
Program aracılığıyla kullanarak Web hizmetiniz için bir uç nokta ekleyebilirsiniz [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir uç nokta ekleme
1. Machine Learning Studio'da Web Hizmetleri sol gezinti sütunu,'ı tıklatın.
2. Web hizmeti Pano altındaki tıklatın **uç yönetin**. Web Hizmeti uç noktaları sayfasına Azure Machine Learning Web Hizmetleri Portalı'nı açar.
3. **Yeni**’ye tıklayın.
4. Bir ad ve yeni uç noktası için bir açıklama yazın. Uç nokta adları 24 karakter veya daha az uzunlukta olmalı ve küçük harfler veya numaraların yapılmalıdır. Günlüğe kaydetme düzeyi ve örnek verileri etkinleştirilip etkinleştirilmediğini seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Machine Learning Web Hizmetleri için günlüğe kaydetmeyi etkinleştirmek](web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Klasik Azure portalını kullanarak bir uç nokta ekleme
1. Oturum [Klasik Azure portalı](http://manage.windowsazure.com), tıklatın **Machine Learning** sol sütunda. İçinde ilgilendiğiniz Web hizmeti içeren çalışma alanını tıklatın.
   
    ![Çalışma alanına gidin](./media/create-endpoint/figure-1.png)
2. Tıklatın **Web Hizmetleri**.
   
    ![Web Hizmetlerine gidin](./media/create-endpoint/figure-2.png)
3. Kullanılabilir uç noktaları listesini görmek ilgilendiğiniz Web hizmeti tıklatın.
   
    ![Bitiş noktasına gidin](./media/create-endpoint/figure-3.png)
4. Sayfanın alt kısmındaki tıklatın **uç nokta Ekle**. Bir ad ve açıklama yazın, diğer uç nokta bu Web hizmeti aynı ada sahip olduğundan emin olun. Özel gereksinimleriniz olmadıkça kısıtlama düzeyini varsayılan değerini bırakın. Azaltma hakkında daha fazla bilgi için bkz: [ölçeklendirme API uç noktaları](scaling-webservice.md).
   
    ![Uç noktası oluşturma](./media/create-endpoint/figure-4.png)

## <a name="next-steps"></a>Sonraki Adımlar
[Bir Azure Machine Learning Web hizmeti kullanmak nasıl](consume-web-services.md).

