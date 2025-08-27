# Instalacja Gulp na serwerze

## Co to jest Gulp?
- **Gulp** to **task runner** dla środowiska **Node.js**.  
  Służy do automatyzacji powtarzalnych zadań w projekcie, np.:
  - kompresja plików CSS i JavaScript,  
  - kompilacja Sass/LESS do CSS,  
  - optymalizacja obrazków,  
  - automatyczny reload przeglądarki,  
  - budowanie projektu do folderu produkcyjnego.  

Dzięki Gulp piszesz raz plik `gulpfile.js`, a potem wszystko uruchamiasz jedną komendą `gulp`.

## Instalacja Gulp na serwerze

### Ubuntu

```bash
# Aktualizacja systemu
sudo apt update && sudo apt upgrade -y

# Instalacja Node.js i npm (NodeSource)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Sprawdzenie wersji
node -v
npm -v

# Instalacja Gulp globalnie
sudo npm install --global gulp-cli

# Sprawdzenie instalacji
gulp --version
```
### CentOS

# Aktualizacja systemu
sudo yum update -y

# Dodanie Node.js z NodeSource
curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
sudo yum install -y nodejs

# Sprawdzenie wersji
node -v
npm -v

# Instalacja Gulp globalnie
sudo npm install --global gulp-cli

# Sprawdzenie instalacji
gulp --version

## Jak zacząć korzystać z Gulp

Przejdź do katalogu projektu:
```bash
cd /var/www/html/moj_projekt
```

Zainicjuj projekt Node.js:

```bash
npm init -y
```

Zainstaluj Gulp w projekcie (lokalnie):

```bash
npm install --save-dev gulp
```

Utwórz plik gulpfile.js w katalogu projektu:

```bash
const gulp = require('gulp');

gulp.task('hello', function(done) {
  console.log('🚀 Witaj w świecie Gulp!');
  done();
});
```

Uruchom zadanie:

```bash
gulp hello
```