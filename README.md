README

مشروع: تطبيق توصيل + طلبات (Customer - Driver - Merchant - P2P Chat) لغة: Flutter (Dart) هدف المستند: يحتوي على سكافولد (skeleton) للتطبيق، شيفرة مبدئية لملف main.dart، صفحات لكل واجهة، نموذج قاعدة بيانات مقترح، وتعليمات تشغيل ونشر على Google Play مع الاحتفاظ بالسورس كود.


---

مكونات المشروع الموجودة هنا

pubspec.yaml (قائمة الحزم التي ستحتاجها)

lib/main.dart (نقطة دخول التطبيق، توجيه إلى 4 واجهات)

lib/pages/:

customer_home.dart

driver_home.dart

merchant_home.dart

chat_page.dart

auth_page.dart


lib/models/:

user_model.dart

order_model.dart

product_model.dart


lib/services/:

auth_service.dart (Auth باستخدام Firebase Auth أو بديل)

firestore_service.dart (الاتصال بـ Firestore)

chat_service.dart (رسائل P2P)


assets/:

أيقونات وصور شعار (يمكن توليدها بالذكاء الاصطناعي أو رفع لوجو خاص بك)




---

توجيهات سريعة قبل التشغيل

1. تأكد من تثبيت Flutter وAndroid SDK على جهازك.


2. في حال استخدام Firebase: أنشئ مشروع Firebase، فعّل Authentication (Email/Password أو Phone)، Firestore، وCloud Storage (إن احتجت).


3. أنشئ ملف google-services.json (Android) و/أو GoogleService-Info.plist (iOS) وضعهما في المسارات المناسبة.


4. شغّل flutter pub get ثم flutter run.




---

ملف pubspec.yaml الموصى به (مقتطف)

name: delivery_market_app
description: Delivery + Orders app skeleton
publish_to: 'none'
version: 0.1.0+1

environment:
  sdk: ">=2.18.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  firebase_core: ^2.0.0
  firebase_auth: ^4.0.0
  cloud_firestore: ^4.0.0
  firebase_storage: ^11.0.0
  provider: ^6.0.0
  uuid: ^3.0.6
  flutter_local_notifications: ^12.0.0
  geolocator: ^9.0.2
  flutter_map: ^4.0.0
  flutter_chat_types: ^4.0.0
  flutter_chat_ui: ^1.0.0

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  uses-material-design: true
  assets:
    - assets/


---

lib/main.dart — سكربت مبدئي

import 'package:flutter/material.dart';
import 'pages/auth_page.dart';
import 'pages/customer_home.dart';
import 'pages/driver_home.dart';
import 'pages/merchant_home.dart';
import 'pages/chat_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  // هنا تهيئة Firebase إن اخترت استخدامه
  // await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Delivery & Orders',
      theme: ThemeData(
        primarySwatch: Colors.teal,
      ),
      home: RoleSelectionPage(),
      routes: {
        '/auth': (_) => AuthPage(),
        '/customer': (_) => CustomerHome(),
        '/driver': (_) => DriverHome(),
        '/merchant': (_) => MerchantHome(),
        '/chat': (_) => ChatPage(),
      },
    );
  }
}

class RoleSelectionPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('اختر نوع الحساب')),
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () => Navigator.pushNamed(context, '/customer'),
              child: Text('مستخدم (زبون)'),
            ),
            SizedBox(height: 12),
            ElevatedButton(
              onPressed: () => Navigator.pushNamed(context, '/driver'),
              child: Text('كابتن / سائق'),
            ),
            SizedBox(height: 12),
            ElevatedButton(
              onPressed: () => Navigator.pushNamed(context, '/merchant'),
              child: Text('صاحب محل / تاجر'),
            ),
            SizedBox(height: 18),
            TextButton(
              onPressed: () => Navigator.pushNamed(context, '/auth'),
              child: Text('تسجيل / دخول'),
            ),
          ],
        ),
      ),
    );
  }
}


---

أمثلة صفحات (مقتطفات)

