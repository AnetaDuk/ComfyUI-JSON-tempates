# Modele do ComfyUI: darmowe vs partner/płatne

## Najlepsze darmowe modele do fotorealistycznego compositingu (cloud-friendly)
- **Stable Diffusion XL Base 1.0 (`sd_xl_base_1.0.safetensors`)** – uniwersalny checkpoint do generacji i inpaintingu w wysokiej jakości.
- **Stable Diffusion XL Refiner 1.0 (`sd_xl_refiner_1.0.safetensors`)** – drugi etap poprawiający detale i faktury; działa na tym samym latencie, więc nie wymaga nowych promptów.
- **SDXL Inpainting (np. `sd_xl_base_1.0` + `VAEEncodeForInpaint`)** – tryb inpaint w ComfyUI wykorzystuje ten sam checkpoint bazowy, więc nie potrzebujesz osobnego płatnego modelu.
  - W ComfyUI Cloud standardowe nody inpaint są dostępne od razu, bez instalacji rozszerzeń.

- **IP-Adapter/CLIP Vision** – w wersji cloud często niedostępne bez dodatkowych kredytów lub instalacji niestandardowych nodów, dlatego szablon ich nie używa.
## Typowe modele/źródła, które konsumują kredyty (unikać w tym workflow)
- Pozycje oznaczone w ComfyUI Managerze jako **Partner** lub **Cloud** (np. modele hostowane na RunDiffusion, Together, Replicate). Zwykle wymagają płatnych kredytów za inferencję.
- Komercyjne/limitowane checkpointy typu **FLUX** czy **Playground v2.5** dostępne poprzez chmurowych dostawców – bez lokalnego pliku nie uruchomisz ich bez opłat.
- Wbudowane presety „one-click” od dostawców partnerów (np. z logo dostawcy w opisie). Jeśli nie masz lokalnej kopii modelu `.safetensors`, będą rozliczane jak usługa.

## Dobre praktyki
- Pobieraj checkpointy z oficjalnych repozytoriów lub Hugging Face i trzymaj je lokalnie – ComfyUI nie nalicza kredytów za lokalne pliki.
- W ComfyUI Managerze filtruj listę po „Local only”, aby nie wczytywać przypadkiem modeli partnerów.
- Jeśli zamieniasz model w szablonie, wybieraj warianty SDXL (base/refiner) lub kompatybilne open-source IP-Adaptery, żeby zachować zgodność nodów.
