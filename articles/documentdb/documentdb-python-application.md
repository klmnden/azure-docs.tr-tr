---
title: DocumentDB ile Python Flask Web Uygulaması Geliştirme | Microsoft Docs
description: Azure'da barındırılan bir Python Flask web uygulamasından veri depolamak ve verilere erişmek için DocumentDB kullanma konulu veritabanı öğreticisini inceleyin. Uygulama geliştirme çözümleri bulun.
keywords: Uygulama geliştirme, veritabanı öğreticisi, python flask, python web uygulaması, python web geliştirme, documentdb, Azure, Microsoft Azure
services: documentdb
documentationcenter: python
author: syamkmsft
manager: jhubbard
editor: cgronlun

ms.service: documentdb
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 08/25/2016
ms.author: syamk

---
# <a name="python-flask-web-application-development-with-documentdb"></a>DocumentDB ile Python Flask Web Uygulaması Geliştirme
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Bu öğretici, Azure'da barındırılan bir Python Flask web uygulamasından veri depolamak ve verilere erişmek için Azure DocumentDB'nin nasıl kullanılacağını gösterir ve Python ve Azure web sitelerini kullanma konusunda biraz deneyim sahibi olduğunuzu varsayar.

Bu veritabanı öğreticisi şunlara değinir:

1. DocumentDB hesabı oluşturma ve hazırlama.
2. Python MVC uygulaması oluşturma.
3. Web uygulamanızdan Azure DocumentDB'ye bağlanma ve bunu kullanma.
4. Web uygulamasını Azure Web Sitelerine dağıtma.

Bu öğreticiyi izleyerek, bir yoklama için oy kullanmanıza olanak tanıyan basit bir oylama uygulaması oluşturacaksınız.

![Bu veritabanı öğreticisi tarafından oluşturulan yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/documentdb-python-application/image1.png)