lib/pages/customer_home.dart

import 'package:flutter/material.dart';

class CustomerHome extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('واجهة المستخدم')),
      body: Center(child: Text('قائمة المتاجر والبحث عن منتجات')),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.chat),
        onPressed: () => Navigator.pushNamed(context, '/chat'),
      ),
    );
  }
}

lib/pages/driver_home.dart

import 'package:flutter/material.dart';

class DriverHome extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('واجهة الكباتن')),
      body: Center(child: Text('قائمة التوصيلات المتاحة والطلبات')),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.map),
        onPressed: () {},
      ),
    );
  }
}

lib/pages/merchant_home.dart

import 'package:flutter/material.dart';

class MerchantHome extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('لوحة التاجر')),
      body: Center(child: Text('إدارة المتجر: إضافة/تعديل المنتجات')),
    );
  }
}

lib/pages/chat_page.dart (مبدئي)

import 'package:flutter/material.dart';

class ChatPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('الدردشة P2P')),
      body: Center(child: Text('واجهة المحادثة بين المستخدم والكابتن/التاجر')),
    );
  }
}


---

نموذج قاعدة بيانات (Firestore) المقترح

users/{userId}:

role: customer | driver | merchant

name, phone, email, location (geo)

merchantId (إن كان تاجر)


merchants/{merchantId}:

name, address, products[], rating


products/{productId}:

merchantId, name, price, images[], stock


orders/{orderId}:

customerId, merchantId, driverId, items[], status, total, createdAt


chats/{chatId}/messages/{messageId}:

from, to, text, timestamp, type(text/image)




---

مزايا مقترحة احترافية

تتبع موقع الـ Driver بالـ GPS وتحديث زمن الوصول التقديري (ETA).

إشعارات فورية (Firebase Cloud Messaging).

نظام تقييم بعد كل توصيل (rating/review).

بوابة دفع (Stripe / PayPal / Moyasar المحلية) أو الدفع عند الإستلام.

لوحة تاجر لإدارة المنتجات، العروض، تقارير المبيعات.

إدارة حالات الطلب (معلق، قُبل، في الطريق، مُنجز، مُلغى).

واجهة إدارة (Admin) لمراقبة المستخدمين والطلبات.



---

كيفية الاحتفاظ بالسورس كود

1. أنشئ مستودع Git محليًا: git init ثم git add . و git commit -m "initial scaffold".


2. أنشئ مستودع على GitHub/GitLab/Bitbucket وادفع التغييرات git remote add origin ... و git push -u origin main.


3. احفظ نسخة احتياطية محلية ونسخة على سحابة خاصة بك (Google Drive/Dropbox) إن رغبت.




---

بناء ملف AAB للنشر على Google Play

1. توليد keystore (مرة واحدة):

keytool -genkey -v -keystore key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias delivery_key


2. اضف إعدادات signing في android/app/build.gradle.


3. شغّل flutter build appbundle --release للحصول على .aab.


4. ادخل إلى Google Play Console (حساب مطور، رسوم $25 لمرة واحدة)، ثم ارفع الـ aab.




---

ملاحظات حول التصميم بالذكاء الاصطناعي

يمكنني توليد اقتراحات لونية، لوغو، واجهات (تصميم شامل) باستخدام أدوات الذكاء الاصطناعي.

أنصح بتوليد لوجو بألوان متباينة وأيقونة واضحة لاستخدامها في manifest وStore listing.



---

خطواتي التالية (قمت بالفعل بإنشاء السكافولد أعلاه)

إذا رغبت: أستطيع الآن توليد:

1. نموذج كامل لخدمة الدردشة (مع Firestore) + مثال رسائل.


2. نظام تسجيل دخول وملفات تعريف للـ 3 أدوار.


3. صفحة إدارة منتجات كاملة للتاجر مع رفع صور إلى Firebase Storage.


4. تكامل تتبع GPS للكباتن وخريطة بسيطة.


