# PLAYBOX 게임 연동 가이드

PLAYBOX에 등록되는 게임은 PLAYBOX의 플레이어 프로필 정보를 사용할 수 있습니다.

게임에 아래 코드를 추가하세요.

```js
const PLAYBOX={

  profile:null,

  getProfile(callback){

    const handler=e=>{

      if(e.origin!==location.origin)
        return;

      if(e.data?.type!=='PLAYBOX_PROFILE')
        return;

      window.removeEventListener('message',handler);

      this.profile=e.data.profile;

      callback(this.profile);

    };

    window.addEventListener('message',handler);

    parent.postMessage(
      {type:'PLAYBOX_GET_PROFILE'},
      location.origin
    );

  }

};
```

## 프로필 가져오기

```js
PLAYBOX.getProfile(profile=>{

  console.log(profile.name);

});
```

받을 수 있는 정보:

```js
{
  name: "Guest3821",

  avatar: "<svg>...</svg>",

  appearance: {
    skin: "#f1c27d",
    shirt: "#ffffff",
    pants: "#444444",
    face: 0,
    hat: 2
  }
}
```

## 닉네임 사용

```js
PLAYBOX.getProfile(profile=>{

  document.getElementById('playerName').textContent=
    profile.name;

});
```

## 아바타 표시

HTML:

```html
<div id="playerAvatar"></div>
```

JavaScript:

```js
PLAYBOX.getProfile(profile=>{

  document.getElementById('playerAvatar').innerHTML=
    profile.avatar;

});
```

CSS:

```css
#playerAvatar{
  width:100px;
  height:150px;
}

#playerAvatar svg{
  width:100%;
  height:100%;
}
```

PLAYBOX에서 설정한 아바타가 게임 안에서도 동일하게 표시됩니다.

## 참고

게임은 PLAYBOX 내부 저장소를 직접 수정하지 않는 것을 권장합니다.

프로필 정보가 필요한 경우 PLAYBOX 프로필 API를 사용하세요.
