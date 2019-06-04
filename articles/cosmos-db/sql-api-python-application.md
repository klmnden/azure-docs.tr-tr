---
title: Azure Cosmos DB için Python Flask web uygulaması Öğreticisi
description: Azure'da barındırılan bir Python Flask web uygulamasından veri depolamak ve verilere erişmek için Azure Cosmos DB kullanma konulu veritabanı öğreticisini inceleyin. Uygulama geliştirme çözümleri bulun.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: tutorial
ms.date: 12/23/2018
ms.author: sngun
ms.openlocfilehash: 4f4d5f89e56211e9135dba33200751b59b508123
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66478896"
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Azure Cosmos DB kullanarak bir Python Flask web uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)

Bu öğreticide, Azure App Service'te barındırılan bir Python Flask Web uygulamasından veri depolamak ve veriye erişmek için Azure Cosmos DB'nin nasıl kullanılacağı gösterilmektedir. Bu öğretici, Python ve Azure Web sitelerini kullanma konusunda önceden biraz deneyim sahibi olduğunuzu varsayar.

Bu veritabanı öğreticisi şunlara değinir:

1. Bir Azure Cosmos DB hesabı oluşturma ve sağlama.
2. Bir Python Flask uygulaması oluşturma.
3. Web uygulamanızdan Azure Cosmos DB'ye bağlanma ve bu uygulamayı kullanma.
4. Azure App Service'e Web uygulaması dağıtma.

Bu öğreticiyi izleyerek, bir yoklama için oy kullanmanıza olanak tanıyan basit bir oylama uygulaması oluşturacaksınız.

