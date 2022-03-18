---
title: oath
date: 2022-03-18 10:03:50
category: web
thumbnail: { thumbnailSrc }
draft: false
---

최근 많은 웹서비스들을 방문해 보면 이전에 이메일과 비밀번호를 사용한 로그인 방식 대신에 구글이나 페이스북과 같은 다른 서비스의 계정을 통한 로그인 방식을 많이 볼 수 있다.

이런 흐름의 변화에는 이전 방법의 문제를 해결하기 위함에 있으므로 이메일과 비밀번호를 사용하는 로그인 방식에 어떤 문제점이 있는지 알아보자.

## 기존 로그인 방식의 문제점

- 복잡한 회원가입
- 패스워드 기억에 대한 부담감

사용자의 입장에서 크게 위의 두 가지 문제가 존재한다.

먼저, 로그인을 하기 위해서 회원가입을 해야 하는데 이 과정이 복잡하고 귀찮은 경우가 많다.

(얼마나 복잡하겠느냐만은...)

그리고, 사용자는 회원가입을 하는 서비스 이외에도 비밀번호를 기억해야할 서비스들이 많이 있을 것이다.

거기에 비밀번호를 기억해야 할 부담감을 +1 해줄 필요가 있을까?

이런 저런 이유로 oAuth라는 기술이 등장했다고 생각한다.

## What oAuth

oAuth란 사용자 인증을 위한 표준 프로토콜의 한 종류를 의미하며, 사용자 정보에 접근하기 위해 클라이언트(웹서비스)에게 권한을 제공하는 프로세스를 단순화하는 방법이다.

(구글 로그인, 페이스북 로그인과 같이 외부 서비스를 통한 로그인 방식이 oAuth 기술의 일환이다.)

## Why oAuth

- 로그인 절차 간소화
- 보안성

oAuth를 사용하면 기존 로그인에서 진행해야했던 회원가입 대신에 외부 서비스로부터 사용자의 정보를 사용할 수 있는 권한을 허가 받는 것으로 로그인 절차를 간소화할 수 있다.

또한, 사용자의 정보가 웹서비스에서 사용자 정보의 사용을 사용자에게 구해야하기 때문에 사용자 정보가 노출될 일이 적어 보안성에도 이점이 있다.

## oAuth 용어

- Resource Owner : 사용자
- Client : Resource owner를 대신하여 보호된 리소스에 액세스하는 응용프로그램
- Resource server : client의 요청을 수락하고 응답할 수 있는 서버
- Authorization server : Resource server가 액세스 토큰을 발급받는 서버, 즉 클라이언트 및 리소스 소유자를 성공적으로 인증한 후 액세스 토큰을 발급하는 서버
- Authorization grant : 클라이언트가 액세스 토큰을 얻을 때 사용하는 자격 증명의 유형
- Authorization code : access token을 발급받기 전에 필요한 code이며, client ID로 이 code를 받아온 후, client secret과 code를 이용해 Access token 을 받아온다.
- Access token : 보호된 리소스에 액세스하는 데 사용되는 credentials이며, Authorization code와 client secret을 이용해 받아온 이 Access token으로 이제 resource server에 접근할 수 있다.
- Scope : 토큰의 권한 범위 (주어진 액세스 토큰을 사용하여 액세스할 수 있는 리소스의 범위)

## oAuth 로직 플로우

<img src="https://i.ibb.co/Tv4njzD/oauth.jpg">

1. Client에서 oAuth로 `Authorization code` 요청 (인가 코드 요청)
2. oAuth에서 Client로 redirect URI를 통해 `Authorization code` 부여 (인가 코드 부여)
3. Client에서 Server로 `Authorization code` 전달 (인가 코드 전달)
4. Server에서 oAuth로 `Authorization code`를 보내서 `Access Token` 요청 (인가 코드 전달, 토큰 요청)
5. oAuth에서 Server로 `Access Token` 부여 (토큰 부여)
6. Server에서 Client로 `Access Token` 전달 (토큰 전달)

## 마치며

몇번 oAuth 로그인 방식을 적용해 보려고 시도해 봤지만 번번히 실패했었는데, oAuth의 동작 흐름을 정리해 보고 나니 어떤 부분에서 실수를 해왔었는지 명확히 이해할 수 있었다.

이후에 oAuth 로그인 방식을 적용하는 연습을 꾸준히 해봐야겠다.
