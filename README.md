# sprawdzanie polaczenia
//sprawdzenie, czy workflow ma poprawny dostęp do repo
- name: Check out repo

  uses: actions/checkout@v3

# instalacja środowiska Docker + Buildx
//uruchomienie możliwości budowania obrazów na wiele architektur
- name: Buildx set-up

  uses: docker/setup-buildx-action@v2

# instalacja QEMU
- name: Docker Setup Quemu

  uses: docker/setup-quemu-action@v2

# logowanie
- name: Login to Dockerhub

  uses: docker/login-action@v2
  
  with:
      username: ${{secrets.DOCKER_HUB_USERNAME}}
    
      password: ${{secrets.DOCKER_HUB_TOKEN}}

# budowa obrazu oraz przesyłanie na Dockerhub
- name: Build and push

  uses: docker/build-push-action@v2
  
  with:
  
      platforms: linux/arm64/v8,linux/amd64
    
      context: .
    
      file: ./Dockerfile_prod
    
      push: true
    
      tags:
    
        AdrianK/lab10:ghlab10example

