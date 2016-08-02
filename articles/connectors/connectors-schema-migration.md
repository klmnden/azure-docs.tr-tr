<properties
    pageTitle="Mantıksal uygulamaları 2015-08-01-önizleme şema sürümüne geçirme | Microsoft Azure App Service"
    description="Mantıksal uygulamalarınızı en son şema sürümüne kolayca geçirebilirsiniz. Şu adımları izlemeniz yeterlidir."
    services="app-service\logic"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="deonhe"/>

# Mantıksal uygulamaları şema sürümü 2015-08-01-önizlemeye geçirme

Mevcut mantıksal uygulamalarınızı yeni şemaya taşımak için aşağıdakileri yapın:  
1. Azure portalda mantıksal uygulamanızı açın  
2. Şemayı Güncelleştir’e tıklayın:

 ![API Icon][step1]   
Şemayı Güncelleştir sayfası yeni şemadaki geliştirmelerin ayrıntılarını sağlayan bir belgeyi görüntüler ve buna ilişkin bir bağlantı sağlar: ![API Icon][step2]

>[AZURE.NOTE] **Şemayı Güncelleştir**’i seçtiğinizde, otomatik olarak geçiş adımlarını çalıştırırız ve size kod çıkışı sağlarız. Bunu tanımınızı güncelleştirmek için kullanabilirsiniz, ancak aşağıda **En İyi Uygulamalar** bölümünde belirtilenler gibi iyi kodlama uygulamalarını izlediğinizden emin olun.

## Mantıksal uygulamalarınızı en son şema sürümüne geçirirken en iyi uygulamalar:  

- Geçirilen betiği yeni Mantıksal Uygulamaya kopyalayın; testinizi tamamlayıncaya ve geçirilen uygulamalarının beklendiği gibi çalıştığını onaylayıncaya kadar eskisinin üzerine yazmayın.
- Üretime geçmeden **önce** Mantıksal uygulamanızı test edin
- Geçiş işlemi tamamlandığında, mümkün olduğunda [yönetilen API’leri](./apis-list.md) kullanmak için Mantıksal uygulamalarınızı güncelleştirmeye başlayın. Örneğin, DropBox v1 kullandığınız durumda Dropbox v2 kullanmaya başlayabilirsiniz.


## Sırada ne var?
-  [Mantıksal uygulamalarınızı el ile geçirmeyi öğrenme](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Jun16_HO2-->


