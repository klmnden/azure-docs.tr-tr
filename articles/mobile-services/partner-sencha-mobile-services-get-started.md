---
title: Azure Mobile Services ve Sencha Kullanmaya Başlama
description: Mobile Services ve Sencha HTML5 mobil uygulama çerçevesi ile geliştirmeye başlamak için bu öğreticiyi izleyin.
services: mobile-services
documentationcenter: ''
author: ggailey777
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-sencha
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: glenga

---
# <a name="getting-started"> </a>Mobile Services ve Sencha Touch kullanmaya başlama
[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

## Genel Bakış
Bu öğretici Azure Mobile Services’i Sencha Touch uygulamanızda nasıl kullanacağınızı göstermektedir. Klasik Azure portalı üzerinden tanımladığınız bir mobil hizmet kullanan Sencha Touch kullanarak basit bir *Yapılacaklar Listesi* oluşturacaksınız. Bu öğretici JavaScript konusunda yeterli bilgiye sahip olan ve Sencha Touch çerçevesini bilen orta ile ileri düzeyde web uygulaması geliştiricileri için tasarlanmıştır.

Bir video izlemeyi tercih ederseniz, bu klip de bu öğretici ile aynı adımları izlemektedir. Videoda Arthur Kay bir Azure Mobile Services arka ucu kullanarak Sencha Touch uygulamasının nasıl oluşturulacağını açıklamaktadır.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-Windows-Azure-for-Sencha-Touch/player]
> 
> 

Tamamlanmış uygulamanın bir ekran görüntüsü aşağıda gösterilmiştir:

![][0]

