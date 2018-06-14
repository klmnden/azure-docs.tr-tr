---
title: Birden çok hizmet yapılandırmalarını kullanarak projenizi Azure yapılandırma | Microsoft Docs
description: Bir Azure bulut hizmeti projesi ServiceDefinition.csdef, ServiceConfiguration.Local.cscfg ve ServiceConfiguration.Cloud.cscfg dosyaları değiştirerek yapılandırmayı öğrenin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 9047d7a8a6efdd41a48b6fa83b43a8c87d05d1de
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31798556"
---
# <a name="configuring-your-azure-project-in-visual-studio-to-use-multiple-service-configurations"></a>Birden çok hizmet yapılandırması kullanmak için Visual Studio'da Azure projenizi yapılandırma

Visual Studio'da bir Azure bulut hizmeti projesi üç yapılandırma dosyalarını içerir: `ServiceDefinition.csdef`, `ServiceConfiguration.Local.cscfg`, ve `ServiceConfiguration.Cloud.cscfg`:

- `ServiceDefinition.csdef` Azure bulut hizmeti ve rollerini gereksinimlerini tanımlamak ve tüm örnekleri için geçerli olan ayarları sağlamak için dağıtılır. Azure hizmet barındırma çalışma zamanı API'sini kullanarak çalışma zamanında ayarlarını okuyabilirsiniz. Yalnızca bulut hizmeti durdurulduğunda bu dosyayı Azure üzerinde güncelleştirilebilir.
- `ServiceConfiguration.Local.cscfg` ve `ServiceConfiguration.Cloud.cscfg` tanımı'ndaki ayarları dosya ve her rol için çalıştırılacak örneklerinin sayısını belirtmek için değerler sağlayın. "Yerel" dosya yerel hata ayıklama kullanılan değerleri içeren; "Bulut" dosyası olarak Azure dağıtılır `ServiceConfiguration.cscfg` ve sunucu ortamınız için ayarları sağlar. Azure'da, bulut hizmetinizin çalışırken bu dosyanın güncelleştirilebilir.

Yapılandırma ayarları yönetilen ve Visual Studio'da geçerli rol için özellik sayfalarını kullanma değiştirdi (role sağ tıklayıp seçin **özellikleri**, veya rol çift tıklatın). Değişiklikleri hangi yapılandırma seçilir için kapsamı olan **hizmet yapılandırmasını** açılır. Web ve çalışan rolleri özelliklerini aşağıdaki bölümlerde açıklanan burada dışında benzerdir.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Hizmet yapılandırma dosyalarını ve hizmet tanımı için temel alınan şemaları hakkında daha fazla bilgi için bkz: [.csdef XML Şeması](cloud-services/schema-csdef-file.md) ve [.cscfg XML Şeması](cloud-services/schema-cscfg-file.md) makaleleri. Hizmet yapılandırması hakkında daha fazla bilgi için bkz: [bulut hizmetlerini yapılandırma](cloud-services/cloud-services-how-to-configure-portal.md).


## <a name="configuration-page"></a>Yapılandırma sayfası

### <a name="service-configuration"></a>Hizmet yapılandırması

Hangi seçer `ServiceConfiguration.*.cscfg` dosya değişikliklerden etkilenir. Varsayılan olarak, yerel ve bulut çeşitlemesi vardır ve kullanabileceğiniz **Yönet...**  kopyalama, yeniden adlandırın ve yapılandırma dosyalarını kaldırmak için komutu. Bu dosyalar bulut hizmeti projenizi eklenir ve görünür **Çözüm Gezgini**. Ancak, yeniden adlandırma veya yapılandırmaları kaldırma yalnızca bu denetimden yapılabilir.

### <a name="instances"></a>Örnekler

Ayarlama **örneği** sayısı, hizmetin, bu rol için çalıştırılması örnek sayısı özelliğine.

Ayarlama **VM boyutu** özelliğine **ek küçük**, **küçük**, **orta**, **büyük**, veya **Çok büyük**.  Daha fazla bilgi için bkz: [Cloud Services boyutları](cloud-services/cloud-services-sizes-specs.md).

### <a name="startup-action-web-role-only"></a>Başlatma eylemi (yalnızca Web rolü)

Visual Studio hata ayıklama başlattığınızda bir web tarayıcısı için HTTP uç noktaları veya HTTPS uç noktaları ya da hem belirmelidir belirtmek için bu özelliği ayarlayın.

