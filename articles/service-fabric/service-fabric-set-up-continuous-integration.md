---
title: "Azure mikro için sürekli tümleştirme ayarlama | Microsoft Docs"
description: "Visual Studio Team Services (VSTS) kullanarak sürekli tümleştirme ve dağıtım Service Fabric uygulaması için ayarlama hakkında genel bakış alın."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: ryanwi
ms.openlocfilehash: d73120018fa02a64c2a895a93b43661fccd4e5aa
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Service Fabric sürekli tümleştirme ve Visual Studio Team Services ile dağıtımı ayarlama
Bu makalede, Visual Studio Team Services (VSTS) kullanarak sürekli tümleştirme ve bir Azure Service Fabric uygulaması için dağıtım ayarlama adımlarını açıklar.

Bu belge, geçerli yordamı yansıtır ve zaman içinde değişiklik bekleniyor.

## <a name="prerequisites"></a>Ön koşullar
Başlamak için aşağıdaki adımları izleyin:

1. Bir Team Services hesabına erişimi olduğundan emin olun veya [oluşturmak](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) kendiniz.
2. Team Services takım projesine erişimi olduğundan emin olun veya [oluşturmak](https://www.visualstudio.com/docs/setup-admin/create-team-project) kendiniz.
3. Uygulamanızı dağıtmak veya kullanarak bir tane oluşturun Service Fabric kümesi olduğundan emin olun [Azure portal](service-fabric-cluster-creation-via-portal.md), bir [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md), veya [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).
4. Bir Service Fabric uygulaması (.sfproj) projesi oluşturmuş emin olun. Oluşturulmuş veya Service Fabric SDK 2.1 ya da daha yüksek (.sfproj dosya ProjectVersion özellik değeri 1.1 veya üzeri olmalıdır) yükseltilmiş bir proje olması gerekir.

> [!NOTE]
> Özel derleme aracıları artık gerekli değildir. Team Services şimdi Service Fabric küme yönetimi yazılımını ile önceden yüklenmiş olarak gelen aracıları doğrudan bu aracıların, uygulamaların dağıtımını izin vermeyi barındırılan.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Yapılandırma ve kaynak dosyaları paylaşma
Yapmak istediğiniz ilk Team Services içinde yürütür dağıtım işlemi tarafından kullanım için bir yayımlama profili hazırlama şeydir.  Yayımlama profili, önceden hazırladığınız küme hedefleyecek şekilde yapılandırılmalıdır:

1. Sürekli Tümleştirme iş akışınız için kullanmak istediğiniz uygulama projenizin içinde bir yayımlama profili seçin. İzleyin [yönergeleri yayımlama](service-fabric-publish-app-remote-cluster.md) uzak bir küme için bir uygulamanın nasıl yayımlanacağını üzerinde. Aslında, uygulamanızın ancak yayımlama gerekmez. Tıklayabilirsiniz **kaydetmek** şeyler uygun şekilde yapılandırdıktan sonra Yayımla iletişim kutusunda köprü.
2. Uygulamanızın Team Services içinde oluşan her bir dağıtım için yükseltilecek istiyorsanız, yükseltmeyi etkinleştirmek için yayımlama profili yapılandırmak istiyorsunuz. 1. adımda kullanılan aynı Yayımla iletişim kutusunda, emin **uygulama yükseltme** onay kutusu işaretli.  Daha fazla bilgi edinmek [ek yükseltme ayarlarını yapılandırma](service-fabric-visualstudio-configure-upgrade.md). Tıklatın **kaydetmek** yayımlama profili ayarları kaydetmek için köprü.
3. Değişikliklerinizi yayımlama profili kaydettiğiniz ve yayımlama iletişim kutusunu iptal emin olun.
4. Şimdi süresi [uygulama projesi kaynak dosyaları paylaşma](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) Team Services ile. Kaynak dosyalarınız Team Services içinde erişilebilir olduktan sonra artık derlemeleri oluşturma sonraki adıma geçebilirsiniz. 

## <a name="create-a-build-definition"></a>Yapı tanımı oluşturma
Bir Team Services yapı tanımı sırayla yürütülen derleme adımları kümesinden oluşan bir iş akışını açıklar. Oluşturmakta olduğunuz yapı tanımının bir Service Fabric uygulama paketi ve uygulama dağıtmak için kullanılan diğer yapıları üretmek için hedeftir. Team Services hakkında daha fazla bilgi [yapı tanımları](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-the-build-template"></a>Yapı şablonundan bir tanımı oluşturun
1. Takım projenizin Visual Studio Team Services içinde açın.
2. Seçin **yapı** sekmesi.
3. Yeşil seçin  **+**  yeni derleme tanımı oluşturmak oturum açın.
4. Açılan iletişim kutusunda, seçin **Azure Service Fabric uygulaması** içinde **yapı** şablon kategorisi.
5. Seçin **sonraki**.
6. Depo ve Service Fabric uygulamanızla ilişkili dalı seçin.
7. Kullanmak istediğiniz aracı sıra seçin. Barındırılan aracılar desteklenir.
8. **Oluştur**’u seçin.
9. Yapı tanımı kaydedin ve bir ad sağlayın.
10. Şablon tarafından oluşturulan derleme adımları açıklamasını paragraftır:

| Adım oluşturma | Açıklama |
| --- | --- |
| NuGet geri yükleme |Çözüm için NuGet paketlerini geri yükler. |
| Çözümü derlemek \*.sln |Çözümün tamamında oluşturur. |
| Çözümü derlemek \*.sfproj |Uygulamayı dağıtmak için kullanılan Service Fabric uygulama paketi oluşturur. Uygulama paketi konumu yapı yapı dizin içinde olarak belirtilir. |
| Güncelleştirme hizmeti doku uygulama sürümleri |Bildirim dosyası için yükseltme desteği sağlamak için uygulama paketin içinde yer alan sürümüne güncelleştirir. Bkz: [görev belge sayfasının](https://go.microsoft.com/fwlink/?LinkId=820529) daha fazla bilgi için. |
| Dosyaları kopyalama |Dağıtım için kullanılacak yapı yapılara yayımlama profili ve uygulama parametreleri dosyalarını kopyalar. |
| Yapı yayımlama |Yapı yapıları yayımlar. Yapı yapıları kullanmak bir yayın tanımlanmasına olanak tanır. |

### <a name="verify-the-default-set-of-tasks"></a>Görevler varsayılan kümesini doğrulayın
1. Doğrulama **çözüm** giriş alanını **NuGet restore** ve **yapı çözümü** derleme adımları.  Varsayılan olarak, bu derleme adımları ilişkili deposunda bulunan tüm çözüm dosyalarını üzerine yürütün.  Bu çözüm dosyaları biri üzerinde çalışması için derleme tanımını yalnızca istiyorsanız, açıkça dosya yolunu güncelleştirmeniz gerekir.
2. Doğrulama **çözüm** giriş alanını **Uygulama paketleme** adım oluşturma.  Varsayılan olarak, yalnızca bir Service Fabric uygulaması projesi (.sfproj) deposunda mevcut bu derleme adımı varsayar.  Deponuzda içinde birden çok dosya varsa ve bu derleme tanımı için bunlardan yalnızca birini hedeflemek istediğiniz yolun dosya açıkça güncelleştirmeniz gerekir.  Birden çok uygulama projeleri deponuzun paketini istiyorsanız, ek oluşturmanıza gerek **Visual Studio derleme** her bir uygulama proje hedef yapı tanımında adımları.  Sonra da güncellemeniz gerekir **MSBuild bağımsız değişkenleri** bunların her biri için alan oluşturmak adımları paket konumu bunların her biri için benzersiz olmasını sağlayın.
3. Tanımlanan sürüm oluşturma davranışı doğrulayın **güncelleştirme Service Fabric uygulaması sürümleri** adım oluşturma.  Varsayılan olarak, bu derleme adımı yapı numarası uygulama paket bildirim dosyalarında tüm sürüm değerler ekler. Bkz: [görev belge sayfasının](https://go.microsoft.com/fwlink/?LinkId=820529) daha fazla bilgi için. Bu, her bir yükseltme dağıtımı önceki dağıtımından farklı sürüm değerleri gerektirdiğinden, uygulamanızın yükseltme desteklemek için yararlıdır. Uygulama yükseltmesi, iş akışında kullanmak üzere amaçlanmıyorsa, bu derleme adımı devre dışı bırakma düşünebilirsiniz. Var olan bir Service Fabric uygulamasını üzerine yazmak için kullanılan bir derleme üretme varsa devre dışı bırakılmalıdır. Yapı tarafından üretilen uygulama sürümü kümedeki uygulama sürümü eşleşmezse, dağıtım başarısız olur.
4. Çözümünüzü .NET Core proje içeriyorsa, yapı tanımınızı bağımlılıkları yükler bir derleme adımı içerdiğinden emin olun gerekir:
   1. Seçin **derleme adımı Ekle...** .
   2. Bulun **komut satırı** yardımcı programı sekmede görev ve onun Ekle düğmesini tıklatın.
   3. Görev için giriş alanları, aşağıdaki değerleri kullanın:
   4. Aracı: dotnet
   5. Bağımsız değişkenler: geri yükleme
   6. Hemen sonra olmasını sağlamak görev sürükleyin **NuGet restore** adım.
5. Derleme tanımı için yaptığınız değişiklikleri kaydedin.

### <a name="try-it"></a>Deneyin
Seçin **Yapıyı Sıraya Al** bir yapı el ile başlatmak için. Ayrıca Tetikleyiciler İtme veya iade oluşturur. Derleme başarıyla yürütüyor doğruladıktan sonra artık uygulamanızı bir kümeye dağıtan bir yayın tanımı tanımlamak için geçebilirsiniz.

## <a name="create-a-release-definition"></a>Bir yayın tanımı oluşturun
Bir Team Services sürüm tanımı sırayla yürütülen görevler kümesinden oluşan bir iş akışını açıklar. Oluşturmakta olduğunuz yayın tanımı bir uygulama paketi alıp bir kümeye dağıtma hedefidir. Birlikte kullanıldığında, derleme tanımı ve yayın tanımının tüm iş akışının kümenizde çalışan bir uygulama ile biten kaynak dosyaları ile başlayan yürütebilir. Team Services hakkında daha fazla bilgi [yayın tanımları](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-the-release-template"></a>Yayın şablonu bir tanımı oluşturun
1. Projenizi Visual Studio Team Services içinde açın.
2. Seçin **sürüm** sekmesi.
3. Yeşil seçin  **+**  yeni bir sürüm tanımı oluşturun ve seçmek oturum **oluşturma yayın tanımı** menüde.
4. Açılan iletişim kutusunda, seçin **Azure Service Fabric dağıtımı** içinde **dağıtım** şablon kategorisi.
5. Seçin **sonraki**.
6. Bu sürüm tanımı kaynağı olarak kullanmak istediğiniz yapı tanımı'nı seçin.  Seçili yapı tanımı tarafından üretilmiş olan yapıları yayın tanımına başvurur.
7. Denetleme **sürekli dağıtım** takım hizmetleri otomatik olarak yeni bir sürüm oluşturmak ve bir yapı tamamlandıktan her Service Fabric uygulaması dağıtmak isterseniz, kutuyu.
8. Kullanmak istediğiniz aracı sıra seçin. Barındırılan aracılar desteklenir.
9. **Oluştur**’u seçin.
10. Sayfanın üstündeki kalem simgesine tıklayarak tanımı adını düzenleyin.
11. İçin uygulamanızın dağıtılmalıdır gelen kümeyi seçin **küme bağlantısı** giriş alanı görev. Küme bağlantısı kümeye bağlanmak dağıtım görev sağlayan gerekli bilgileri sağlar. Bir küme bağlantısı kümeniz için henüz yoksa, seçin **Yönet** eklemesini alanın yanındaki köprü. Açılır sayfasında, aşağıdaki adımları gerçekleştirin:
    1. Seçin **yeni hizmet uç noktası** ve ardından **Azure Service Fabric** menüsünde.
    2. Bu bitiş noktası tarafından hedeflenen küme tarafından kullanılan kimlik doğrulama türünü seçin.
    3. Bağlantınız için bir ad tanımlayın **bağlantı adı** alan.  Genellikle, kümenin adını kullanın.
    4. İstemci Bağlantı uç nokta URL'sini tanımlamak **küme uç noktası** alan.  Örnek: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Azure Active Directory kimlik bilgileri kümeye bağlanmak için kullanmak istediğiniz kimlik bilgilerini tanımlamak **kullanıcıadı** ve **parola** alanları.
    6. Sertifika tabanlı kimlik doğrulaması için istemci sertifika dosyası Base64 kodlaması tanımlamak **istemci sertifikası** alan.  Bu değer alma konusunda bilgi için bu alanı açılır yardımına bakın.  Sertifikanız parola korumalıysa parolayı tanımlayın **parola** alan.
    7. Yaptığınız değişiklikleri onaylamak **Tamam**. Yayın tanımınızı geri gezinme sonra Yenile simgesine tıklayın **küme bağlantısı** eklediğiniz uç nokta görmek için alanı.
12. Yayın tanımını kaydedin.

> [!NOTE]
> Microsoft Accounts (örneğin, @hotmail.com veya @outlook.com) Azure Active Directory kimlik doğrulaması ile desteklenmez.
> 
> 

Türünde bir görevi oluşturulur tanımı oluşur **Service Fabric uygulama dağıtımı**. Bkz: [görev belge sayfasının](https://go.microsoft.com/fwlink/?LinkId=820528) bu görev hakkında daha fazla bilgi için.

### <a name="verify-the-template-defaults"></a>Şablon Varsayılanları doğrulayın
1. Doğrulama **yayımlama profili** giriş alanını **Service Fabric uygulaması dağıtma** görev. Varsayılan olarak, bu alan yapı yapıtlarla birlikte bulunan Cloud.xml adlı bir yayımlama profili başvurur. Farklı bir yayımlama profili başvurmak istiyorsanız ya da derleme yapılarının birden çok uygulama paketlerinde içeriyorsa yolunu uygun şekilde güncelleştirmeniz gerekir.
2. Doğrulama **uygulama paketi** giriş alanını **Service Fabric uygulaması dağıtma** görev. Varsayılan olarak, bu alana derleme tanımı şablonda kullanılan varsayılan uygulama paket yolu başvurur.  Yapı tanımı'ndaki varsayılan uygulama paket yolu değiştirdiyseniz yolunu uygun şekilde burada da güncelleştirmeniz gerekir.

### <a name="try-it"></a>Deneyin
Seçin **oluşturma yayın** gelen **yayın** el ile bir sürüm oluşturmak için düğmesi menüsü. Açılan iletişim kutusunda, yayın temel ve ardından istediğiniz yapıyı **oluşturma**. Sürekli dağıtım etkinleştirilirse, ilişkili derleme tanımı bir yapı tamamlandığında sürümleri otomatik olarak oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric uygulamaları ile sürekli tümleştirme hakkında daha fazla bilgi için aşağıdaki makaleler okuyun:

* [Team Services belgelerine giriş](https://www.visualstudio.com/docs/overview)
* [Yapılandırma yönetimi Team Services](https://www.visualstudio.com/docs/build/overview)
* [Yayın Yönetimi Team Services](https://www.visualstudio.com/docs/release/overview)

