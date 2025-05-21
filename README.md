# f3

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  MobileAds.instance.initialize();
  FirebaseMessaging.onBackgroundMessage(onBackgroundMessageHandler);
  await Firebase.initializeApp();

  FirebaseNotificationServices services = FirebaseNotificationServices();
  services.requestNotificationPermission();
  SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp]);

  runApp(const MyApp());
}

@pragma('vm:entry-point')
Future<void> onBackgroundMessageHandler(RemoteMessage message) async {
  // print('hii hellow');
  // print(message.notification!.title.toString());
}
// ignore_for_file: prefer_const_constructors, avoid_print, prefer_const_declarations, use_build_context_synchronously, no_duplicate_case_values, file_names

import 'dart:io';

import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter/material.dart';

import 'package:flutter_local_notifications/flutter_local_notifications.dart';
import 'package:meaningapp/DrawerPages/antonyms.dart';
import 'package:meaningapp/DrawerPages/common_sentence.dart';
import 'package:meaningapp/DrawerPages/synonyms.dart';

import 'package:meaningapp/Form/f_s_t.dart';
import 'package:meaningapp/page2/chemistry.dart';
import 'package:meaningapp/page2/geography.dart';
import 'package:meaningapp/page2/law.dart';
import 'package:meaningapp/page2/mathematic.dart';
import 'package:meaningapp/page2/physics.dart';
import 'package:meaningapp/page2/post.dart';
import 'package:meaningapp/page2/science.dart';
import 'package:meaningapp/page2/typesoffish.dart';
import 'package:meaningapp/page2/war.dart';
import 'package:meaningapp/page2/weather.dart';
import 'package:meaningapp/page3/manandhuman.dart';
import 'package:meaningapp/page3/nation.dart';
import 'package:meaningapp/page4/geometry.dart';
import 'package:meaningapp/page4/politices.dart';
import 'package:meaningapp/pages/education.dart';
import 'package:meaningapp/pages/meaning/flowers.dart';
import 'package:meaningapp/pages/meaning/fruits.dart';
import 'package:meaningapp/pages/meaning/occupation.dart';
import 'package:meaningapp/pages/meaning/relative.dart';
import 'package:meaningapp/pages/natural.dart';
import 'package:meaningapp/pages/page2/colors.dart';
import 'package:meaningapp/pages/page2/diseases.dart';
import 'package:meaningapp/pages/page2/dresses.dart';
import 'package:meaningapp/pages/page2/foods.dart';
import 'package:meaningapp/pages/page2/insects.dart';
import 'package:meaningapp/pages/page2/medicine.dart';
import 'package:meaningapp/pages/page2/religion.dart';
import 'package:meaningapp/pages/page2/tools.dart';
import 'package:meaningapp/pages1/animals.dart';
import 'package:meaningapp/pages1/birds.dart';
import 'package:meaningapp/pages1/crops.dart';
import 'package:meaningapp/pages1/cultivation.dart';
import 'package:meaningapp/pages1/fish.dart';
import 'package:meaningapp/pages1/games.dart';
import 'package:meaningapp/pages1/householder.dart';
import 'package:meaningapp/pages1/humanbody.dart';
import 'package:meaningapp/pages1/ornament.dart';
import 'package:meaningapp/pages1/ourhouse.dart';
import 'package:meaningapp/vocabulary/A.dart';
import 'package:meaningapp/vocabulary/B.dart';
import 'package:meaningapp/vocabulary/C.dart';
import 'package:meaningapp/vocabulary/D.dart';
import 'package:meaningapp/vocabulary/e.dart';
import 'package:meaningapp/vocabulary/f.dart';
import 'package:meaningapp/vocabulary/g.dart';
import 'package:meaningapp/vocabulary/h.dart';
import 'package:meaningapp/vocabulary/i.dart';
import 'package:meaningapp/vocabulary/j.dart';
import 'package:meaningapp/vocabulary/k.dart';
import 'package:meaningapp/vocabulary/l.dart';
import 'package:meaningapp/vocabulary/m.dart';
import 'package:meaningapp/vocabulary/n.dart';
import 'package:meaningapp/vocabulary/o.dart';
import 'package:meaningapp/vocabulary/p.dart';
import 'package:meaningapp/vocabulary/q.dart';
import 'package:meaningapp/vocabulary/r.dart';
import 'package:meaningapp/vocabulary/s.dart';
import 'package:meaningapp/vocabulary/t.dart';
import 'package:meaningapp/vocabulary/u.dart';
import 'package:meaningapp/vocabulary/v.dart';
import 'package:meaningapp/vocabulary/w.dart';
import 'package:meaningapp/vocabulary/x.dart';
import 'package:meaningapp/vocabulary/y.dart';
import 'package:meaningapp/vocabulary/z.dart';
import 'package:store_redirect/store_redirect.dart';