**HTTPS uç noktası** seçenek, yalnızca zaten rolünüz için bir HTTPS uç noktası tanımladıysanız kullanılabilir. Üzerinde bir HTTPS uç noktası tanımlayabilir **uç noktaları** özellik sayfası.

Bir HTTPS uç nokta zaten eklediyseniz, HTTPS uç noktası seçeneği varsayılan olarak etkindir ve Visual Studio, HTTP uç noktası için bir tarayıcı ek olarak hata ayıklama hem de başlangıç seçeneği etkin olduğu varsayılarak başlattığınızda bu uç nokta için bir tarayıcı başlatır.

### <a name="diagnostics"></a>Tanılama

Varsayılan olarak, tanılama Web rolü için etkinleştirilir. Azure bulut hizmeti projesi ve depolama hesabı yerel depolama öykünücüsünü kullanmak için ayarlanır. Azure'a dağıtmak hazır olduğunuzda Oluşturucu düğmesini seçebilir (**...** ) Azure storage kullanmayı. İsteğe bağlı veya otomatik olarak zamanlanan aralıklarla depolama hesabına tanılama verilerini aktarabilirsiniz. Azure Tanılama hakkında daha fazla bilgi için bkz: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Ayarlar sayfası

Üzerinde **ayarları** sayfası, ad-değer çiftleri olarak bir yapılandırma ayarları ekleyebilirsiniz. Rolünde çalışan kodu okuyabilir, yapılandırma ayarlarınızı tarafından sağlanan sınıflarını kullanarak çalışma zamanında değerlerini [Azure yönetilen kitaplık](http://go.microsoft.com/fwlink?LinkID=171026), özellikle [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) yöntemi.

### <a name="configuring-a-connection-string-for-a-storage-account"></a>Bir depolama hesabı için bir bağlantı dizesi yapılandırma

Bir bağlantı dizesi veya bir Azure depolama hesabı depolama öykünücüsü için bağlantı ve kimlik doğrulama bilgileri sağlayan bir ayardır. Bir rolü kodda Azure storage (BLOB, kuyruk veya tablo) eriştiğinde, bir bağlantı dizesi gerekir.

> [!Note]
> Azure depolama hesabı için bir bağlantı dizesi tanımlı bir biçimi kullanmanız gerekir (bkz [Azure Storage bağlantı dizelerini yapılandırma](storage/common/storage-configure-connection-string.md)).

Bulut hizmeti, uygulamayı dağıttığınızda bir Azure depolama hesabı ayarlama gerektiğinde yerel depolama kullanılacak bağlantı dizesini ayarlayabilirsiniz. Bağlantı dizesi düzgün şekilde rolünüze başlatılmamasına veya başlatılırken, meşgul ve durdurma durumları arasında geçiş yapmak için başarısız olabilir.

Bir bağlantı dizesi oluşturmak için seçin **ayar Ekle** ve **türü** "Bağlantı dizesi".

Yeni veya var olan bağlantı dizeleri için seçin **...** * sağ tarafındaki **değeri** açmak için alan **depolama bağlantı dizesi oluştur** iletişim:

1. Altında **bağlanırken**, seçin **aboneliğinizi** seçeneği bir abonelikten bir depolama hesabı seçin. Visual Studio sonra depolama hesabı kimlik bilgileri otomatik olarak alır `.publishsettings` dosya.
1. Seçme **kimlik bilgileri'el ile girilen** hesap adını belirtin ve doğrudan Azure portalından bilgileri kullanarak anahtar sağlar. Hesap anahtarı kopyalamak için: bir. Azure portal ve select depolama hesabına gidin **anahtarları Yönet**.
    2. Hesap anahtarı kopyalamak için depolama hesabı seçin Azure Portal'da gidin **ayarlar > erişim anahtarları**, birincil erişim anahtarını panoya kopyalamak için Kopyala düğmesini kullanın.
1. Bağlantı seçeneklerden birini seçin. **Özel uç noktaları belirtin** kuyruklar ve BLOB'lar, tablolar için belirli URL'leri belirtmenizi ister. Özel uç noktaları kullanmanıza izin [özel etki alanlarını](storage/blobs/storage-custom-domain-name.md) ve erişim daha kesin olarak denetlemek için. Bkz: [Azure Storage bağlantı dizelerini yapılandırma](./storage/common/storage-configure-connection-string.md).
1. Seçin **Tamam**, ardından **Dosya > Kaydet** yeni bağlantı dizesi ile yapılandırmasını güncelleştirmek için.

Yeniden, uygulamanızı Azure'a yayımladığınızda, Azure depolama hesabı bağlantı dizesi için içeren hizmet yapılandırması seçin. Uygulamanızı yayımlandıktan sonra uygulamayı Azure storage hizmetlerine karşı beklendiği gibi çalıştığını doğrulayın.

Hizmet yapılandırması güncelleştirme hakkında daha fazla bilgi için bkz [depolama hesapları için bağlantı dizelerini Yönet](vs-azure-tools-configure-roles-for-cloud-service.md#manage-connection-strings-for-storage-accounts).

## <a name="endpoints-page"></a>Uç noktaları sayfası

Web rolü genellikle bağlantı noktası 80 üzerinde tek bir HTTP uç noktası vardır. Çalışan rolü, diğer yandan, herhangi bir sayıda HTTP, HTTPS veya TCP uç noktaları olabilir. Uç noktaları, dış istemcilere kullanılabilir giriş uç noktaları veya hizmetinde çalışan diğer rollerin kullanılabilir iç uç noktalar olabilir.

- Bir HTTP uç noktası dış istemcileri ve Web tarayıcılarının değişiklik yapmak için giriş uç noktası türünü değiştirin ve bir ad ve bir ortak bağlantı noktası numarası belirtin.
- Dış istemcilere ve Web tarayıcıları bir HTTPS uç noktası kullanılabilir hale getirmek için uç nokta türü değiştirme **giriş**, bir ad, bir ortak bağlantı noktası numarası ve bir yönetim sertifikası adı belirtin. Üzerinde sertifika tanımlamalısınız **sertifikaları** bir yönetim sertifikası belirtebilmeniz için önce özellik sayfası. 
- Bir uç nokta iç erişim bulut hizmetindeki diğer rolleri tarafından kullanılabilmesi için uç nokta türü için iç değiştirin ve bir ad ve bu uç nokta için olası özel bağlantı noktalarını belirtin.

## <a name="local-storage-page"></a>Yerel depolama sayfası

Kullanabileceğiniz **yerel depolama** bir rol için bir veya daha fazla yerel depolama kaynaklarını ayırmak için özellik sayfası. Yerel depolama kaynağı, bir rol örneği çalıştıran Azure sanal makinesinin dosya sistemindeki ayrılmış bir dizindir.

## <a name="certificates-page"></a>Sertifikalar sayfası

**Sertifikaları** özellik sayfası, hizmet yapılandırması, sertifikalar hakkında bilgi ekler. Hizmetiniz ile sertifikalarınızı paketlenmiş değil unutmayın; sertifikalarınızı ayrı olarak Azure karşıya yüklediğiniz gerekir [Azure portal](http://portal.azure.com).

Burada bir sertifika ekleme, sertifikalar hakkında bilgi hizmeti yapılandırmanızı ekler. Sertifikaları hizmetiyle paketlenmiş değil; sertifikalarınızı Azure portalı üzerinden ayrı olarak yüklemeniz gerekir.

Bir sertifika, rolüyle ilişkilendirmek için sertifika için bir ad sağlayın. Üzerinde bir HTTPS uç noktası yapılandırdığınızda sertifikaya başvurmak için bu adı kullanın **uç noktaları** sayfası. Ardından, sertifika deposuna olup olmadığını belirtin **yerel makine** veya **geçerli kullanıcı** ve depo adını. Son olarak, sertifikanın parmak izini girin. Sertifika (My) deposunda geçerli User\Personal içinde ise, sertifika doldurulan bir listeden seçerek sertifikanın parmak izi girebilirsiniz. Başka bir konumda bulunuyorsa, parmak izi değeri el ile girin.

Sertifika deposundan bir sertifikayı eklediğinizde, herhangi bir ara sertifika, yapılandırma ayarlarını otomatik olarak eklenir. Ayrıca, bu ara sertifika SSL için hizmet doğru şekilde yapılandırmak için Azure yüklenmelidir.

Yalnızca bulutta çalışırken hizmetiniz ile ilişkilendirmek istediğiniz yönetim sertifikaları hizmetiniz için geçerlidir. Hizmetinizi yerel geliştirme ortamında çalışan işlem öykünücüsü tarafından yönetilen standart bir sertifika kullanır.
