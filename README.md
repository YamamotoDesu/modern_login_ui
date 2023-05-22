## [modern_login_ui #1](https://www.youtube.com/watch?v=Dh-cTQJgM-Q&t=9s)

<img width="300" alt="スクリーンショット 2023-05-14 13 34 48" src="https://github.com/YamamotoDesu/modern_login_ui/assets/47273077/4f9a10c5-1c71-4538-9474-e06ff92d3985">

## [Email Login & Logout • Flutter Auth Tutorial ](https://www.youtube.com/watch?v=_3W-JuIVFlg)

https://github.com/YamamotoDesu/modern_login_ui/assets/47273077/24016337-d151-477e-baae-daff0a797a57

main.dart
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: AuthPage(),
    );
  }
}
```

auth_page.dart
```dart
class AuthPage extends StatelessWidget {
  const AuthPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: StreamBuilder<User?>(
        stream: FirebaseAuth.instance.authStateChanges(),
        builder: (context, snapshot) {
          // user is logged in
          if (snapshot.hasData) {
            return HomePage();
          }

          // user is logged out
          else {
            return LoginPage();
          }
        },
      ),
    );
  }
}
```

login_page.dart
```dart
  void signUserIn() async {
    showDialog(
      context: context,
      builder: (context) {
        return const Center(
          child: CircularProgressIndicator(),
        );
      },
    );

    try {
      await FirebaseAuth.instance.signInWithEmailAndPassword(
        email: emailController.text,
        password: passwordController.text,
      );
      Navigator.pop(context);
    } on FirebaseAuthException catch (e) {
      Navigator.pop(context);
      if (e.code == 'user-not-found') {
        print('No user found for that email');
        wrongEmailMessage();
      } else if (e.code == 'wrong-password') {
        print('wrong password buddy');
        wrongPasswordMessage();
      }
    }
  }

  void wrongEmailMessage() {
    showDialog(
      context: context,
      builder: (context) {
        return const AlertDialog(
          title: Text('Invalid Email'),
        );
      },
    );
  }

  void wrongPasswordMessage() {
    showDialog(
      context: context,
      builder: (context) {
        return const AlertDialog(
          title: Text('Invalid Password'),
        );
      },
    );
  }

```

home_page.dart
```dart
  final user = FirebaseAuth.instance.currentUser!;

  void signUserOut() {
    FirebaseAuth.instance.signOut();
  }
```
