Restore form klipper backup
ssh to host
'nano restore_klipper.sh'
paste:
'
#!/bin/bash

# --- KONFIGURACJA ---
# Wklej tutaj swój link do repozytorium
REPO_URL="https://github.com/TWOJA_NAZWA/TWOJE_REPO.git"
CONFIG_DIR="$HOME/printer_data/config"
TEMP_DIR="$HOME/temp_klipper_restore"

echo "🚀 Rozpoczynam przywracanie konfiguracji Klippera..."

# 1. Czyszczenie starego folderu tymczasowego
rm -rf "$TEMP_DIR"

# 2. Klonowanie repozytorium
echo "📥 Pobieranie danych z Git..."
git clone "$REPO_URL" "$TEMP_DIR"

if [ $? -ne 0 ]; then
    echo "❌ Błąd: Nie udało się pobrać repozytorium!"
    exit 1
fi

# 3. Logika przenoszenia (zapobiega podwójnym folderom)
echo "📂 Porządkowanie plików..."

# Sprawdzamy czy w repo jest struktura printer_data/config
if [ -d "$TEMP_DIR/printer_data/config" ]; then
    # Kopiujemy zawartość z głębokiej ścieżki
    cp -r "$TEMP_DIR/printer_data/config/." "$CONFIG_DIR/"
else
    # Jeśli pliki są bezpośrednio w repozytorium
    cp -r "$TEMP_DIR/." "$CONFIG_DIR/"
fi

# 4. Naprawa uprawnień (Kluczowe dla Mainsail)
echo "🔐 Naprawianie uprawnień..."
sudo chown -R $USER:$USER "$CONFIG_DIR"
find "$CONFIG_DIR" -type f -exec chmod 664 {} +
find "$CONFIG_DIR" -type d -exec chmod 775 {} +

# 5. Sprzątanie
rm -rf "$TEMP_DIR"

echo "✅ Gotowe! Pliki zostały nadpisane w $CONFIG_DIR"
echo "🔄 Pamiętaj, aby wykonać FIRMWARE_RESTART w panelu Mainsail."
'
remember to change repository  for your own link
ctl x, y , enter
Restart firmware in clipper/mainsail