## Gereksinimler
* [Sencha Touch](http://wwww.sencha.com/products/touch/download" target="_blank") uygulamasını indirip yükleyin.
* [Sencha Cmd Aracı](http://www.sencha.com/products/sencha-cmd/download" target="_blank")’nı indirip yükleyin.
* Java Çalışma Zamanı Ortamı (JRE) veya Java Geliştirme Seti (Android uygulamaları oluşturuyorsanız)
* Ruby ve SASS gem.

## <a name="create-new-service"> </a>Yeni bir mobil hizmet oluşturma
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## TodoItems Tablosu oluşturma
Mobil hizmetinizi oluşturduktan sonra klasik Azure portalındaki kolay hızlı başlangıcı izleyerek mobil hizmetinizde kullanılacak yeni bir veritabanı tablosu oluşturabilirsiniz.

1. [Klasik Azure Portalı]’nda, **Mobile Services**’a ve ardından yeni oluşturduğunuz mobil hizmete tıklayın.
2. Hızlı başlangıç sekmesindeki **Platform seçin** altında **HTML**’e tıklayın ve **Yeni HTML uygulaması oluştur** seçeneğini genişletin.
   
    ![Mobil hızlı başlangıç html](./media/partner-sencha-mobile-services-get-started/mobile-portal-quickstart-html.png)
   
    Burada, mobil hizmetinize bağlanan bir HTML uygulaması barındırmanın üç kolay adımı gösterilmiştir.
   
    ![Mobil hızlı başlangıç html](./media/partner-sencha-mobile-services-get-started/mobile-quickstart-steps-html.png)
3. Uygulama verilerini depolamak üzere bir tablo oluşturmak için **TodoItems tablosu oluştur**’a tıklayın.
   
   > [!NOTE]
   > Klasik Azure portalından HTML uygulamasını İNDİRMEYİN. Bunun yerine, Sencha Touch uygulamasını aşağıdaki bölümde el ile oluşturacağız.
   > 
   > 
4. Klasik Azure portalındaki **appKey** ve **appUrl** öğelerini not edin. Bunları bu öğreticinin diğer bölümlerinde kullanacaksınız.
   
    ![app key](./media/partner-sencha-mobile-services-get-started/mobile-app-key-portal.png)
5. **Yapılandır** sekmesinde `localhost` öğesinin **Çıkış noktaları arası kaynak paylaşma (CORS)** altındaki **Ana bilgisayar adlarından gelen isteklere izin ver** listesinde zaten olduğunu doğrulayın. Listede yoksa, **ana bilgisayar adı** alanına `localhost` yazın ve ardından **Kaydet**’e tıklayın.
   
    ![Localhost için CORS kurulumu](./media/partner-sencha-mobile-services-get-started/mobile-services-set-cors-localhost.png)

## Touch uygulamanızı oluşturma
Sencha Touch şablon uygulamasının oluşturulması Sencha Cmd kullanılarak gerçekleştirilen basit bir görevdir ve bir uygulamayı hızlıca çalışır duruma getirmenin harika bir yoludur.

Touch çerçevesini yüklediğiniz dizinde aşağıdaki komutu yürütün:

    $ sencha generate app Basic /path/to/application

Bunun yapılması uygulama adı 'Temel' olan bir şablon Touch uygulaması oluşturur. Uygulamanızı başlatmak için tarayıcınızı /path/to/application dizinine yönlendirin; standart Touch örnek uygulaması sunulacaktır.

## Azure için Sencha Touch Uzantılarını yükleme
Azure uzantısı el ile veya bir Sencha Paketi halinde yüklenir. Kullandığınız yöntem tamamen size bağlıdır.

### El ile yükleme
Birçok Touch uygulamasında bir dış sınıf kitaplığı eklemek isterseniz paketi indirmeniz, uygulama dizininizde açmanız ve Touch yükleyicisini kitaplığın konumu ile yapılandırmanız yeterlidir.

Aşağıdaki adımları kullanarak Azure uzantılarını uygulamanıza el ile ekleyebilirsiniz:

1. Azure uzantıları paketini [buradan](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure) indirin. (Bu alana erişmek için Sencha Forumları Kimliğinizi kullanabilirsiniz.)
2. Azure uzantı paketini indirme dizininden son olarak kalmasını istediğiniz yere kopyalayın ve paketinden çıkarın:
   
        $ cd /path/to/application
        $ mv /download-location/azure.zip .
        $ unzip azure.zip
   
    Bunun yapılması tüm paket kaynağını, örneklerini ve belgelerini içeren bir **azure** dizini oluşturur. Kaynak **azure/src** dizininde kalır.

### Sencha paketi olarak yükleme
> [!NOTE]
> Yalnızca uygulamanızı <code>sencha generate app</code> komutu ile oluşturduğunuzda bu yöntemi kullanabilirsiniz.
> 
> 

Sencha Cmd tarafından oluşturulan tüm uygulamaların kökünde "paketler" klasörü bulunur. Bu klasörün konumu yapılandırılabilir, ancak konumundan bağımsız olarak "paketler" klasörünün rolü uygulamanız (veya bir Sencha Çalışma Alanı oluşturduysanız uygulamalarınız) tarafından kullanılan tüm paketlerin deposu olarak görev yapmaktır.

Ext.Azure bir Sencha Cmd "paketi" olduğundan kaynak kodu Sencha Cmd kullanılarak kolayca yüklenebilir ve uygulamanıza eklenebilir. (Daha fazla bilgi için bkz. [Sencha Cmd Paketleri](http://docs.sencha.com/cmd/6.x/cmd_packages/cmd_packages.html)).

Azure uzantı paketini Sencha Paketleri deposundan indirip yüklemek için paket adını **app.json** dosyanıza ekleyip uygulamanızı oluşturmanız gerekir:

1. Azure paketini app.json dosyanızın gerekli bölümüne ekleyin:
   
        {
            "name": "Basic",
            "requires": [
                "touch-azure"
            ]
        }
2. Paketi getirip yüklemek için **sencha cmd** kullanarak uygulamanızı yeniden oluşturun:
   
        $ sencha app build

Bu durumda hem **sencha app build** hem de **sencha app refresh** komutu, paketi uygulamanızla bütünleştirmek için gereken adımları gerçekleştirecektir. Genellikle, paket gereksinimlerini değiştirdikten sonra “geliştirme modunu” desteklemek için gerekli meta verilerin güncel olabilmesi için **sencha app refresh** komutunu çalıştırmanız gerekir.

Hangi komutu çalıştırdığınıza bakılmaksızın, Sencha Cmd paketi indirir ve "paketler" klasöründe genişletir. Bundan sonra çalışma alanınızda bir "packages/touch-azure" klasörü bulabilirsiniz.

## Azure’u dahil etme ve yapılandırma
**Dosya adı**: app.js

Azure uzantısı indirilip uygulama dizininize yüklendiğine göre sonraki adım, uygulamanıza kaynak dosyaları nerede bulacağını ve bu dosyaları istemesini söylemeyi içerir:

1. Sencha Yükleyicisini kaynak kodunun konumu ile yapılandırın:
   
        Ext.Loader.setConfig({
            enabled : true,
            paths   : {
                'Ext'       : 'touch/src',
                'Ext.azure' : '/path-to/azure-for-touch/azure/src'
            }
        });
2. Azure sınıf dosyalarını isteyin:
   
        Ext.application({
   
            requires: [ 'Ext.azure.Azure' ],
   
            // ...
   
        });
3. Azure yapılandırma
   
    Azure paketi, uygulamanızın başlatma bölümünde **Ext.Azure.init** yöntemi çağrılarak başlatılır. Bu yöntem, mobil hizmet kimlik bilgilerinin yanı sıra kullanmak istediğiniz diğer kimlik bilgilerini ve özellikleri içeren bir yapılandırma nesnesinden geçirilir.
   
    Yapılandırma nesnesini doğrudan init yöntemine geçirebilmenize karşın, **azure** adlı bir Sencha uygulama yapılandırma özelliği oluşturmanız ve tüm uygun bilgileri buraya yerleştirmeniz önerilir. Ardından bu özellik değerini Ext.Azure.init yöntemine geçirebilirsiniz.
   
    Azure’da bir mobil hizmet oluşturduğunuzda (bkz. [Azure Kullanmaya Başlama](http://senchaazuredocs.azurewebsites.net/#!/guide/getting_started)) ilgili hizmete bir uygulama anahtarı ve URL atanır. Azure paketinin hizmetinize bağlanabilmesi için bu bilgiler sağlanmalıdır.
   
    Bu örnekte çok basit bir Azure yapılandırması ve yalnızca uygulama anahtarı ve URL sağlayan başlatma işlemi gösterilmektedir:
   
        Ext.application({
            name: 'Basic',
   
            requires: [ 'Ext.azure.Azure' ],
   
            azure: {
                appKey: 'myazureservice-access-key',
                appUrl: 'myazure-service.azure-mobile.net'
            },
   
            launch: function() {
   
                // Call Azure initialization
   
                Ext.Azure.init(this.config.azure);
   
           }
        });
   
    Azure yapılandırma seçenekleri hakkında daha fazla bilgi için lütfen Ext.Azure API belgelerine başvurun.

Tebrikler! Uygulamanızı şimdi mobil hizmetinize erişebilir.

## ToDo uygulaması oluşturma
Uygulamanızı Azure uzantısı içerecek şekilde yapılandırıp mobil hizmet kimlik bilgilerinizi sağladığınıza göre, hizmete depolanmış ToDo listesi verilerini görüntülemek ve düzenlemek için mobil hizmetinizden yararlanan bir Touch uygulaması oluşturma işlemine geçebiliriz.

### Azure veri proxy’sini yapılandırma
**Dosya adı:** app/model/TodoItem.js

Touch uygulamanız mobil hizmetiniz ile bir veri proxy’si üzerinden iletişim kuracaktır. Proxy hem istekleri mobil hizmete gönderme hem de istekleri mobil hizmetten alma işlerinin tamamını gerçekleştirir. Bir Touch veri modeli ve deposu ile birlikte kullanıldığında uzak verileri işleme ve uygulamanıza alma ile ilgili tüm zorlu işler Touch tarafından kaldırılır ve gerçekleştirilir.

Sencha Touch modelleri uygulamanızda kullanacağınız veri kayıtlarının tanımını sağlar. Bunlar yalnızca veri alanları tanımlamanızı sağlamaz, aynı zamanda uygulama ile Azure mobil hizmeti arasındaki iletişimi gerçekleştiren proxy ile ilgili yapılandırma sağlar.

Aşağıdaki kodda modelin alanlarını (ve türlerini) tanımladığımızı ve ayrıca bir proxy yapılandırması sağladığımızı görebilirsiniz. Proxy’nizi yapılandırırken ona bir tür (bu örnekte 'azure'), mobil hizmeti tablo adı (ToDoItem) ve diğer isteğe bağlı parametreleri atamanız gerekir. Bu örnekte liste öğeleri arasında sorunsuzca ileri ve geri gidebilmemiz için proxy sayfalama özelliğini etkinleştireceğiz.

Azure proxy tüm HTTP üst bilgilerini Azure API’si tarafından beklenen uygun CRUD işlemleri ile ayarlar (varsa kimlik doğrulama kimlik bilgileri dahil).

    Ext.define('Basic.model.TodoItem', {
        extend : 'Ext.data.Model',

        requires : [
            'Ext.azure.Proxy'
        ],

        config : {
            idProperty : 'id',
            useCache   : false,

            fields     : [
                {
                    name : 'id',
                    type : 'int'
                },
                {
                    name : 'text',
                    type : 'string'
                },
                {
                    name : 'complete',
                    type : 'boolean'
                }
            ],

            proxy : {
                type               : 'azure',
                tableName          : 'TodoItem',
                enablePagingParams : true
            }
        }
    });


### ToDo öğelerinizi depolama
**Dosya adı**: app/store/TodoItems.js

Sencha Touch depoları, veri kayıtlarının (modellerin) çeşitli yöntemlerle kayıtları görüntülemek amacıyla Touch bileşenleri için kullanılabilen koleksiyonlarını depolamak için kullanılır. Buna Kılavuzlar, Grafikler, Listeler ve daha fazlası dahil olabilir.

Burada Azure mobil hizmetinden alınan tüm depo ToDo listesi öğelerinizi tutmak için kullanılacak bir depo tanımlanmaktadır. Depo yapılandırması, model türünün adını (Basic.model.TodoItem - yukarıda tanımlanmıştır) içerir. Bu ad depoda yer alacak kayıtların yapısını tanımlar.

Ayrıca depo için sayfa boyutunu belirtme (8 kayıt) gibi ek yapılandırma seçenekleri mevcuttur ve bu depo için kayıt sıralaması Azure mobil hizmeti tarafından uzaktan yapılır (deponun içinde yerel olarak sıralama yapılmaz).

    Ext.define('Basic.store.TodoItems', {
        extend : 'Ext.data.Store',

        requires : [
            'Basic.model.TodoItem'
        ],

        config : {
            model        : 'Basic.model.TodoItem',
            pageSize     : 8,
            remoteSort   : true,
            remoteFilter : true
        }
    });


### ToDo öğelerinizi görüntüleme ve düzenleme
**Dosya adı**: app/view/DataItem.js

Her bir ToDo öğesinin yapısı tanımlandığına ve tüm kayıtların yerleştirileceği bir depo oluşturulduğuna göre bu bilgilerin uygulama kullanıcısına nasıl gösterileceği üzerine düşünmemiz gerekir. Normalde bilgiler kullanıcıya **Görünümler** kullanılarak gösterilir. Görünüm tek tek veya diğerleriyle birlikte herhangi bir sayıda Touch bileşeni olabilir.

Aşağıdaki görünüm her bir kaydın, her bir öğeyi silme eylemlerine uyum sağlayacak bazı düğmelerle birlikte nasıl gösterileceğini tanımlayan bir ListItem içerir.

    Ext.define('Basic.view.DataItem', {
        extend : 'Ext.dataview.component.ListItem',
        xtype  : 'basic-dataitem',

        requires : [
            'Ext.Button',
            'Ext.layout.HBox',
            'Ext.field.Checkbox'
        ],

        config : {
            checkbox : {
                docked     : 'left',
                xtype      : 'checkboxfield',
                width      : 50,
                labelWidth : 0
            },

            text : {
                flex : 1
            },

            button : {
                docked   : 'right',
                xtype    : 'button',
                ui       : 'plain',
                iconMask : true,
                iconCls  : 'delete',
                style    : 'color: red;'
            },

            dataMap : {
                getText : {
                    setHtml : 'text'
                },

                getCheckbox : {
                    setChecked : 'complete'
                }
            },

            layout : {
                type : 'hbox',
                align: 'stretch'
            }
        },

        applyCheckbox : function(config) {
            return Ext.factory(config, Ext.field.Checkbox, this.getCheckbox());
        },

        updateCheckbox : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        },

        applyButton : function(config) {
            return Ext.factory(config, Ext.Button, this.getButton());
        },

        updateButton : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        }

    });


### Birincil görünümünüzü oluşturma
**Dosya adı**: app/view/Main.js

Tek bir ToDo listesi öğesini (yukarıda) tanımladığımıza göre bu listenin etrafında öğelerin gerçek listesini, bir uygulama başlığını ve yeni görev eklemeye yönelik bir düğmeyi içeren tam bir kullanıcı arabirimi sarmalamak istiyoruz.

    Ext.define('Basic.view.Main', {
        extend : 'Ext.dataview.List',
        xtype  : 'main',

        requires : [
            'Ext.TitleBar',
            'Ext.dataview.List',
            'Ext.data.Store',
            'Ext.plugin.PullRefresh',
            'Ext.plugin.ListPaging',
            'Basic.view.DataItem'
        ],

        config : {
            store : 'TodoItems',

            useSimpleItems : false,
            defaultType    : 'basic-dataitem',

            plugins : [
                {
                    xclass          : 'Ext.plugin.PullRefresh',
                    pullRefreshText : 'Pull down to refresh!'
                },
                {
                    xclass     : 'Ext.plugin.ListPaging',
                    autoPaging : true
                }
            ],

            scrollable : {
                direction     : 'vertical',
                directionLock : true
            },

            items : [
                {
                    docked : 'top',
                    xtype  : 'titlebar',
                    title  : 'Azure Mobile - Basic Data Example'
                },
                {
                    xtype  : 'toolbar',
                    docked : 'bottom',
                    items  : [
                        {
                            xtype       : 'textfield',
                            placeHolder : 'Enter new task',
                            flex        : 1
                        },
                        {
                            xtype  : 'button',
                            action : 'add',
                            text   : 'Add'
                        }
                    ]
                }
            ]
        }
    });

### Her şeyi birlikte çalışır duruma getirin
**Dosya adı**: app/controller/Main.js

Uygulamamızdaki son adım düğme basma işlemlerine (sil, kaydet vb) yanıt vermek ve tüm bu isteklerin ardındaki mantığı açıklamaktır. Sencha Touch bu olayları dinleyen ve uygun şekilde yanıtlayan denetleyiciler kullanır.

    Ext.define('Basic.controller.Main', {
        extend : 'Ext.app.Controller',

        config : {
            refs : {
                todoField : 'main toolbar textfield',
                main      : 'main'
            },

            control : {
                'button[action=add]'    : {
                    tap : 'onAddItem'
                },
                'button[action=reload]' : {
                    tap : 'onReload'
                },

                main : {
                    activate      : 'loadInitialData',
                    itemdoubletap : 'onItemEdit'
                },

                'basic-dataitem checkboxfield' : {
                    change : 'onItemCompleteTap'
                },

                'basic-dataitem button' : {
                    tap : 'onItemDeleteTap'
                }
            }
        },

        loadInitialData : function () {
            Ext.getStore('TodoItems').load();
        },

        onItemDeleteTap : function (button, e, eOpts) {
            var store    = Ext.getStore('TodoItems'),
                dataItem = button.up('dataitem'),
                rec      = dataItem.getRecord();

            rec.erase({
                success: function (rec, operation) {
                    store.remove(rec);
                },
                failure: function (rec, operation) {
                    Ext.Msg.alert(
                        'Error',
                        Ext.util.Format.format('There was an error deleting this task.<br/><br/>    Status Code: {0}<br/>Status Text: {1}',
                        operation.error.status,
                        operation.error.statusText)
                    );
                }
            });
        },

        onItemCompleteTap : function (checkbox, newVal, oldVal, eOpts) {
            var dataItem = checkbox.up('dataitem'),
                rec      = dataItem.getRecord(),
                recVal   = rec.get('complete');

            // this check is needed to prevent an issue where multiple creates get triggered from one create
            if (newVal !== recVal) {
                rec.set('complete', newVal);
                rec.save({
                    success: function (rec, operation) {
                        rec.commit();
                    },
                    failure: function (rec, operation) {
                        // since there was a failure doing the update on the server then silently reject the change
                        rec.reject(true);
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        },

        onItemEdit : function (list, index, target, record, e, eOpts) {
            var rec = list.getSelection()[0];

            Ext.Msg.prompt('Edit', 'Rename task',
                function (buttonId, value) {
                    if (buttonId === 'ok') {
                        rec.set('text', value);
                        rec.save({
                            success: function (rec, operation) {
                                rec.commit();
                            },
                            failure: function (rec, operation) {
                                // since there was a failure doing the update on the server then reject the change
                                rec.reject();
                                Ext.Msg.alert(
                                    'Error',
                                    Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                                    operation.error.status,
                                    operation.error.statusText)
                                );
                            }
                        });
                    }
                },
                null,
                false,
                record.get('text')
            );
        },

        onReload : function () {
            Ext.getStore('TodoItems').load();
        },

        onAddItem : function () {
            var me = this,
                rec,
                store = Ext.getStore('TodoItems'),
                field = me.getTodoField(),
                value = field.getValue();

            if (value === '') {
                Ext.Msg.alert('Error', 'Please enter Task name', Ext.emptyFn);
            }
            else {
                rec = Ext.create('Basic.model.TodoItem', {
                    complete : false,
                    text     : value
                });
                //store.insert(0, rec); //insert at the top
                //store.sync();
                rec.save({
                    success: function (rec, operation) {
                        store.insert(0, rec); //insert at the top
                        field.setValue('');
                    },
                    failure: function (rec, operation) {
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error creating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        }
    });

### Hepsini bir araya getirin
**Dosya adı**: app.js

Son adımımız ana uygulama dosyasının düzenlenmesini tamamlamayı ve tanımlanan modeller, depolar, görünümler ve denetleyiciler hakkında bilgi sağlamayı içermektedir. Bu kaynakların kaynak dosyaları uygulamaya otomatik olarak yüklenir. Son olarak, 'Basic.main.View' adlı birincil uygulama görünümünü oluşturup görüntüleyen başlatma yöntemi çağrılır.

    Ext.Loader.setConfig({
        enabled : true,
        paths   : {
            'Ext'       : 'touch/src',
            'Ext.azure' : 'packages/azure/src'
        }
    });

    Ext.application({
        name : 'Basic',

        requires : [
            'Ext.MessageBox',
            'Ext.azure.Azure'
        ],

        views : [
            'Main'
        ],

        controllers : [
            'Main'
        ],

        stores : [
            'TodoItems'
        ],

        azure : {
            appUrl : 'YOUR_APP_URL.azure-mobile.net',
            appKey : 'YOUR_APP_KEY'
        },

        icon : {
            '57'  : 'resources/icons/Icon.png',
            '72'  : 'resources/icons/Icon~ipad.png',
            '114' : 'resources/icons/Icon@2x.png',
            '144' : 'resources/icons/Icon~ipad@2x.png'
        },

        isIconPrecomposed : true,

        startupImage : {
            '320x460'   : 'resources/startup/320x460.jpg',
            '640x920'   : 'resources/startup/640x920.png',
            '768x1004'  : 'resources/startup/768x1004.png',
            '748x1024'  : 'resources/startup/748x1024.png',
            '1536x2008' : 'resources/startup/1536x2008.png',
            '1496x2048' : 'resources/startup/1496x2048.png'
        },

        launch : function () {
            // Destroy the #appLoadingIndicator element
            Ext.fly('appLoadingIndicator').destroy();

            // Initialize Azure
            Ext.Azure.init(this.config.azure);

            // Initialize the main view
            Ext.Viewport.add(Ext.create('Basic.view.Main'));
        },

        onUpdated : function () {
            Ext.Msg.confirm(
                "Application Update",
                "This application has just successfully been updated to the latest version. Reload now?",
                function (buttonId) {
                    if (buttonId === 'yes') {
                        window.location.reload();
                    }
                }
            );
        }
    });

### Sencha Touch uygulamanızı barındırma ve çalıştırma
Bu öğreticinin son aşaması yerel bilgisayarda yeni uygulamanızı barındırmak ve çalıştırmaktır.

1. Terminalinizde sıkıştırması açılmış uygulamanızın konumuna göz atın.
2. Sencha Cmd kullanarak aşağıdaki komutları çalıştırın:
   
   * *sencha app refresh*: Bu komut Sencha Cmd’den tüm uygulama bağımlılıklarını bulmasını ve gereken tüm paketleri indirmesini (örneğin, [Azure için Sencha Touch Uzantıları](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)) ister.
   * *sencha web start*: Bu komut, uygulamamızı test etmek üzere bir yerel web sunucusu başlatır.
   
   ![sencha web start](./media/partner-sencha-mobile-services-get-started/sencha-web-start.png)
3. Terminalinizde listelenen URL’yi bir web tarayıcısında açarak uygulamayı başlatın (örn. http://localhost:1841).
4. Uygulamada, “Öğreticiyi tamamla” gibi anlamlı bir metin yazın ve ardından **Ekle** seçeneğine tıklayın.
   
   ![new todo item](./media/partner-sencha-mobile-services-get-started/new-todo-item.png)
   
   Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir.
5. [Klasik Azure Portalı] geri dönün, **Veri** sekmesine ve sonra TodoItems tablosuna tıklayın.
   
   ![Todo Items tablosu](./media/partner-sencha-mobile-services-get-started/mobile-data-tab.png)
   
   Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.
   
   ![browse todo table](./media/partner-sencha-mobile-services-get-started/mobile-data-browse.png)

## Sonraki Adımlar
Başlangıç Kılavuzunu tamamladığınıza göre, Sencha ile Mobile Services’taki diğer önemli görevleri nasıl gerçekleştireceğinizi öğrenin.

Ek stile ve özelliklere sahip tamamlanmış bir örnek uygulama [indirin](https://github.com/arthurakay/sencha-touch-azure-example) ve Sencha Touch’ın başka neler yapabildiğini görün!

Ardından, Azure için Sencha Touch Uzantıları hakkında daha fazla bilgi alın:

* Örnek uygulama için [izlenecek yol](http://docs.sencha.com/touch-azure/1.0.0/#!/guide/data_filters)
* [Sencha Forumları](http://www.sencha.com/forum)’nda yardım alın
* [Sencha Belgeleri](http://docs.sencha.com/)’ne göz atın
* Sencha’yı Azure Mobile Services ile kullanın: [(Video)](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-126-Using-Sencha-With-Windows-Azure-Mobile-Services)

## Ek Kaynaklar
* [Sencha Touch indirin](http://pages.sencha.com/touch-for-azure.html)
* [Azure için Sencha Touch Uzantıları](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)

## Özet
Burada ana hatlarıyla verilen örnek, Azure için Sencha Touch Uzantısı paketinde verilmektedir ve Temel Veri örneği olarak örnek dizinde bulunur. Bu uzantının diğer işlevlerini ayrıntılı yorumlar ve açıklamalar ile birlikte gösteren birkaç örnek daha sunulmuştur.

Sencha Touch kullanmaya başlama hakkında daha fazla bilgi için lütfen tüm [kılavuzları](http://docs.sencha.com/touch/#!/guide) ziyaret edin

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- images -->
[0]: ./media/partner-sencha-mobile-services-get-started/finished-app.png

[Klasik Azure Portalı]: https://manage.windowsazure.com/


<!--HONumber=Sep16_HO3-->


