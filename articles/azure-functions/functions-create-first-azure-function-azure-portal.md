<properties
   pageTitle="Azure Portal'da işlev oluşturma | Microsoft Azure"
   description="İki dakikadan kısa bir süre içinde ilk Azure İşlevinizi (sunucusuz bir uygulama) oluşturun."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="glenga"/>

#Azure portalında işlev oluşturma

##Genel Bakış
Azure İşlevleri, diğer Azure hizmetlerinde, SaaS ürünlerinde ve şirket içi sistemlerde gerçekleşen olaylar tarafından tetiklenen kodları uygulama işlevine sahip, var olan Azure uygulama platformunu genişleten olay temelli bir isteğe bağlı işlem deneyimidir. Azure İşlevleri ile uygulamalarınız isteğe göre ölçeklenir ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Azure İşlevleri, çeşitli programlama dillerinde uygulanan zamanlanmış veya tetiklenmiş kod birimleri oluşturmanızı sağlar. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konu başlığında Azure portalını kullanarak HTTP tetikleyicisi tarafından çağrılan basit bir "merhaba dünya"  Node.js Azure İşlevini nasıl oluşturacağınız gösterilmektedir. Azure portalında işlev oluşturmadan önce, Azure Uygulama Hizmeti'nde açık olarak bir işlev uygulaması oluşturmanız gerekir. İşlev uygulamasının sizin için otomatik olarak oluşturulmasını istiyorsanız [diğer Azure İşlevleri hızlı başlangıç öğreticisine](functions-create-first-azure-function.md) bakın. Burada, daha basit bir hızlı başlangıç deneyiminin yanı sıra bir video sunulmaktadır.

##İşlev uygulaması oluşturma

İşlev uygulaması, Azure'daki işlev yürütme işlemlerinizi barındırır. Azure portalında bir işlev uygulaması oluşturmak için aşağıdaki adımları uygulayın.

İlk işlevinizi oluşturabilmeniz için etkin bir Azure hesabınızın olması gerekir. Azure hesabınız yoksa [ücretsiz hesap edinebilirsiniz](https://azure.microsoft.com/free/).

1. [Azure portalına](https://portal.azure.com) gidip Azure hesabınızla oturum açın.

2. **+Yeni** > **Web + Mobil** > **İşlev Uygulaması**'na tıklayıp **Aboneliğinizi** seçin ve işlev uygulamanızı tanımlayan benzersiz bir **Uygulama adı** yazdıktan sonra şu ayarları belirtin:

    + **[Kaynak Grubu](../azure-portal/resource-group-portal.md/)**: **Yeni oluştur**'u seçip yeni kaynak grubunuz için bir ad girin. Var olan bir kaynak grubunu da seçebilirsiniz ancak işlev uygulamanız için dinamik bir App Service planı oluşturamayabilirsiniz.
    + **[App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**: *Dinamik* veya *klasik* seçeneklerinden birini belirleyin. 
        + **Dinamik**: Azure İşlevleri için varsayılan plan türü. Dinamik plan seçeneğini belirlediğinizde, **Konum** seçmeniz ve **Bellek Ayırma**'yı (MB cinsinden) ayarlamanız da gerekir. Bellek ayırmanın, maliyeti nasıl etkilediği hakkında bilgi edinmek için bkz. [Azure İşlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/). 
        + **Klasik**: Klasik App Service planı için **App Service planı/konumu** oluşturmanız veya var olan bir planı/konumu seçmeniz gerekir. Bu ayarlar, uygulamanızla ilişkili [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) belirler.  
    + **Depolama hesabı**: Her işlev uygulaması için bir depolama hesabı gerekir. Var olan bir depolama hesabını seçebilir veya yeni bir depolama hesabı oluşturabilirsiniz. 

    ![Azure portalında yeni işlev uygulaması oluşturma](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)

3. Yeni işlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'a tıklayın.  

İşlev uygulaması sağlandıktan sonra ilk işlevinizi oluşturabilirsiniz.

## İşlev oluşturma

Bu adımlarda, Azure İşlevleri hızlı başlangıcından bir işlev oluşturulur.

1. **Hızlı Başlangıç** sekmesinde, **Web Kancası + API** ve **JavaScript**'e tıklayın ve ardından **İşlev oluştur**'a tıklayın. Önceden tanımlanmış yeni bir Node.js işlevi oluşturulur. 

    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)

2. (İsteğe bağlı) Hızlı başlangıcın bu noktasında, Azure İşlevleri ile ilgili özellikler için portalda hızlı bir tura çıkabilirsiniz.   Turu tamamladıktan veya atladıktan sonra, HTTP tetikleyicisini kullanarak yeni işlevinizi test edebilirsiniz.

##İşlevi test etme

Azure İşlevleri hızlı başlangıçlarında işlevsel kod olduğundan, yeni işlevinizi hemen test edebilirsiniz.

1. **Geliştir** sekmesinde **Kod** penceresini inceleyin ve bu Node.js kodunun, ileti gövdesinde veya sorgu dizesinde *ad* değerinin olduğu bir HTTP isteğini beklediğine dikkat edin. İşlev çalıştığında bu değer, yanıt iletisinde döndürülür.

    ![](./media/functions-create-first-azure-function-azure-portal/function-app-develop-tab-testing.png)

2. **İstek gövdesi** metin kutusuna doğru aşağı kaydırın, *ad* özelliğinin değerini adınızla değiştirin ve **Çalıştır**'a tıklayın. Bir deneme HTTP isteği tarafından yürütmenin tetiklendiğini, bilgilerin akış günlüklerine yazıldığını ve **Çıktıda** "merhaba" yanıtının görüntülendiğini göreceksiniz. 

3. Aynı işlevin başka bir tarayıcı penceresinden veya sekmesinden yürütülmesini tetiklemek için, **Geliştir** sekmesindeki **İşlev URL'si** değerini kopyalayın ve bir tarayıcının adres çubuğuna yapıştırın, ardından `&name=yourname` sorgu dizesi değerini ekleyin ve Enter'a basın. Aynı bilgiler günlüklere yazılır ve tarayıcı önceki gibi "merhaba" yanıtını görüntüler.

##Sonraki adımlar

Bu hızlı başlangıçta, HTTP ile tetiklenen temel bir işleve yönelik çok basit bir yürütme işlemi gösterilmektedir. Uygulamalarınızda Azure İşlevlerinin gücünü kullanma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın.

+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
İşlevleri kodlamak, tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu.
+ [Azure İşlevlerini test etme](functions-test-a-function.md)  
İşlevlerinizi test etmeye yönelik çeşitli araçları ve teknikleri açıklar.
+ [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
Dinamik hizmet planı da dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
+ [Azure Uygulama Hizmeti nedir?](../app-service/app-service-value-prop-what-is.md)  
Azure İşlevleri; dağıtım, ortam değişkenleri ve tanılama gibi temel işlevler için Azure Uygulama Hizmeti platformunu kullanır. 

[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=sep12_HO2-->


