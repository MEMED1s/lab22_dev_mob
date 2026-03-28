#  Android JNI & NDK Lab : Communication Java / C++

Ce projet est une application Android de démonstration (JNIDemo) développée pour explorer l'intégration de code natif **C++** au sein d'une application **Java** en utilisant **JNI (Java Native Interface)**, **CMake**, et le **NDK Android**.

L'objectif de ce laboratoire est de comprendre comment déporter des calculs vers le C++ (pour des raisons de performances ou de sécurité) tout en gérant correctement la mémoire et les erreurs de part et d'autre.

---

##  Architecture et Configuration (CMake & Gradle)

Pour que le projet Android reconnaisse et compile le C++, plusieurs étapes de configuration ont été nécessaires :

1. **Intégration Gradle :** Ajout du bloc `externalNativeBuild` dans le fichier `build.gradle` du module `app` pour indiquer le chemin vers le script CMake.
2. **Configuration de `CMakeLists.txt` :** C'est le cœur de la compilation. J'ai configuré ce fichier pour :
   * Créer une bibliothèque partagée (`.so`) nommée `native-lib` à partir du fichier source `jnidemo.cpp`.
   * Rechercher et lier la bibliothèque système Android `log` pour permettre au code C++ d'écrire dans le Logcat.

---

##  Implémentation du Code Natif (`jnidemo.cpp`)

Le fichier C++ contient toute la logique métier. Pour que Java puisse communiquer avec ces fonctions, j'ai dû respecter des règles JNI strictes :
* **Signature de nommage :** Chaque fonction exportée doit s'appeler `Java_nom_du_package_NomDeLaClasse_nomDeLaMethode` (ex: `Java_com_example_jnidemo_MainActivity_factorial`).
* **Gestion de la mémoire :** Les objets complexes passés par Java (comme les `String` ou les tableaux `int[]`) sont gérés par l'environnement JNI (`JNIEnv*`). Il a fallu extraire les données en C++ (ex: `GetStringUTFChars`), faire le traitement, puis **absolument libérer la mémoire** avec `ReleaseStringUTFChars` ou `ReleaseIntArrayElements` pour éviter les fuites mémoire.

Quatre fonctions ont été développées :
1. `helloFromJNI` : Renvoi d'une chaîne de caractères simple.
2. `factorial` : Calcul mathématique avec détection d'overflow (`INT_MAX`).
3. `reverseString` : Manipulation de pointeurs pour inverser une chaîne UTF-8.
4. `sumArray` : Parcours et calcul de la somme d'un tableau d'entiers.

---

##  Déroulement du Laboratoire et Résultats

### Étape 1 : Exécution standard (Le chemin nominal)
La première étape consistait à vérifier que la liaison entre l'interface Java et le code natif fonctionnait avec des valeurs correctes. L'interface affiche le Hello World, le factoriel de 10, la chaîne inversée et la somme du tableau.

![pic1](https://github.com/user-attachments/assets/05c33b53-a488-4b83-8d79-5812e2ffa825)

*Résultat de l'exécution avec des paramètres valides.*

### Étape 2 : Journalisation native via Logcat
Pour vérifier que les traitements se faisaient bien côté C++, j'ai utilisé les macros `__android_log_print` dans `jnidemo.cpp`. Cela permet d'envoyer des logs `INFO` (verts) directement depuis le code natif vers la console Android Studio.

![pic2](https://github.com/user-attachments/assets/d0d1a368-53f3-4480-a798-dce9decab61e)

*Traces des exécutions C++ visibles dans le Logcat avec le tag `JNI_DEMO`.*

### Étape 3 : Tests de robustesse (Crash Tests)
Pour éprouver le code, des valeurs invalides ont été envoyées depuis Java : un factoriel négatif (`-5`), une chaîne vide et un tableau vide. Le C++ intercepte ces erreurs, génère un log d'erreur `ERROR` (rouge) et renvoie un code de statut (ex: `-1`) au lieu de faire planter l'application.

![pic3](https://github.com/user-attachments/assets/f4e9e088-35c0-4f58-95cb-d17ce2e8b05f)

*Le code C++ rejette les mauvaises données, visible via le code d'erreur sur l'écran et la ligne rouge dans le Logcat.*

### Étape 4 (Bonus avancé) : Levée d'exceptions Java depuis le C++
Au lieu de renvoyer un simple code d'erreur (`-1`) silencieux, le code C++ a été modifié pour utiliser `FindClass` et `ThrowNew`. Cela permet au C++ de **lancer une véritable exception Java** (ex: `IllegalArgumentException`). Côté Java, un bloc `try...catch` intercepte cette exception et affiche le message généré par le C++. C'est l'approche recommandée en production.

![pic4](https://github.com/user-attachments/assets/957c90bb-7475-46be-9661-18ea39569237)

*L'application affiche l'exception attrapée : "Le nombre doit etre positif ou nul", prouvant la parfaite synergie Java/C++.*

---

##  Compétences acquises
* Interfaçage Java/C++ via JNI.
* Configuration de builds natifs avec CMake dans Android Studio.
* Gestion sécurisée des pointeurs et de la mémoire croisée.
* Débogage natif via Android Logcat.
* Gestion avancée des erreurs et exceptions JNI.