## <a name="database-tutorial-prerequisites"></a>Veritabanı öğreticisi önkoşulları
Bu makaledeki yönergeleri izlemeden önce aşağıdakilerin yüklenmiş olduğundan emin olmanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio 2013](http://www.visualstudio.com/) veya sonraki bir sürümü ya da ücretsiz sürüm olan [Visual Studio Express](). Bu öğreticideki yönergeler özellikle Visual Studio 2015 için yazılmıştır. 
* [GitHub](http://microsoft.github.io/PTVS/)'dan Visual Studio için Python Araçları. Bu öğreticide, VS 2015 için Python Araçları kullanır. 
* Visual Studio için Azure Python SDK 2.4 veya sonraki bir sürüm [azure.com](https://azure.microsoft.com/downloads/) adresinde bulunabilir. Python 2.7 için Microsoft Azure SDK'yı kullandık.
* [Python.org][2] adresinden Python 2.7. Biz, Python 2.7.11 sürümünü kullandık. 

> [!IMPORTANT]
> Python 2.7'yi ilk kez yüklüyorsanız Python 2.7.11'i Özelleştirme ekranında **Yola python.exe'yi ekle** seçeneğini belirlediğinizden emin olun.
> 
> ![Yola python.exe'yi eklemeyi seçmeniz gereken Python 2.7.11'i Özelleştirme ekranının ekran görüntüsü](./media/documentdb-python-application/image2.png)
> 
> 

* [Microsoft İndirme Merkezi][3] adresinden Python 2.7 için Microsoft Visual C++ Derleyicisi.

## <a name="step-1:-create-a-documentdb-database-account"></a>1. Adım: DocumentDB veritabanı hesabı oluşturma
Bir DocumentDB hesabı oluşturarak başlayalım. Hesabınız zaten varsa [2. Adım: Yeni bir Python Flask web uygulaması oluşturma](#step-2:-create-a-new-python-flask-web-application) adımına atlayabilirsiniz.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

<br/>
Şimdi en başından başlayarak yeni bir Python Flask web uygulamasının nasıl oluşturulacağını göstereceğiz.

## <a name="step-2:-create-a-new-python-flask-web-application"></a>2. Adım: Yeni bir Python Flask web uygulaması oluşturma
1. Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine gidin ve ardından **Proje**'ye tıklayın.
   
    **Yeni Proje** iletişim kutusu görünür.
2. Sol bölmede **Şablonlar**'ı ve ardından **Python**'u genişletin ve sonra **Web**'e tıklayın. 
3. Orta bölmede **Flask Web Projesi**'ni seçin ve ardından **Ad** kutusuna **öğretici** yazın ve sonra **Tamam**'a tıklayın. Python paket adlarının [Python Kodu için Stil Kılavuzu](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)'nda açıklandığı gibi tamamen küçük harflerden oluşması gerektiğini unutmayın.
   
    Yeni olanlar için bilgi vermek gerekirse Python Flask, Python'da web uygulamalarını daha hızlı oluşturmanıza yardımcı olan bir web uygulaması geliştirme altyapısıdır.
   
    ![Solda Python vurgulanmış, ortada Python Flask Web Projesi seçilmiş ve Ad kutusunda öğretici adı bulunur şekilde Visual Studio'da Yeni Proje penceresinin ekran görüntüsü](./media/documentdb-python-application/image9.png)
4. **Visual Studio için Python Araçları** penceresinde **Sanal bir ortama yükleme**'ye tıklayın. 
   
    ![Veritabanı öğreticisinin ekran görüntüsü - Visual Studio için Python Araçları penceresi](./media/documentdb-python-application/image10.png)
5. Şu anda Python 3.x PyDocumentDB tarafından desteklenmediği için, **Sanal Ortama Ekleme** penceresinde varsayılanları kabul edebilir ve Python 2.7'yi temel ortam olarak kullanabilirsiniz ve ardından **Oluştur**'a tıklayın. Böylece projeniz için gereken Python sanal ortamı kurulmuş olur.
   
    ![Veritabanı öğreticisinin ekran görüntüsü - Visual Studio için Python Araçları penceresi](./media/documentdb-python-application/image10_A.png)
   
    Ortam başarılı bir şekilde yüklendiğinde çıktı penceresi şunu görüntüler: `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`

## <a name="step-3:-modify-the-python-flask-web-application"></a>3. Adım: Python Flask web uygulamasını değiştirme
### <a name="add-the-python-flask-packages-to-your-project"></a>Python Flask paketlerini projenize ekleme.
Projeniz kurulduktan sonra, pydocumentdb ve DocumentDB için Python paket dahil olmak üzere gerekli Flask paketlerini projenize eklemeniz gerekir.

1. Çözüm Gezgini'nde **requirements.txt** adlı dosyayı açın ve içeriği aşağıdakiyle değiştirin:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. **requirements.txt** dosyasını kaydedin. 
3. Çözüm Gezgini'nde **env**'e sağ tıklayın ve **requirements.txt'ten yükle**'ye tıklayın.
   
    ![Listede requirements.txt'ten yükle vurgulanmış şekilde env (Python 2.7) seçildiğini gösteren ekran görüntüsü](./media/documentdb-python-application/image11.png)
   
    Başarılı yüklemeden sonra, çıktı penceresi aşağıdakini görüntüler:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > Nadir durumlarda çıktı penceresinde bir hata görebilirsiniz. Bu durumda hatanın temizleme ile ilgili olup olmadığını denetleyin. Bazen temizleme başarısız olur ancak yükleme yine de başarılı olabilir (bunu doğrulamak için çıktı penceresinde yukarı kaydırın). [Sanal ortamı doğrulama](#verify-the-virtual-environment) ile yüklemenizi denetleyebilirsiniz. Yükleme başarısız ancak doğrulama başarılı olduysa devam etmede bir sorun yoktur.
   > 
   > 

### <a name="verify-the-virtual-environment"></a>Sanal ortamı doğrulama
Her şeyin doğru şekilde yüklendiğinden emin olmamız gerekir.

1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Böylece Flask geliştirme sunucusu başlatılmış ve web tarayıcınız başlamış olur. Aşağıdaki sayfayı göreceksiniz.
   
    ![Bir tarayıcıda görüntülenen boş Python Flask web geliştirme projesi](./media/documentdb-python-application/image12.png)
3. Visual Studio'da **Shift**+**F5**'e basarak web sitesinin hata ayıklamasını durdurun.

### <a name="create-database,-collection,-and-document-definitions"></a>Veritabanı, koleksiyon ve belge tanımları oluşturma
Şimdi yeni dosyalar ekleyip diğer dosyaları güncelleştirerek oylama uygulamanızı oluşturalım.

1. Çözüm Gezgini'nde **öğretici** projeye sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Yeni Öğe**'ye tıklayın. **Boş Python Dosyası**'nı seçin ve dosyaya **forms.py** adını verin.  
2. Aşağıdaki kodu forms.py dosyasına ekleyin ve ardından dosyayı kaydedin.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-views.py"></a>Gerekli içeri aktarmaları views.py'ye ekleme
1. Çözüm Gezgini'nde **öğretici** klasörünü genişletin ve **views.py** dosyasını açın. 
2. Aşağıdaki içeri aktarma deyimlerini **views.py** dosyasının üstüne ekleyin ve ardından dosyayı kaydedin. Bunlar, DocumentDB'nin PythonSDK'sını ve Flask paketlerini içeri aktarır.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database,-collection,-and-document"></a>Veritabanı, koleksiyon ve belge oluşturma
* Yine **views.py**'de dosyanın sonuna aşağıdaki kodu ekleyin. Böylece form tarafından kullanılan veritabanı oluşturulmuş olur. **views.py**'de var olan hiçbir kodu silmeyin. Yalnızca bunu sona ekleyin.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```

> [!TIP]
> **CreateCollection** yöntemi, üçüncü parametre olarak isteğe bağlı bir **RequestOptions** alır. Bu, koleksiyon için Teklif Türü belirtmek için kullanılabilir. Hiçbir offerType değeri sağlanmazsa koleksiyon varsayılan Teklif Türü kullanılarak oluşturulur. DocumentDB Teklif Türleri hakkında daha fazla bilgi için bkz. [DocumentDB'de performans düzeyleri](documentdb-performance-levels.md).
> 
> 

### <a name="read-database,-collection,-document,-and-submit-form"></a>Veritabanı, koleksiyon ve belge okuma ve form gönderme
* Yine **views.py**'de dosyanın sonuna aşağıdaki kodu ekleyin. Böylece form kurma ve veritabanı, koleksiyon ve belge okuma gerçekleşmiş olur. **views.py**'de var olan hiçbir kodu silmeyin. Yalnızca bunu sona ekleyin.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a>HTML dosyaları oluşturma
1. Çözüm Gezgini'ndeki **öğretici** klasöründe **şablonlar** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Yeni Öğe**'ye tıklayın. 
2. **HTML Sayfası**'nı seçin ve ardından ad kutusuna **create.html** yazın. 
3. İki ek HTML dosyası oluşturmak için 1 ve 2. adımı tekrarlayın: results.html ve vote.html.
4. Aşağıdaki kodu `<body>` öğesindeki **create.html**'ye ekleyin. Yeni bir veritabanı, koleksiyon ve belge oluşturduğumuzu bildiren bir ileti görüntülenir.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Aşağıdaki kodu `<body` öğesindeki **results.html**'ye ekleyin. Yoklama sonuçları görüntülenir.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Aşağıdaki kodu `<body` öğesindeki **vote.html**'ye ekleyin. Yoklamayı görüntüler ve oyları kabul eder. Oy kaydetmede denetim, atılan oyları tanıyacağımız ve buna uygun şekilde belgeye ekleyeceğimiz views.py'ye geçirilir.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. **Şablonlar** klasöründe, **index.html** içeriğini aşağıdakiyle değiştirin. Bu, uygulamanız için giriş sayfası görevi görür.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + DocumentDB Voting Application.</h2>
    <h3>This is a sample DocumentDB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-\_\_init\_\_.py"></a>Bir yapılandırma dosyası ekleme ve \_\_init\_\_.py'yi değiştirme
1. Çözüm Gezgini'nde **öğretici** projesine sağ tıklayın, **Ekle**'ye tıklayın, **Yeni Öğe**'ye tıklayın, **Boş Python Dosyası**'nı seçin ve ardından dosyaya **config.py** adını verin. Bu yapılandırma dosyası, Flask'taki formlar için gereklidir. Bunu gizli bir anahtar sağlamak için de kullanabilirsiniz. Ancak bu anahtar bu öğretici için gerekli değildir.
2. Aşağıdaki kodu config.py'ye ekleyin. Bir sonraki adımda **DOCUMENTDB\_HOST** ve **DOCUMENTDB\_KEY** değerlerini değiştirmeniz gerekecektir.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. [Azure Portalı](https://portal.azure.com/)'nda **Anahtarlar** dikey penceresine gitmek için **Gözat**, **DocumentDB Hesapları**'na tıklayın, kullanılacak hesabın adına çift tıklayın ve ardından **Temel Bileşenler** alanındaki **Anahtarlar** düğmesine tıklayın. **Anahtarlar** dikey penceresinde **URI** değerini kopyalayın ve **DOCUMENTDB\_HOST** özelliğinin değeri olarak **config.py** dosyasına yapıştırın. 
4. Azure Portalı'ndaki **Anahtarlar** dikey penceresinde **Birincil Anahtar** veya **İkincil Anahtar** değerini kopyalayın ve **DOCUMENTDB\_KEY** özelliğinin değeri olarak **config.py** dosyasına yapıştırın.
5. **\_\_init\_\_.py** dosyasında, aşağıdaki satırı ekleyin. 
   
        app.config.from_object('config')
   
    Böylece dosyanın içeriği şu olur:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. Tüm dosyaları ekledikten sonra, Çözüm Gezgini şöyle görünecektir:
   
    ![Visual Studio Çözüm Gezgini penceresinin ekran görüntüsü](./media/documentdb-python-application/image15.png)

## <a name="step-4:-run-your-web-application-locally"></a>4. Adım: Web uygulamanızı yerel olarak çalıştırma
1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Ekranınızda şunu göreceksiniz:
   
    ![Python + DocumentDB Oylama uygulamasının bir web tarayıcısında görüntülenen ekran görüntüsü](./media/documentdb-python-application/image16.png)
3. Veritabanını oluşturmak için **Oylama Veritabanını Oluştur/Temizle**'ye tıklayın.
   
    ![Web uygulamasının Oluşturma Sayfası'nın ekran görüntüsü - geliştirme ayrıntıları](./media/documentdb-python-application/image17.png)
4. Ardından **Oy Ver**'e tıklayın ve seçiminizi belirleyin.
   
    ![Bir oylama sorusu sorulmuş web uygulamasının ekran görüntüsü](./media/documentdb-python-application/image18.png)
5. Verdiğiniz her oy için uygun sayaç artırılır.
   
    ![Oylamanın Sonuçları sayfasını gösteren ekran görüntüsü](./media/documentdb-python-application/image19.png)
6. Shift + F5'e basarak projenin hata ayıklamasını durdurun.

## <a name="step-5:-deploy-the-web-application-to-azure-websites"></a>5. Adım: Web uygulamasını Azure Web Sitelerine dağıtma
Artık uygulamanın tamamı doğrudan DocumentDB'de çalıştığına göre, bunu Azure Web Sitelerine dağıtacağız.

1. Çözüm Gezgini'nde projeye sağ tıklayın (yerel olarak çalıştırmaya devam etmediğinizden emin olun) ve **Yayımla**'yı seçin.  
   
    ![Yayımla seçeneği vurgulanmış şekilde Çözüm Gezgini'nde öğreticinin seçili olduğu ekran görüntüsü](./media/documentdb-python-application/image20.png)
2. **Web'i Yayımla** penceresinde **Microsoft Azure Web Apps**'i seçin ve **İleri**'ye tıklayın.
   
    ![Microsoft Azure Web Apps vurgulanmış Web'i Yayımla penceresinin ekran görüntüsü](./media/documentdb-python-application/image21.png)
3. **Microsoft Azure Web Apps Penceresi** penceresinde **Yeni**'ye tıklayın.
   
    ![Microsoft Azure Web Apps Penceresi penceresinin ekran görüntüsü](./media/documentdb-python-application/select-existing-website.png)
4. **Microsoft Azure'da site oluşturma** penceresinde bir **Web uygulaması adı**, **App Service planı**, **Kaynak grubu** ve **Bölge** girin ve ardından **Oluştur**'a tıklayın.
   
    ![Microsoft Azure'da site oluşturma penceresinin ekran görüntüsü](./media/documentdb-python-application/create-site-on-microsoft-azure.png)
5. **Web'i Yayımlama** penceresinde **Yayımla**'ya tıklayın.
   
    ![Microsoft Azure'da site oluşturma penceresinin ekran görüntüsü](./media/documentdb-python-application/publish-web.png)
6. Visual Studio birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure'da çalışırken görebileceğiniz bir tarayıcıyı başlatacak!

## <a name="troubleshooting"></a>Sorun giderme
Bilgisayarınızda çalıştırdığınız ilk Python uygulaması buysa YOL değişkeninize aşağıdaki klasörlerin (veya eşdeğer yükleme konumlarının) dahil olduğundan emin olun:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Oy sayfanızda bir hata alırsanız ve projenize **öğretici** dışında bir ad verdiyseniz **\_\_init\_\_.py**'nin doğru proje adına şu satırda başvurduğundan emin olun: `import tutorial.view`.

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure DocumentDB kullanarak ilk Python web uygulamanızı tamamladınız ve bunu Azure Web Siteleri'nde yayımladınız.

Geri bildirimlerinizi temel alarak bu konu başlığını sıklıkla güncelleştirip iyileştiriyoruz.  Öğreticiyi tamamladıktan sonra, lütfen bu sayfanın üst ve alt kısmındaki oylama düğmelerini kullanın ve yapılmasını görmek istediğiniz iyileştirmeler hakkında geri bildiriminizi ekleyin. Doğrudan sizinle iletişim kurmamızı isterseniz yorumlarınıza e-posta adresinizi ekleyin.

Web uygulamanıza ek işlevsellik eklemek için [DocumentDB Python SDK'sında](documentdb-sdk-python.md) kullanılabilen API'leri inceleyin.

Azure, Visual Studio ve Python hakkında daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/). 

Ek Python Flask öğreticileri için bkz. [Büyük Flask Öğreticisi, 1. Bölüm: Merhaba Dünya!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platformu Yükleyicisi]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com



<!--HONumber=Oct16_HO3-->


