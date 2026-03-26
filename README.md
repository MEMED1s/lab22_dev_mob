# 📱 Android NDK Lab — JNI Integration (Java & C++)

## 🎓 Présentation

Ce projet illustre l’utilisation de *JNI (Java Native Interface)* pour connecter une application Android écrite en Java avec du code natif en C++.

L’objectif est de déléguer certains traitements au C++ afin d’améliorer les performances et comprendre l’architecture hybride Android.

---

## ⚙️ Environnement technique

- Android Studio
- Java (Android SDK)
- C++ (NDK)
- CMake
- Émulateur Android

---

## 🧩 Organisation du projet


app/
└── src/
    └── main/
        ├── java/com/example/lab22mobile/MainActivity.java
        ├── cpp/
        │   ├── lab22mobile.cpp
        │   └── CMakeLists.txt
        └── res/layout/activity_main.xml


---

## 🔗 Communication Java ↔ C++

Le projet utilise JNI pour faire le lien entre Java et C++.

Exemple de déclaration côté Java :

java
public native int factorial(int n);


La bibliothèque native est chargée avec :

java
static {
    System.loadLibrary("lab22mobile");
}


---

## 🧠 Fonctionnalités implémentées

### 🔢 Calcul du factoriel
- Retourne le résultat normal
- Retourne -1 si négatif
- Retourne -2 si overflow

### 🔁 Inversion de chaîne
- Inverse une chaîne
- Gère les chaînes vides

### ➕ Somme d’un tableau
- Additionne les éléments
- Retourne 0 si tableau vide

---

## 📱 Aperçu de l'application

![pic1](https://github.com/user-attachments/assets/0ff66b76-a7d8-41a1-afc8-142c320924b8)


---

## 📊 Logs natifs (C++)

Les logs sont visibles dans Logcat avec le tag :


JNI_DEMO


Exemples :
- Appel JNI
- Résultat du factoriel
- String inversée
- Somme du tableau

![pic2](https://github.com/user-attachments/assets/ae8180ac-62fd-4648-b651-ccf979345213)


---

## 🧪 Tests réalisés

Tests effectués pour valider le comportement :

- factorial(10) → 3628800
- factorial(-5) → -1
- factorial(20) → -2 (overflow)
- reverseString("") → ""
- sumArray([]) → 0

Résultats dans Logcat :

![pic3](https://github.com/user-attachments/assets/a6314957-4e95-4e7e-baba-b19c7ed97999)


---

## 🐞 Problèmes rencontrés

- Erreur de chargement de bibliothèque JNI
- Mauvaise signature JNI
- Erreurs de compilation C++
- Mauvaise gestion mémoire JNI

---

## 💡 Apprentissage

Ce TP permet de comprendre :

- l’interfaçage Java ↔ C++
- l’utilisation du NDK
- le rôle de CMake
- le debugging avec Logcat

---

## 🚀 Utilisation de JNI

JNI est utile pour :

- calcul intensif
- traitement d’image
- moteurs de jeu
- bibliothèques natives

---

## 📌 Bonnes pratiques

- Minimiser les appels JNI
- Libérer les ressources (Release...)
- Tester les cas limites
- Utiliser les logs intelligemment

---

## ✅ Conclusion

Ce projet montre comment intégrer efficacement du code natif dans une application Android.

JNI offre de meilleures performances mais augmente la complexité. Son utilisation doit être justifiée par un besoin réel.

---

