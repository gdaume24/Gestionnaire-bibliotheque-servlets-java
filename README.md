# Gestionnaire Bibliothèque Servlets Java

Projet Java web réalisé en TP pour gérer une petite bibliothèque depuis une interface web.

## Ce que fait l'application

L'application permet de :

- afficher une page d'accueil ;
- ajouter un livre ;
- consulter la liste des livres ;
- rechercher un livre par titre ;
- modifier un livre existant ;
- supprimer un livre.

Les livres possèdent un titre, un auteur, un ISBN, une année de publication et un genre.

## Structure du projet

```text
GestionnaireDeBibliotheque-Hibernate/
├── src/main/java/com/koor/hello/controller/   Servlets HTTP
├── src/main/java/com/koor/hello/DAO/          Accès aux données
├── src/main/java/com/koor/hello/model/        Modèle Livre
├── src/main/java/com/koor/hello/bdd/          Connexion SQL Server
└── src/main/webapp/jsp/                       Pages JSP
```

Le nom de dossier garde l'historique du TP, mais l'application présentée est une application Java Servlets/JSP avec JDBC et SQL Server.

## Stack technique

- Java
- Maven
- Jakarta Servlets
- JSP
- JDBC
- SQL Server
- Tomcat 10 pour lancer l'application web

## Rappel rapide : Servlet

Une servlet est une classe Java qui reçoit une requête HTTP et renvoie une réponse.

Dans ce projet :

- `Home` répond à `/home` ;
- `AjouterLivre` répond à `/ajouterLivre` ;
- `ConsulterLivres` répond à `/consulterLivres` ;
- `ModifierLivre` répond à `/modifierLivre` ;
- `SupprimerLivre` répond à `/supprimerLivre`.

Les méthodes importantes sont :

- `doGet(...)` : afficher une page ou charger des données ;
- `doPost(...)` : traiter un formulaire envoyé par l'utilisateur.

## Configuration SQL Server

Avant de lancer l'application, configurer la connexion à SQL Server :

```bash
export DB_URL="jdbc:sqlserver://localhost:1433;databaseName=BibliothequeDB;encrypt=true;trustServerCertificate=true"
export DB_USERNAME="SA"
export DB_PASSWORD="votre_mot_de_passe"
```

Table attendue :

```sql
CREATE TABLE Livres (
    id INT IDENTITY(1,1) PRIMARY KEY,
    titre VARCHAR(255) NOT NULL,
    auteur VARCHAR(255) NOT NULL,
    isbn VARCHAR(50) NOT NULL UNIQUE,
    annee_publication INT NOT NULL,
    genre VARCHAR(100) NOT NULL
);
```

## Build et lancement

`mvn package` ne lance pas l'application. Cette commande compile le projet et génère un fichier `.war` à déployer sur Tomcat.

### 1. Compiler le projet

```bash
cd GestionnaireDeBibliotheque-Hibernate
mvn package
```

Après compilation, le fichier WAR est créé ici :

```text
target/gestionnaire-bibliotheque-servlets-java.war
```

### 2. Lancer avec Tomcat 10

Copier le WAR dans le dossier `webapps` de Tomcat :

```bash
cp target/gestionnaire-bibliotheque-servlets-java.war $CATALINA_HOME/webapps/
```

Puis démarrer Tomcat :

```bash
$CATALINA_HOME/bin/startup.sh
```

Ouvrir ensuite l'application dans le navigateur :

```text
http://localhost:8080/gestionnaire-bibliotheque-servlets-java/home
```

### Variante IDE

Depuis IntelliJ/Eclipse, créer une configuration Tomcat 10, déployer l'artifact WAR du projet, puis ouvrir :

```text
http://localhost:8080/<nom-du-contexte>/home
```

Si le contexte est configuré à la racine `/`, l'URL devient simplement :

```text
http://localhost:8080/home
```

## Résumé

Ce dépôt contient un gestionnaire de bibliothèque en Java web. Les servlets reçoivent les requêtes, les JSP affichent les pages, et le DAO exécute les opérations CRUD sur la table `Livres`.