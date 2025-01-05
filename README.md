Kuziora Karol 95468
05.01.2025r.

### 1. Konfiguracja przestrzeni nazw
Plik namespaces.yaml tworzy dwie oddzielne przestrzenie nazw dla aplikacji.

### 2. Konfiguracja Deployments i Services
Zdefiniowano trzy deployments:
- app-a w namespace appns-a
  - Obraz: nginx
  - Liczba replik: 2
- app-b w namespace appns-b
  - Obraz: nginx
  - Liczba replik: 2
- default-backend w namespace default
  - Obraz: spcr.spg51.dev/lab/hello-app:1.0
  - Liczba replik: 1

### 3. Konfiguracja NetworkPolicy
Polityki sieciowe dla namespace'ów appns-a i appns-b Domyślnie blokujące ruch przychodzący i wychodzący. Zezwalające na ruch tylko wewnątrz tej samej przestrzeni nazw oraz z przestrzeni nazw ingress-nginx.

### 4. Konfiguracja Ingress
Konfiguracja routingu dla dwóch domen:
- a.lab9.net -> app-a-proxy
- b.lab9.net -> app-b-proxy

#### Default Backend
Default Backend to specjalna usługa obsługująca żądania, które nie trafiają do żadnej z określonych ścieżek Ingress. W naszej konfiguracji:
- Uruchamia kontener hello-app
- Nasłuchuje na porcie 8080
- Służy jako domyślna strona błędu lub strona rezerwowa
- Zapewnia podstawową obsługę żądań, które nie są kierowane do konkretnej aplikacji

### 5. Wdrożenie
Włączenie kontrolera Ingress w Minikube: minikube addons enable ingress
Utworzenie przestrzeni nazw: kubectl apply -f namespaces.yaml
Wdrożenie aplikacji i usług: kubectl apply -f deployments.yaml
Zastosowanie polityk sieciowych: kubectl apply -f network-policies.yaml
Konfiguracja Ingress: kubectl apply -f ingress.yaml
Uruchomienie tunelu Minikube: minikube tunnel


