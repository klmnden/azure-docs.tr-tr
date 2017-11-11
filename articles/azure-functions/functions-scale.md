---
title: "Azure işlevleri barındırma planları karşılaştırma | Microsoft Docs"
description: "Azure işlevleri tüketim planı ve uygulama hizmeti planı arasında seçim yapma hakkında bilgi edinin."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, tüketimi planı, uygulama hizmeti planı, olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimarisi"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 423eee65040a11695d9f6c18d64948e4c3d3aafe
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="azure-functions-hosting-plans-comparison"></a>Azure işlevleri barındırma planları karşılaştırma

## <a name="introduction"></a>Giriş

Azure işlevlerinin iki farklı modda çalıştırabilirsiniz: Tüketim planı ve Azure uygulama hizmeti planı. Kodunuzu çalışıyorsa, çıkışı yükü işlemek için gerekli olan ölçeklendirir ve kod çalışmadığı zaman sonra ölçeklendirir tüketim planı otomatik olarak işlem gücü ayırır. Bu nedenle, boşta VM'ler için ödeme gerekmez ve yedek kapasite önceden gerekmez. Bu makalede tüketim plan üzerinde odaklanan bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) uygulama modeli. Uygulama hizmeti planı nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Azure işlevleriyle alışık değilseniz, bkz: [Azure işlevlerine genel bakış](functions-overview.md).

Bir işlev uygulaması oluşturduğunuzda, uygulamayı içeren işlevleri için bir barındırma planı yapılandırmanız gerekir. Her iki modda örneği *Azure işlevleri konak* işlevleri yürütür. Plan denetimleri türü:

* Nasıl konak örnekleri çıkışı ölçeklenir.
* Her ana bilgisayara kullanılabilir kaynaklar.

Şu anda, işlev uygulaması oluşturma sırasında barındırma planı türünü seçmeniz gerekir. Daha sonra değiştiremezsiniz. 

Bir uygulama hizmeti planı farklı miktarda kaynak ayırmak için Katmanlar arasında ölçeklendirebilirsiniz. Tüketim plan üzerinde Azure işlevleri, tüm kaynak ayırma otomatik olarak yönetir.

## <a name="consumption-plan"></a>Tüketim planı

Tüketim planı kullanırken, Azure işlevleri konak örnekleri dinamik olarak eklenir ve gelen olayların sayısına göre kaldırıldı. Bu plan otomatik olarak ölçeklendirir ve yalnızca işlevlerinizi çalıştırırken işlem kaynakları için ücretlendirilirsiniz. Tüketim plan üzerinde bir işlev en fazla 10 dakika için çalıştırabilirsiniz. 

> [!NOTE]
> Tüketim planı işlevleri için varsayılan zaman aşımı 5 dakikadır. Değer 10 dakika için işlev uygulaması özelliğini değiştirerek artırılabilir `functionTimeout` içinde [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

Faturalama yürütmeleri, yürütme zamanı ve kullanılan bellek sayısına bağlıdır. Faturalama işlevi uygulamasında tüm işlevleri üzerinden toplanır. Daha fazla bilgi için bkz: [Azure fiyatlandırma sayfası işlevleri].

Tüketim plan barındırma planı varsayılandır ve aşağıdaki avantajları sunar:
- Yalnızca işlevlerinizi çalışmadığı zaman ücret ödersiniz.
- Otomatik ölçeklendirme, bile yüksek dönemlerde yük.

## <a name="app-service-plan"></a>App Service planı

Uygulama hizmeti planında işlevi uygulamalarınızı özel VM'ler temel, standart, Premium ve yalıtılmış SKU'ları, Web Apps, API uygulamaları ve mobil uygulamaları için benzer üzerinde çalıştırın. Ayrılmış sanal işlevleri ana her zaman çalışan anlamına gelir, uygulama hizmeti uygulamalarınız için ayrılır.

Aşağıdaki durumlarda bir uygulama hizmeti planı göz önünde bulundurun:
- Diğer uygulama hizmet örneği zaten çalıştıran var olan, az VM'ye sahip.
- Sürekli veya neredeyse sürekli olarak çalıştırmak için işlevi uygulamalarınızı bekler. Bu durumda, bir uygulama hizmeti planı daha uygun maliyetli olabilir.
- Tüketim plan üzerinde sağlanan değerinden daha fazla CPU veya bellek seçenekleri gerekir.
- Tüketim planı (10 dakika) üzerinde izin verilen en fazla yürütme süresini daha uzun çalıştırmanız gerekir.
- Uygulama hizmeti ortamı VNET/VPN bağlantısı ve daha büyük VM boyutları için destek gibi bir uygulama hizmeti planı yalnızca kullanılabilen özellikleri gerektirir. 

VM maliyet dizi yürütmeleri, yürütme zamanı ve kullanılan bellek ayırır. Sonuç olarak, en fazla tahsis VM örneği maliyetini ücret ödersiniz olmaz. Uygulama hizmeti planı nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Bir uygulama hizmeti planı ile elle çıkışı daha fazla VM örnekleri ekleyerek ölçeklendirebilirsiniz veya otomatik ölçeklendirme etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Aynı zamanda farklı bir uygulama hizmeti planı seçerek ölçeği artırabilirsiniz. Daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](../app-service/web-sites-scale.md). 

