<properties
    pageTitle="Azure App Service’deki Node.js API uygulaması | Microsoft Azure"
    description="Node.js RESTful API’si oluşturma ve Azure App Service’deki bir API uygulamasına dağıtma hakkında bilgi edinin."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="bradygaster"/>

# Node.js RESTful API’si derleme ve Azure’daki bir API uygulamasına dağıtma

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Bu öğreticide [Azure App Service](../app-service/app-service-value-prop-what-is.md)’de [Git](http://git-scm.com) kullanarak nasıl basit bir [Node.js](http://nodejs.org) API’si oluşturulacağı ve bir [API uygulamasına](app-service-api-apps-why-best-platform.md) dağıtılacağı gösterilmektedir. Node.js çalıştırabilen herhangi bir işletim sistemi kullanabilirsiniz ve tüm çalışmanızı cmd.exe veya bash gibi komut satırı araçlarını kullanarak yaparsınız.

## Ön koşullar

1. Microsoft Azure hesabı ([buradan ücretsiz bir hesap açın](https://azure.microsoft.com/pricing/free-trial/))
1. Yüklü [Node.js](http://nodejs.org) (bu örnekte Node.js sürüm 4.2.2’nin yüklü olduğu varsayılır)
2. Yüklü [Git](https://git-scm.com/)
1. [GitHub](https://github.com/) hesabı

App Service kodunuzun bir API uygulamasına dağıtılması için birçok yolu desteklese de bu öğretici Git yöntemini göstermekte ve Git ile nasıl çalışılacağı konusunda temel bilgiye sahip olduğunuzu varsaymaktadır. Diğer dağıtım yöntemleri hakkında daha fazla bilgi için bkz. [Uygulamanızı Azure App Service’e dağıtma](../app-service-web/web-sites-deploy.md).

## Örnek kodunu alma

1. Node.js ve Git komutları çalıştırabilen bir komut satırı arabirimi açın.

1. Yerel Git deposu için kullanabileceğiniz bir klasöre gidin ve [örnek kodunu içeren GitHub deposunu](https://github.com/Azure-Samples/app-service-api-node-contact-list) kopyalayın.

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Örnek API iki uç nokta sağlar: `/contacts` hedefine yapılan Get isteği bir JSON biçiminde bir ad ve e-posta adresi listesi döndürürken, `/contacts/{id}` yalnızca seçilen kişiyi döndürür.

## Swagger meta verilerine göre Node.js kodu iskelesini kurma (otomatik oluşturma)

[Swagger](http://swagger.io/) bir RESTful API’sini açıklayan meta verilere yönelik dosya biçimidir. Azure App Service, [Swagger meta verileri için yerleşik desteğe](app-service-api-metadata.md) sahiptir. Öğreticinin bu bölümü, içinde ilk olarak Swagger meta verilerini oluşturduğunuz ve bunları API sunucu kodunun iskelesini kurmak (otomatik oluşturmak) için kullandığınız bir API geliştirme iş akışını modeller. 

>[AZURE.NOTE] Swagger meta veri dosyasından bir Node.js kodu iskelesinin nasıl kurulacağını öğrenmek istemiyorsanız bu bölümü atlayabilirsiniz. Yalnızca örnek kodunu yeni bir API uygulamasına dağıtmak istiyorsanız doğrudan [Azure'da API uygulaması oluşturma](#createapiapp) bölümüne gidin.

### Swaggerize yükleme ve yürütme

1. **yo** ve **generator-swaggerize** NPM modüllerini genel olarak yüklemek için aşağıdaki komutları yürütün.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize, Swagger meta veri dosyası tarafından tanımlanan bir API için sunucu kodu oluşturan bir araçtır. Kullanacağınız Swagger dosyasının adı *api.json*’dur ve kopyaladığınız deponun *start* klasöründe bulunur.

2. *start* klasörüne gidin ve ardından `yo swaggerize` komutunu yürütün. Swaggerize bir dizi soru sorar.  **Bu projenin adı** alanına "contactlist" yazın, **swagger belgesinin yolu** alanına "api.json" yazın ve **Express, Hapi veya Restify** alanına "express" seçeneğini girin.

        yo swaggerize

    ![Swaggerize Komut Satırı](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Not**: bu adımda bir hatayla karşılaşırsanız nasıl düzeltileceği sonraki adımda açıklanmaktadır.

    Swaggerize bir uygulama klasörü oluşturur, işleyicilerin ve yapılandırma dosyalarının iskelesini kurar ve bir **package.json** dosyası oluşturur. Swagger yardım sayfasını oluşturmak için hızlı görünüm altyapısı kullanılır.  

3. `swaggerize` komutu "beklenmeyen belirteç" veya "geçersiz kaçış dizisi" hatası ile başarısız olursa oluşturulan *package.json* dosyasını düzenleyerek hatanın nedenini düzeltin. `scripts` altındaki `regenerate` satırında satırın aşağıdaki örnekteki gibi görünmesi için *api.json*’dan önceki ters eğik çizgiyi eğik çizgi olarak değiştirin:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. İskele kurulan kodu içeren klasöre gidin (bu örnekte *ContactList* alt klasörü).

1. `npm install` öğesini çalıştırın.
    
        npm install
        
2. **jsonpath** NPM modülünü yükleyin. 

        npm install --save jsonpath
        
    ![Jsonpath Yükleme](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. **swaggerize-ui** NPM modülünü yükleyin. 

        npm install --save swaggerize-ui
        
    ![Swaggerize Ui Yükleme](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### İskelesi kurulan kodu özelleştirme

1. **lib** klasörünü **start** klasöründen iskele kurucu tarafından oluşturulan **ContactList** klasörüne kopyalayın. 

1. **handlers/contacts.js** dosyasındaki kodu aşağıdaki kodla değiştirin. 

    Bu kod **lib/contactRepository.js** tarafından sunulan **lib/contacts.json** dosyasına depolanmış JSON verilerini kullanır. Yeni contacts.js kodu tüm kişileri almak ve bunları bir JSON yükü olarak döndürmek için HTTP isteklerine yanıt verir. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. **handlers/contacts/{id}.js** dosyasındaki kodu aşağıdaki kodla değiştirin. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. **server.js** içindeki kodu aşağıdaki kodla değiştirin. 

    Server.js dosyasında yapılan değişiklikler, yorumlar kullanılarak çağrıldığı için yapılan değişiklikleri görebilirsiniz. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### Yerel olarak çalışan API ile test etme

1. Node.js komut satırı yürütülebilir dosyasını kullanarak sunucuyu etkinleştirin. 

        node server.js

1. **http://localhost:8000/contacts** adresine göz atarken kişi listesinin JSON çıktısını görürsünüz (veya tarayıcınıza bağlı olarak indirmeniz istenir). 

    ![Tüm Kişiler Api Çağrısı](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. **http://localhost:8000/contacts/2** adresine göz atarken o kimlik değerinin temsil ettiği kişiyi görürsünüz.

    ![Belirli Kişi Api Çağrısı](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger JSON verileri **/swagger** uç noktası aracılığıyla sunulur:

    ![Kişiler Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Swagger kullanıcı arabirimi **/docs** uç noktası aracılığıyla sunulur. Swagger kullanıcı arabiriminde API'nizi test etmek için zengin HTML istemci özelliklerini kullanabilirsiniz.

    ![Swagger kullanıcı arabirimi](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> Yeni bir API uygulaması oluşturma

Bu bölümde Azure portalı kullanarak Azure içinde yeni bir API uygulaması oluşturursunuz. Bu API uygulaması kodunuzu çalıştırmak için Azure tarafından sağlanacak işlem kaynaklarını temsil eder. Sonraki bölümlerde kodunuzu yeni API uygulamasına dağıtacaksınız.

1. [Azure Portal](https://portal.azure.com/)’a göz atın. 

1. **Yeni > Web + Mobil > API uygulaması**’na tıklayın. 

    ![Portalda yeni API uygulaması](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. *azurewebsites.net* etki alanında NodejsAPIApp ve onu benzersiz kılacak bir sayı gibi benzersiz bir **Uygulama adı** girin. 

    Örneğin, adı `NodejsAPIApp` ise URL `nodejsapiapp.azurewebsites.net` olur.

    Başka birinin önceden kullandığı bir adı girerseniz, sağda kırmızı bir ünlem işareti görürsünüz.

6. **Kaynak Grubu** açılır menüsünde **Yeni**’ye tıklayın ve ardından **Yeni kaynak grubu adı** içinde "NodejsAPIAppGroup" ya da tercih ederseniz başka bir ad girin. 

    [Kaynak grubu](../azure-portal/resource-group-portal.md) API uygulamaları, veritabanları ve sanal makineler gibi Azure kaynakları koleksiyonudur. Bu öğreticide, en iyi uygulama yeni bir kaynak grubu oluşturulmasıdır; böylece, öğretici için oluşturduğunuz Azure kaynaklarını tek bir adımda kolayca silebilirsiniz.

4. **App Service Planı/Konumu** ve ardından **Yeni Oluştur** öğesine tıklayın.

    ![App Service planı oluşturma](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    Aşağıdaki adımlarda yeni kaynak grubu için bir App Service planı oluşturursunuz. App Service planı, API uygulamanızın üzerinde çalışacağı işlem kaynaklarını belirtir. Örneğin, ücretsiz katmanı seçerseniz API uygulamanız paylaşılan sanal makineler üzerinde çalışır, bazı ücretli katmanlarda ise ayrılmış sanal makineler üzerinde çalışır. App Service planları hakkında bilgi için bkz. [App Service planlarına genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. **App Service Planı** dikey penceresinde "NodejsAPIAppPlan" veya tercih ederseniz başka bir ad girin.

5. **Konum** açılır listesinde, size en yakın konumu seçin.

    Bu ayar, uygulamanızın hangi Azure veri merkezinde çalıştırılacağını belirtir. Bu öğretici için herhangi bir bölgeyi seçebilirsiniz, gözle görülür bir fark yaratmayacaktır. Bununla birlikte bir üretim uygulaması için [gecikmeyi](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090) en aza indirmek amacıyla sunucunuzun eriştiği istemcilere mümkün olduğunca yakın olmasını sağlayabilirsiniz.

5. **Fiyatlandırma katmanı > Tümünü Görüntüle > F1 Ücretsiz** öğesine tıklayın.

    Bu öğretici için ücretsiz fiyatlandırma katmanı yeterli ölçüde performans sunacaktır.

    ![Ücretsiz fiyatlandırma katmanı seçme](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. **App Service Planı** dikey penceresinde **Tamam**’a tıklayın.

7. **API uygulaması** dikey penceresinde **Oluştur**’a tıklayın.

## Yeni API uygulamanızı Git dağıtımı için ayarlama

İşlemeleri Azure UApp Service’dei bir Git deposuna ileterek kodunuzu API uygulamasına dağıtırsınız. Öğreticinin bu bölümünde Azure’da dağıtım için kullanacağınız kimlik bilgilerini ve Git deposunu oluşturursunuz.  

1. API uygulamanız oluşturulduktan sonra portal giriş sayfasında **App Services > {API uygulamanız}** öğesine tıklayın. 

    Portalda **API uygulaması** ve **Ayarlar** dikey pencereleri gösterilir.

    ![Portal API uygulaması ve Ayarlar dikey penceresi](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. **Ayarlar** dikey penceresinde **Yayımlama** bölümüne inin ve ardından **Dağıtım kimlik bilgileri** öğesine tıklayın.
 
3. **Dağıtım kimlik bilgilerini ayarla** dikey penceresinde bir kullanıcı adı ve parola girip **Kaydet** öğesine tıklayın.

    Bu kimlik bilgilerini Node.js kodunuzu API uygulamasına yayımlarken kullanacaksınız. 

    ![Dağıtım Kimlik Bilgileri](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. **Ayarlar** dikey penceresinde **Dağıtım kaynağı > Kaynak Seç > Yerel Git Deposu**’na ve ardından **Tamam**’a tıklayın.

    ![Git Deposu Oluşturma](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Git deponuz oluşturulduktan sonra dikey pencere etkin dağıtımlarınızı gösterecek şekilde değişir. Depo yeni olduğundan listede etkin bir dağıtımınız yoktur. 

    ![Etkin Dağıtım Yok](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Git deposu URL'sini kopyalayın. Bunu yapmak için yeni API uygulamanızın dikey penceresine gidin ve dikey pencerenin **Temel Bilgiler** bölümüne bakın. **Temel Bilgiler** bölümündeki **Git kopyalama URL'si** seçeneğine dikkat edin. Bu URL’nin üzerine geldiğinizde sağ tarafta URL’yi panonuza kopyalayacak bir simge görürsünüz. URL'yi kopyalamak için bu simgeye tıklayın.

    ![Portal’dan Git URL'sini Alma](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Not**: O anda bir yere kaydettiğinizden emin olmak için sonraki bölümde verilen Git kopyalama URL’si gerekli olacaktır.

Yedekleyen bir Git deposu ile birlikte bir API uygulaması oluşturduğunuza göre kodu API uygulamasına dağıtmak için depoya iletebilirsiniz. 

## API kodunuzu Azure’a dağıtma

Bu bölümde API için sunucu kodunuzu içeren yerel bir Git deposu oluşturursunuz ve ardından kodunuzu bu depodan Azure’da daha önce oluşturduğunuz depoya iletirsiniz.

1. `ContactList` klasörünü yeni yerel Git deposu için kullanabileceğiniz bir konuma kopyalayın. Öğreticinin ilk bölümünü tamamladıysanız `start` klasöründen `ContactList` öğesini kopyalayın; aksi takdirde `end` klasöründen `ContactList` öğesini kopyalayın.

1. Komut satırı aracınızda yeni klasöre gidin ve ardından yeni bir yerel Git deposu oluşturmak üzere aşağıdaki komutu yürütün. 

        git init

     ![Yeni Yerel Git Deposu](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. API uygulamanızın deposu için uzak Git eklemek üzere aşağıdaki komutu yürütün. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Not**: "YOUR_GIT_CLONE_URL_HERE" dizesini daha önce kopyaladığınız kendi Git kopyalama URL’niz ile değiştirin. 

1. Kodunuzun tamamını içeren bir işleme oluşturmak üzere aşağıdaki komutları yürütün. 

        git add .
        git commit -m "initial revision"

    ![Git İşleme Çıktısı](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Kodunuzu Azure'a iletmek için komutu yürütün. Bir parola istendiğinde Azure portalında daha önce oluşturduğunuz parolayı girin.

        git push azure master

    Bunun yapılması API uygulamanıza dağıtımı başlatır.  

1. Tarayıcınızda API uygulamanızın **Dağıtımlar** dikey penceresine geri gittiğinizde dağıtımın gerçekleşmekte olduğunu görürsünüz. 

    ![Dağıtım Gerçekleşiyor](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Aynı anda komut satırı arabirimi, gerçekleştiği sırada dağıtımınızın durumunu yansıtır. 

    ![Node Js Dağıtımı Gerçekleşiyor](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Dağıtım tamamlandıktan sonra **Dağıtımlar** dikey penceresinde kod değişikliklerinizin API uygulamasına başarıyla dağıtıldığı gösterilir. 

## Azure’da çalışan API ile test etme
 
3. API Uygulaması dikey pencerenizin **Temel Bilgiler** bölümündeki **URL**’yi kopyalayın. 

    ![Dağıtım Tamamlandı](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Postman veya Fiddler gibi bir REST API istemcisi (ya da web tarayıcınızı) kullanarak, kişi API çağrınızın API uygulaması `/contacts` uç noktası olan URL’sini sağlayın. URL şu şekilde olacaktır: `https://{your API app name}.azurewebsites.net/contacts`

    Bu uç noktaya bir GET isteği gönderdiğinizde API uygulamanızın JSON çıktısını alırsınız.

    ![Postman Bulma Api’si](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. Bir tarayıcıda `/docs` uç noktasına giderek Azure'da çalışan Swagger kullanıcı arabirimini deneyin.

Şu anda kesintisiz teslim yapıldığına göre kod değişiklikleri yapabilir ve bunları Azure Git deponuza işlemeleri ileterek Azure’a dağıtabilirsiniz.

## Sonraki adımlar

Bu noktada bir API uygulamasını başarıyla oluşturdunuz ve Node.js API kodunu dağıttınız. Sonraki öğreticide [CORS kullanarak JavaScript istemcilerinden API uygulamalarını kullanma](app-service-api-cors-consume-javascript.md) işlemi gösterilmektedir.



<!----HONumber=Jun16_HO2-->


