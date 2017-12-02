---
title: "Azure Cosmos DB için Python Flask web uygulaması öğreticisi | Microsoft Docs"
description: "Azure'da barındırılan bir Python Flask web uygulamasından veri depolamak ve verilere erişmek için Azure Cosmos DB kullanma konulu veritabanı öğreticisini inceleyin. Uygulama geliştirme çözümleri bulun."
keywords: "Uygulama geliştirme, python flask, python web uygulaması, python web geliştirme"
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 10/17/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ef01271fd4885f9bdac80194bbf72e2a10df0d27
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Azure Cosmos DB kullanarak bir Python Flask web uygulaması derleme
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Bu öğreticide Azure App Service üzerinde barındırılan bir Python Flask web uygulamasından erişim verileri ve Azure Cosmos DB depolamak için nasıl kullanılacağını gösterir. Bu öğretici, Python ve Azure Web sitelerini kullanma konusunda biraz deneyim sahibi olduğunuzu varsayar.

Bu veritabanı öğreticisi şunlara değinir:

1. Oluşturma ve bir Azure Cosmos DB hesap sağlama.
2. Bir Python Flask uygulaması oluşturuluyor.
3. Bağlanma ve Azure Cosmos DB web uygulamanızı kullanarak.
4. Azure App Service web uygulamasına dağıtma.

Bu öğreticiyi izleyerek, bir yoklama için oy kullanmanıza olanak tanıyan basit bir oylama uygulaması oluşturacaksınız.