![Bu veritabanı Öğreticisi tarafından oluşturulan oylama uygulamasının ekran görüntüsü](./media/sql-api-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Veritabanı öğreticisi önkoşulları

Bu makaledeki yönergeleri izlemeden önce aşağıdakilerin yüklenmiş olduğundan emin olmanız gerekir:

* [Bir Azure aboneliği](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* **Azure geliştirme** ve **Python geliştirme** etkinleştirilmiş olarak [Visual Studio 2017](https://www.visualstudio.com/downloads/). Bu önkoşulların yüklü olup olmadığını kontrol edebilir ve **Visual Studio Yükleyicisi**'ni yerel olarak açarak bunları yükleyebilirsiniz.   
* [Python 2.7 için Microsoft Azure SDK](https://azure.microsoft.com/downloads/). 
* [Python 2.7](https://www.python.org/downloads/windows/). Yüklemenin 32 bit veya 64 bit sürümünü kullanabilirsiniz.

> [!IMPORTANT]
> Python 2.7'yi ilk kez yüklüyorsanız Python 2.7.13'i Özelleştirme ekranında **Python.exe'yi yola ekle** seçeneğini belirlediğinizden emin olun.
>
> ![Yola Python.exe'yi Ekle seçin gerek duyduğunuz senaryolara Python 2.7.11'i özelleştirme ekranının ekran görüntüsü](./media/sql-api-python-application/cosmos-db-python-install.png)

* [Python 2.7 için Microsoft Visual C++ Derleyicisi](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>1. adım: Bir Azure Cosmos DB veritabanı hesabı oluşturma

İlk olarak bir Azure Cosmos DB hesabı oluşturalım. Zaten bir hesabınız varsa veya Bu öğretici için Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, adımına atlayabilirsiniz [2. adım: Yeni bir Python Flask web uygulaması oluşturma](#step-2-create-a-new-python-flask-web-application).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Şimdi en başından başlayarak yeni bir Python Flask web uygulamasının nasıl oluşturulacağını görelim.

## <a name="step-2-create-a-new-python-flask-web-application"></a>2. adım: Yeni bir Python Flask web uygulaması oluşturma

1. Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine gidin ve ardından **Proje**'ye tıklayın.

    **Yeni Proje** iletişim kutusu görünür.
2. Sol bölmede **Şablonlar**'ı ve ardından **Python**'u genişletin ve sonra **Web**'e tıklayın. 
3. Orta bölmede **Flask Web Projesi**'ni seçin ve ardından **Ad** kutusuna **öğretici** yazın ve sonra **Tamam**'a tıklayın. Python paket adlarının [Python Kodu için Stil Kılavuzu](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)'nda açıklandığı gibi tamamen küçük harflerden oluşması gerektiğini unutmayın.

    Yeni olanlar için bilgi vermek gerekirse Python Flask, Python'da web uygulamalarını daha hızlı oluşturmanıza yardımcı olan bir web uygulaması geliştirme altyapısıdır.

    ![Orta ve adı öğreticinin ad kutusuna seçili Python Flask Web projesi sol tarafta vurgulanan Python ile Visual Studio'da yeni proje penceresinin ekran görüntüsü](./media/sql-api-python-application/image9.png)

4. **Visual Studio için Python Araçları** penceresinde **Sanal bir ortama yükleme**'ye tıklayın. 

    ![Veritabanı Öğreticisi - penceresi Visual Studio için Python araçları ekran görüntüsü](./media/sql-api-python-application/python-install-virtual-environment.png)
5. **Sanal Ortam Ekle** penceresinde, Yorumlayıcı seç kutusunda Python 2.7'yi veya Python 3.5'i seçin, diğer varsayılanları kabul edin ve **Oluştur**'u tıklatın. Böylece projeniz için gereken Python sanal ortamı kurulmuş olur.

    ![Veritabanı Öğreticisi - penceresi Visual Studio için Python araçları ekran görüntüsü](./media/sql-api-python-application/image10_A.png)

    Ortam başarılı bir şekilde yüklendiğinde çıktı penceresi şunu görüntüler: `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`

## <a name="step-3-modify-the-python-flask-web-application"></a>3. adım: Python Flask web uygulamasını değiştirme

### <a name="add-the-python-flask-packages-to-your-project"></a>Python Flask paketlerini projenize ekleme.

Projeniz kurulduktan sonra, Azure Cosmos DB SQL API'si için Python paketi olan pydocumentdb dahil olmak üzere gerekli Flask paketlerini projenize eklemeniz gerekir.

1. Çözüm Gezgini'nde **requirements.txt** adlı dosyayı açın ve içeriği aşağıdakiyle değiştirin:

    ```text
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
    ```

2. **requirements.txt** dosyasını kaydedin.
3. Çözüm Gezgini'nde **env**'e sağ tıklayın ve **requirements.txt'ten yükle**'ye tıklayın.

    ![Listede vurgulanmış requirements.txt yüklemesiyle seçildiği ekran görüntüsü gösteren env (Python 2.7)](./media/sql-api-python-application/cosmos-db-python-install-from-requirements.png)

    Başarılı yüklemeden sonra, çıktı penceresi aşağıdakini görüntüler:

    ```output
    Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
    ```

   > [!NOTE]
   > Nadir durumlarda çıktı penceresinde bir hata görebilirsiniz. Bu durumda hatanın temizleme ile ilgili olup olmadığını denetleyin. Bazen temizleme başarısız, ancak yükleme yine de başarılı olabilir (bunu doğrulamak için çıktı penceresini yukarı kaydırın). [Sanal ortamı doğrulama](#verify-the-virtual-environment) ile yüklemenizi denetleyebilirsiniz. Yükleme başarısız ancak doğrulama başarılı olduysa devam etmede bir sorun yoktur.

### <a name="verify-the-virtual-environment"></a>Sanal ortamı doğrulama

Her şeyin doğru şekilde yüklendiğinden emin olmamız gerekir.

1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Böylece Flask geliştirme sunucusu başlatılmış ve web tarayıcınız başlamış olur. Aşağıdaki sayfayı göreceksiniz.

    ![Bir tarayıcıda görüntülenen boş Python Flask web geliştirme projesi](./media/sql-api-python-application/image12.png)

3. Visual Studio'da **Shift**+**F5**'e basarak web sitesinin hata ayıklamasını durdurun.

### <a name="create-database-collection-and-document-definitions"></a>Veritabanı, koleksiyon ve belge tanımları oluşturma

Şimdi yeni dosyalar ekleyip diğer dosyaları güncelleştirerek oylama uygulamanızı oluşturalım.

1. Çözüm Gezgini'nde **öğretici** projeye sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Yeni Öğe**'ye tıklayın. **Boş Python Dosyası**'nı seçin ve dosyaya **forms.py** adını verin.  
2. Aşağıdaki kodu forms.py dosyasına ekleyin ve ardından dosyayı kaydedin.

```python
from flask_wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```

### <a name="add-the-required-imports-to-viewspy"></a>Gerekli içeri aktarmaları views.py'ye ekleme

1. Çözüm Gezgini'nde **öğretici** klasörünü genişletin ve **views.py** dosyasını açın. 
2. Aşağıdaki içeri aktarma deyimlerini **views.py** dosyasının üstüne ekleyin ve ardından dosyayı kaydedin. Bunlar, Azure Cosmos DB'nin PythonSDK'sını ve Flask paketlerini içeri aktarır.

    ```python
    from forms import VoteForm
    import config_cosmos
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Veritabanı, koleksiyon ve belge oluşturma

* Yine **views.py**'de dosyanın sonuna aşağıdaki kodu ekleyin. Böylece form tarafından kullanılan veritabanı oluşturulmuş olur. **views.py**'de var olan hiçbir kodu silmeyin. Yalnızca bunu sona ekleyin.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config_cosmos.COSMOSDB_HOST, {'masterKey': config_cosmos.COSMOSDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config_cosmos.COSMOSDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config_cosmos.COSMOSDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config_cosmos.COSMOSDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config_cosmos.COSMOSDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config_cosmos.COSMOSDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```

### <a name="read-database-collection-document-and-submit-form"></a>Veritabanı, koleksiyon ve belge okuma ve form gönderme

* Yine **views.py**'de dosyanın sonuna aşağıdaki kodu ekleyin. Böylece form kurma ve veritabanı, koleksiyon ve belge okuma gerçekleşmiş olur. **views.py**'de var olan hiçbir kodu silmeyin. Yalnızca bunu sona ekleyin.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config_cosmos.COSMOSDB_HOST, {'masterKey': config_cosmos.COSMOSDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config_cosmos.COSMOSDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config_cosmos.COSMOSDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config_cosmos.COSMOSDB_DOCUMENT))

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

6. Aşağıdaki kodu `<body` öğesindeki **vote.html**'ye ekleyin. Yoklamayı görüntüler ve oyları kabul eder. Oy kaydetmede denetim, Azure Cosmos DB'nin atılan oyları tanıyacağı ve belgeyi buna göre ekleyeceği views.py'ye geçirilir.

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
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a>Bir yapılandırma dosyası ekleme ve \_\_init\_\_.py'yi değiştirme

1. Çözüm Gezgini'nde **öğretici** projesine sağ tıklayın, **Ekle**'ye tıklayın, **Yeni Öğe**'ye tıklayın, **Boş Python Dosyası**'nı seçin ve ardından dosyaya **config_cosmos.py** adını verin. Bu yapılandırma dosyası, Flask'taki formlar için gereklidir. Bunu gizli bir anahtar sağlamak için de kullanabilirsiniz. Ancak bu anahtar bu öğretici için gerekli değildir.
2. Config_cosmos.py dosyasına aşağıdaki kodu ekleyin; sonraki adımda **COSMOSDB\_HOST** ve **COSMOSDB\_KEY** değerlerini değiştirmeniz gerekecektir.

    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'

    COSMOSDB_HOST = 'https://YOUR_COSMOSDB_NAME.documents.azure.com:443/'
    COSMOSDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='

    COSMOSDB_DATABASE = 'voting database'
    COSMOSDB_COLLECTION = 'voting collection'
    COSMOSDB_DOCUMENT = 'voting document'
    ```

3. [Azure Portalı](https://portal.azure.com/)'nda **Anahtarlar** sayfasına gitmek için **Gözat**, **Azure Cosmos DB Hesapları**'na tıklayın, kullanılacak hesabın adına çift tıklayın ve ardından **Temel Bileşenler** alanındaki **Anahtarlar** düğmesine tıklayın. **Anahtarlar** sayfasında **URI** değerini kopyalayın ve **COSMOSDB\_HOST** özelliğinin değeri olarak **config.py** dosyasına yapıştırın. 
4. Azure Portalı'ndaki **Anahtarlar** sayfasında **Birincil Anahtar** veya **İkincil Anahtar** değerini kopyalayın ve **COSMOSDB\_KEY** özelliğinin değeri olarak **config.py** dosyasına yapıştırın.
5. **\_\_init\_\_.py** dosyasına, yapılandırma dosyası okuma ve bazı temel günlük kaydı işlemlerini dahil etmek için aşağıdaki satırları ekleyin:

        app.config.from_object('config_cosmos')
        logging.basicConfig(level=logging.INFO,format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
        logger = logging.getLogger(__name__)

    Böylece dosyanın içeriği şu olur:

    ```python
    import logging
    from flask import Flask
    app = Flask(__name__)
    app.config.from_pyfile('config_cosmos')
    logging.basicConfig(level=logging.INFO,format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    logger = logging.getLogger(__name__)

    import tutorial.views
    ```

6. Tüm dosyaları ekledikten sonra, Çözüm Gezgini şöyle görünecektir:

    ![Visual Studio Çözüm Gezgini penceresinin ekran görüntüsü](./media/sql-api-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>4. Adım: Web uygulamanızı yerel olarak çalıştırma

1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Ekranınızda şunu göreceksiniz:

    ![Python + Azure Cosmos DB oylama bir web tarayıcısında görüntülenen uygulamasının ekran görüntüsü](./media/sql-api-python-application/cosmos-db-pythonr-run-application.png)

3. Veritabanını oluşturmak için **Oylama Veritabanını Oluştur/Temizle**'ye tıklayın.

    ![Ekran görüntüsü-geliştirme ayrıntıları web uygulamasının oluşturma sayfası](./media/sql-api-python-application/cosmos-db-python-run-create-page.png)

4. Ardından **Oy Ver**'e tıklayın ve seçiminizi belirleyin.

    ![İle bir oylama sorusu sorulmuş web uygulamasının ekran görüntüsü](./media/sql-api-python-application/cosmos-db-vote.png)
5. Verdiğiniz her oy için uygun sayaç artırılır.

    ![Sonuçları gösterilen oy sayfasının ekran görüntüsü](./media/sql-api-python-application/cosmos-db-voting-results.png)

6. Shift + F5'e basarak projenin hata ayıklamasını durdurun.

## <a name="step-5-deploy-the-web-application-to-azure"></a>5. Adım: Web uygulamasını azure'a dağıtma

Artık uygulamanın tamamının Azure Cosmos DB ile yerel olarak düzgün çalıştığından emin olduğunuza göre, bir web.config dosyası oluşturacak, sunucudaki dosyaları yerel ortamla uyuşacak şekilde güncelleştirecek ve sonra tamamlanan uygulamayı Azure'da görüntüleyeceğiz. Bu yordam, Visual Studio 2017'ye özeldir. Visual Studio'nun farklı bir sürümünü kullanıyorsanız, bkz. [Azure App Service'te Yayımlama](/visualstudio/python/publishing-to-azure).

1. Visual Studio **Çözüm Gezgini**'nde, projeye sağ tıklayın ve **Ekle > Yeni Öğe...** 'yi seçin. Açılan iletişim kutusunda, **Azure web.config (Fast CGI)** şablonunu ve **Tamam**'ı seçin. Bu, proje kök dizininizde bir `web.config` dosyası oluşturur. 

2. Yolun Python yüklemesiyle eşleşmesi için `web.config` içindeki `<system.webServer>` bölümünü değiştirin. Örneğin Python 2.7 x64 için giriş aşağıdaki gibi görünmelidir:

    ```xml
    <system.webServer>
        <handlers>
            <add name="PythonHandler" path="*" verb="*" modules="FastCgiModule" scriptProcessor="D:\home\Python27\python.exe|D:\home\Python27\wfastcgi.py" resourceType="Unspecified" requireAccess="Script"/>
        </handlers>
    </system.webServer>
    ```

3. `web.config` içindeki `WSGI_HANDLER` girişini, proje adınızla eşleşmesi için `tutorial.app` olarak ayarlayın. 

    ```xml
    <!-- Flask apps only: change the project name to match your app -->
    <add key="WSGI_HANDLER" value="tutorial.app"/>
    ```

4. Visual Studio **Çözüm Gezgini**'nde, **öğretici** klasörünü genişletin, `static` klasörünü sağ tıklatın, **Ekle > Yeni Öğe...** 'yi seçin, "Azure statik dosyaları web.config" şablonunu ve **Tamam**'ı seçin. Bu eylem `static` klasöründe, Python işlemesini bu klasör için devre dışı bırakan başka bir `web.config` oluşturur. Bu yapılandırma, statik dosyalar için, Python uygulamasını kullanmak yerine varsayılan web sunucusuna istekleri gönderir.

5. Dosyaları kaydedin, sonra Çözüm Gezgini'nde projeye sağ tıklayın (projeyi hala yerel olarak çalıştırmadığınızdan emin olun) ve **Yayımla**'yı seçin.  

     ![Çözüm Gezgini'nde Yayımla seçeneğinin vurgulandığı seçili Öğreticisi ekran görüntüsü](./media/sql-api-python-application/image20.png)

6. **Yayımla** iletişim kutusunda, **Microsoft Azure App Service**'i, **Yeni Oluştur**'u seçin ve **Yayımla**'ya tıklayın.

    ![Microsoft Azure App Service vurgulanmış Web'i Yayımla penceresinin ekran](./media/sql-api-python-application/cosmos-db-python-publish.png)

7. **Uygulama Hizmeti Oluştur** iletişim kutusunda, Web uygulamanızın adının yanı sıra **Abonelik**, **Kaynak Grubu** ve **App Service Planı** bilgilerinizi girin ve **Oluştur**'a tıklayın.

    ![Microsoft Azure Web Apps penceresi penceresinin ekran görüntüsü](./media/sql-api-python-application/cosmos-db-python-create-app-service.png)
8. Visual Studio birkaç saniye içinde dosyalarınızı sunucuya kopyalamayı bitirir ve "İç sunucu hatası oluştuğundan sayfa görüntülenemiyor." iletisini `http://<your app service>.azurewebsites.net/` sayfasında görüntüler.

9. Azure portalında yeni App Service hesabınızı açın, sonra gezinti menüsünde **Geliştirme Araçları** bölümüne ilerleyin, **Uzantılar**'ı seçin ve **+ Ekle**'ye tıklayın.

10. **Uzantı seç** sayfasında, en yeni Python 2.7 yüklemesine ilerleyin ve x86 veya x64 bit seçeneğini belirleyin, sonra yasal koşulları kabul etmek için **Tamam**'a tıklayın.  

11. Uygulamanızın`requirements.txt` dosyasında listelenen paketleri yüklemek için, `https://<your app service name>.scm.azurewebsites.net/DebugConsole` üzerinde göz atabileceğiniz Kudu konsolunu kullanın. Bunu yapmak için, Kudu Tanılama Konsolu'nda `D:\home\Python27` Python klasörünüze gidin, sonra aşağıdaki komutu [Kudu konsolu](/visualstudio/python/managing-python-on-azure-app-service#azure-app-service-kudu-console) bölümünde anlatıldığı şekilde çalıştırın:

    ```ps
    D:\home\Python27>python -m pip install --upgrade -r /home/site/wwwroot/requirements.txt
    ```

12. Yeni paketleri yükledikten sonra, Azure portalında App Service'i, **Yeniden Başlat** düğmesine basarak yeniden başlatın.

    > [!Tip]
    > Uygulamanızın `requirements.txt` dosyasında herhangi bir değişiklik yaparsanız, bu dosyada listelenen paketleri yüklemek için yeniden Kudu konsolunu kullanmayı unutmayın. 

13. Sunucu ortamını tamamen yapılandırdıktan sonra, sayfayı tarayıcıda yenilediğinizde Web uygulamasının görünmesi gerekir.

    ![Bottle, Flask ve Django uygulamalarının App Service'te yayımlanmasının sonuçları](./media/sql-api-python-application/python-published-app-services.png)

    > [!Tip] 
    > Web sayfası görünmüyorsa veya hala "İç sunucu hatası oluştuğundan sayfa görüntülenemiyor." iletisini almaya devam ediyorsanız, web.config dosyasını Kudo'da açın ve system.webServer bölümüne `<httpErrors errorMode="Detailed"></httpErrors>` ifadesini ekleyin, sonra sayfayı yenileyin. Bu, tarayıcıya ayrıntılı hata çıkışı sağlar. 

## <a name="troubleshooting"></a>Sorun giderme

Bilgisayarınızda çalıştırdığınız ilk Python uygulaması buysa YOL değişkeninize aşağıdaki klasörlerin (veya eşdeğer yükleme konumlarının) dahil olduğundan emin olun:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Oy sayfanızda bir hata alırsanız ve projenize **öğretici** dışında bir ad verdiyseniz **\_\_init\_\_.py**'nin doğru proje adına şu satırda başvurduğundan emin olun: `import tutorial.view`.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Azure Cosmos DB kullanan ilk Python web uygulamanızı tamamladınız ve bunu Azure'da yayımladınız.

Web uygulamanıza ek işlevsellik eklemek için [Azure Cosmos DB Python SDK'sında](sql-api-sdk-python.md) bulunan API'leri inceleyin.

Azure, Visual Studio ve Python hakkında daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/). 

Ek Python Flask öğreticileri için bkz. [Flask Mega-Eğitmeni, bölüm ı: Merhaba Dünya! ](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).
