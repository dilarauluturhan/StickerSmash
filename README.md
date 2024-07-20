<div align="center">
  <h1 align="center">StickerSmash</h1>
</div>

**Bu projede Expo dokümantasyonunda yer alan StickerSmash uygulamasını yapmayı denedim ve adım adım aşağıda anlattım. Dokümantasyondaki uygulamanın detaylarına aşağıdaki linkten ulaşabilirsiniz:**

[StickerSmash App](https://docs.expo.dev/tutorial/introduction/)

### Uygulama Kurulumu

Yeni bir expo uygulaması başlatmak için `create-expo-app` komutunu kullanacağız. Bu komut Expo paketi kurulu halde bir React Native projesi kurmamızı sağlıyor. Terminalinize aşağıdaki komutunu yazıp çalıştırın:

```
npx create-expo-app StickerSmash --template blank
```

Bu komut StickerSmash uygulamızın kurulumu yapacak. `create-expo-app`, önceden yüklenmiş kütüphanelerle yeni bir proje oluşturmak ve kurmak için kullanabileceğimiz bir `--template` seçeneğine sahiptir. Bu yüzden gerekli minimum sayıda kütüphaneleri yükleyen `blank`i kullanıyoruz.

Projeyi web üzerinde çalıştırmak isterseniz terminale aşağıdaki komutu yazıp çalıştırın:

```
npx expo install react-dom react-native-web @expo/metro-runtime
```

Daha sonra yine terminalde aşağıdaki komutu çalıştırıp projeyi ayağa kaldırabilirsiniz:

```
npx expo start
```

Uygulamanın bu aşamadaki durumu şağıdaki gibi olmalıdır:

![01-app-running-on-all-platforms](https://github.com/user-attachments/assets/e6b4f003-470d-4140-97cc-0d26a6baa505)

### Ekran Oluşturma

Bu aşamada StickerSmash'in ilk ekranını yapıyoruz. Ekranda bir görsel alanı ve iki buton var. İlk buton cihazdan görsel seçmeyi sağlayacak. İkinci buton seçilen görselle uygulamaya devam etmeyi sağlayacak. Kullanıcı bir görsel seçtikten sonra bir çıkartma seçip görsele ekleyebilecek.
Kod yazmaya başlamadan önce ekranı parçalara ayıralım. Bu parçaları React Native'in sağladığı [Temel Component'ler](https://reactnative.dev/docs/components-and-apis) ile kodlayacağız.

![2-breakdown-of-layout](https://github.com/user-attachments/assets/09ec83a1-e5b9-4862-9dc7-d9ed1b9b1082)

Üç temel unsur var:

- Ekranın bir arka plan rengi var.
- Ekranın ortasında görüntülenen büyük bir resim var.
- Ekranın alt tarafında iki tane buton var.

O zaman arka plan rengini değiştirerek kodlmaya başlayalım.
_React Native, web ile aynı renk formatını kullanır._

Görüntüyü uygulamada görüntülemek için React Native'in `Image` component'ini kullanırız. Image component'ine statik veya URL kaynağı verirken `uri` property'sini kullanırız.

React Native, native platformlarda ekrana dokunma olayını yönetmek için çeşitli component'ler sağlıyor. Bu projede `Pressable` kullanılmış. Pressable; dokunma, basma, basılı tutma gibi olaylara tepki vermek üzere tasarlanmıştır. Uygulamada kullanacağımız iki buton var ve ikisinin birbirinden farklı stilleri var. Bu yüzden yeniden kullanacağımız buton component'ini yazıyoruz.

Uygulamanın bu aşamadaki durumu şağıdaki gibi olmalıdır:

![3-initial-layout](https://github.com/user-attachments/assets/a8f689d2-84f6-46c3-a95c-49d3ff99ca52)

### Resim Seçici Kullanma

Cihazımızın galerisinden görsel seçmek için kütüphaneye ihtiyacımız var. Bunun için Expo SDK kütüphanesi olan `expo-image-picker`'ı kullanacağız. Terminale geliyoruz ve aşağıdaki komutu çalıştırıyoruz:

```
npx expo install expo-image-picker
```

expo-image-picker, cihazın galerisinden bir görsel veya video seçmek için sistem kullanıcı arayüzünü görüntüleyen `launchImageLibraryAsync()` metodunu kullanır. Cihazın galerisinden görsel seçmek için oluşturduğumuz butonu kullanabiliriz.
Görüntü seçildikten sonra seçili görüntünün uri'sini içeren bir array oluşur. Bu array'den değeri alıp görseli uygulamada göstermek için kullanmalıyız.

### Modal Oluşturma

React Native, `modal` component'i sağlıyor. Genel olarak modal'lar, kullanıcının dikkatini kritik bilgilere çekmek veya onları harekete geçmeye yönlendirmek için kullanılır.
Bu uygulamada modal'ı kullanıcının mevcut emoji listesinden bir emoji seçmesine olanak tanıması için kullanacağız. Modal'ı oluşturduktan sonra da kullanıcının seçtiği emojiyi görselin üstüne koymasını sağlayacağız.

Uygulama dokümantasyonunun bu bölümünde emoji seçici modal'ını başarıyla oluşturduk ve bir emoji seçip görüntünün üzerinde görüntüleme mantığını uyguladık.

### Hareket Ekleme

Bu bölümde seçtiğimiz emojiyi hareket ettirmek için [React Native Gesture Handler](https://docs.swmansion.com/react-native-gesture-handler/docs/) kütüphanesini kullanacağız. Gesture Handler kütüphanesi kaydırma, dokunma, döndürme ve diğer hareketleri tanımak için platformun yerel dokunma işleme sistemini kullanır. Hareketler arasına animasyon eklemek için de [Reanimated](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/handling-gestures/) kütüphanesini kullanacağız.

Gesture Handler kütüphanesini kullanarak iki farklı hareket ekleyeceğiz:

- Emoji etiketinin boyutunu büyütmek için iki kez dokunmak.
- Çıkartmayı görselin herhangi bir yerine yerleştirebilmek için emoji etiketini ekranda hareket ettirmek üzere kaydırmak.

### Ekran Görüntüsü Alma ve Galeriye Kaydetme

Bu bölümde, üçüncü taraf bir kitaplık kullanarak ekran görüntüsünün nasıl alınacağını ve cihazın galerisine nasıl kaydedileceğini öğrendim. Ekran görüntüsü almaya olanak tanıyan `react-native-view-shot` ve bir görüntüyü kaydetmek için cihazın galerisine erişmeye olanak tanıyan `expo-media-library` kütüphanelerini kullanacağız.

Galeriye erişim gibi hassas bilgilere erişim gerektiren bir uygulama oluştururken öncelikle kullanıcının iznini istememiz gerekir. expo-media-library, izin durumunu veren bir `usePermissions()` hook'u ve izin verilmediğinde galeriye erişim istemek için bir `requestPermission()` metodu sağlar.

![4-permission](https://github.com/user-attachments/assets/cbf642ac-7640-4b1b-9207-9ac5b145d604)

### Platform Farklılıklarını Yönetme

Android, iOS ve Web'in farklı yetenekleri vardır. Bizim durumumuzda hem Android hem de iOS react-native-view-shot kitaplığıyla ekran görüntüsü yakalayabilir ancak web tarayıcıları bunu yapamaz. Bu bölümde, web tarayıcılarının tüm platformlarda aynı işlevselliği elde edebilmesi için nasıl istisna oluşturulacağını öğreneceğiz.

Web'de bir ekran görüntüsü yakalamak ve bunu resim olarak kaydetmek için [dom-to-image](https://github.com/tsayen/dom-to-image#readme) kütüphanesini kullanacağız. Bu kütüphane, DOM'da ekran görüntüsünün alınmasına ve bunun bir vektör (SVG) veya raster (PNG veya JPEG) görüntüsüne dönüştürülmesine olanak tanır.

### Status Bar, Açılış Ekranı ve Uygulama İkonunu Ayarlama

Uygulamanın tamamen başlatılabilir olduğunu düşünmeden önce durum çubuğunu yapılandırmamız, bir açılış ekranı eklememiz ve bir uygulama simgesi eklememiz gerekiyor.
[](https://docs.expo.dev/tutorial/configuration/)

# Tebrikler! Aynı kod tabanında Android, iOS ve Web üzerinde çalışan bir uygulama geliştirdik.

### Proje Kurulumu
Projeyi local'de çalışır duruma getirmek için şu adımları izleyin:

Projeyi klonlayın:
````
git clone https://github.com/dilarauluturhan/StickerSmash.git
````
Projeye gidin:
````
cd StickerSmash
````
Bağımlılıkları yükleyin:
````
npm install
````
Uygulamayı başlatın:
````
npx expo start
````
`http://localhost:8081` portunda uygulamanız başlayacaktır.

### İletişim
Dilara Uluturhan - [LinkedIn](https://www.linkedin.com/in/dilarauluturhan/) - dilarauluturhan@outlook.com