![Bu veritabanı Öğreticisi tarafından oluşturulan oylama uygulamasının ekran görüntüsü](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Veritabanı öğreticisi önkoşulları
Bu makaledeki yönergeleri izlemeden önce aşağıdakilerin yüklenmiş olduğundan emin olmanız gerekir:

* [Bir Azure aboneliği](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) ile **Azure geliştirme** ve **Python geliştirme** etkin. Bu Önkoşullar yüklü olduğundan ve bunları açarak yükleyen olup olmadığını kontrol edebilirsiniz **Visual Studio yükleyicisi** yerel olarak.   
* [Python 2.7 için Microsoft Azure SDK](https://azure.microsoft.com/downloads/). 
* [Python 2.7](https://www.python.org/downloads/windows/). 32 bit veya 64 bit yükleme kullanabilirsiniz.

> [!IMPORTANT]
> Python 2.7 ilk kez yüklüyorsanız Python 2.7.13 özelleştirme ekranında belirlediğinizden emin olun **yola Python.exe'yi eklemeyi**.
> 
> ![Yola python.exe'yi eklemeyi seçmeniz gereken Python 2.7.11'i Özelleştirme ekranının ekran görüntüsü](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* [Python 2.7 için Microsoft Visual C++ derleyicisi](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>1. Adım: Azure Cosmos DB veritabanı hesabı oluşturma
İlk olarak bir Azure Cosmos DB hesabı oluşturalım. Zaten bir hesabınız varsa veya bu öğretici için Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız [2. Adım: Yeni bir Python Flask web uygulaması oluşturma](#step-2-create-a-new-python-flask-web-application) adımına atlayabilirsiniz.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Şimdi şimdi yukarı sıfırdan yeni bir Python Flask web uygulaması oluşturmak nasıl yol.

## <a name="step-2-create-a-new-python-flask-web-application"></a>2. Adım: Yeni bir Python Flask web uygulaması oluşturma
1. Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine gidin ve ardından **Proje**'ye tıklayın.
   
    **Yeni Proje** iletişim kutusu görünür.
2. Sol bölmede **Şablonlar**'ı ve ardından **Python**'u genişletin ve sonra **Web**'e tıklayın. 
3. Orta bölmede **Flask Web Projesi**'ni seçin ve ardından **Ad** kutusuna **öğretici** yazın ve sonra **Tamam**'a tıklayın. Python paket adlarının [Python Kodu için Stil Kılavuzu](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)'nda açıklandığı gibi tamamen küçük harflerden oluşması gerektiğini unutmayın.
   
    Yeni olanlar için bilgi vermek gerekirse Python Flask, Python'da web uygulamalarını daha hızlı oluşturmanıza yardımcı olan bir web uygulaması geliştirme altyapısıdır.
   
    ![Solda Python vurgulanmış, ortada Python Flask Web Projesi seçilmiş ve Ad kutusunda öğretici adı bulunur şekilde Visual Studio'da Yeni Proje penceresinin ekran görüntüsü](./media/documentdb-python-application/image9.png)
4. **Visual Studio için Python Araçları** penceresinde **Sanal bir ortama yükleme**'ye tıklayın. 
   
    ![Veritabanı öğreticisinin ekran görüntüsü - Visual Studio için Python Araçları penceresi](./media/documentdb-python-application/python-install-virtual-environment.png)
5. İçinde **sanal ortam Ekle** penceresinde, select Python 2.7 ya da bir yorumlayıcı kutusunu işaretleyin, Python 3.5 diğer Varsayılanları kabul edin ve ardından **oluşturma**. Böylece projeniz için gereken Python sanal ortamı kurulmuş olur.
   
    ![Veritabanı öğreticisinin ekran görüntüsü - Visual Studio için Python Araçları penceresi](./media/documentdb-python-application/image10_A.png)
   
    Ortam başarılı bir şekilde yüklendiğinde çıktı penceresi şunu görüntüler: `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`

## <a name="step-3-modify-the-python-flask-web-application"></a>3. Adım: Python Flask web uygulamasını değiştirme
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
   
    ![Listede requirements.txt'ten yükle vurgulanmış şekilde env (Python 2.7) seçildiğini gösteren ekran görüntüsü](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    Başarılı yüklemeden sonra, çıktı penceresi aşağıdakini görüntüler:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > Nadir durumlarda çıktı penceresinde bir hata görebilirsiniz. Bu durumda, hata temizlemek için ilgili olup olmadığını denetleyin. Bazı durumlarda temizleme işlemi başarısız olur, ancak yükleme başarılı (yukarı bunu doğrulamak için çıktı penceresinde) olmaya devam eder. [Sanal ortamı doğrulama](#verify-the-virtual-environment) ile yüklemenizi denetleyebilirsiniz. Yükleme başarısız ancak doğrulama başarılı olduysa devam etmede bir sorun yoktur.
   > 
   > 

### <a name="verify-the-virtual-environment"></a>Sanal ortamı doğrulama
Her şeyin doğru şekilde yüklendiğinden emin olmamız gerekir.

1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Böylece Flask geliştirme sunucusu başlatılmış ve web tarayıcınız başlamış olur. Aşağıdaki sayfayı göreceksiniz.
   
    ![Bir tarayıcıda görüntülenen boş Python Flask web geliştirme projesi](./media/documentdb-python-application/image12.png)
3. Visual Studio'da **Shift**+**F5**'e basarak web sitesinin hata ayıklamasını durdurun.

### <a name="create-database-collection-and-document-definitions"></a>Veritabanı, koleksiyon ve belge tanımları oluşturma
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


### <a name="add-the-required-imports-to-viewspy"></a>Gerekli içeri aktarmaları views.py'ye ekleme
1. Çözüm Gezgini'nde **öğretici** klasörünü genişletin ve **views.py** dosyasını açın. 
2. Aşağıdaki içeri aktarma deyimlerini **views.py** dosyasının üstüne ekleyin ve ardından dosyayı kaydedin. Bunlar Azure Cosmos veritabanı pythonsdk'sını ve Flask paketlerini içeri aktarın.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Veritabanı, koleksiyon ve belge oluşturma
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


### <a name="read-database-collection-document-and-submit-form"></a>Veritabanı, koleksiyon ve belge okuma ve form gönderme
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
1. Çözüm Gezgini'nde, içinde **öğretici** klasörü, sağ tıklatın **şablonları** klasörünü tıklatın **Ekle**ve ardından **yeni öğe**. 
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
6. Aşağıdaki kodu `<body` öğesindeki **vote.html**'ye ekleyin. Yoklamayı görüntüler ve oyları kabul eder. Oy kaydetmede Denetim burada Azure Cosmos DB oyu atama tanır ve belgeyi uygun şekilde ekler Views.py'ye geçirilir.
   
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
3. İçinde [Azure portal](https://portal.azure.com/), gitmek **anahtarları** tıklatarak sayfa **Gözat**, **Azure Cosmos DB hesapları**, adına çift tıklayın kullanılacak hesap ve ardından **anahtarları** düğmesini **Essentials** alanı. Üzerinde **anahtarları** sayfasında, kopya **URI** değer ve yapıştırın **documentdb** değeri olarak dosya **DOCUMENTDB\_KONAK**özelliği. 
4. Azure portalında yedekleme **anahtarları** sayfasında, değerini kopyalayın **birincil anahtar** veya **ikincil anahtar**ve yapıştırın **documentdb**değeri olarak dosya **DOCUMENTDB\_anahtar** özelliği.
5. İçinde  **\_ \_init\_\_.py** dosya, aşağıdaki satırı ekleyin: 
   
        app.config.from_object('config')
   
    Böylece dosyanın içeriği şu olur:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. Tüm dosyaları ekledikten sonra, Çözüm Gezgini şöyle görünecektir:
   
    ![Visual Studio Çözüm Gezgini penceresinin ekran görüntüsü](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>4. Adım: Web uygulamanızı yerel olarak çalıştırma
1. **Ctrl**+**Shift**+**B** tuşlarına basarak çözümü oluşturun.
2. Derleme başarılı olduktan sonra, **F5**'e basarak web sitesini başlatın. Ekranınızda şunu göreceksiniz:
   
    ![Python + Azure Cosmos DB Oylama Uygulamasının bir web tarayıcısında görüntülenen ekran görüntüsü](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. Veritabanını oluşturmak için **Oylama Veritabanını Oluştur/Temizle**'ye tıklayın.
   
    ![Web uygulamasının Oluşturma Sayfası'nın ekran görüntüsü - geliştirme ayrıntıları](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. Ardından **Oy Ver**'e tıklayın ve seçiminizi belirleyin.
   
    ![Bir oylama sorusu sorulmuş web uygulamasının ekran görüntüsü](./media/documentdb-python-application/cosmos-db-vote.png)
5. Verdiğiniz her oy için uygun sayaç artırılır.
   
    ![Oylamanın Sonuçları sayfasını gösteren ekran görüntüsü](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. Shift + F5'e basarak projenin hata ayıklamasını durdurun.

## <a name="step-5-deploy-the-web-application-to-azure"></a>5. adım: Azure web uygulamasına dağıtma
Doğru karşı Azure Cosmos DB yerel olarak çalışan uygulamanın tamamı, bir web.config dosyası oluşturun, yerel ortamıyla eşleşecek şekilde sunucudaki dosyalar güncelleştirin ve sonra Azure üzerinde tamamlanan uygulama görüntülemek için yapacağız. Bu yordam, Visual Studio 2017 özeldir. Visual Studio farklı bir sürümünü kullanıyorsanız bkz [Azure App Service'te yayımlama](/visualstudio/python/publishing-to-azure.md).

1. Visual Studio'da **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle > Yeni öğe...** . Görünür, seçme iletişim **Azure web.config (Fast CGI)** şablonu ve select **Tamam**. Bu oluşturur bir `web.config` proje kök dosyasında. 

2. Değiştirme `<system.webServer>` bölümüne `web.config` böylece Python yükleme yolunu eşleşir. Örneğin, Python 2.7 x64 giriş aşağıdaki gibi görünmelidir:
    
    ```xml
    <system.webServer>
        <handlers>
            <add name="PythonHandler" path="*" verb="*" modules="FastCgiModule" scriptProcessor="D:\home\Python27\python.exe|D:\home\Python27\wfastcgi.py" resourceType="Unspecified" requireAccess="Script"/>
        </handlers>
    </system.webServer>
    ```

3. Ayarlama `WSGI_HANDLER` girişi `web.config` olarak `tutorial.app` proje adı ile eşleşmesi için. 

    ```xml
    <!-- Flask apps only: change the project name to match your app -->
    <add key="WSGI_HANDLER" value="tutorial.app"/>
    ```

4. Visual Studio'da **Çözüm Gezgini**, genişletin **öğretici** klasörünü sağ tıklatın `static` klasöründe seçin **Ekle > Yeni öğe...** , "Azure statik web.config dosyaları" şablonunu seçin ve Seç **Tamam**. Bu eylem başka oluşturur `web.config` içinde `static` bu klasör için işleme Python devre dışı bırakır klasör. Bu yapılandırma, varsayılan web sunucusu için Python uygulama kullanmak yerine statik dosyaları için istekleri gönderir.

5. Dosyaları kaydedin, sonra Çözüm Gezgini'nde projeye sağ tıklayın (etmediğinizden emin olun hala yerel olarak çalışan) seçip **Yayımla**.  
   
     ![Yayımla seçeneği vurgulanmış şekilde Çözüm Gezgini'nde öğreticinin seçili olduğu ekran görüntüsü](./media/documentdb-python-application/image20.png)
6. İçinde **Yayımla** iletişim kutusunda **Microsoft Azure App Service**seçin **Yeni Oluştur**ve ardından **Yayımla**.
   
    ![Microsoft Azure App Service vurgulanmış Web'de Yayımla penceresinin ekran görüntüsü](./media/documentdb-python-application/cosmos-db-python-publish.png)
7. İçinde **App Service Oluştur** iletişim kutusunda, birlikte web uygulamanız için bir ad girin, **abonelik**, **kaynak grubu**, ve **App Service planı**, ardından **oluşturma**.
   
    ![Microsoft Azure Web Apps Penceresi penceresinin ekran görüntüsü](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
8. Visual Studio birkaç saniye içinde sunucuya dosyalarınızı kopyalama tamamlandıktan ve "İç sunucu hatası oluştuğundan sayfa görüntülenemiyor." görüntüler üzerinde `http://<your app service>.azurewebsites.net/` sayfası.

9. Azure portalı, yeni uygulama hizmeti hesabınızı açın ve ardından Gezinti menüsünde doğru aşağı kaydırın **geliştirme araçları** bölümünde, select **uzantıları**, ardından **+ Ekle**.

10. İçinde **uzantı seçin** sayfasında, kaydırma aşağıya doğru en son Python 2.7 yükleme ve x86 veya x64 bit seçeneğini seçin ve ardından **Tamam** yasal koşulları kabul etmek için.  
   
11. Konumundaki gözatabilirsiniz Kudu Konsolu'nu kullanma `https://<your app service name>.scm.azurewebsites.net/DebugConsole`, uygulamanızın içinde listelenen paketleri yüklemek için `requirements.txt` dosya. Bu, Kudu tanılama konsolunda yapmak için Python klasöre gidin `D:\home\Python27` sonra açıklandığı şekilde aşağıdaki komutu çalıştırın [Kudu konsol](/visual-studio/python/managing-python-on-azure-app-service.md#azure-app-service-kudu-console) bölümü:

    ```
    D:\home\Python27>python -m pip install --upgrade -r /home/site/wwwroot/requirements.txt
    ```          

12. Uygulama hizmeti tuşlarına basarak yeni paketler yükledikten sonra Azure portalında yeniden başlatmanız **yeniden** düğmesi. 

    > [!Tip] 
    > Uygulamanızın herhangi bir değişiklik yaparsanız `requirements.txt` dosya, artık bu dosyada listelenen paketleri yüklemek için yeniden Kudu konsol kullandığınızdan emin olun. 

13. Sunucu ortamı tam olarak yapılandırdıktan sonra tarayıcıda sayfayı yenileyin ve web uygulaması görünmelidir.

    ![Uygulama hizmeti Bottle, Flask ve Django uygulamaları yayımlama sonuçları](./media/documentdb-python-application/python-published-app-services.png)

    > [!Tip] 
    > Web sayfası görünmez ya da "Sayfa görüntülenemiyor bir iç sunucu hatası oluştuğundan." Al ileti, Kudo içinde web.config dosyasını açın ve eklemek ` <httpErrors errorMode="Detailed"></httpErrors>` system.webServer bölümü için sonra sayfayı yenileyin. Bu tarayıcı için ayrıntılı hata çıkış sağlanır. 

## <a name="troubleshooting"></a>Sorun giderme
Bilgisayarınızda çalıştırdığınız ilk Python uygulaması buysa YOL değişkeninize aşağıdaki klasörlerin (veya eşdeğer yükleme konumlarının) dahil olduğundan emin olun:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Oy sayfanızda bir hata alırsanız ve projenize **öğretici** dışında bir ad verdiyseniz **\_\_init\_\_.py**'nin doğru proje adına şu satırda başvurduğundan emin olun: `import tutorial.view`.

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Azure Cosmos DB kullanarak ilk Python web uygulamanızı tamamladınız ve Azure'a yayımlanan.

Web uygulamanıza ek işlevsellik eklemek için kullanılabilen API'leri inceleyin [Azure Cosmos DB Python SDK'sı](documentdb-sdk-python.md).

Azure, Visual Studio ve Python hakkında daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/). 

Ek Python Flask öğreticileri için bkz. [Büyük Flask Öğreticisi, 1. Bölüm: Merhaba Dünya!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
