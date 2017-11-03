---
title: "Birden çok hizmet yapılandırmalarını kullanarak projenizi Azure yapılandırma | Microsoft Docs"
description: "Bir Azure bulut hizmeti projesi ServiceDefinition.csdef ve ServiceConfiguration.cscfg dosyaları değiştirerek yapılandırmayı öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: deb69101e855bcad56b9212736c52ace72631f0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Birden çok hizmet yapılandırmalarını kullanarak projenizi Azure yapılandırma
Bir Azure bulut hizmeti projesi iki yapılandırma dosyalarını içerir: ServiceDefinition.csdef ve ServiceConfiguration.cscfg. Bu dosyalar Azure bulut hizmeti uygulamanız ile paketlenir ve Azure'da dağıtılabilir.

* **ServiceDefinition.csdef** dosyası Azure ortamı içerdiği hangi rolleri de dahil olmak üzere, bulut hizmeti uygulaması gereksinimleri için gerekli meta veriler içeriyor. Bu dosya ayrıca tüm örneklerine uygulanan yapılandırma ayarlarını içerir. Bu yapılandırma ayarları Azure hizmet barındırma çalışma zamanı API'sini kullanarak çalışma zamanında okuyabilir. Hizmetinizi Azure üzerinde çalışırken bu dosyayı güncelleştirilemiyor.
* **ServiceConfiguration.cscfg** dosya hizmet tanımı dosyasında tanımlanan yapılandırma ayarları için değerleri ayarlar ve her rol için çalıştırılacak örneklerinin sayısını belirtir. Azure'da, bulut hizmetinizin çalışırken bu dosyanın güncelleştirilebilir.

Microsoft Visual Studio için Azure Araçları bu dosyalarda depolanan yapılandırma ayarları ayarlamak için kullanabileceğiniz özellik sayfaları sağlar. Özellik sayfaları erişmek için Çözüm Gezgini'nde, Azure bulut hizmeti projesi altındaki rol başvurusu çift tıklatın veya rol başvurusu sağ tıklatın ve seçin **özellikleri**, aşağıdaki çizimde gösterildiği gibi.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Hizmet yapılandırma dosyalarını ve hizmet tanımı için temel alınan şemaları hakkında daha fazla bilgi için bkz: [şema başvurusu](https://msdn.microsoft.com/library/azure/dd179398.aspx). Hizmet yapılandırması hakkında daha fazla bilgi için bkz: [bulut hizmetlerini yapılandırma](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Rol özelliklerini yapılandırma
Aşağıdaki bölümlerde işaret bazı farklar olsa da bir web rolü ve çalışan rolü için özellik sayfalarını benzerdir.

Gelen **önbelleğe alma** sayfa, Azure Hizmetleri önbelleği yapılandırabilirsiniz.

### <a name="configuration-page"></a>Yapılandırma sayfası
Üzerinde **yapılandırma** sayfasında, bu özellikleri ayarlayabilirsiniz:

**Örnekleri**

Ayarlama **örneği** sayısı, hizmetin, bu rol için çalıştırılması örnek sayısı özelliğine.

Ayarlama **VM boyutu** özelliğine **ek küçük**, **küçük**, **orta**, **büyük**, veya **Çok büyük**.  Daha fazla bilgi için bkz: [Cloud Services boyutları](cloud-services/cloud-services-sizes-specs.md).

**Başlatma eylemi** (Web rolü yalnızca)

Visual Studio hata ayıklama başlattığınızda bir web tarayıcısı için HTTP uç noktaları veya HTTPS uç noktaları ya da hem belirmelidir belirtmek için bu özelliği ayarlayın.

HTTPS uç noktası seçenek, yalnızca zaten rolünüz için bir HTTPS uç noktası tanımladıysanız kullanılabilir. Üzerinde bir HTTPS uç noktası tanımlayabilir **uç noktaları** özellik sayfası.

Bir HTTPS uç nokta zaten eklediyseniz, HTTPS uç noktası seçeneği varsayılan olarak etkindir ve HTTP uç noktanız için bir tarayıcı ek olarak hata ayıklama başlattığınızda, Visual Studio Bu uç noktası için bir tarayıcı açılır. Bu, her iki başlangıç seçeneklerini etkinleştirildiğini varsayar.