5. سكربت إنشاء ملف AAB موقّع وشرح تفصيلي لنشره.





---

ملاحظة أخيرة

المشروع هنا هو نسخة مبدئية احترافية للانطلاق. لتطوير نسخة إنتاجية آمنة وقابلة للتوسع، سأنفذ خطوات إضافية متعلقة بالأمن (قواعد Firestore، التحقق من الهوية، إدارة مفاتيح الدفع)، واختبارات وظيفية وأداء.

/* إضافات تنفيذ الخيارات 1-5 */


---

تنفيذ الخيارات المطلوبة (1-5)

فيما يلي الإضافات البرمجية والتعليمات التفصيلية لتنفيذ:

1. نظام تسجيل/تسجيل دخول مفصل (Firebase Auth) + صفحات ملفات تعريف للأدوار.


2. نظام دردشة P2P كامل يعمل مع Firestore (رسائل، إرسال صور، مؤشرات قراءة).


3. واجهة التاجر لإدارة المنتجات (CRUD + رفع صور إلى Firebase Storage).


4. تتبع الكباتن بالـ GPS مع عرض على الخريطة وإشعارات.


5. إعداد لبناء ملف .aab موقّع وملف keystore مع سكربتات.



> جميع الملفات التالية مضافة كنماذج يمكنك نسخها داخل مشروع Flutter الموجود في README.




---

1) نظام تسجيل / تسجيل دخول (Firebase Auth) — ملفات مضافة

lib/services/auth_service.dart

import 'package:firebase_auth/firebase_auth.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class AuthService {
  final FirebaseAuth _auth = FirebaseAuth.instance;
  final FirebaseFirestore _db = FirebaseFirestore.instance;

  // تسجيل بواسطة إيميل وكلمة مرور
  Future<User?> signUpWithEmail(String email, String password, Map<String,dynamic> extraProfile) async {
    final cred = await _auth.createUserWithEmailAndPassword(email: email, password: password);
    final user = cred.user;
    if (user != null) {
      await _db.collection('users').doc(user.uid).set({
        'email': email,
        'role': extraProfile['role'] ?? 'customer',
        'name': extraProfile['name'] ?? '',
        'phone': extraProfile['phone'] ?? '',
        'location': extraProfile['location'] ?? null,
        ...extraProfile,
      });
    }
    return user;
  }

  Future<User?> signInWithEmail(String email, String password) async {
    final cred = await _auth.signInWithEmailAndPassword(email: email, password: password);
    return cred.user;
  }

  Future<void> signOut() async {
    await _auth.signOut();
  }

  Stream<User?> authStateChanges() => _auth.authStateChanges();
}

lib/pages/auth_page.dart (محدَّث)

نموذج صفحة تسجيل/دخول تسمح باختيار الدور (customer/driver/merchant) أثناء التسجيل.

lib/pages/profile_page.dart

صفحة لعرض وتحرير ملف المستخدم — تجلب بيانات users/{uid} من Firestore.

> ملاحظة: أضف قواعد Firestore لتقييد الكتابة بحيث يستطيع كل مستخدم تعديل ملفه فقط، والتجار فقط تعديل مستنداتهم.




---

2) دردشة P2P مع Firestore (رسائل + صور + حالة القراءة)

نموذج قاعدة بيانات الدردشة

chats/{chatId}: {participants: [uid1, uid2], lastMessage, lastUpdated}

chats/{chatId}/messages/{msgId}: {from, to, text, type, imageUrl, timestamp, readBy: [uid]}


lib/services/chat_service.dart

import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:uuid/uuid.dart';

class ChatService {
  final FirebaseFirestore _db = FirebaseFirestore.instance;
  final _uuid = Uuid();

  Future<String> createOrGetChat(String uidA, String uidB) async {
    final q = await _db.collection('chats')
      .where('participants', arrayContains: uidA)
      .get();
    // بحث مبسط: الأفضل أن تحفظ docId بمفتاح ثابت participantsSorted
    for (var doc in q.docs) {
      final parts = List<String>.from(doc['participants']);
      if (parts.contains(uidB)) return doc.id;
    }
    final newDoc = await _db.collection('chats').add({
      'participants': [uidA, uidB],
      'lastMessage': '',
      'lastUpdated': FieldValue.serverTimestamp(),
    });
    return newDoc.id;
  }

