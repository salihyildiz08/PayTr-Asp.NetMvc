
# Asp.Net MVC ile Paytr Entegrasyonu

Merhabalar,

Bu proje, **Asp.Net MVC** kullanarak **Paytr** ödeme entegrasyonunun nasıl gerçekleştirileceğini adım adım açıklamaktadır. Aşağıda, entegrasyon sürecini başarılı bir şekilde tamamlamak için gereken adımlar detaylı bir şekilde yer almaktadır.

## Entegrasyon Adımları

### 1. **Form Oluşturma**
   İlk olarak, kullanıcıdan **e-posta**, **tutar**, **ad** ve **soyad** gibi bilgileri almak için bir form oluşturmanız gerekmektedir. Bu formu `HomeController` içinde `Index` ActionResult'ında oluşturabilirsiniz.

   **HomeController** içinde şu şekilde bir form kodu yer almalıdır:

   ```csharp
   [HttpGet]
   public ActionResult Index()
   {
       return View();
   }

   [HttpPost]
   public ActionResult Index(PaytrPaymentModel model)
   {
       // Kullanıcıdan gelen verileri işleyin
       return RedirectToAction("Paytr");
   }
   ```

   Bu form kullanıcıdan ödeme bilgilerini almak için kullanılır.

### 2. **Paytr ActionResult'ına Veri Gönderme**
   Formu doldurduktan sonra, kullanıcı tarafından gönderilen verileri almak için `HomeController` içinde `Paytr` ActionResult'ına post etmelisiniz. Bu aşamada **Paytr mağaza verilerini** doldurmanız gerekmektedir.

   **Paytr ActionResult**'ı şu şekilde olabilir:

   ```csharp
   [HttpPost]
   public ActionResult Paytr(PaytrPaymentModel model)
   {
       // Paytr için gerekli parametreleri hazırlayın
       var paymentRequest = new PaytrRequest
       {
           // Paytr API'ye göndereceğiniz gerekli veriler
       };

       // Paytr API'ye istek gönderme işlemi burada yapılır
       return View("Paytr", paymentRequest);
   }
   ```

### 3. **Paytr View’ına Iframe Ekleme**
   Ödeme işlemini gerçekleştirmek için **Paytr iframe**'ini **Paytr view**'ına eklemeniz gerekmektedir. Bu iframe, ödeme işlemini Paytr üzerinden gerçekleştirecektir. Aşağıda bir iframe örneği bulunmaktadır:

   ```html
   <iframe src="https://www.paytr.com/odeme" width="100%" height="600px"></iframe>
   ```

   Bu iframe, kullanıcıyı Paytr ödeme sayfasına yönlendirecek ve ödeme işlemi burada gerçekleştirilecektir.

### 4. **Ödeme Verilerini Alma (Bildirim)**
   Paytr tarafından ödeme işlemi tamamlandıktan sonra, **Paytr'ın** göndereceği ödeme verilerini almak için `HomeController` içinde **Bildirim ActionResult'ı** oluşturmanız gerekmektedir. Bu action, **hem GET hem de POST** metodları ile kullanılacaktır.

   **Bildirim ActionResult'ı** şu şekilde olmalıdır:

   ```csharp
   [HttpGet]
   public ActionResult Bildirim()
   {
       // Yönlendirme yapılacak alan
       return View();
   }

   [HttpPost]
   public ActionResult Bildirim(PaytrNotificationModel notification)
   {
       // Ödeme işlemi sonrası yapılacak işlemler (veritabanı güncelleme, mail gönderme vs.)
       return Content("OK");
   }
   ```

   **GET metodunda**, kullanıcıyı yönlendirecek alan belirtilebilir. **POST metodunda** ise sipariş veritabanına kaydedilir ve ödeme ile ilgili diğer işlemler yapılabilir.

### 5. **Bildirim Sayfası ve Paytr Ayarları**
   Bildirim ActionResult'ı ekledikten sonra, sadece ekranda "OK" yazması yeterlidir. Bu sayfa, kullanıcı tarafından görüntülenmeyecek ve yalnızca **Paytr** tarafından bildirim almak için kullanılacaktır.

   Paytr bildirimlerini alabilmek için, **Paytr Mağaza paneli**'ne giriş yaptıktan sonra, **Ayarlar** menüsünden **Bildirim URL**'inin doğru şekilde ayarlanması gerekmektedir. Bu URL, `HomeController` içindeki **Bildirim POST metodu** adresine yönlendirilecektir.

---

## Paytr Entegrasyonu İçin Önemli Notlar
- **Paytr API anahtarları** ve mağaza bilgileri doğru şekilde doldurulmalıdır.
- **Bildirim URL**'ini doğru şekilde ayarladığınızdan emin olun. Bu, ödeme sonrası işlemler için çok önemlidir.
- İframe ile yapılan ödeme işlemi sonrası, kullanıcıya başarılı ödeme mesajları ve yönlendirme işlemleri yapılmalıdır.

---

## Katkı Sağlama
Bu projeye katkı sağlamak için aşağıdaki adımları takip edebilirsiniz:

1. Fork'layın.
2. Yeni bir branch oluşturun (`git checkout -b feature-branch`).
3. Değişikliklerinizi yapın ve commit edin (`git commit -am 'Paytr entegrasyonu eklendi'`).
4. Push yapın (`git push origin feature-branch`).
5. Bir pull request gönderin.

---