Bir uygulama hizmeti planı JavaScript işlevleri çalıştırmayı planlıyorsanız, daha az Vcpu'lar sahip bir plan seçmeniz gerekir. Daha fazla bilgi için bkz: [seçin tek çekirdek uygulama hizmeti planları](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Her zaman açık

Bir uygulama hizmeti planı çalıştırırsanız, etkinleştirmelisiniz **her zaman açık** işlevi uygulamanızın düzgün çalıştığını ayarını. Bir uygulama hizmeti planı yalnızca HTTP Tetikleyicileri "işlevlerinizi uyandırmak şekilde" işlevleri çalışma zamanı boşta kalma birkaç dakika sonra geçer. Bu, nasıl Web işleri her zaman etkin olması gerekir için benzer. 

Her zaman açık yalnızca bir uygulama hizmeti planı üzerinde kullanılabilir. Tüketim plan üzerinde platform işlev uygulamalarının otomatik olarak etkinleştirir.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Ya tüketimini planı ya da bir uygulama hizmeti planı, bir işlev uygulaması Azure Blob, kuyruk, dosya ve tablo depolama destekler genel bir Azure depolama hesabı gerektirir. Dahili olarak, Azure işlevleri Tetikleyicileri yönetme ve işlev yürütmeleri günlüğü gibi işlemler için Azure Storage kullanır. Bazı depolama hesapları, kuyruklar ve tablolar, yalnızca blob depolama hesapları (premium depolama dahil) ve bölge olarak yedekli depolama çoğaltma ile genel amaçlı depolama hesapları gibi desteklemez. Bu hesapları filtre **depolama hesabı** bir işlev uygulaması oluştururken dikey.

Depolama hesabı türleri hakkında daha fazla bilgi için bkz: [Azure Storage hizmetlerine giriş](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Tüketim planı nasıl çalışır?

Tüketim planında ölçek denetleyicisi otomatik olarak CPU ve bellek kaynakları işlevleri üzerinde tetiklenen olayların sayısına dayalı işlevler konak ek örneklerini ekleyerek ölçeklendirir. İşlevler konak her örneği için 1,5 GB bellek sınırlıdır.

Barındırma planı tüketimini kullandığınızda işlevi kod dosyaları Azure dosya paylaşımlarının işlevin ana depolama hesabında depolanır. İşlev uygulaması ana depolama hesabına sildiğinizde, işlevi kod dosyaları silinir ve kurtarılamaz.

> [!NOTE]
> Tüketim plan üzerinde bir blob tetikleyici kullanırken olabilir en fazla 10 dakikalık bir gecikmeyle bir işlev uygulaması boşta geçti, yeni BLOB'lar işleme. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Bu ilk gecikmeyi önlemek için aşağıdaki seçeneklerden birini göz önünde bulundurun:
> - Always On özellikli ile bir uygulama hizmeti planınız işlev uygulaması barındırır.
> - Başka bir mekanizma, blob adı içeren bir kuyruk iletisi gibi işleme blob tetiklemek için kullanın. Bir örnek için bkz: [C# betiği ve JavaScript örnek blob'a giriş ve bağlamaları çıkış](functions-bindings-storage-blob.md#input--output---example).

### <a name="runtime-scaling"></a>Çalışma zamanı ölçeklendirme

Azure işlevlerini kullanan adlı bir bileşen *ölçek denetleyicisi* olaylarının hızı izlemek ve ölçeğini veya içinde ölçeklendirmek belirleyin. Ölçek denetleyicisi her tetikleyici türü için buluşsal yöntemler kullanır. Örneğin, bir Azure kuyruk depolama tetikleyicisi kullanırken, kuyruk uzunluğu ve eski kuyruk iletisini yaşını göre ölçeklendirir.

İşlev uygulaması ölçek birimidir. İşlev uygulaması çıkışı ölçeklenir, ek kaynaklar Azure işlevleri ana bilgisayarın birden çok örneği çalıştırmak için ayrılır. Buna karşılık, talep azalır işlem gibi ölçek denetleyicisi işlevi ana bilgisayar örneklerini kaldırır. Bir işlev uygulaması içinde hiçbir işlevleri çalıştırırken örneklerinin sayısını sonunda sıfır olarak ölçeklendirilir.

![İzleme olayları ve örnekleri oluşturma ölçek denetleyicisi](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Faturalandırma modeli

Tüketim plan üzerinde ayrıntılı olarak açıklanmıştır faturalama [Azure fiyatlandırma sayfası işlevleri]. Kullanım işlevi uygulama düzeyinde toplanır ve işlev kodu yürütülen zaman sayar. Faturalama birimleri şunlardır: 
* **Kaynak tüketimini gigabayt (GB-s) saniye**. Bellek boyutu ve bir işlev uygulaması içinde tüm işlevleri için yürütme süresi bileşimini olarak hesaplanır. 
* **Yürütmeleri**. Bir olay tetikleyicisine yanıt olarak bir işlev yürütülen her zaman sayılır.

[Azure fiyatlandırma sayfası işlevleri]: https://azure.microsoft.com/pricing/details/functions
