---
title: "Öğretici: Çevirme, sentezlemek ve metin - Translator Text API'analiz etmek için bir Flask uygulaması derleme"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Azure Bilişsel hizmetler metin çevirme, duyguları çözümleyin ve çevrilen metni konuşmaya sentezlemek için kullanan Flask tabanlı web uygulaması oluşturacaksınız. Bizim Python kodu ve uygulamamızı sağlayan yollar Flask biridir. Size olmaz uygulamayı denetleyen JavaScript'i çok vakit ancak incelemek, tüm dosyaları sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: erhopf
ms.openlocfilehash: 69e6797e91fc645e3bd3e3b300cea6852a662214
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007393"
---
# <a name="tutorial-build-a-flask-app-with-azure-cognitive-services"></a>Öğretici: Azure Bilişsel hizmetler ile bir Flask uygulaması derleme

Bu öğreticide, Azure Bilişsel hizmetler metin çevirme, duyguları çözümleyin ve çevrilen metni konuşmaya sentezlemek için kullandığı bir Flask web uygulaması oluşturacaksınız. Python kodu ve uygulamamızı sağlayan yollar Flask kazanmasının, ancak uygulama birlikte çeker Javascript ve HTML ile yardımcı olacağız. Herhangi bir sorunla karşılaşırsanız bizi çalıştırırsanız geri bildirim düğmesini kullanarak bildirin.

İşte bu öğretici, neleri kapsar:

> [!div class="checklist"]
> * Azure Abonelik anahtarları alma
> * Geliştirme ortamınızı ayarlama ve bağımlılıkları yükler
> * Bir Flask uygulaması oluşturma
> * Translator metin çevirisi API'si metni çevirmek için kullanın
> * Giriş metin çevirileri ve olumlu/olumsuz düşüncelerini çözümleme için metin analizi kullanma
> * Çevrilmiş metin Sentezlenen konuşmaya dönüştürme konuşma hizmetlerini kullanma
> * Flask uygulamanızı yerel olarak çalıştırma

> [!TIP]
> İleriye ve tüm kodu tek seferde tüm örnek görmek istiyorsanız, derleme ile birlikte yönergeleri bulunur [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial).

## <a name="what-is-flask"></a>Flask nedir?

Flask web uygulamaları oluşturmaya yönelik bir microframework ' dir. Başka bir deyişle, Araçlar, kitaplıklar ve bir web uygulaması oluşturma olanak tanıyan teknolojileri ile Flask size sağlar. Bu web uygulaması, bazı web sayfaları, blog, wiki veya Git Takvim web tabanlı uygulama veya ticari Web sitesi olarak substantive olabilir.

Olanlar için bu öğreticiyi sonra yakından incelemek isteyen birkaç faydalı bağlantılar şunlardır:

* [Flask belgeleri](http://flask.pocoo.org/)
* [Flask Dummies için-Flask için Başlangıç Kılavuzu](https://codeburst.io/flask-for-dummies-a-beginners-guide-to-flask-part-uno-53aec6afc5b1)

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için ihtiyacınız olan yazılım ve abonelik anahtar gözden geçirelim.

* [Python 3.5.2 veya üzeri](https://www.python.org/downloads/)
* [Git araçları](https://git-scm.com/downloads)
* Bir IDE ya da metin düzenleyicisi gibi [Visual Studio Code](https://code.visualstudio.com/) veya [Atom](https://atom.io/)  
* [Chrome](https://www.google.com/chrome/browser/) veya [Firefox](https://www.mozilla.org/firefox)
* A **Translator metin çevirisi** abonelik anahtarı (Not bir bölgeyi seçmek için gerekli değildir.)
* A **metin analizi** abonelik anahtarı **Batı ABD** bölge.
* A **konuşma Hizmetleri** abonelik anahtarı **Batı ABD** bölge.

## <a name="create-an-account-and-subscribe-to-resources"></a>Bir hesap oluşturun ve kaynaklara abone olma

Daha önce belirtildiği gibi Bu öğretici için üç Abonelik anahtarları gerek yedekleyeceksiniz. Bu, Azure hesabınızda kaynak oluşturmak gerektiği anlamına gelir:
* Translator Metin Çevirisi
* Metin Analizi
* Konuşma Hizmetleri

Kullanım [Azure portalında bir Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) kaynakları oluşturmak adım adım yönergeler için.

> [!IMPORTANT]
> Bu öğretici için lütfen kaynaklarınızı Batı ABD bölgesinde oluşturun. Farklı bir bölgeye kullanıyorsanız, her Python dosyalarınızın temel URL'sini ayarlamak gerekir.

## <a name="set-up-your-dev-environment"></a>Geliştirme ortamınızı kurma

Flask web uygulamanızı oluşturmadan önce projeniz için bir çalışma dizini oluşturun ve birkaç Python paketleri yüklemeniz gerekir.

### <a name="create-a-working-directory"></a>Bir çalışma dizini oluşturma

1. Komut satırı (Windows) veya terminal (Linux/macOS) açın. Ardından, çalışan bir dizine ve projeniz için alt dizinleri oluşturun:  

   ```
   mkdir -p flask-cog-services/static/scripts && mkdir flask-cog-services/templates
   ```
2. Projenizin çalışma dizinine değiştirin:  

   ```
   cd flask-cog-services
   ```

### <a name="create-and-activate-your-virtual-environment-with-virtualenv"></a>Oluşturup ile sanal ortamınızı etkinleştirin `virtualenv`

Bizim Flask uygulamasını kullanarak bir sanal ortamı oluşturalım `virtualenv`. Sanal ortamı kullanarak çalışabilmesi için temiz bir ortama sahip olmasını sağlar.

1. Çalışma dizininizde bir sanal ortam oluşturmak için şu komutu çalıştırın: **macOS/Linux:**
   ```
   virtualenv venv --python=python3
   ```
   Biz Python 3 sanal ortam kullanması gerektiğini açıkça bildirdikten. Bu, birden çok Python yüklemeleri sahip kullanıcılar'ın doğru sürümü kullandığınızdan emin sağlar.

   **Windows CMD / Windows Bash:**
   ```
   virtualenv venv
   ```
   Örneği basit tutmak için sanal ortama venv adlandırıyorsunuz.

2. Sanal ortamınıza etkinleştirmek için komutları, platform/kabuğunuza bağlı olarak farklılık gösterir:   

   | Platform | Shell | Komut |
   |----------|-------|---------|
   | macOS/Linux | bash/zsh | `source venv/bin/activate` |
   | Windows | Bash | `source venv/Scripts/activate` |
   | | Komut Satırı | `venv\Scripts\activate.bat` |
   | | PowerShell | `venv\Scripts\Activate.ps1` |

   Bu komutu çalıştırdıktan sonra komut satırı veya terminal oturumu başında `venv`.

3. Bu komut satırı veya terminal yazarak dilediğiniz zaman oturum devre dışı bırakabilirsiniz: `deactivate`.

> [!NOTE]
> Python sahip oluşturmak ve sanal ortamları yönetmek için kapsamlı belgeler için bkz: [virtualenv](https://virtualenv.pypa.io/en/latest/).

### <a name="install-requests"></a>İstekleri yükleyin

HTTP 1.1 istek göndermek için kullanılan popüler bir modül istektir. Sorgu dizeleri, URL'leri el ile eklemek ya da POST verilerinizi formu kodlamak için gerek yoktur.

1. İstekleri yüklemek için çalıştırın:

   ```
   pip install requests
   ```

> [!NOTE]
> İstekleri hakkında daha fazla bilgi edinmek istiyorsanız bkz [istekleri: İnsanlar için HTTP](http://docs.python-requests.org/en/master/).

### <a name="install-and-configure-flask"></a>Yükleme ve Flask yapılandırma

Sonraki Flask yüklememiz gerekir. Flask web uygulamamız için yönlendirmeyi işler ve son kullanıcının bizim Abonelik anahtarları Gizle sunucudan sunucuya çağrıları yapmak sağlıyor.

1. Flask yüklemek için çalıştırın:
   ```
   pip install Flask
   ```
   Flask yüklendiği emin olalım. Çalıştırın:
   ```
   flask --version
   ```
   Sürüm terminale yazdırılan. Başka bir şey, bir sorun oluştu anlamına gelir.

2. Flask uygulamasını çalıştırmak için yapabilirsiniz veya Flask ile flask komutu veya Python'un -m anahtarını kullanın. Terminalinizi vererek çalışmak için hangi uygulama söylemeniz gerekebilir, bunu yapmadan önce `FLASK_APP` ortam değişkeni:

   **macOS/Linux**:
   ```
   export FLASK_APP=app.py
   ```

   **Windows**:
   ```
   set FLASK_APP=app.py
   ```

## <a name="create-your-flask-app"></a>Flask uygulamanızı oluşturma

Bu bölümde, bir HTML dosyası kullanıcılar, uygulamanızın kök ulaştığınızda döndüren bir temel Flask uygulaması oluşturacağız. Olası bir kod seçin çalışılırken çok fazla vaktinizi, biz bu dosyayı daha sonra güncelleştirmek için geri dönen.

### <a name="what-is-a-flask-route"></a>Flask yolu nedir?

Şimdi hakkında konuşmak için bir dakikanızı ayırın "[yollar](http://flask.pocoo.org/docs/1.0/api/#flask.Flask.route)". Yönlendirme, belirli bir işlev için bir URL bağlamak için kullanılır. Flask rota dekoratörler işlevleri belirli URL'lere kaydetmek için kullanır. Örneğin, ne zaman bir kullanıcı gittiğinde kök dizinine (`/`) web uygulamamızın `index.html` işlenir.  

```python
@app.route('/')
def index():
    return render_template('index.html')
```

Bu giriş hammer için daha fazla örnek bir göz atalım.

```python
@app.route('/about')
def about():
    return render_template('about.html')
```

Bu kod, bir kullanıcı gittiğinde sağlar `http://your-web-app.com/about` , `about.html` dosyası işlenir.

Bu örnekleri html sayfalarını bir kullanıcı için nasıl oluşturulacağını gösterir, ancak yolları bir düğmeye basıldığında veya giriş sayfası uzağa gitmek zorunda kalmadan istediğiniz sayıda eylemi ele API'leri çağırmak için de kullanılabilir. Çeviri, yaklaşımını ve konuşma sentezi için rota oluşturduğunuzda, bu eylem görürsünüz.

### <a name="get-started"></a>başlarken

1. Proje, IDE'de açın, ardından adlı bir dosya oluşturun `app.py` çalışma dizininizin kökünde. Ardından, bu kodları kopyalayın `app.py` ve kaydedin:

   ```python
   from flask import Flask, render_template, url_for, jsonify, request

   app = Flask(__name__)
   app.config['JSON_AS_ASCII'] = False

   @app.route('/')
   def index():
       return render_template('index.html')
   ```

   Bu kod bloğu görüntülenmek üzere bir uygulama söyler `index.html` her bir kullanıcı gittiğinde, web uygulamanızın kök dizinine (`/`).

2. Ardından, ön uç web uygulamamız için oluşturalım. Adlı bir dosya oluşturun `index.html` içinde `templates` dizin. Ardından bu kodları kopyalayın `templates/index.html`.

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <!-- Required metadata tags -->
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       <meta name="description" content="Translate and analyze text with Azure Cognitive Services.">
       <!-- Bootstrap CSS -->
       <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
       <title>Translate and analyze text with Azure Cognitive Services</title>
     </head>
     <body>
       <div class="container">
         <h1>Translate, synthesize, and analyze text with Azure</h1>
         <p>This simple web app uses Azure for text translation, text-to-speech conversion, and sentiment analysis of input text and translations. Learn more about <a href="https://docs.microsoft.com/azure/cognitive-services/">Azure Cognitive Services</a>.
        </p>
        <!-- HTML provided in the following sections goes here. -->

        <!-- End -->
       </div>

       <!-- Required Javascript for this tutorial -->
       <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
       <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
       <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
       <script type = "text/javascript" src ="static/scripts/main.js"></script>
     </body>
   </html>
   ```

3. Şimdi Flask uygulamasını test edin. Terminalden çalıştırın:

   ```
   flask run
   ```

4. Bir tarayıcı açın ve sağlanan URL'ye gidin. Tek sayfalı uygulamanızı görmeniz gerekir. Tuşuna **Ctrl + c** uygulamayı sonlandırmak için.

## <a name="translate-text"></a>Metin çevirme

Basit bir Flask uygulaması, şimdi işleyişi hakkında fikir sahip olduğunuza göre:

* Translator metin API'si çağrısı ve yanıt döndürmek için bazı Python yazma
* Flask, Python kodu çağırmak için yönlendirme oluşturma
* HTML metin girişi ve çeviri, bir dil seçici bir alanla güncelleştirin ve düğme Çevir
* Flask uygulamanız HTML ile etkileşim kurmak kullanıcılara Javascript yazma

### <a name="call-the-translator-text-api"></a>Translator metin çevirisi API'si çağırma

Yapmanız gereken ilk şey, Translator Text API çağırmak için fonksiyon yazdığınız yerdedir. Bu işlev iki bağımsız değişken sürer: `text_input` ve `language_output`. Bu işlev, her bir kullanıcı uygulamanızda Çevir düğmesine bastığında çağrılır. HTML metin alanına gönderilen `text_input`, ve dil seçimi değerini HTML olarak gönderilir `language_output`.

1. Adlı bir dosya oluşturarak başlayalım `translate.py` çalışma dizininizin kökünde.
2. Ardından, bu kodu ekleyin `translate.py`. Bu işlev iki bağımsız değişkeni alır: `text_input` ve `language_output`.
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   # If you prefer to use environment variables, see Extra Credit for more info.
   subscription_key = 'YOUR_TRANSLATOR_TEXT_SUBSCRIPTION_KEY'

   # Our Flask route will supply two arguments: text_input and language_output.
   # When the translate text button is pressed in our Flask app, the Ajax request
   # will grab these values from our web app, and use them in the request.
   # See main.js for Ajax calls.
   def get_translation(text_input, language_output):
       base_url = 'https://api.cognitive.microsofttranslator.com'
       path = '/translate?api-version=3.0'
       params = '&to=' + language_output
       constructed_url = base_url + path + params

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = [{
           'text' : text_input
       }]
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Translator metin çevirisi abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Yol Ekle `app.py`

Ardından, bir rota çağıran Flask uygulamanızı oluşturmak ihtiyacınız olacak `translate.py`. Bu yol her zaman bir kullanıcı uygulamanızda Çevir düğmesine bastığında çağrılır.

Bu uygulama için rotanız kabul edecek `POST` istekleri. İşlevi Çevrilecek metin çeviri için bir çıkış dil bekliyor olmasıdır.

Flask ayrıştırma ve her isteğin yönetmenize yardımcı olmak için yardımcı işlevleri sağlar. Sağlanan kod `get_json()` verilerden döndürür `POST` JSON olarak istek. Ardından kullanarak `data['text']` ve `data['to']`, metin ve çıktı dil değerleri geçirilen `get_translation()` işlevi kullanılabilir `translate.py`. Son adım, bu verileri web uygulamanızda görüntülemek ihtiyaç duyacağınız yanıtı JSON olarak döndürür. sağlamaktır.

Aşağıdaki bölümlerde, yaklaşım analizi ve konuşma sentezi için rotalar oluştururken bu işlemi yineleyin.

1. Açık `app.py` en üstündeki import deyimini bulun `app.py` ve aşağıdaki satırı ekleyin:

   ```python
   import translate
   ```
   Flask uygulamamızı aracılığıyla ulaşılabilir metodu artık `translate.py`.

2. Bu kod sonuna kadar Kopyala `app.py` ve kaydedin:

   ```python
   @app.route('/translate-text', methods=['POST'])
   def translate_text():
       data = request.get_json()
       text_input = data['text']
       translation_output = data['to']
       response = translate.get_translation(text_input, translation_output)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>Güncelleştirme `index.html`

Metin ve Flask uygulamanız çağırmak için bir rota çevirmek için bir işlev olduğuna göre sonraki adıma ait HTML uygulamanızı oluşturmaya başlamak için ' dir. Aşağıdaki HTML birkaç şey yapar:

* Burada, kullanıcıların Çevrilecek metin girebilirsiniz bir metin alanı sağlar.
* Dil Seçici içerir.
* Algılanan dilin ve çeviri sırasında döndürülen güven puanlarını işlemek için HTML öğeleri içerir.
* Çeviri çıkış görüntülendiği bir salt okunur metin alanı sağlar.
* Öğreticide daha sonra bu dosyaya ekleyeceksiniz yaklaşım analizi ve konuşma sentezi kod için yer tutucular içerir.

Güncelleştirelim `index.html`.

1. Açık `index.html` ve bu kod açıklamaları bulun:
   ```html
   <!-- HTML provided in the following sections goes here. -->

   <!-- End -->
   ```

2. Kod açıklamaları, bu HTML bloğunu ile değiştirin:
   ```html
   <div class="row">
     <div class="col">
       <form>
         <!-- Enter text to translate. -->
         <div class="form-group">
           <label for="text-to-translate"><strong>Enter the text you'd like to translate:</strong></label>
           <textarea class="form-control" id="text-to-translate" rows="5"></textarea>
         </div>
         <!-- Select output language. -->
         <div class="form-group">
           <label for="select-language"><strong>Translate to:</strong></label>
           <select class="form-control" id="select-language">
             <option value="ar">Arabic</option>
             <option value="ca">Catalan</option>
             <option value="zh-Hans">Chinese (Simplified)</option>
             <option value="zh-Hant">Chinese (Traditional)</option>
             <option value="hr">Croatian</option>
             <option value="en">English</option>
             <option value="fr">French</option>
             <option value="de">German</option>
             <option value="el">Greek</option>
             <option value="he">Hebrew</option>
             <option value="hi">Hindi</option>
             <option value="it">Italian</option>
             <option value="ja">Japanese</option>
             <option value="ko">Korean</option>
             <option value="pt">Portuguese</option>
             <option value="ru">Russian</option>
             <option value="es">Spanish</option>
             <option value="th">Thai</option>
             <option value="tr">Turkish</option>
             <option value="vi">Vietnamese</option>
           </select>
         </div>
         <button type="submit" class="btn btn-primary mb-2" id="translate">Translate text</button></br>
         <div id="detected-language" style="display: none">
           <strong>Detected language:</strong> <span id="detected-language-result"></span><br />
           <strong>Detection confidence:</strong> <span id="confidence"></span><br /><br />
         </div>

         <!-- Start sentiment code-->

         <!-- End sentiment code -->

       </form>
     </div>
     <div class="col">
       <!-- Translated text returned by the Translate API is rendered here. -->
       <form>
         <div class="form-group" id="translator-text-response">
           <label for="translation-result"><strong>Translated text:</strong></label>
           <textarea readonly class="form-control" id="translation-result" rows="5"></textarea>
         </div>

         <!-- Start voice font selection code -->

         <!-- End voice font selection code -->

       </form>

       <!-- Add Speech Synthesis button and audio element -->

       <!-- End Speech Synthesis button -->

     </div>
   </div>
   ```

Sonraki adım, bazı JavaScript'ler yazmaktır. HTML ve Flask rotanız arasında köprü budur.

### <a name="create-mainjs"></a>Oluşturma `main.js`  

`main.js` HTML ve Flask rotanız arasında köprü dosyasıdır. Uygulamanızın içeriğini işlemek ve jQuery ve Ajax XMLHttpRequest bir birleşimini kullanır `POST` Flask yollarınızı istekleri.

Aşağıdaki kod, HTML içeriği, Flask rotanız için bir istek oluşturmak için kullanılır. Özellikle, metin alanı ve dil Seçici içeriğini değişkenine atanır ve ardından boyunca isteğinde geçirilen `translate-text`.

Kod yanıt yinelenir ve HTML çeviri, algılanan dilin ve güvenilirlik puanı ile güncelleştirir.

1. Adlı bir dosya, IDE'NİZDEN oluşturun `main.js` içinde `static/scripts` dizin.
2. Bu kodu kopyalayın `static/scripts/main.js`:
   ```javascript
   //Initiate jQuery on load.
   $(function() {
     //Translate text with flask route
     $("#translate").on("click", function(e) {
       e.preventDefault();
       var translateVal = document.getElementById("text-to-translate").value;
       var languageVal = document.getElementById("select-language").value;
       var translateRequest = { 'text': translateVal, 'to': languageVal }

       if (translateVal !== "") {
         $.ajax({
           url: '/translate-text',
           method: 'POST',
           headers: {
               'Content-Type':'application/json'
           },
           dataType: 'json',
           data: JSON.stringify(translateRequest),
           success: function(data) {
             for (var i = 0; i < data.length; i++) {
               document.getElementById("translation-result").textContent = data[i].translations[0].text;
               document.getElementById("detected-language-result").textContent = data[i].detectedLanguage.language;
               if (document.getElementById("detected-language-result").textContent !== ""){
                 document.getElementById("detected-language").style.display = "block";
               }
               document.getElementById("confidence").textContent = data[i].detectedLanguage.score;
             }
           }
         });
       };
     });
     // In the following sections, you'll add code for sentiment analysis and
     // speech synthesis here.
   })
   ```

### <a name="test-translation"></a>Test çeviri

Şimdi çeviri uygulamasında test edin.

```
flask run
```

Sağlanan sunucu adresine gidin. Metin giriş alanı, bir dil seçin ve ENTER tuşuna çevir. Bir çeviri almanız gerekir. Bu işe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikleri gösteren olmayan ya da uygulama şekilde çalışmaması denemek için beklediğiniz önbelleğinizi temizlemek veya InPrivate/gizli penceresini açma.

Tuşuna **CTRL + c** uygulamayı sonlandırmak ve ardından sonraki bölüme geçelim.

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

[Metin analizi API'si](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) yaklaşım analizi gerçekleştirin, anahtar tümcecikleri ayıklayın metinden veya kaynak dili algılama için kullanılabilir. Bu uygulamada, yaklaşım analizi sağlanan metin pozitif, nötr veya negatif olup olmadığını belirlemek için kullanılacak ekleyeceğiz. API, 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması yaklaşımın olumlu olduğunu, 0’a yakın olması ise olumsuz olduğunu gösterir.

Bu bölümde, bazı şeyler oluşturacağız:

* Yaklaşım analizi gerçekleştirmek ve bir yanıt için metin analizi API'sini çağırmak için bazı Python yazma
* Flask, Python kodu çağırmak için yönlendirme oluşturma
* HTML duyarlılığı puanlarını ve analizi yapmak için bir düğme için bir alanla güncelleştirin
* Flask uygulamanız HTML ile etkileşim kurmak kullanıcılara Javascript yazma

### <a name="call-the-text-analytics-api"></a>Metin Analizi API’sini çağırma

Metin analizi API'sini çağırmak için fonksiyon yazalım. Bu işlev, dört bağımsız değişken sürer: `input_text`, `input_language`, `output_text`, ve `output_language`. Bu işlev, her bir kullanıcı uygulamanızda çalışma yaklaşım analizi düğmesine bastığında çağrılır. Veri metni alanına ve dil Seçici yanı sıra algılanan dil ve çeviri çıkış kullanıcı tarafından sağlanan her bir istekle sağlanır. Kaynak ve çeviri için duyarlılığı puanlarını yanıt nesnesini içerir. Aşağıdaki bölümlerde, yanıtları ayrıştırmak ve uygulamanızda kullanmak için bazı Javascript yazma dağıtacağız. Şimdilik, metin analizi API'si çağrıda şimdi odaklanın.

1. Adlı bir dosya oluşturalım `sentiment.py` çalışma dizininizin kökünde.
2. Ardından, bu kodu ekleyin `sentiment.py`.
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   subscription_key = 'YOUR_TEXT_ANALYTICS_SUBSCRIPTION_KEY'

   # Our Flask route will supply four arguments: input_text, input_language,
   # output_text, output_language.
   # When the run sentiment analysis button is pressed in our Flask app,
   # the Ajax request will grab these values from our web app, and use them
   # in the request. See main.js for Ajax calls.

   def get_sentiment(input_text, input_language, output_text, output_language):
       base_url = 'https://westus.api.cognitive.microsoft.com/text/analytics'
       path = '/v2.0/sentiment'
       constructed_url = base_url + path

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = {
           'documents': [
               {
                   'language': input_language,
                   'id': '1',
                   'text': input_text
               },
               {
                   'language': output_language,
                   'id': '2',
                   'text': output_text
               }
           ]
       }
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Metin analizi abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Yol Ekle `app.py`

Bir rota Flask uygulamanızı çağırır oluşturalım `sentiment.py`. Bu yol her zaman bir kullanıcı uygulamanızda çalışma yaklaşım analizi düğmesine bastığında çağrılır. Çeviri için bir yol gibi bu rota kabul edecek `POST` işlevi bağımsız değişken bekler. bu yana ister.

1. Açık `app.py` en üstündeki import deyimini bulun `app.py` ve güncelleştirin:

   ```python
   import translate, sentiment
   ```
   Flask uygulamamızı aracılığıyla ulaşılabilir metodu artık `sentiment.py`.

2. Bu kod sonuna kadar Kopyala `app.py` ve kaydedin:
   ```python
   @app.route('/sentiment-analysis', methods=['POST'])
   def sentiment_analysis():
       data = request.get_json()
       input_text = data['inputText']
       input_lang = data['inputLanguage']
       output_text = data['outputText']
       output_lang =  data['outputLanguage']
       response = sentiment.get_sentiment(input_text, input_lang, output_text, output_lang)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>Güncelleştirme `index.html`

Yaklaşım analizi ve yol Flask uygulamanızı çağırmak için çalıştırılacak bir işlev olduğuna göre sonraki adım uygulamanız için HTML yazma başlatmaktır. Aşağıdaki HTML birkaç şey yapar:

* Yaklaşım analizi çalıştırmak için uygulamanıza bir düğme ekler
* Yaklaşım Puanlama açıklayan bir öğe ekler
* Duyarlılık puanlarını görüntülemek için bir öğe ekler

1. Açık `index.html` ve bu kod açıklamaları bulun:
   ```html
   <!-- Start sentiment code-->

   <!-- End sentiment code -->
   ```

2. Kod açıklamaları, bu HTML bloğunu ile değiştirin:
   ```html
   <button type="submit" class="btn btn-primary mb-2" id="sentiment-analysis">Run sentiment analysis</button></br>
   <div id="sentiment" style="display: none">
      <p>Sentiment scores are provided on a 1 point scale. The closer the sentiment score is to 1, indicates positive sentiment. The closer it is to 0, indicates negative sentiment.</p>
      <strong>Sentiment score for input:</strong> <span id="input-sentiment"></span><br />
      <strong>Sentiment score for translation:</strong> <span id="translation-sentiment"></span>
   </div>
   ```

### <a name="update-mainjs"></a>Güncelleştirme `main.js`

Aşağıdaki kod, HTML içeriği, Flask rotanız için bir istek oluşturmak için kullanılır. Özellikle, metin alanı ve dil Seçici içeriğini değişkenine atanır ve ardından boyunca isteğinde geçirilen `sentiment-analysis` rota.

Kod yanıt yinelenir ve HTML ile duyarlılığı puanlarını güncelleştirir.

1. Adlı bir dosya, IDE'NİZDEN oluşturun `main.js` içinde `static` dizin.

2. Bu kodu kopyalayın `static/scripts/main.js`:
   ```javascript
   //Run sentinment analysis on input and translation.
   $("#sentiment-analysis").on("click", function(e) {
     e.preventDefault();
     var inputText = document.getElementById("text-to-translate").value;
     var inputLanguage = document.getElementById("detected-language-result").innerHTML;
     var outputText = document.getElementById("translation-result").value;
     var outputLanguage = document.getElementById("select-language").value;

     var sentimentRequest = { "inputText": inputText, "inputLanguage": inputLanguage, "outputText": outputText,  "outputLanguage": outputLanguage };

     if (inputText !== "") {
       $.ajax({
         url: "/sentiment-analysis",
         method: "POST",
         headers: {
             "Content-Type":"application/json"
         },
         dataType: "json",
         data: JSON.stringify(sentimentRequest),
         success: function(data) {
           for (var i = 0; i < data.documents.length; i++) {
             if (typeof data.documents[i] !== "undefined"){
               if (data.documents[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.documents[i].score;
               }
               if (data.documents[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.documents[i].score;
               }
             }
           }
           for (var i = 0; i < data.errors.length; i++) {
             if (typeof data.errors[i] !== "undefined"){
               if (data.errors[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.errors[i].message;
               }
               if (data.errors[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.errors[i].message;
               }
             }
           }
           if (document.getElementById("input-sentiment").textContent !== '' && document.getElementById("translation-sentiment").textContent !== ""){
             document.getElementById("sentiment").style.display = "block";
           }
         }
       });
     }
   });
   // In the next section, you'll add code for speech synthesis here.
   ```

### <a name="test-sentiment-analysis"></a>Test yaklaşım analizi

Şimdi duygu analizi uygulamasında test edin.

```
flask run
```

Sağlanan sunucu adresine gidin. Metin giriş alanı, bir dil seçin ve ENTER tuşuna çevir. Bir çeviri almanız gerekir. Ardından, çalışma yaklaşım analizi düğmesine basın. İki puanları görmeniz gerekir. Bu işe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikleri gösteren olmayan ya da uygulama şekilde çalışmaması denemek için beklediğiniz önbelleğinizi temizlemek veya InPrivate/gizli penceresini açma.

Tuşuna **CTRL + c** uygulamayı sonlandırmak ve ardından sonraki bölüme geçelim.

## <a name="convert-text-to-speech"></a>Metin okumayı dönüştürme

[Metin okuma API'si](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech) metni doğal insan benzeri Sentezlenen konuşmaya dönüştürün olanak tanır. Hizmet, standart, sinir ve özel seslerle destekler. Örnek uygulamamız tam listesi için birkaç kullanılabilir seslerini kullanır, bkz: [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech).

Bu bölümde, bazı şeyler oluşturacağız:

* Metin okuma API'si ile metin okuma dönüştürmek için bazı Python yazma
* Flask, Python kodu çağırmak için yönlendirme oluşturma
* HTML metin okuma ve ses kayıttan yürütme için bir öğe dönüştürmek için bir düğme ile güncelleştirme
* Kullanıcıların Flask uygulamanız ile etkileşime girmesine izin veren Javascript yazma

### <a name="call-the-text-to-speech-api"></a>Metin okuma API çağırma

Metin okuma dönüştürmek için işlevi yazalım. Bu işlev iki bağımsız değişken sürer: `input_text` ve `voice_font`. Bu işlev, her bir kullanıcı uygulamanızda dönüştürme metin okuma düğmesine bastığında çağrılır. `input_text` metni çevirmek için çağrı tarafından döndürülen çeviri çıktı `voice_font` HTML dosyasındaki ses yazı tipi Seçici değerdir.

1. Adlı bir dosya oluşturalım `synthesize.py` çalışma dizininizin kökünde.

2. Ardından, bu kodu ekleyin `synthesize.py`.
   ```Python
   import os, requests, time
   from xml.etree import ElementTree

   class TextToSpeech(object):
       def __init__(self, input_text, voice_font):
           subscription_key = 'YOUR_SPEECH_SERVICES_SUBSCRIPTION_KEY'
           self.subscription_key = subscription_key
           self.input_text = input_text
           self.voice_font = voice_font
           self.timestr = time.strftime('%Y%m%d-%H%M')
           self.access_token = None

       # This function performs the token exchange.
       def get_token(self):
           fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
           headers = {
               'Ocp-Apim-Subscription-Key': self.subscription_key
           }
           response = requests.post(fetch_token_url, headers=headers)
           self.access_token = str(response.text)

       # This function calls the TTS endpoint with the access token.
       def save_audio(self):
           base_url = 'https://westus.tts.speech.microsoft.com/'
           path = 'cognitiveservices/v1'
           constructed_url = base_url + path
           headers = {
               'Authorization': 'Bearer ' + self.access_token,
               'Content-Type': 'application/ssml+xml',
               'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
               'User-Agent': 'YOUR_RESOURCE_NAME',
           }
           # Build the SSML request with ElementTree
           xml_body = ElementTree.Element('speak', version='1.0')
           xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
           voice = ElementTree.SubElement(xml_body, 'voice')
           voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
           voice.set('name', 'Microsoft Server Speech Text to Speech Voice {}'.format(self.voice_font))
           voice.text = self.input_text
           # The body must be encoded as UTF-8 to handle non-ascii characters.
           body = ElementTree.tostring(xml_body, encoding="utf-8")

           #Send the request
           response = requests.post(constructed_url, headers=headers, data=body)

           # Write the response as a wav file for playback. The file is located
           # in the same directory where this sample is run.
           return response.content
   ```
3. Konuşma Hizmetleri abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Yol Ekle `app.py`

Bir rota Flask uygulamanızı çağırır oluşturalım `synthesize.py`. Bu yol her zaman bir kullanıcı uygulamanızda dönüştürme metin okuma düğmesine bastığında çağrılır. Çeviri ve yaklaşım analizi için yolları gibi bu rota kabul edecek `POST` işlevi iki bağımsız değişken bekliyor. bu yana istekleri: sentezlemek için metin ve ses tipi kayıttan yürütme için.

1. Açık `app.py` en üstündeki import deyimini bulun `app.py` ve güncelleştirin:

   ```python
   import translate, sentiment, synthesize
   ```
   Flask uygulamamızı aracılığıyla ulaşılabilir metodu artık `synthesize.py`.

2. Bu kod sonuna kadar Kopyala `app.py` ve kaydedin:

   ```Python
   @app.route('/text-to-speech', methods=['POST'])
   def text_to_speech():
       data = request.get_json()
       text_input = data['text']
       voice_font = data['voice']
       tts = synthesize.TextToSpeech(text_input, voice_font)
       tts.get_token()
       audio_response = tts.save_audio()
       return audio_response
   ```

### <a name="update-indexhtml"></a>Güncelleştirme `index.html`

Metin okuma ve Flask uygulamanız çağırmak için bir rota dönüştürmek için işlevi olduğuna göre sonraki adım uygulamanız için HTML yazma başlatmaktır. Aşağıdaki HTML birkaç şey yapar:

* Sesli seçimi açılan sağlar
* Metin okuma dönüştürmek için bir düğme ekler
* Sentezlenen konuşma çalmak için kullanılan bir audio öğesi ekler

1. Açık `index.html` ve bu kod açıklamaları bulun:
   ```html
   <!-- Start voice font selection code -->

   <!-- End voice font selection code -->
   ```

2. Kod açıklamaları, bu HTML bloğunu ile değiştirin:
   ```html
   <div class="form-group">
     <label for="select-voice"><strong>Select voice font:</strong></label>
     <select class="form-control" id="select-voice">
       <option value="(ar-SA, Naayf)">Arabic | Male | Naayf</option>
       <option value="(ca-ES, HerenaRUS)">Catalan | Female | HerenaRUS</option>
       <option value="(zh-CN, HuihuiRUS)">Chinese (Mainland) | Female | HuihuiRUS</option>
       <option value="(zh-CN, Kangkang, Apollo)">Chinese (Mainland) | Male | Kangkang, Apollo</option>
       <option value="(zh-HK, Tracy, Apollo)">Chinese (Hong Kong)| Female | Tracy, Apollo</option>
       <option value="(zh-HK, Danny, Apollo)">Chinese (Hong Kong) | Male | Danny, Apollo</option>
       <option value="(zh-TW, Yating, Apollo)">Chinese (Taiwan)| Female | Yaiting, Apollo</option>
       <option value="(zh-TW, Zhiwei, Apollo)">Chinese (Taiwan) | Male | Zhiwei, Apollo</option>
       <option value="(hr-HR, Matej)">Croatian | Male | Matej</option>
       <option value="(en-US, Jessa24kRUS)">English (US) | Female | Jessa24kRUS</option>
       <option value="(en-US, Guy24kRUS)">English (US) | Male | Guy24kRUS</option>
       <option value="(en-IE, Sean)">English (IE) | Male | Sean</option>
       <option value="(fr-FR, Julie, Apollo)">French | Female | Julie, Apollo</option>
       <option value="(fr-FR, HortenseRUS)">French | Female | Julie, HortenseRUS</option>
       <option value="(fr-FR, Paul, Apollo)">French | Male | Paul, Apollo</option>
       <option value="(de-DE, Hedda)">German | Female | Hedda</option>
       <option value="(de-DE, HeddaRUS)">German | Female | HeddaRUS</option>
       <option value="(de-DE, Stefan, Apollo)">German | Male | Apollo</option>
       <option value="(el-GR, Stefanos)">Greek | Male | Stefanos</option>
       <option value="(he-IL, Asaf)">Hebrew (Isreal) | Male | Asaf</option>
       <option value="(hi-IN, Kalpana, Apollo)">Hindi | Female | Kalpana, Apollo</option>
       <option value="(hi-IN, Hemant)">Hindi | Male | Hemant</option>
       <option value="(it-IT, LuciaRUS)">Italian | Female | LuciaRUS</option>
       <option value="(it-IT, Cosimo, Apollo)">Italian | Male | Cosimo, Apollo</option>
       <option value="(ja-JP, Ichiro, Apollo)">Japanese | Male | Ichiro</option>
       <option value="(ja-JP, HarukaRUS)">Japanese | Female | HarukaRUS</option>
       <option value="(ko-KR, HeamiRUS)">Korean | Female | Haemi</option>
       <option value="(pt-BR, HeloisaRUS)">Portuguese (Brazil) | Female | HeloisaRUS</option>
       <option value="(pt-BR, Daniel, Apollo)">Portuguese (Brazil) | Male | Daniel, Apollo</option>
       <option value="(pt-PT, HeliaRUS)">Portuguese (Portugal) | Female | HeliaRUS</option>
       <option value="(ru-RU, Irina, Apollo)">Russian | Female | Irina, Apollo</option>
       <option value="(ru-RU, Pavel, Apollo)">Russian | Male | Pavel, Apollo</option>
       <option value="(ru-RU, EkaterinaRUS)">Russian | Female | EkaterinaRUS</option>
       <option value="(es-ES, Laura, Apollo)">Spanish | Female | Laura, Apollo</option>
       <option value="(es-ES, HelenaRUS)">Spanish | Female | HelenaRUS</option>
       <option value="(es-ES, Pablo, Apollo)">Spanish | Male | Pablo, Apollo</option>
       <option value="(th-TH, Pattara)">Thai | Male | Pattara</option>
       <option value="(tr-TR, SedaRUS)">Turkish | Female | SedaRUS</option>
       <option value="(vi-VN, An)">Vietnamese | Male | An</option>
     </select>
   </div>
   ```

3. Ardından, bu kod açıklamaları bulun:
   ```html
   <!-- Add Speech Synthesis button and audio element -->

   <!-- End Speech Synthesis button -->
   ```

4. Kod açıklamaları, bu HTML bloğunu ile değiştirin:

```html
<button type="submit" class="btn btn-primary mb-2" id="text-to-speech">Convert text-to-speech</button>
<div id="audio-playback">
  <audio id="audio" controls>
    <source id="audio-source" type="audio/mpeg" />
  </audio>
</div>
```

5. Çalışmanızı kaydetmek emin olun.

### <a name="update-mainjs"></a>Güncelleştirme `main.js`

Aşağıdaki kod, HTML içeriği, Flask rotanız için bir istek oluşturmak için kullanılır. Özellikle, çeviri ve ses tipi değişkenine atanır ve ardından boyunca isteğinde geçirilen `text-to-speech` rota.

Kod yanıt yinelenir ve HTML ile duyarlılığı puanlarını güncelleştirir.

1. Adlı bir dosya, IDE'NİZDEN oluşturun `main.js` içinde `static` dizin.
2. Bu kodu kopyalayın `static/scripts/main.js`:
   ```javascript
   // Convert text-to-speech
   $("#text-to-speech").on("click", function(e) {
     e.preventDefault();
     var ttsInput = document.getElementById("translation-result").value;
     var ttsVoice = document.getElementById("select-voice").value;
     var ttsRequest = { 'text': ttsInput, 'voice': ttsVoice }

     var xhr = new XMLHttpRequest();
     xhr.open("post", "/text-to-speech", true);
     xhr.setRequestHeader("Content-Type", "application/json");
     xhr.responseType = "blob";
     xhr.onload = function(evt){
       if (xhr.status === 200) {
         audioBlob = new Blob([xhr.response], {type: "audio/mpeg"});
         audioURL = URL.createObjectURL(audioBlob);
         if (audioURL.length > 5){
           var audio = document.getElementById("audio");
           var source = document.getElementById("audio-source");
           source.src = audioURL;
           audio.load();
           audio.play();
         }else{
           console.log("An error occurred getting and playing the audio.")
         }
       }
     }
     xhr.send(JSON.stringify(ttsRequest));
   });
   // Code for automatic language selection goes here.
   ```
3. Neredeyse bitti. Bazı kod ekleyin, yapacağımız son bir şey olduğunu `main.js` otomatik çeviri için seçilen dil için temel bir ses tipi seçin. Bu kod bloğuna ekleyin `main.js`:
   ```javascript
   // Automatic voice font selection based on translation output.
   $('select[id="select-language"]').change(function(e) {
     if ($(this).val() == "ar"){
       document.getElementById("select-voice").value = "(ar-SA, Naayf)";
     }
     if ($(this).val() == "ca"){
       document.getElementById("select-voice").value = "(ca-ES, HerenaRUS)";
     }
     if ($(this).val() == "zh-Hans"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "zh-Hant"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "hr"){
       document.getElementById("select-voice").value = "(hr-HR, Matej)";
     }
     if ($(this).val() == "en"){
       document.getElementById("select-voice").value = "(en-US, Jessa24kRUS)";
     }
     if ($(this).val() == "fr"){
       document.getElementById("select-voice").value = "(fr-FR, HortenseRUS)";
     }
     if ($(this).val() == "de"){
       document.getElementById("select-voice").value = "(de-DE, HeddaRUS)";
     }
     if ($(this).val() == "el"){
       document.getElementById("select-voice").value = "(el-GR, Stefanos)";
     }
     if ($(this).val() == "he"){
       document.getElementById("select-voice").value = "(he-IL, Asaf)";
     }
     if ($(this).val() == "hi"){
       document.getElementById("select-voice").value = "(hi-IN, Kalpana, Apollo)";
     }
     if ($(this).val() == "it"){
       document.getElementById("select-voice").value = "(it-IT, LuciaRUS)";
     }
     if ($(this).val() == "ja"){
       document.getElementById("select-voice").value = "(ja-JP, HarukaRUS)";
     }
     if ($(this).val() == "ko"){
       document.getElementById("select-voice").value = "(ko-KR, HeamiRUS)";
     }
     if ($(this).val() == "pt"){
       document.getElementById("select-voice").value = "(pt-BR, HeloisaRUS)";
     }
     if ($(this).val() == "ru"){
       document.getElementById("select-voice").value = "(ru-RU, EkaterinaRUS)";
     }
     if ($(this).val() == "es"){
       document.getElementById("select-voice").value = "(es-ES, HelenaRUS)";
     }
     if ($(this).val() == "th"){
       document.getElementById("select-voice").value = "(th-TH, Pattara)";
     }
     if ($(this).val() == "tr"){
       document.getElementById("select-voice").value = "(tr-TR, SedaRUS)";
     }
     if ($(this).val() == "vi"){
       document.getElementById("select-voice").value = "(vi-VN, An)";
     }
   });
   ```

### <a name="test-your-app"></a>Uygulamanızı test etme

Şimdi konuşma sentezi uygulamasında test edin.

```
flask run
```

Sağlanan sunucu adresine gidin. Metin giriş alanı, bir dil seçin ve ENTER tuşuna çevir. Bir çeviri almanız gerekir. Ardından, bir ses seçin ve sonra dönüştürme metin okuma düğmesine basın. Çeviri Sentezlenen konuşma olarak kayıttan yürütülebilir. Bu işe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikleri gösteren olmayan ya da uygulama şekilde çalışmaması denemek için beklediğiniz önbelleğinizi temizlemek veya InPrivate/gizli penceresini açma.

İşte bu kadar çevirileri gerçekleştirir, duygu ve Sentezlenen konuşma çözümler çalışan bir uygulamayı vardır. Tuşuna **CTRL + c** uygulamayı sonlandırmak için. Diğer kontrol ettiğinizden emin olun [Azure Bilişsel Hizmetler](https://docs.microsoft.com/azure/cognitive-services/).

## <a name="get-the-source-code"></a>Kaynak kodu alma

Bu proje için kaynak kodu kullanılabilir [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial).

## <a name="next-steps"></a>Sonraki adımlar

* [Translator Metin Çevirisi API'si başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
* [Metin Analizi API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)
* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech)