**Tanılama**

Varsayılan olarak, tanılama Web rolü için etkinleştirilir. Azure bulut hizmeti projesi ve depolama hesabı yerel depolama öykünücüsünü kullanmak için ayarlanır. Azure'a dağıtmak hazır olduğunuzda Oluşturucu düğmesini seçebilir (**...** ) Azure storage bulutta kullanılacak depolama hesabı güncelleştirilemedi. İsteğe bağlı veya otomatik olarak zamanlanan aralıklarla depolama hesabına tanılama verilerini aktarabilirsiniz. Azure Tanılama hakkında daha fazla bilgi için bkz: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Ayarları sayfası
Üzerinde **ayarları** sayfasında, hizmetiniz için yapılandırma ayarları ekleyebilirsiniz. Yapılandırma, ad-değer çiftleri ayarlardır. Rolünde çalışan kodu okuyabilir, yapılandırma ayarlarınızı tarafından sağlanan sınıflarını kullanarak çalışma zamanında değerlerini [Azure yönetilen kitaplık](http://go.microsoft.com/fwlink?LinkID=171026). Özellikle, [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) yöntemi adlı yapılandırma ayarı çalışma zamanında değerini döndürür.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Bir depolama hesabı bağlantı dizesi yapılandırma
Depolama öykünücüsü veya bir Azure depolama hesabı bağlantısı ve kimlik doğrulama bilgileri sağlayan bir yapılandırma ayarı bir bağlantı dizesidir. Kodunuzu Azure depolama hizmetleri veri – başka bir deyişle, blob, kuyruk veya tablo verisi – bir rolde çalışan kodundan erişim gerekir olduğunda, bu depolama hesabı için bir bağlantı dizesi tanımlayın gerekecektir.

Bir Azure depolama hesabı işaret eden bir bağlantı dizesi, tanımlı bir biçimi kullanmanız gerekir. Bağlantı dizeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Storage bağlantı dizelerini yapılandırma](storage/common/storage-configure-connection-string.md).

Hizmetinizi Azure storage hizmetlerine karşı test etmek hazır olduğunuzda ya da bulut hizmetiniz Azure'a dağıtmak hazır olduğunuzda, Azure depolama hesabınıza işaret edecek şekilde herhangi bir bağlantı dizesi değerini değiştirebilirsiniz. Seçin (**...** ), select **depolama hesabının kimlik bilgilerini girin**. Hesap adı ve hesap anahtarını içeren hesap bilgilerini girin. İçinde **depolama hesabı bağlantı dizesi** iletişim kutusunda, ayrıca belirtebilirsiniz varsayılan HTTPS uç noktaları (varsayılan seçenek), varsayılan HTTP uç noktaları ya da özel uç noktaları kullanmak isteyip istemediğinizi. Özel etki alanı adı, hizmetiniz için kaydolduysanız açıklandığı gibi özel uç noktaları kullanmak karar verebilirsiniz [bir Azure depolama hesabında blob verileri için bir özel etki alanı adı yapılandırma](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> Bağlantı Dizelerinizin hizmetinizi dağıtmadan önce bir Azure depolama hesabı işaret edecek şekilde değiştirmeniz gerekir. Bunu yapmak başarısız olan rolünüze başlatılmamasına veya başlatılırken, meşgul ve durdurma durumları arasında geçiş yapmak için neden olabilir.
> 
> 

## <a name="endpoints-page"></a>Uç noktaları sayfası
Çalışan rolü herhangi bir sayıda HTTP, HTTPS veya TCP uç noktaları olabilir. Uç noktaları, dış istemcilere kullanılabilir giriş uç noktaları veya hizmetinde çalışan diğer rollerin kullanılabilir iç uç noktalar olabilir.

