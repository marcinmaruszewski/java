# Java Minecraft Server

Poniżej znajdziesz gotowe rozwiązanie oparte o `docker-compose`, które uruchamia:

- **Crafty Controller** jako panel do zarządzania serwerami (Java, konta, whitelist, konsola, kopie zapasowe).
- **Dwa serwery Java** (lobby + survival) działające równolegle.
- **BlueMap** do podglądu map światów w przeglądarce (instalowany automatycznie).
- **TeamSpeak 3**.
- **Caddy** jako reverse proxy z automatycznym TLS dla panelu.

> Jeśli wolisz jedną instancję Java lub inne światy, usuń/zmodyfikuj odpowiednie serwisy w `docker-compose.yml`.

## Wymagania

- VPS z Dockerem i Docker Compose v2.
- Własna domena (rekord A/AAAA kierujący na VPS).
- Otwarta komunikacja TCP dla Java (domyślnie 25565/25566), porty map (8100/8101) oraz porty TeamSpeak.

## Szybki start

1. Skopiuj plik `.env.example` jako `.env` i uzupełnij dane:

```bash
cp .env.example .env
```

2. Zaktualizuj zmienną `CRAFTY_DOMAIN` w `.env`, np. `panel.twojadomena.pl`.

3. Uruchom całość:

```bash
docker compose up -d
```

> **Certyfikat TLS (Let’s Encrypt):** Caddy automatycznie pobierze i odnowi certyfikat Let’s Encrypt dla domeny podanej w `CRAFTY_DOMAIN`, o ile rekord DNS wskazuje na VPS i porty 80/443 są otwarte.

## Dostęp do usług

- **Panel Crafty**: `https://<CRAFTY_DOMAIN>`
- **Java lobby**: `<IP>:25565`
- **Java survival**: `<IP>:25566`
- **Mapy świata (BlueMap)**:
  - Lobby: `http://<IP>:8100`
  - Survival: `http://<IP>:8101`
- **TeamSpeak 3**: `udp://<IP>:9987`

## Dalsza konfiguracja

### Crafty Controller

Po pierwszym uruchomieniu:
1. Wejdź na panel i stwórz konto administratora.
2. W Crafty możesz tworzyć dodatkowe instancje Java (np. Forge/Fabric), zarządzać dostępami, wysyłać komendy oraz robić backupy.

### BlueMap (mapy)

BlueMap jest instalowany z Modrinth przez zmienną `MODRINTH_PROJECTS`. Mapy będą dostępne na portach 8100/8101 po wygenerowaniu świata.

### TeamSpeak 3

Dane serwera są w wolumenie `teamspeak_data`. Licencja jest akceptowana przez `TS3SERVER_LICENSE=accept`.

## Najczęstsze problemy

- **Brak mapy**: poczekaj aż świat się wygeneruje i upewnij się, że porty 8100/8101 są otwarte.
- **Brak połączenia z Java**: sprawdź otwarte porty TCP (25565/25566).
- **Błędy TLS w panelu**: sprawdź rekord DNS domeny i logi Caddy.

## Struktura wolumenów

- `crafty_config` – konfiguracja panelu.
- `crafty_backups` – backupy z Crafty.
- `crafty_servers` – dane serwerów zarządzanych przez Crafty.
- `crafty_logs` – logi Crafty.
- `crafty_import` – folder importu instancji.
- `java_lobby_data` / `java_survival_data` – dane światów.
- `teamspeak_data` – konfiguracja TeamSpeak.

## Wyłączanie

```bash
docker compose down
```
