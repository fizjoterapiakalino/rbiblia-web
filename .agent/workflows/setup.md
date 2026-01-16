---
description: Przygotowanie środowiska developerskiego dla rBiblia Web
---

# Przygotowanie środowiska rBiblia Web

## Wymagania
- Docker Desktop zainstalowany i uruchomiony
- Dostęp do internetu (pobieranie obrazów Docker)

## Kroki

### 1. Uruchom Docker Desktop
Upewnij się, że Docker Desktop jest uruchomiony. Możesz go uruchomić ręcznie lub za pomocą polecenia:
```powershell
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
```
Poczekaj około 30-60 sekund aż Docker się zainicjalizuje.

### 2. Zbuduj i uruchom kontener
// turbo
```powershell
docker-compose up -d --build
```
Ten krok zbuduje obraz PHP 8.2 z Apache i uruchomi kontener.

### 3. Zainstaluj zależności PHP (Composer)
// turbo
```powershell
docker exec -t rbiblia-web-web-1 composer install
```

### 4. Zainstaluj zależności Node.js (Yarn)
// turbo
```powershell
docker exec -t rbiblia-web-web-1 yarn install
```

### 5. Zbuduj assety frontendowe
// turbo
```powershell
docker exec -t rbiblia-web-web-1 yarn encore dev
```

### 6. Skonfiguruj bazę danych
Skopiuj plik konfiguracyjny szablonu:
```powershell
Copy-Item ".\src\config\app.php.dist" ".\src\config\app.php"
```

Następnie edytuj plik `src/config/app.php` i uzupełnij dane bazy danych:
- **Opcja A**: Skontaktuj się z [Rafałem](https://kontakt.toborek.info) w celu uzyskania konfiguracji zewnętrznej bazy deweloperskiej
- **Opcja B**: Skonfiguruj własną bazę MySQL i zaimportuj strukturę z `docs/db_structure.sql`

### 7. Otwórz aplikację
Aplikacja jest dostępna pod adresem: http://localhost

## Przydatne komendy

### Tryb watch (automatyczna rekompilacja assets)
```powershell
docker exec -t rbiblia-web-web-1 yarn encore dev --watch
```

### Zatrzymanie kontenera
```powershell
docker-compose down
```

### Logi kontenera
```powershell
docker-compose logs -f
```

### Wejście do kontenera
```powershell
docker exec -it rbiblia-web-web-1 bash
```

## Rozwiązywanie problemów

### Docker Desktop nie odpowiada
Zrestartuj Docker Desktop:
```powershell
Stop-Process -Name "Docker Desktop" -Force -ErrorAction SilentlyContinue
Start-Sleep 5
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
```

### Błąd "Settings file not exists"
Upewnij się, że plik `src/config/app.php` istnieje (krok 6).

### Błąd "The given 'driver' is unknown"
Uzupełnij dane bazy danych w pliku `src/config/app.php`.