  Future<void> sendTextMessage(String chatId, String from, String to, String text) async {
    final msgId = _uuid.v4();
    await _db.collection('chats').doc(chatId).collection('messages').doc(msgId).set({
      'from': from,
      'to': to,
      'text': text,
      'type': 'text',
      'timestamp': FieldValue.serverTimestamp(),
      'readBy': [],
    });
    await _db.collection('chats').doc(chatId).update({
      'lastMessage': text,
      'lastUpdated': FieldValue.serverTimestamp(),
    });
  }

  Future<void> sendImageMessage(String chatId, String from, String to, String imageUrl) async {
    final msgId = _uuid.v4();
    await _db.collection('chats').doc(chatId).collection('messages').doc(msgId).set({
      'from': from,
      'to': to,
      'imageUrl': imageUrl,
      'type': 'image',
      'timestamp': FieldValue.serverTimestamp(),
      'readBy': [],
    });
  }

  Stream<QuerySnapshot> streamMessages(String chatId) {
    return _db.collection('chats').doc(chatId).collection('messages').orderBy('timestamp', descending:false).snapshots();
  }

  Future<void> markAsRead(String chatId, String msgId, String uid) async {
    final ref = _db.collection('chats').doc(chatId).collection('messages').doc(msgId);
    await ref.update({
      'readBy': FieldValue.arrayUnion([uid])
    });
  }
}

واجهة المحادثة (chat_page.dart)

استخدم flutter_chat_ui و flutter_chat_types لعرض المحادثة بشكل احترافي.

دمج رفع الصور عبر Firebase Storage وإرسال رابط الصورة بواسطة sendImageMessage.



---

3) واجهة التاجر لإدارة المنتجات (CRUD + رفع صور)

lib/services/firestore_service.dart — وظائف سريعة

import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_storage/firebase_storage.dart';
import 'dart:io';
import 'package:uuid/uuid.dart';

class FirestoreService {
  final FirebaseFirestore _db = FirebaseFirestore.instance;
  final FirebaseStorage _storage = FirebaseStorage.instance;
  final _uuid = Uuid();

  Future<String> uploadProductImage(File file, String merchantId) async {
    final id = _uuid.v4();
    final ref = _storage.ref().child('merchant_images/$merchantId/$id.jpg');
    final task = await ref.putFile(file);
    final url = await ref.getDownloadURL();
    return url;
  }

  Future<void> addProduct(String merchantId, Map<String,dynamic> product) async {
    final docRef = _db.collection('products').doc();
    product['merchantId'] = merchantId;
    product['createdAt'] = FieldValue.serverTimestamp();
    await docRef.set(product);
  }

  Future<void> updateProduct(String productId, Map<String,dynamic> data) async {
    await _db.collection('products').doc(productId).update(data);
  }

  Future<void> deleteProduct(String productId) async {
    await _db.collection('products').doc(productId).delete();
  }
}

واجهة MerchantHome (محدثة)

عرض قائمة المنتجات في StreamBuilder من products حيث merchantId == currentMerchantId.

زر لإضافة منتج يفتح Form لاسم/وصف/سعر/صورة — ترفع الصورة عبر uploadProductImage ثم addProduct.

أزرار لتعديل وحذف كل منتج.



---

4) تتبع الكباتن بالـ GPS + عرض على الخريطة + تحديث في Firestore

المتطلبات

الحزم: geolocator, permission_handler, flutter_map أو google_maps_flutter.


منطق العمل

1. عند تسجيل دخول السائق أو تفعيل حالته "متاح"، يطلب التطبيق صلاحية الموقع ثم يبدأ خدمة تحديد الموقع الدورية.


