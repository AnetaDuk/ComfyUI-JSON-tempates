# Szablony ComfyUI (JSON)

Repozytorium zawiera gotowe do załadowania do ComfyUI szablony w formacie JSON. Każdy plik opisuje kompletne drzewo nodów wraz z parametrami, tak aby po wczytaniu w UI od razu móc podstawić własne obrazy i prompty.

- **templates/product_background_inpaint.json** – szablon do realistycznego wstawiania produktu na wskazane tło z użyciem maski pod inpaint. Używa wyłącznie bazowych nodów dostępnych w ComfyUI Cloud (bez dodatkowych rozszerzeń).
- Więcej informacji o darmowych modelach i partnerach znajdziesz w [`docs/models.md`](docs/models.md).

## Jak pobrać pliki z repozytorium
- **Pobranie całego repo jako ZIP**: wejdź na stronę repozytorium na GitHubie i wybierz **Code → Download ZIP**. Rozpakuj archiwum i użyj plików w katalogu `templates/` oraz dokumentacji w `README.md` i `docs/`.
- **Klonowanie git**: jeśli masz zainstalowanego gita, uruchom `git clone https://github.com/<twoj-username>/ComfyUI-JSON-tempates.git` (podstaw swój adres repozytorium). Aktualizacje pobierzesz później przez `git pull`.
- **Pobranie pojedynczego pliku**: na GitHubie otwórz plik (np. `templates/product_background_inpaint.json`), kliknij **Raw** i zapisz stronę jako plik (`Ctrl/Cmd+S`). Alternatywnie użyj `curl`/`wget`, np.:
  ```bash
  curl -L -o templates/product_background_inpaint.json \
    https://raw.githubusercontent.com/<twoj-username>/ComfyUI-JSON-tempates/main/templates/product_background_inpaint.json
  ```
  Upewnij się, że ścieżki katalogów istnieją przed zapisem.

## Jak używać szablonu wstawiania produktu
1. Pobierz wymagane modele (wszystkie darmowe i dostępne w ComfyUI Cloud):
   - `sd_xl_base_1.0.safetensors` (checkpoint bazowy SDXL 1.0)
   - `sd_xl_refiner_1.0.safetensors` (opcjonalny refiner SDXL 1.0)
   Umieść je w katalogu z modelami ComfyUI (np. `models/checkpoints/`).
2. Przygotuj pliki wejściowe w tej samej ścieżce co ComfyUI:
   - `background.png` – tło, na które chcesz wstawić produkt.
   - `background_mask.png` – czarno-biała maska z zaznaczoną przestrzenią na produkt (biały = miejsce do wypełnienia). Możesz ją przygotować w dowolnym edytorze lub z narzędzi do segmentacji.
   - Opcjonalnie `product_reference.png` – zdjęcie produktu do podglądu ręcznego; w wersji cloud workflow nie wykorzystuje IP-Adaptera, więc opis produktu dodaj w promptach.
3. W ComfyUI wybierz **Load** i wskaż plik `templates/product_background_inpaint.json`.
4. Podmień ścieżki do obrazów w nodach `LoadImage` oraz dopasuj prompt w nodach `CLIPTextEncode` (dodatni i ujemny) tak, aby dokładnie opisać produkt z referencji.
5. Uruchom workflow. Maska kontroluje skalę i pozycję produktu, a prompt opisuje wygląd. W razie potrzeby poszerz lub zwęź białą część maski i uruchom ponownie.
   - Do precyzyjnych poprawek krawędzi maski możesz dodać nod `Erode`/`Dilate` między `LoadImage` maski a `VAEEncodeForInpaint` (również dostępne w cloud).

## Dlaczego ten zestaw?
- SDXL 1.0 w trybie inpaint daje fotorealistyczny blend z tłem przy użyciu wyłącznie bazowych nodów.
- Refiner SDXL poprawia detale krawędzi i integrację z tłem (można go pominąć, jeśli chcesz szybsze wyniki).
- Brak zależności od niestandardowych nodów (np. IP-Adapter), więc workflow działa od razu w ComfyUI Cloud.

## Unikanie modeli płatnych
Szablon korzysta wyłącznie z modeli dostępnych za darmo do pobrania. W ustawieniach ComfyUI Managera unikaj pozycji oznaczonych jako **Partner** lub wymagających kredytów (np. modele hostowane w chmurze). Zamiast tego używaj lokalnych checkpointów wymienionych powyżej.

## Szybkie wskazówki do integracji z własnym repozytorium
- Plik JSON możesz wrzucić do własnego repo (np. GitHub) bez zmian – ComfyUI czyta go bezpośrednio z systemu plików.
- Jeśli hostujesz repo publicznie, dodaj do `.gitignore` ewentualne prywatne obrazy referencyjne (`background.png`, `product.png`, maski), żeby nie trafiły do historii.