* Bir HTTP uç noktası dış istemcileri ve Web tarayıcılarının değişiklik yapmak için giriş uç noktası türünü değiştirin ve bir ad ve bir ortak bağlantı noktası numarası belirtin.
* Dış istemcilere ve Web tarayıcıları bir HTTPS uç noktası kullanılabilir hale getirmek için uç nokta türü değiştirme **giriş**, bir ad, bir ortak bağlantı noktası numarası ve bir yönetim sertifikası adı belirtin.
  
    Bir yönetim sertifikası belirtebilmeniz için önce sertifika üzerinde tanımlamanız gerektiğini unutmayın **sertifikaları** özellik sayfası.
* Bir uç nokta iç erişim bulut hizmetindeki diğer rolleri tarafından kullanılabilmesi için uç nokta türü için iç değiştirin ve bir ad ve bu uç nokta için olası özel bağlantı noktalarını belirtin.

## <a name="local-storage-page"></a>Yerel depolama sayfası
Kullanabileceğiniz **yerel depolama** bir rol için bir veya daha fazla yerel depolama kaynaklarını ayırmak için özellik sayfası. Yerel depolama kaynağı, bir rol örneği çalıştıran Azure sanal makinesinin dosya sistemindeki ayrılmış bir dizindir.

## <a name="certificates-page"></a>Sertifikalar sayfası
Üzerinde **sertifikaları** sayfasında, sertifikalar rolünüze ile ilişkilendirebilirsiniz. Eklediğiniz sertifikalar HTTPS uç noktalarınızı yapılandırmak için kullanılabilir **uç noktaları** özellik sayfası.

**Sertifikaları** özellik sayfası, hizmet yapılandırması, sertifikalar hakkında bilgi ekler. Hizmetiniz ile sertifikalarınızı paketlenmiş değil unutmayın; sertifikalarınızı ayrı olarak Azure karşıya yüklediğiniz gerekir [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).

Bir sertifika, rolüyle ilişkilendirmek için sertifika için bir ad sağlayın. Üzerinde bir HTTPS uç noktası yapılandırdığınızda sertifikaya başvurmak için bu adı kullanın **uç noktaları** özellik sayfası. Ardından, sertifika deposuna olup olmadığını belirtin **yerel makine** veya **geçerli kullanıcı** ve depo adını. Son olarak, sertifikanın parmak izini girin. Sertifika (My) deposunda geçerli User\Personal içinde ise, sertifika doldurulan bir listeden seçerek sertifikanın parmak izi girebilirsiniz. Başka bir konumda bulunuyorsa, parmak izi değeri el ile girin.

Sertifika deposundan bir sertifikayı eklediğinizde, herhangi bir ara sertifika, yapılandırma ayarlarını otomatik olarak eklenir. Bu Ara sertifikalar da Azure'a hizmetiniz SSL için doğru şekilde yapılandırmak için karşıya yüklenmelidir.

Yalnızca bulutta çalışırken hizmetiniz ile ilişkilendirmek istediğiniz yönetim sertifikaları hizmetiniz için geçerlidir. Hizmetinizi yerel geliştirme ortamında çalışan işlem öykünücüsü tarafından yönetilen standart bir sertifika kullanır.

## <a name="configuring-the-azure-cloud-service-project"></a>Azure bulut hizmeti projesi yapılandırma
Tüm Azure bulut hizmeti projesi için geçerli ayarları yapılandırmak için önce bu proje düğümünün kısayol menüsünü açın ve, özellik sayfalarının açmak için Özellikler'i seçin. Aşağıdaki tabloda, bu özellik sayfaları gösterilmektedir.

| Özellik sayfası | Açıklama |
| --- | --- |
| Uygulama |Bu sayfada, bu bulut hizmeti projesini kullanan ve Araçları'nın geçerli sürüme yükseltebilirsiniz Azure Araçları sürümüyle ilgili bilgileri görüntüleyebilirsiniz. |
| Derleme olayları |Bu sayfadan oluşturma öncesi ve sonrası derleme olaylarını ayarlayabilirsiniz. |
| Geliştirme |Bu sayfadan yapı yapılandırma yönergeleri ve altında oluşturma sonrası olay çalıştığı koşulları belirtebilirsiniz. |
| Web |Bu sayfada, web sunucusu ile ilgili ayarları yapılandırabilirsiniz. |