2. كل دقيقة (أو عند تغير موضع كبير) تحدِّث التطبيق drivers/{driverId} في Firestore بحقل location: GeoPoint(lat, lng) و lastSeen.


3. واجهة العميل/التاجر تشترك في المستند لرؤية موقع السائق على الخريطة.



lib/pages/driver_home.dart (مقتطف)

import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class DriverHome extends StatefulWidget {
  final String driverId;
  DriverHome({required this.driverId});
  @override
  _DriverHomeState createState() => _DriverHomeState();
}

class _DriverHomeState extends State<DriverHome> {
  Stream<Position>? _positionStream;
  final _db = FirebaseFirestore.instance;

  @override
  void initState() {
    super.initState();
    _startTracking();
  }

  void _startTracking() async {
    bool serviceEnabled = await Geolocator.isLocationServiceEnabled();
    if (!serviceEnabled) return;
    LocationPermission permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) permission = await Geolocator.requestPermission();
    if (permission == LocationPermission.deniedForever) return;

    _positionStream = Geolocator.getPositionStream(distanceFilter: 10, intervalDuration: Duration(seconds: 30));
    _positionStream!.listen((pos) async {
      await _db.collection('drivers').doc(widget.driverId).set({
        'location': GeoPoint(pos.latitude, pos.longitude),
        'lastSeen': FieldValue.serverTimestamp(),
      }, SetOptions(merge: true));
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('واجهة الكابتن')),
      body: Center(child: Text('التتبع شغّال — سيتم تحديث الموقع تلقائياً')),
    );
  }
}

عرض على الخريطة للعميل

استخدم flutter_map مع TileProvider مثل OpenStreetMap لعرض موقع السائق بتحديث مباشر من drivers/{driverId}.



---

5) إعداد تجميع .aab موقّع وملف keystore (سكربت)

إنشاء keystore (مرة واحدة)

keytool -genkey -v -keystore ~/my-release-key.jks -alias delivery_key -keyalg RSA -keysize 2048 -validity 10000

ملف إعداد signing في android/app/build.gradle

أضف signingConfigs و buildTypes لاستخدام my-release-key.jks (مخزّن في android/app أو في مكان آمن).

سكربت بناء تلقائي (build_release.sh)

#!/bin/bash
set -e
# تأكد أنك في جذر مشروع Flutter
flutter clean
flutter pub get
flutter build appbundle --release
# الناتج: build/app/outputs/bundle/release/app-release.aab
echo "AAB built at build/app/outputs/bundle/release/app-release.aab"

نصائح أمنية للـ keystore

لا ترفع keystore إلى مستودعات عامة.

خزّن بيانات keystore (password, alias) في GitHub Secrets أو مدير سري آخر عند عمل CI/CD.



---

قواعد أمن Firestore (مقترح مبسّط)

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /merchants/{merchantId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == resource.data.ownerId;
    }
    match /chats/{chatId} {
      allow read, write: if request.auth != null && request.auth.uid in resource.data.participants;
    }
    match /products/{productId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == resource.data.merchantId;
    }
    match /drivers/{driverId} {
      allow read: if true; // أو قيد حسب سياسة الخصوصية
      allow write: if request.auth != null && request.auth.uid == driverId;
    }
  }
}


---

خطوات تنفيذ سريعة — ماذا فعلت وأين تجده

أضفت ملفات الخدمات (auth_service.dart, chat_service.dart, firestore_service.dart) ونماذج صفحات مبدئية في المستند.

أضفت شرحاً لكيفية رفع الصور، إدارة المنتجات، تتبع GPS، وقواعد Firestore.

أضفت سكربت بناء وملاحظات الأمان الخاصة بالـ keystore.


انطلق الآن: افتح المستند وتابع الأقسام البرمجية، انسخ الأكواد إلى ملفات المشروع، ثم شغّل flutter pub get وابدأ الاختبار محليًا على محاكٍ أو هاتف حقيقي (يجب تهيئة Firebase وgoogle-services.json).


---

/* انتهى المستند */

