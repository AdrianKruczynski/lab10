# sprawdzanie polaczenia
//Pierwszy krok, "Check out repo", używa akcji "actions/checkout@v3" do sklonowania repozytorium. Jest to częsty pierwszy krok, który umożliwia dostęp do kodu źródłowego i plików w repozytorium.
- name: Check out repo

  uses: actions/checkout@v3

# instalacja środowiska Docker + Buildx
//Następnie, krok "Buildx set-up" korzysta z akcji "docker/setup-buildx-action@v2" w celu skonfigurowania możliwości budowania obrazów Docker na różne architektury. Dzięki temu, pozwala na budowanie obrazów, które będą działać na różnych platformach sprzętowych.
- name: Buildx set-up

  uses: docker/setup-buildx-action@v2

# instalacja QEMU
//Krok "Docker Setup Quemu" używa akcji "docker/setup-quemu-action@v2" do konfiguracji środowiska QEMU w kontekście Dockera. QEMU to narzędzie umożliwiające emulację różnych architektur procesorów, co jest przydatne, gdy budujemy obrazy Docker dla różnych platform.

- name: Docker Setup Quemu

  uses: docker/setup-quemu-action@v2

# logowanie
//Następnie, krok "Login to Dockerhub" korzysta z akcji "docker/login-action@v2" do zalogowania się do Docker Hub. Używa on tajnych zmiennych (nazwy użytkownika i tokena) przechowywanych w repozytorium, aby uzyskać dostęp do konta Docker Hub.

- name: Login to Dockerhub

  uses: docker/login-action@v2
  
  with:
      username: ${{secrets.DOCKER_HUB_USERNAME}}
    
      password: ${{secrets.DOCKER_HUB_TOKEN}}

# budowa obrazu oraz przesyłanie na Dockerhub
//Ostatni krok, "Build and push", wykorzystuje akcję "docker/build-push-action@v2" do budowy obrazu Docker na podstawie pliku Dockerfile znajdującego się w bieżącym katalogu kontekstowym. Obraz jest budowany dla dwóch platform: linux/arm64/v8 (architektura ARM) i linux/amd64 (architektura x86_64). Następnie, obraz jest przesyłany (push) na Docker Hub i oznaczany tagiem "AdrianK/lab10:ghlab10example".
- name: Build and push

  uses: docker/build-push-action@v2
  
  with:
  
      platforms: linux/arm64/v8,linux/amd64
    
      context: .
    
      file: ./Dockerfile_prod
    
      push: true
    
      tags:
    
        AdrianK/lab10:ghlab10example