class FirebaseNotificationServices {
  FirebaseMessaging messaging = FirebaseMessaging.instance;
  final FlutterLocalNotificationsPlugin flutterLocalNotificationsPlugin =
      FlutterLocalNotificationsPlugin();
  void initializedNotification(
      BuildContext context, RemoteMessage message) async {
    final AndroidInitializationSettings androidInitializationSettings =
        const AndroidInitializationSettings("@mipmap/ic_launcher");
    InitializationSettings initializationSettings =
        InitializationSettings(android: androidInitializationSettings);
    await flutterLocalNotificationsPlugin.initialize(initializationSettings,
        onDidReceiveNotificationResponse: (payload) {
      handleMessage(context, message);
    });
  }

  void firebaseInit(BuildContext context) {
    FirebaseMessaging.onMessage.listen((message) {
      // print(message.notification!.title.toString());
      // print(message.notification!.body.toString());
      if (Platform.isAndroid) {
        initializedNotification(context, message);
        sendNotification(message);
      }
    });
  }

  void sendNotification(RemoteMessage message) async {
    AndroidNotificationChannel channel = const AndroidNotificationChannel(
        'high_importance_channel', 'high_importance_channel',
        importance: Importance.max);
    AndroidNotificationDetails androidNotificationDetails =
        AndroidNotificationDetails(
            channel.id.toString(), channel.name.toString(),
            channelDescription: "your channel description",
            importance: Importance.high,
            priority: Priority.high,
            ticker: "ticker");

    NotificationDetails notificationDetails =
        NotificationDetails(android: androidNotificationDetails);

    await flutterLocalNotificationsPlugin.show(
        0,
        message.notification!.title.toString(),
        message.notification!.body.toString(),
        notificationDetails);
  }

  void requestNotificationPermission() async {
    NotificationSettings settings = await messaging.requestPermission(
      alert: true,
      criticalAlert: true,
      announcement: true,
      badge: true,
      carPlay: true,
      provisional: true,
      sound: true,
    );

    if (settings.authorizationStatus == AuthorizationStatus.authorized) {
      print('User permissin is granted');
    } else if (settings.authorizationStatus ==
        AuthorizationStatus.provisional) {
      print("user provisional is granted");
    } else {
      //  AppSettings.openAppSettings(type: AppSettingsType.notification);
      print('permission is denied');
    }
  }

  Future<String> getDeviceToken() async {
    String? token = await messaging.getToken();

    return token!;
  }

  void isRefressToken() {
    messaging.onTokenRefresh.listen((event) {
      event.toString();
      print('refress Device Token');
    });
  }

  void setupInteratMessage(BuildContext context) async {
    //treminate the app
    RemoteMessage? initializedMessage =
        await FirebaseMessaging.instance.getInitialMessage();

    if (initializedMessage != null) {
      handleMessage(context, initializedMessage);
    }

    FirebaseMessaging.onMessageOpenedApp.listen((event) {
      handleMessage(context, event);
    });
  }

  void handleMessage(BuildContext context, RemoteMessage message) {
    switch (message.data['id']) {
      case '1':
        Navigator.push(
            context, MaterialPageRoute(builder: (context) => Education()));
        break;
      case '2':
        Navigator.push(
            context, MaterialPageRoute(builder: (context) => Flowers()));
        break;
     

      case '101':
        StoreRedirect.redirect(androidAppId: "com.meaning.mean");
        break;

      default:
        Navigator.push(
            context, MaterialPageRoute(builder: (context) => ManyForm()));
    }
  }
}
