# Shell-Optimization-Tools

쉘 로딩 최적화 및 성능 측정 스크립트

## 환경

- iTerm2 + oh my zsh

- macOS Monterey 12.4

## 목적

초기 로딩에 시간이 오래걸려서 최적화하려고함

## 참고 자료

[쉘 최적화 문제 분석 블로그 포스팅](https://blog.mattclemente.com/2020/06/26/oh-my-zsh-slow-to-load/)

[zsh-nvm](https://github.com/lukechilds/zsh-nvm)

[evalcache](https://github.com/mroth/evalcache)

## 배운점

1. 처음에는 단순히 플러그인이 많아서 오래 걸리는 줄로만 지레짐작했는데 실제로 스크립트를 써가면서 시간 측정해보니 플러그인은 그닥 큰 문제가 되지않음
2. nvm이나 rbenv같은 가상 환경들이 사용되지도 않는데, 쉘을 초기화할 때 로드되고 있었음(아마 설치하면서 자동으로 들어간 듯함)
3. zsh을 사용하면서 모니터링 하거나 로깅은 전혀 살펴보지 않았는데, 다양한 부분에서 문제가 될 수 있었고, 이 문제의 원인은 여기서 찾을 수 있음을 다시 한 번 깨우친다 이래서 로깅하는구나
4. evalcache, nvm lazy loading 쉘 플러그인들을 알게됨
5. 내 경우는 오히려 zsh-nvm 플러그인이 플러그인 로드에서 대부분의 시간을 차지해서 오히려 시간이 더 걸렸다. 그래서 zsh-nvm은 적용하지 않고, evalcache만 적용했음, 참고한 블로그의 필자는 zsh-nvm플러그인을 적용하고 다시 플러그인의 각 로딩시간을 측정하지는 않았는데, 오히려 배보다 배꼽인 느낌이다
6. 쉘을 리로딩 할 때 `source .zshrc`를 많이 사용했는데 [공식 문서](https://github.com/ohmyzsh/ohmyzsh/wiki/FAQ#how-do-i-reload-the-zshrc-file)를 보니 이미 zsh 세션에 있는 일부 항목(변수, 함수, 후크 등)이 제거되지 않았기 때문에 문제가 발생할 수도 있고 init 스크립트를 반복적으로 실행하여 점점 더 많은 프로세스를 시작할 수도 있다고 한다. 대신 리로딩을 제대로 하면 `exec zsh`을 사용해야함
7. 사실은 neofetch라는 멋있는 스크립트를 사용하는데 이게 제일문제다. 포기할 수는 없다. 역시 I/O는 오래걸리는 작업이다. 헛 하지만 이런 것도 가끔하면 재밌고 또 몰랐던 것들도 알게돼서 좋다 

## 측정

evalcache 적용 전과 후를 측정

1. timezsh

- before

  ![1](https://user-images.githubusercontent.com/48282185/175234353-acafb22d-f34f-4070-81f1-5891d0164c55.png)

- after

  ![2](https://user-images.githubusercontent.com/48282185/175234349-61d849ba-23b2-48f1-8b4c-3b36d9a0c86b.png)

2. plugintimes

- before

  ![3](https://user-images.githubusercontent.com/48282185/175234346-f53b735b-09cd-4d39-b7cf-60789008753b.png)

- after

  ![4](https://user-images.githubusercontent.com/48282185/175234339-21364f8e-7464-47ac-8560-d7ce71077357.png)
